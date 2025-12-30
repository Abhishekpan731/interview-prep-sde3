# High-Level Design: Dropbox / Google Drive (Cloud File Storage)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **File Upload/Download:** Support for storing files of any size (small docs to large videos).
*   **Synchronization:** Automatic syncing across multiple devices (Desktop, Mobile, Web).
*   **Versioning:** Keep history of file changes and allow restoration.
*   **Sharing:** Share files/folders via public links or user permissions.
*   **Offline Support:** Users can edit files offline; changes sync when online.

### 1.2 Non-Functional Requirements
*   **Consistency:** Strong consistency. All devices must eventually see the exact same file state.
*   **Durability:** 99.999999999% (11 nines). Data loss is unacceptable.
*   **Availability:** 99.99%.
*   **Efficiency:** Minimize bandwidth usage (sync only changed parts).
*   **Scalability:** 500M users, Petabytes of data.

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **DAU:** 50M (10% of 500M registered).
*   **QPS:** 50M * 20 requests/day ≈ **12,000 QPS**.
*   **Peak QPS:** ~25,000 QPS.

### 2.2 Storage Estimates
*   **Average Storage per User:** 10 GB.
*   **Total Storage:** 500M * 10 GB = **5 Exabytes (EB)**.
*   **Metadata:** average 500 files per user. 500M * 500 * 1KB ≈ **250 TB** of metadata.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **Client Application:** Heavy client logic. Hashing, Chunking, Local Database, Watcher.
2.  **Block Server:** Handles uploading/downloading raw file blocks (chunks) to Cloud Storage (S3).
3.  **Metadata Service:** Manages file hierarchy, permissions, and versions.
4.  **Synchronization Service:** Notification system to push updates to connected clients.
5.  **Offline Backup:** Cold storage for old versions.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client[Desktop Client]
    
    subgraph "Block Layer (Data)"
        BlockSvc[Block Service]
        S3[(Amazon S3 / Cold Storage)]
    end
    
    subgraph "Metadata Layer (Control)"
        LB[Load Balancer]
        Gateway[API Gateway]
        MetaSvc[Metadata Service]
        SyncSvc[Notification / Sync Service]
        DB_Meta[(Sharded MySQL / TiDB)]
        Cache[(Redis: Meta Cache)]
    end
    
    subgraph "Async Processing"
        MQ[(Kafka)]
        DedupWorker[Deduplication Worker]
    end

    Client -->|Upload Blocks (Chunked)| BlockSvc
    BlockSvc --> S3
    
    Client -->|Update File Metadata| LB
    LB --> Gateway
    Gateway --> MetaSvc
    
    MetaSvc --> DB_Meta
    MetaSvc --> Cache
    
    MetaSvc -->|File Updated| MQ
    MQ --> SyncSvc
    MQ --> DedupWorker
    
    SyncSvc -.->|WebSocket / Long Poll| Client
