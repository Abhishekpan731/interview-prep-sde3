# High-Level Design: Amazon (E-commerce Platform)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Search & Browse:** Users can search products and view details (Catalog).
*   **Shopping Cart:** Add/Remove items, persistent state across devices.
*   **Checkout & Order:** Place orders, handle inventory checks, and decrement stock.
*   **Payment:** Secure integration with payment gateways.
*   **Order History:** View past orders and status updates.

### 1.2 Non-Functional Requirements
*   **Consistency:** **Strong Consistency** for Inventory (Cannot oversell). **Eventual Consistency** for Recommendations/Reviews is fine.
*   **Availability:** High availability for Product Catalog and Search (browsing). Order service favors Consistency (CP over AP during partition).
*   **Latency:** Low latency for browsing (< 200ms). Checkout can take slightly longer (< 2s).
*   **Scalability:** Handle massive spikes (e.g., Black Friday - 10x traffic).

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **DAU:** 100 Million.
*   **Orders:** 10 Million orders/day.
*   **Browsing:** 100M users * 20 items = 2 Billion views/day.
*   **QPS (Read - Catalog):** ~23,000 QPS (Average). Peak 100k+.
*   **QPS (Write - Order):** 10M / 86400 ≈ 115 orders/sec. Peak (Black Friday) can reach **5,000+ orders/sec**.

---

## 3. High-Level Architecture

### 3.1 Logical Microservices
1.  **Catalog Service:** Manages product metadata. Read-heavy.
2.  **Cart Service:** Manages temporary user state. Write-heavy.
3.  **Inventory Service:** critical component for stock management.
4.  **Order Service:** Orchestrates the checkout process.
5.  **Payment Service:** Interface with Stripe/PayPal.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client([User App])
    
    subgraph "Edge Layer"
        CDN[CDN (Images/Static Assets)]
        LB[Load Balancer]
        Gateway[API Gateway]
    end
    
    subgraph "Core Services"
        CatalogSvc[Catalog Search Service]
        CartSvc[Shopping Cart Service]
        OrderSvc[Order Service]
        InventorySvc[Inventory Service]
        PaymentSvc[Payment Service]
    end
    
    subgraph "Data Storage"
        ES[(ElasticSearch)]
        DBCatalog[(NoSQL: Mongo/Dynamo)]
        DBCart[(Redis / DynamoDB)]
        DBOrder[(Sharded PostgreSQL)]
        DBInventory[(Relational DB / Spanner)]
    end
    
    subgraph "Async / External"
        Kafka[(Kafka: Order Events)]
        PaymentGateway[External Payment Gateway]
        NotifSvc[Notification Service]
    end

    Client --> CDN
    Client --> LB
    LB --> Gateway
    
    Gateway --> CatalogSvc
    Gateway --> CartSvc
    Gateway --> OrderSvc
    
    CatalogSvc --> DBCatalog
    CatalogSvc --> ES
    
    CartSvc --> DBCart
    
    OrderSvc -->|1. Reserve Stock| InventorySvc
    InventorySvc --> DBInventory
    
    OrderSvc -->|2. Process $$| PaymentSvc
    PaymentSvc --> PaymentGateway
    
    OrderSvc -->|3. Save Order| DBOrder
    OrderSvc -->|4. Publish Event| Kafka
    
    Kafka --> NotifSvc
    Kafka -->|Data Warehouse| Analytics
