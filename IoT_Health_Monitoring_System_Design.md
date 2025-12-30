# High-Level Design: IoT Health Monitoring System

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Device Connectivity:** Monitor real-time status (Online/Offline) of 1 Million IoT devices.
*   **Telemetry:** Collect sensor data (Battery, Temperature, CPU) periodically.
*   **Command & Control:** Send reboot/config commands to devices.
*   **Dashboard:** Visualize cluster health, map view, and specific device details.
*   **Alerting:** Trigger alarms if > 10% of devices in a region go offline.

### 1.2 Non-Functional Requirements
*   **Protocol Efficiency:** Devices often have poor internet (2G/3G). HTTP is too heavy. Must use **MQTT**.
*   **Scalability:** Active active connections for 1M devices.
*   **Latency:** Status status change reflection < 5 seconds.
*   **Reliability:** Packet loss is expected. System must handle "flapping" devices.

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **Devices:** 1 Million.
*   **Heartbeat Interval:** Every 30 seconds.
*   **Ingest QPS:** $1,000,000 / 30 \approx \mathbf{33,000 \text{ msg/sec}}$.
*   **Telemetry Payload:** 0.5 KB.
*   **Bandwidth:** $33k \times 0.5 \text{ KB} \approx 16.5 \text{ MB/s}$ Ingress (Manageable).

### 2.2 Connection Limits
*   **Socket Descriptors:** A single server cannot hold 1M TCP connections.
*   *Solution:* We need a cluster of MQTT Brokers. If 1 server handles 50k connections, we need **20+ Brokers**.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **IoT Gateway (MQTT Broker):** Entry point. Handles TLS termination, auth, and effectively "holds" the connection.
2.  **Message Bus (Kafka):** Buffers high-speed telemetry.
3.  **Device Shadow Service:** Stores the "Desired" vs "Reported" state of a device (JSON document).
4.  **Stream Processor (Flink/KStream):** Detects anomalies and offline states.
5.  **Time Series DB:** Stores history.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Device([IoT Device])
    
    subgraph "Edge / Ingestion"
        LB[Network Load Balancer]
        MQTT[MQTT Broker Cluster]
        Auth[mTLS / Auth Service]
    end
    
    subgraph "Processing Layer"
        Kafka[(Kafka: Telemetry Topic)]
        Flink[Stream Processor (Status Detector)]
        ShadowSvc[Device Shadow Service]
    end
    
    subgraph "Storage & Serving"
        Redis[(Redis: Real-time Status)]
        TSDB[(InfluxDB / Timescale: History)]
        API[Dashboard API]
        UI[Admin Dashboard]
    end
    
    subgraph "Alerting"
        AlertMgr[Alert Manager]
    end

    Device <-->|MQTT (TCP/TLS)| LB
    LB --> MQTT
    MQTT -->|Auth| Auth
    
    %% Data Flow
    MQTT -->|Publish Metrics| Kafka
    MQTT -->|Connect/Disconnect Events| Kafka
    
    Kafka --> Flink
    
    Flink -->|Write Heartbeat| TSDB
    Flink -->|Update Status| Redis
    Flink -->|Sync State| ShadowSvc
    
    Flink -->|Anomaly Detected| AlertMgr
    
    UI --> API
    API -->|Get Status| Redis
    API -->|Get Graphs| TSDB
    API -->|Get Config| ShadowSvc
```

---

## 4. Deep Dive: Protocol & Connectivity (MQTT)

### 4.1 Why MQTT over HTTP?
*   **Overhead:** HTTP headers are huge. MQTT header is 2 bytes.
*   **Keep-Alive:** MQTT maintains a persistent TCP connection. Push is possible.
*   **QoS (Quality of Service):**
    *   `QoS 0`: Fire and forget (Battery update).
    *   `QoS 1`: At least once (Alerts).
    *   `QoS 2`: Exactly once (Billing data).

### 4.2 Handling Offline Detection (The LWT)
How do we know a device died if it just stops sending packets?
*   **Heartbeat Timeout (Passive):** Flink checks "Last seen time". If `now() - last_seen > 1 min`, mark offline. (Slow).
*   **Last Will and Testament (LWT) (Active):**
    1.  Device connects to Broker and registers a "Will" message: `Topic: status/123`, `Payload: OFFLINE`.
    2.  If the Broker detects a socket break (TCP RST) or timeout without a graceful disconnect, the **Broker** automatically publishes the Will message to the topic.
    3.  Flink consumes this message effectively instantly.

---

## 5. Device Shadow (Digital Twin)

**Problem:** You want to reboot a device, but it is currently offline/sleeping.
**Solution:** **Device Shadow Pattern**.
*   **Shadow Document (JSON in MongoDB/DynamoDB):**
    ```json
    {
      "reported": { "fw_version": "1.0", "fan_speed": 50 },
      "desired":  { "fw_version": "1.0", "fan_speed": 100 }
    }
    ```
*   **Workflow:**
    1.  User updates `desired.fan_speed = 100`.
    2.  Shadow Service saves this *intent*.
    3.  When device comes **Online**, it subscribes to `$aws/things/mydevice/shadow/update/delta`.
    4.  Device receives `{ "fan_speed": 100 }`.
    5.  Device applies change and updates `reported.fan_speed = 100`.

---

## 6. Scalability Strategies

### 6.1 Partitioning MQTT Cluster
*   **Session Stickiness:** MQTT connections are stateful. The LB must use **Source IP affinity** or handle session routing.
*   **Cluster Discovery:** If Broker A dies, 50,000 devices reconnect.
    *   *Thundering Herd:* Devices must use **Randomized Exponential Backoff** when reconnecting to avoid crushing the LB.

### 6.2 Data Partitioning (Kafka/Storage)
*   **Partition Key:** `DeviceID`. Ensures all messages from Device X go to the same Kafka partition and Flink worker. Order is preserved.

---

## 7. Storage: Time Series Optimization

**Problem:** 1M devices * 1 telemetry point/min = 1.4 Billion points/day.
**Solution:**
1.  **Retention:** Keep raw data for 7 days.
2.  **Rollups:** Aggregation pipeline (Flink) converts 1-minute samples into 1-hour averages (`avg`, `min`, `max`) for long-term storage.
3.  **TSDB Selection:** **TimescaleDB** (Postgres extension) or **InfluxDB**.
    *   *Why?* Optimized for high ingest rate and time-bucket queries.

---

## 8. Security

*   **mTLS (Mutual TLS):** The gold standard for IoT.
    *   Server verifies device certificate.
    *   Device verifies server certificate.
    *   Prevents rogue devices from connecting.
*   **Just-in-Time (JIT) Provisioning:** When a manufacturing device connects for the first time, it presents a factory cert and is auto-registered in the fleet.

---

## 9. Summary

1.  **Protocol:** MQTT is non-negotiable for scale and battery efficiency.
2.  **State Management:** Use **Device Shadows** to decouple app commands from device availability.
3.  **Real-Time Status:** Use **LWT (Last Will and Testament)** for instant offline detection.
4.  **Scale:** Shard Brokers and Kafka partitions by Device ID.
