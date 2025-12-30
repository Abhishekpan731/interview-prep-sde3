# High-Level Design: Ticketmaster (Flash Sale Ticketing)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **View Events:** Users can browse events and view seat maps.
*   **Seat Selection:** Users can select specific seats or "Best Available".
*   **Temporary Hold:** Selecting a ticket "holds" it for a specific window (e.g., 5-10 minutes) to allow payment.
*   **Booking & Payment:** Complete purchase and issue ticket.

### 1.2 Non-Functional Requirements
*   **High Consistency:** **Strictly Critical**. Double booking a seat is the worst-case failure.
*   **Fairness:** First come (who successfully requests the hold), first served.
*   **Availability:** High availability for viewing; but for booking, we favor consistency (CP).
*   **Burst Scalability:** Must handle 10 million users trying to buy 50,000 tickets in the first 60 seconds.

---

## 2. Capacity Estimation & Scale

### 2.1 The "Taylor Swift" Problem
*   **Inventory:** 50,000 Seats.
*   **Demand:** 10 Million active users in the lobby.
*   **Peak Traffic:** All 10M users hit "Refresh" at 10:00:00 AM.
*   **QPS (Booking):** Could theoretically range from 100k to 1M requests/sec if not gated.

### 2.2 Storage
*   Data size is small (Seat maps, strings). The data *volume* isn't the problem; the data *contention* is.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **Virtual Waiting Room (Queue):** The shield that protects the system from crashing.
2.  **Booking Service:** Handles seat selection logic.
3.  **Active Hold Service (Redis):** Manages temporary locks on seats.
4.  **Persistence Service:** SQL Database for final bookings.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client([User Browser])
    
    subgraph "Entrance Layer"
        CDN[CDN (Static Seat Maps)]
        LB[Load Balancer]
        WaitRoom[Virtual Waiting Room / Queue Service]
    end
    
    subgraph "Core Booking"
        Gateway[API Gateway]
        BookingSvc[Booking Service]
        PaymentSvc[Payment Service]
    end
    
    subgraph "Data & State"
        Redis[(Redis Cluster: Seat Holds)]
        DB[(PostgreSQL: Final Tickets)]
    end

    Client --> CDN
    Client --> LB
    LB --> WaitRoom
    
    WaitRoom --"Admission Token"--> Client
    
    Client --"Token + Request"--> Gateway
    Gateway --> BookingSvc
    
    BookingSvc -->|1. Try Lock| Redis
    Redis --"Success/Fail"--> BookingSvc
    
    BookingSvc -->|2. Payment Webhook| PaymentSvc
    BookingSvc -->|3. Persist Ticket| DB
    
    subgraph "Cleanup"
        Scheduler[Expiry Scheduler]
    end
    Scheduler -.->|Release Expired Locks| Redis
```

---

## 4. Data Model

### 4.1 SQL Schema (Persistence)
```sql
CREATE TABLE Events (
    event_id UUID PRIMARY KEY,
    name VARCHAR(255),
    total_seats INT
);

CREATE TABLE Seats (
    seat_id UUID PRIMARY KEY,
    event_id UUID,
    section INT,
    row INT,
    number INT,
    status ENUM('AVAILABLE', 'BOOKED'), -- Only final statuses
    version INT -- For Optimistic Locking
);