```

---

## 4. Data Model & Storage

### 4.1 Product Catalog (Read Optimize)
Products have flexible schemas (a Shirt has size/color; a Laptop has RAM/CPU).
*   **Choice:** **Document Store (MongoDB)** or **DynamoDB**.
*   **Structure:** JSON document storing attributes.

### 4.2 Shopping Cart (High Throughput Key-Value)
*   **Choice:** **Redis** (for active sessions) or **DynamoDB** (for persistence).
*   **Key:** `cart:{user_id} -> { item_id: qty, timestamp }`.

### 4.3 Inventory & Orders (ACID Required)
*   **Choice:** **PostgreSQL** (Sharded by `order_id` or `user_id`).
*   **Why?** We need Transactional integrity. "Money deduction" and "Order creation" should trigger together.

---

## 5. Core Workflows

### 5.1 Shopping Cart (Add to Cart)
1.  User clicks "Add to Cart".
2.  **State:** Is user logged in?
    *   **Yes:** Store in DB `cart:{user_id}`.
    *   **No:** Store in Browser Cookie or LocalStorage.
3.  **Merge:** When a guest logs in, the client sends the local cart to the server. The Cart Service merges the Cookie-cart with the DB-cart.

### 5.2 Checkout & Payment (The "Saga")
1.  **Checkout Start:** User clicks "Checkout".
2.  **Inventory Reservation:** Order Service calls Inventory Service to "reserve" items for 10 minutes.
    *   *Inventory updates:* `reserved_stock += qty`.
3.  **Payment Processing:** Order Service calls Payment Service.
    *   *Idempotency Key:* `order_id` is sent to Stripe to prevent double charging.
4.  **Completion:**
    *   **Success:** Order Service creates Order. Inventory Service "commits" reservation (`stock -= qty`, `reserved_stock -= qty`).
    *   **Failure:** Payment fails. Order Service initiates "Compensating Transaction" -> Release Inventory (`reserved_stock -= qty`).

---

## 6. Scalability & Performance

### 6.1 Caching Catalog
*   **Strategy:** Cache-Aside with Redis.
*   **TTL:** Product prices don't change often. TTL 30 mins.
*   **Search:** Use ElasticSearch for full-text search and filtering (facets).

### 6.2 Managing Spikes (Black Friday)
*   **Queue-based Load Leveling:** Place order requests into Kafka. Order Workers process them at their own pace.
*   **Note:** This creates an async experience ("We received your order, we will email you when confirmed"). Avoids DB collapse.

---

## 7. Consistency & Inventory Management (The Hardest Part)

### 7.1 The "Overselling" Problem
User A and User B both see "1 item left". Both click buy.
Traditional `READ-MODIFY-WRITE` causes a race condition.

### 7.2 Solution 1: Pessimistic Locking (SQL)
```sql
START TRANSACTION;
SELECT quantity FROM inventory WHERE product_id = 123 FOR UPDATE;
-- Check quantity > 0
UPDATE inventory SET quantity = quantity - 1 WHERE product_id = 123;
COMMIT;
```
*   **Pros:** Safe.
*   **Cons:** **Slow**. Blocks all other users trying to buy item 123. Deadlocks possible.

### 7.3 Solution 2: Optimistic Locking (Conditional Update) - PREFERRED
```sql
UPDATE inventory 
SET quantity = quantity - 1 
WHERE product_id = 123 AND quantity - 1 >= 0;
```
*   **Pros:** Non-blocking. Fast.
*   **Cons:** High failure rate under massive concurrency (1000 users fighting for 1 item).

### 7.4 Solution 3: Redis Lua Script (For Flash Sales)
*   Keep "Hot" inventory in Redis.
*   Execute Lua script (atomic) to decrement.
*   Async sync to PostgreSQL.
*   **Why?** Redis is single-threaded and operations are atomic. Handles 100k OPS.

---

## 8. Reliability & Fault Tolerance

### 8.1 Distributed Transactions (Saga Pattern)
Monolithic apps use **Two-Phase Commit (2PC)**. Microservices use **Saga**.
*   **Choreography:** Services listen to events (InventoryLocked -> ProcessPayment).
*   **Orchestration (Preferred for Orders):** A central "Order Orchestrator" (State Machine) tells services what to do.

### 8.2 Idempotency (Payment)
*   Network timeouts are real. If Order Service sends "Charge $100" and doesn't get a response, it retries.
*   **Must send generic `request_id`** (UUID).
*   Payment Gateway checks: "Did I already process `request_id`? Yes? Return previous success result. No? Charge card."

---

## 9. Security

*   **PCI-DSS Compliance:** Never store raw credit card numbers. Use Tokenization (Stripe Elements).
*   **Fraud Detection:** Analyze purchase velocity, IP geo-location vs Billing Address.

---

## 10. Monitoring

*   **Business Metrics:** Cart Abandonment Rate, Order Success Rate.
*   **Tech Metrics:** DB Lock contention time, Payment Gateway Latency.

---

## 11. Evolution

*   **Personalization:** Machine Learning models (collaborative filtering) for "Users who bought X also bought Y".
*   **Marketplace:** Allow 3rd party sellers. Introduces complexity in inventory (multiple sellers for one SKU).
