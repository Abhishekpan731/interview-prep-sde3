# High-Level Design: Universal Notification Service

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Multi-Channel Support:** Send notifications via Email, SMS, Push (iOS/Android), and Slack/Webhook.
*   **Template Support:** Dynamic content insertion (e.g., "Hello {{user_name}}, your OTP is {{code}}") with multi-language support.
*   **Bulk Sending:** Ability to blast marketing emails to 1M users.
*   **Scheduling:** "Send this email tomorrow at 9 AM".
*   **Tracking:** Track delivery status (Sent, Delivered, Read, Clicked).
*   **Preferences:** Respect user opt-outs (GDPR/CAN-SPAM).

### 1.2 Non-Functional Requirements
*   **Reliability:** **Zero Data Loss**. Even if the email provider fails, the system must retry.
*   **Latency:**
    *   **Critical (OTP/Alerts):** < 5 seconds p99 status.
    *   **Marketing:** Eventual delivery (minutes/hours) is fine.
*   **Throughput:** Handle burst traffic (e.g., Breaking News -> 10M Push Notifications in 5 mins).
*   **Scalability:** Horizontally scalable workers.

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **Daily Notifications:** 100 Million.
    *   Push: 50M
    *   Email: 40M
    *   SMS: 10M
*   **QPS:** 100M / 86400 ≈ **1,150 QPS** (Average).
*   **Peak QPS:** During events (e.g., Black Friday), 50k - 100k QPS.

### 2.2 Storage
*   **Notification Logs:** Storing `status`, `metadata`, `content` for compliance/debugging.
*   **Size:** 1KB per log * 100M = **100 GB/day**.
*   **Retention:** Keep hot logs for 30 days (Postgres/Cassandra), archive to S3 after.

---

## 3. High-Level Architecture

### 3.1 Logical Architecture
The system follows a **Queue-Based Event-Driven Architecture** to decouple the *request* to send from the *actual* sending.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client([Client Services / Microliths])
    
    subgraph "Interface Layer"
        LB[Load Balancer]
        API[Notification Gateway]
        Auth[Auth Service]
    end
    
    subgraph "Core Services"
        TemplateSvc[Template Service]
        PrefSvc[User Preference Service]
        Scheduler[Scheduler Service]
        LogSvc[Archive/Log Service]
    end
    
    subgraph "Message Queues (RabbitMQ / Kafka)"
        Q_Critical[(Queue: Critical / OTP)]
        Q_Standard[(Queue: Transactional)]
        Q_Bulk[(Queue: Marketing/Bulk)]
        DLQ[(Dead Letter Queue)]
    end
    
    subgraph "Workers & Processing"
        Worker[Stateless Workers]
        RetryMgr[Retry Manager]
    end
    
    subgraph "Vendor Integration"
        SendGrid[Email: SendGrid / AWS SES]
        Twilio[SMS: Twilio / Nexmo]
        FCM[Push: FCM / APNS]
    end

    subgraph "Data Store"
        Redis[(Redis: Dedupe/Rate Limit)]
        DB[(PostgreSQL/Cassandra)]
        Mongo[(MongoDB: Templates)]
    end

    %% Flow
    Client -->|POST /send| LB
    LB --> API
    API -->|Auth Check| Auth
    API -->|Rate Limit| Redis
    API -->|Get Prefs| PrefSvc
    
    API -->|Persist Request| DB
    API -->|Enqueue| Q_Critical
    API -->|Enqueue| Q_Standard
    API -->|Enqueue| Q_Bulk
    
    Q_Critical --> Worker
    Q_Standard --> Worker
    
    Worker -->|Fetch Template| TemplateSvc
    TemplateSvc -.-> Mongo
    
    Worker -->|Send Request| SendGrid
    Worker -->|Send Request| Twilio
    Worker -->|Send Request| FCM
    
    %% Handling Failures
    SendGrid --"5xx Error"--> Worker
    Worker -->|Re-queue (Exponential Backoff)| Q_Standard
    Worker -->|Max Retries Exceeded| DLQ
    
    %% Analytics
    Worker -->|Update Status| DB
    SendGrid -.->|Webhook: Delivered| API