CREATE TABLE Bookings (
    booking_id UUID PRIMARY KEY,
    user_id UUID,
    seat_id UUID,
    status ENUM('PENDING', 'CONFIRMED', 'CANCELLED'),
    created_at TIMESTAMP
);
```

### 4.2 Redis Structure (Hot State)
*   **Key:** `seat_lock:{event_id}:{seat_id}`
*   **Value:** `user_id`
*   **TTL:** 600 seconds (10 mins)

---

## 5. Core Workflows

### 5.1 The Virtual Waiting Room (Flow Control)
We cannot let 10M users hit the database.
1.  **User Enters:** At 9:50 AM, user enters lobby. Connects via WebSocket.
2.  **Queue Position:** User receives a random queue number (fairness shuffle).
3.  **Admission:** At 10:00 AM, the system calculates `Throughput Capacity` (e.g., 500 bookings/sec).
4.  **Token Issue:** System grants entry tokens to the first 1000 users.
5.  **User Action:** Only users with a valid, signed JWT token can call the `BookingService`. Others receive `429 Too Many Requests`.

### 5.2 Booking Flow (The Happy Path)
1.  **Select:** User clicks "Seat 12".
2.  **Lock Request:** `BookingService` sends `SET seat_lock:123:45 user_abc NX PX 600000` to Redis.
    *   `NX`: Only set if not exists.
    *   `PX`: Expire in 600,000ms.
3.  **Result:**
    *   **Success:** Redis returns "OK". User sees timer "10:00 to pay".
    *   **Fail:** Redis returns "Null". User sees "Seat taken".
4.  **Payment:** User pays via Stripe.
5.  **Commit:** `BookingService` writes row to **PostgreSQL**.
6.  **Cleanup:** Redis key expires naturally (or is explicitly deleted).

---

## 6. Deep Dive: Concurrency & Locking Strategies

**The Problem:** 100,000 people try to buy the *exact same* specific seat (Front Row, Center) at 10:00:00.001.

### Option 1: Optimistic Locking (Database)
```sql
UPDATE Seats SET user_id = 'ABC' WHERE seat_id = 1 AND user_id IS NULL;
```
*   **Pros:** ACID compliance.
*   **Cons:** Database cannot handle 100,000 concurrent writes for the same row. It will lock up or timeout connections.

### Option 2: Active Queueing
Put all requests for `Seat 1` into a Kafka topic. A single consumer reads and processes.
*   **Pros:** Serializes access perfectly.
*   **Cons:** High latency. User is waiting "Processing..." for seconds/minutes. Complexity of async feedback loop.

### Option 3: Redis Distributed Lock (Chosen Approach)
Use Redis `SET NX` (Set if Not Exists).
*   **Speed:** Redis operations take microseconds.
*   **Throughput:** Single Redis node handles ~100k OPS. Redis Cluster scales linear.
*   **Triage:** The 99,999 failed requests are rejected at the Cache layer, never touching the PostgreSQL Disk.
*   **Durability Risk:** If Redis crashes, locks are lost.
    *   *Mitigation:* Use Redis AOF (Append Only File) with `fsync` every second. It's acceptable if the system "forgets" a lock during a catastrophic crash; the Database remains the source of truth for *purchased* tickets.

---

## 7. Reliability & Fault Tolerance

### 7.1 "Zombie" Holds (Ghost Tickets)
User selects seat -> System holds in Redis -> User closes browser.
*   **Impact:** Seat is locked for 10 mins, nobody can buy it.
*   **Solution:** Redis TTL (Time To Live). The key automatically evaporates after 10 mins. The seat becomes available again for the next user.

### 7.2 Database vs Redis Inconsistency
What if a User pays, but the DB write fails?
*   **Reconciliation:** We need a background "Payment Reconciler".
    *   If Payment Gateway says "Success" but DB says "Pending", the job forces the DB update.
    *   If DB says "Confirmed" but Payment failed, the job refunds (if needed) and frees the seat.

---

## 8. Frontend Optimizations

*   **Seat Map Caching:** The seat map image (SVG/Canvas) is static. Cache strongly on CDN.
*   **State Delta:** Don't reload the map. Use WebSockets to push array of `sold_seat_ids`. Client paints them grey.

---

## 9. Security

*   **Bot Prevention:**
    *   **CAPTCHA:** Required before entering Waiting Room.
    *   **Device Fingerprinting:** Detect headless browsers (Puppeteer/Selenium).
    *   **Limit:** Max 4 tickets per User Account / Credit Card.

## 10. Summary of Key Techniques
1.  **Rate Limiting:** Virtual Waiting Room (Leaky Bucket at the edge).
2.  **Concurrency Control:** Redis `SET NX` for lightweight locking.
3.  **Consistency:** DB is the source of truth; Redis is the traffic shield.