```

---

## 4. Data Model & Storage

### 4.1 Metadata Database (ACID is Critical)
We need transactional integrity. Moving a file from Folder A to Folder B must be atomic.
*   **Choice:** **Sharded MySQL (Vitess)** or **CockroachDB**.
*   **Tables:**
    *   `Users`: User info.
    *   `Namespaces`: Represents a root folder or shared folder.
    *   `Files`: `file_id`, `namespace_id`, `parent_id`, `latest_version`, `is_folder`.
    *   `Versions`: `version_id`, `file_id`, `block_list_order` (A JSON list of block hashes).
    *   `Blocks`: `hash` (SHA-256), `s3_url`, `size`.

### 4.2 Block Storage (The "Blob")
*   **Choice:** **Amazon S3 / GCS**.
*   **Strategy:** Immutable blocks. Once a block is written, it is never modified.

---

## 5. Core Workflows

### 5.1 The "Magic" of Block-Level Sync
We do NOT upload the whole file when 1 byte changes.
1.  **Chunking:** The client splits a 1GB file into smaller blocks (e.g., 4MB fixed size or CDC - Content Defined Chunking).
2.  **Hashing:** Calculate SHA-256 hash for each block.
3.  **Check:** Client asks server: "Do you have a block with hash `X`?"
4.  **Upload:** Only upload blocks the server **does not** have.
5.  **Commit:** Client updates Metadata: "File A now consists of blocks [H1, H2, H3...]".

### 5.2 Write Flow (Upload)
1.  **Client Watcher** detects file change.
2.  Client chunks file and computes hashes.
3.  Client calls `BlockService` to upload *new* blocks (parallel upload).
4.  Client calls `MetadataService` to update `File` table with new `version_id` and list of block hashes.
5.  `MetadataService` commits transaction.
6.  `MetadataService` publishes event to `SyncService`.

### 5.3 Sync Flow (Notification)
1.  `SyncService` receives "File A updated" event.
2.  It finds all active clients belonging to the user (or shared users).
3.  Sends a "Invalidate / Refresh" signal via WebSocket/Long-Polling.
4.  Other Clients request metadata for File A.
5.  Clients determine they are missing block `H2`.
6.  Clients download only `H2` from `BlockService`.

---

## 6. Scalability & Performance

### 6.1 Data Deduplication (Space Saving)
*   **Global Deduplication:** If User A uploads `meme.jpg` and User B uploads the exact same `meme.jpg`, we only store the physical blocks **once** in S3.
*   **Implementation:** The `Blocks` table maps `Hash -> S3_Path`. If the hash exists, increment a `ref_count` (or just don't re-upload).
*   **Savings:** Can save 30-50% storage space.

### 6.2 Caching
*   **Block Cache:** Not needed (CDN handles hot blocks)? actually, Block Service usually pushes hot blocks to a Memcached layer if read frequently.
*   **Metadata Cache:** Redis caches the file tree structure for active users.

---

## 7. Consistency & Conflict Resolution

### 7.1 Conflict Resolution Strategy
Two users edit `report.txt` offline and come online.
*   **Strategy:** **Last Write Wins (LWW)** is dangerous for data loss.
*   **Better Strategy:** **Operational Transformation (OT)** (too complex for files) or **"Keep Both"**.
*   **Dropbox Approach:** Create a "Conflicted Copy".
    *   Version 1 (Base)
    *   User A uploads Version 2.
    *   User B tries to upload Version 2 (based on Version 1).
    *   Server detects `current_version` (2) > `base_version` (1).
    *   Server creates `report (User B conflicted copy).txt`.
    *   User B is notified.

---

## 8. Special Deep-Dive: Content Defined Chunking (CDC)

**Problem:** If we use **Fixed-Size Chunking** (every 4MB), adding 1 byte at the beginning of a file shifts *all* subsequent bytes. All 4MB blocks change. Every block hash changes. You re-upload the **entire** 1GB file.

**Solution:** **Rabin Fingerprinting / Rolling Hash (CDC)**.
*   **Concept:** A sliding window moves across the file. A chunk boundary is set only when the rolling hash creates a specific pattern (e.g., lower 13 bits are zero).
*   **Benefit:** If you insert a byte, only **one** chunk changes (the one containing the insertion). The subsequent chunks naturally realign to the same boundaries as before.
*   **Result:** You genuinely upload only the changed bytes, even for insertions/deletions.

---

## 9. Reliability & Fault Tolerance

*   **Namespace Sharding:** Metadata is sharded by Namespace (Root Folder). Limits blast radius if one DB shard fails.
*   **Erasure Coding:** For the 5 Exabytes of data, we do not use 3x replication (too expensive). We use Erasure Coding (e.g., Reed-Solomon 14+10). We break a block into 14 data pieces + 10 parity pieces. We can tolerate loss of any 10 disks/zones.

---

## 10. Security

*   **Encryption at Rest:** 256-bit AES for blocks in S3.
*   **Encryption in Transit:** SSL/TLS.
*   **Permissions:** ACLs (Access Control Lists) stored in the Metadata DB (`read`, `write`, `owner`) are checked on every metadata request.

---

## 11. Monitoring

*   **Block Upload Latency:** Time from client socket open to close.
*   **Sync Latency:** Time from User A saving file -> User B seeing file.
*   **Dedup Ratio:** Percentage of blocks not uploaded due to existence.

---

## 12. Evolution

*   **Differential Sync (rsync):** For very small changes within a block, send only the diffs (deltas) instead of the whole 4MB block.
*   **Compression:** Apply compression (Zstandard/Brotli) on blocks before uploading to save storage and bandwidth.