```

---

## 4. Deep Dive: Priority Queues

**Problem:** If we dump 1 Million marketing emails into the queue, the "Password Reset OTP" request behind them will be delayed by 30 minutes. This is unacceptable.

**Solution:** **Tiered Priority Queues**.
We use RabbitMQ (or separate Kafka Topics) for isolation.

1.  **Critical Queue (`priority_high`):**
    *   **Content:** OTPs, Security Alerts, Payment Confirmations, Fraud warnings.
    *   **SLA:** < 5 seconds.
    *   **Resources:** Dedicated, reserved active workers. Autoscaling triggers at low queue depth.
2.  **Transactional Queue (`priority_normal`):**
    *   **Content:** "Your order has shipped", "Welcome to the platform".
    *   **SLA:** < 1 minute.
3.  **Bulk/Marketing Queue (`priority_low`):**
    *   **Content:** Weekly Newsletters, Promo offers.
    *   **SLA:** Hours.
    *   **Resources:** Spot instances (cheaper). Throttled to prevent downstream vendor blocking.

**Technical Implementation:**
API receives `priority` flag.
```json
{
  "user_id": "123",
  "type": "OTP",
  "priority": "HIGH",
  "channel": "SMS"
}
```
API Router pushes to `rabbit_queue_high` vs `rabbit_queue_low`.

---

## 5. Deep Dive: Reliability & Retry Mechanism

**Problem:** Third-party providers (Twilio/SendGrid) fail often (Network timeouts, Rate limits, 500 errors).

**Strategy: Distributed Retry with Exponential Backoff**

1.  **Worker Try 1:**
    *   Call SendGrid.
    *   Response: `HTTP 503 Service Unavailable`.
2.  **Retry Decision:**
    *   Is error transient? Yes (5xx). (If 4xx "Invalid Email", do not retry).
    *   Calculate delay: `delay = initial_wait * (2 ^ retry_count)`. e.g., 2s, 4s, 8s, 16s...
3.  **Re-Queueing:**
    *   Push message back to a specialized **Delay Queue** (using RabbitMQ `x-delayed-message` plugin or Redis Sorted Set `ZADD retries timestamp`).
4.  **Dead Letter Queue (DLQ):**
    *   After `MaxRetries` (e.g., 5), move message to DLQ.
    *   **Manual Intervention:** Alerts fire. Engineers check if it's a bug or a vendor outage.

**Circuit Breaker:**
If SendGrid returns 100% errors for 1 minute, **Resilience4j** circuit breaker opens.
*   **Failover:** Automatically switch traffic to backup provider (e.g., AWS SES -> Mailgun).

---

## 6. Template Management Service

**Requirement:** Decouple code from content. Product managers want to change email text without deploying code.

### 6.1 Data Model (MongoDB)
Why NoSQL? Templates vary wildly in structure and size.
```json
{
  "_id": "tpl_welcome_email_v2",
  "name": "Welcome Email",
  "channels": ["EMAIL", "PUSH"],
  "versions": [
    {
      "v_id": 1,
      "locale": "en-US",
      "subject": "Welcome {{name}}!",
      "body_html": "<h1>Hi {{name}}</h1>...",
      "variables": ["name", "activation_link"]
    },
    {
      "v_id": 1,
      "locale": "es-ES",
      "subject": "¡Hola {{name}}!",
      ...
    }
  ]
}
```

### 6.2 Rendering Engine
*   **Mustache / Jinja2 / Handlebars:** Worker fetches template from Cache (Redis) -> Compiles with payload -> Sends HTML.
*   **Validation:** When API accepts a request, it validates that `payload` contains all `variables` required by the template.

---

## 7. Deduplication & Rate Limiting

**Problem:** A buggy microservice triggers the "Order Created" event 50 times in 1 second. The user receives 50 emails.

**Solution:** **Semantic Deduplication using Redis**.
1.  **Key Generation:** `dedup_key = hash(user_id + template_id + timeframe_bucket)`.
    *   e.g., `dedup:user_123:order_conf:2024-01-01T10:00`.
2.  **Check:**
    ```lua
    -- Lua Script for Atomicity
    if redis.call("SET", key, "1", "NX", "EX", 60) then
        return "SEND"
    else
        return "DUPLICATE"
    end
    ```
3.  **Rate Limiting:** Protect users from spam. Max 3 SMS/hour per user.
    *   Implementation: Token Bucket in Redis.

---

## 8. Monitoring & Observability

### 8.1 Tracking Delivery (The Loopback)
Sending is half the battle. Knowing it arrived is the other half.
*   **Webhooks:** Setup callback URLs with providers.
    *   SendGrid -> POST `api.mysystem.com/webhook/email` -> Body: `{ msg_id: "xyz", status: "Delivered" }`.
*   **Consistency:** Providers have different statuses. Normalize them:
    *   `SENT` -> `DELIVERED` -> `READ` -> `CLICKED` -> `FAILED`.

### 8.2 Metrics (Prometheus/Grafana)
*   **Queue Depth:** Helps autoscaling workers.
*   **End-to-End Latency:** Time from `POST /send` to `Vendor Acknowledgement`.
*   **Vendor Success Rate:** Detect if Twilio is degrading.

---

## 9. Security

*   **App Authentication:** Internal services use Client Credentials Flow (OIDC) or Mutual TLS (mTLS) to call Notification Service.
*   **PII Protection:** Do not log the `body` of specific OTPs or sensitive emails. Mask PII in logs.
*   **Credentials:** Store Vendor API Keys in Vault/AWS Secrets Manager, injected into Workers at runtime.

---

## 10. Summary
This design prioritizes **Reliability** (using Retries/DLQ) and **Responsiveness** (using Priority Queues). It abstracts the complexities of different vendors, allowing the core application to simply "Fire and Forget" notifications.
