# System Design: Facebook Messenger / WhatsApp (Detailed)

*Designed by a Principal Engineer for High-Scale Distributed Systems Interviews.*

---

## 1. High-Level Architecture Visualization

Below is the conceptual architecture for a globally distributed chat system.

![Messenger Architecture](messenger_system_architecture_1767126874785.png)

### Proper Architectural Overview
The system is built on a **Stateful Gateway Architecture**. Unlike standard stateless web apps, chat requires the server to "push" data to the client.

| Component | Responsibility |
| :--- | :--- |
| **Chat Gateway (Websockets)** | Maintains persistent TCP/Websocket connections. Per server, it can hold 50k-100k connections. |
| **Session Service** | A global Distributed Hash Table (Redis) mapping `UserID -> Gateway_IP`. |
| **Message Service** | Validates messages, generates IDs, and handles the "Tick" logic. |
| **Presence Service** | Tracks online/offline status via heartbeats. |
| **HBase / Cassandra** | Persistent storage for billions of messages, optimized for sequential range queries. |

---

## 2. Deep Dive: End-to-End Message Delivery Flow

This is the most critical part of the system. We ensure "At-least-once delivery" through a series of acknowledgments.

### Diagram: Message Flow & The "Ticks" Logic
```mermaid
sequenceDiagram
    participant A as Sender (User A)
    participant G1 as Gateway Server 1
    participant MS as Message Service
    participant DB as Chat DB (HBase)
    participant SS as Session Service (Redis)
    participant G2 as Gateway Server 2
    participant B as Recipient (User B)

    Note over A, B: User A sends a message to User B
    A->>G1: [1] Send Message (Payload + TempID)
    G1->>MS: [2] Process Message
    MS->>DB: [3] Persist Message (Status: SENT)
    MS-->>G1: [4] MessageID Generated
    G1-->>A: [5] ACK (Sent) -> (Single Tick ✓)

    Note over MS, SS: Locate User B
    MS->>SS: [6] Where is User B?
    SS-->>MS: [7] B is on Gateway Server 2

    Note over MS, G2: Forward to B's Gateway
    MS->>G2: [8] Deliver MessageID 101
    G2->>B: [9] Push via Websocket
    B-->>G2: [10] App Received ACK
    G2->>MS: [11] Status Update: DELIVERED

    Note over MS, G1: Notify Sender of Delivery
    MS->>G1: [12] Notify A: Delivered
    G1->>A: [13] Push Status Update -> (Double Tick ✓✓)

    Note over B: User B opens the chat
    B->>G2: [14] Read ACK (Manual Action)
    G2->>MS: [15] Status Update: READ
    MS->>G1: [16] Notify A: Read
    G1->>A: [17] Push Status Update -> (Blue Tick ✓✓)
```

### Explanation of the Ticks:
1.  **Single Tick (Sent)**: This means the message has reached the server cluster and is safely stored in the database. If User A's phone dies now, the message is still "Sent".
2.  **Double Tick (Delivered)**: This confirms that the recipient's device received the packet. If User B is in a tunnel, they won't get the double tick until they regain signal.
3.  **Blue Tick (Read)**: A specific application-layer event triggered when the UI renders the message in the foreground.

---

## 3. Handling Offline Messages & Synchronization

What happens when User B is offline? The "Push" fails, and we fall back to "Pull" upon reconnection.

### Diagram: Offline Sync Logic
```mermaid
graph TD
    subgraph "Server Path"
        M[New Message] --> Check{Recipient Online?}
        Check -- No --> Store[(HBase History)]
        Store --> Flag[Set 'Has Unread' in Redis]
    end

    subgraph "Recipient Reconnection"
        B[User B Comes Online] --> Auth[Authenticate]
        Auth --> CheckFlag{Check Redis Flag}
        CheckFlag -- True --> Fetch[Pull Range Request]
        Fetch --> HBase[(HBase)]
        HBase --> Diff[Latest Messages Since last_id]
        Diff --> Sync[Deliver to Device]
    end
```

### Explaining Sync vs. Push:
*   **Push (Real-time)**: High performance, low latency. Used when both parties are connected.
*   **Pull (Reconnection)**: High reliability. When the app starts, it asks: *"Give me everything after MessageID X"*. This ensures no messages are lost during network transitions.

---

## 4. Presence Service: Scalable Heartbeats

### Diagram: The Heartbeat Mechanism
```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant PresenceSvc
    participant Redis

    loop Every 30s
        Client->>Gateway: Ping (Keep-Alive)
        Gateway->>PresenceSvc: User X is Active
        PresenceSvc->>Redis: SETEX presence:X 60 "Online"
    end

    Note over Redis: If no ping for 60s, Key auto-expires

    Note over Client, Redis: Friend checks Profile
    Client->>PresenceSvc: GetStatus(User X)
    PresenceSvc->>Redis: GET presence:X
    Redis-->>PresenceSvc: "Online" or nil
    PresenceSvc-->>Client: "Online" or "Last seen 5m ago"
```

---

## 5. Group Chat Fan-out

### Proper Handling of Large Groups:
1.  **Message Service** identifies the message is for a GroupID.
2.  Fetches Group Member List (Cached in Redis/Memcached).
3.  Iterates through 500 members.
4.  For **Online Members**: Push via their respective Gateways.
5.  For **Offline Members**: Do nothing (they will "Sync" on reconnect).

**Optimization**: We don't send 500 separate status updates for every "Double Tick" in a large group to the sender to prevent "ACK Storms". Instead, we aggregate them.

---

## 6. Storage Schema (Deep Dive)

We use a **Wide-Column Store (HBase/Cassandra)**.

### Key Choice: `chat_id` as Partition Key
*   **Why?**: Range queries are fast. When you scroll up in a chat, you are fetching a range of messages.
*   **Physical Storage**: By partitioning by `chat_id`, all messages between Alice and Bob are stored on the same physical machine and often the same block of disk. This makes "Scroll Up" operations involve **one disk seek** instead of thousands.

### Schema Table: `messages`
| Partition Key (`chat_id`) | Clustering Key (`message_id` DESC) | Attributes |
| :--- | :--- | :--- |
| `Alice_Bob_123` | `20251231_100501` | "Hello!" |
| `Alice_Bob_123` | `20251231_100500` | "Hi" |

---
*Created by Antigravity for Principal Engineer System Design Interviews.*
