# High-Level Design: Global Video Streaming Platform (Netflix/YouTube)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Video Ingestion:** Support for multi-format uploads (MP4, MOV, MKV) with resumable upload capabilities via TUS protocol.
*   **Large-Scale Transcoding:** Asynchronous processing of videos into multiple resolutions (144p, 360p, 720p, 1080p, 4K) and codecs (H.264, HEVC, VP9, AV1).
*   **Adaptive Bitrate Streaming (ABR):** Delivery using HLS (HTTP Live Streaming) and MPEG-DASH.
*   **Content Delivery:** Low-latency delivery via a global Content Delivery Network (CDN).
*   **Metadata Management:** Searchable indices for titles, descriptions, tags, and thumbnails.
*   **Social Interactions:** Real-time view counts (eventual consistency), likes, comments, and sub-second notification for subscriptions.

### 1.2 Non-Functional Requirements
*   **Availability:** 99.999% availability for playback. The system must be resilient to regional outages.
*   **Latency:** Startup latency (Time to First Frame) < 200ms globally.
*   **Scalability:** Seamlessly handle 100M+ DAU and 1M+ QPS at peak.
*   **Durability:** 99.999999999% (11 nines) durability for original video assets.
*   **Cost Efficiency:** Optimize storage and egress costs through tiered storage and proprietary CDN hardware.

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **Daily Active Users (DAU):** 100 Million.
*   **Average Watch Time:** 45 minutes / day.
*   **Read vs Write Ratio:** ~100:1 (Heavy Read).
*   **Egress Bandwidth:**
    *   Avg bitrate (1080p) ≈ 5 Mbps.
    *   100M users * 5 Mbps * 10% concurrency = **50 Tbps bandwidth requirement**.

### 2.2 Storage Estimates
*   **Uploads:** 500,000 videos/day.
*   **Avg Video Size:** 500 MB (Source).
*   **Transcoding Expansion:** Each video is transcoded into ~10 variants. Total storage per upload ≈ 2.5 GB.
*   **Daily Storage Growth:** 500k * 2.5 GB = **1.25 PB / day**.
*   **Yearly Storage:** ~450 PB / year.

---

## 3. High-Level Architecture

### 3.1 Logical Architecture Components
1.  **API Gateway:** Handles Rate Limiting, Authentication, and Request Routing (e.g., Kong or Envoy).
2.  **User Service:** Manages profiles and preferences (PostgreSQL for ACID).
3.  **Video Metadata Service:** High-availability store for video details (Cassandra).
4.  **Ingestion Service:** Handles multipart uploads and triggers the "VOD Pipeline".
5.  **Transcoding Worker Fleet:** Distributed processing cluster (Spot instances for cost).
6.  **Edge/CDN:** Distributed PoPs (Points of Presence) for serving chunks.

### 3.2 Diagram (ASCII Architecture)
```mermaid
graph TD
    Client([User Client])
    
    subgraph "Edge Infrastructure / CDN"
        DNS[Geo-DNS (Route 53)]
        CDN[Open Connect / Global CDN]
        EdgeLB[Anycast Load Balancer]
    end
    
    subgraph "Gateway & Control Plane"
        API[API Gateway]
        Auth[Auth Service]
        MetaSvc[Metadata Service]
        SearchSvc[Search Service]
        Steering[Steering Service (Manifests)]
    end

    subgraph "Data & Storage Plane"
        Cassandra[(Cassandra: Metadata)]
        Elastic[(ElasticSearch: Index)]
        Redis[(Redis: Caches)]
        UserDB[(PostgreSQL: User Data)]
    end
    
    subgraph "Content Ingestion Pipeline"
        UploadSvc[Upload Service]
        RawBucket[(S3: Raw Video)]
        Conductor[Workflow Orchestrator]
        Kafka[(Kafka: Event Bus)]
        Transcoder[Transcoding Workers]
        Packager[Packager Service]
        ReadyBucket[(S3: Transcoded Assets)]
    end

    %% Read/Playback Flow
    Client --> DNS
    DNS --> EdgeLB
    EdgeLB --> API
    Client -.->|Stream Chunks| CDN
    
    API --> Auth
    API --> MetaSvc
    API --> SearchSvc
    API --> Steering
    
    MetaSvc --> Cassandra
    MetaSvc --> Redis
    SearchSvc --> Elastic
    
    Steering -->|Gen Manifest| Redis
    
    %% Write/Upload Flow
    Client -- Upload --> UploadSvc
    UploadSvc --> RawBucket
    RawBucket -->|Event| Kafka
    Kafka --> Conductor
    Conductor --> Transcoder
    Transcoder -->|Fetch Raw| RawBucket
    Transcoder -->|Write Chunks| ReadyBucket
    Transcoder --> Packager
    Packager -->|Push to Edge| CDN
```

---

## 4. Data Model & Storage Design

### 4.1 Database Selection
*   **Cassandra (Metadata):** Chosen for its master-less architecture and linear scalability.
    *   *Partition Key:* `video_id` (distributes load across nodes).
    *   *Clustering Key:* `created_at` (allows efficient retrieval of latest videos by a user).
*   **PostgreSQL (Users/Subscriptions):** Relational integrity for complex mapping between users and channels.
*   **ElasticSearch:** Specialized for fuzzy searches on video titles and transcript-based search.

### 4.2 Schema Design (Cassandra)
```sql
CREATE TABLE video_metadata (
    video_id UUID,
    uploader_id UUID,
    title TEXT,
    description TEXT,
    tags SET<TEXT>,
    manifest_urls MAP<TEXT, TEXT>, -- {'hls': '...', 'dash': '...'}
    thumbnail_uri TEXT,
    duration INT, -- seconds
    view_count COUNTER,
    PRIMARY KEY (video_id)
);
```

### 4.3 Sharding & Partitioning
*   **Database Sharding:** For MySQL/PostgreSQL, shard by `user_id` or `channel_id`.
*   **Consistent Hashing:** Used in the Cache layer (Redis) and Load Balancers to minimize remapping during node failures.

---

## 5. Core Workflows

### 5.1 The Media Pipeline (Write Path)
1.  **Ingestion:** Client initiates a multipart upload. Chunks are uploaded to a temporary S3 bucket.
2.  **Notification:** S3 triggers an event to **Kafka**.
3.  **DAG Scheduler:** A workflow engine (like Netflix Conductor) initiates a Directed Acyclic Graph (DAG) for:
    *   Audio extraction.
    *   Video chunking (GOP-based splitting).
    *   Parallel transcoding (using FFmpeg).
    *   Thumbnail generation (AI-based best frame selection).
4.  **Packaging:** Chunks are packaged into `.ts` (HLS) or `.m4s` (DASH) segments.
5.  **Manifest Creation:** Master manifest (`m3u8`) is generated listing all resolution variants.

### 5.2 The Playback Flow (Read Path)
1.  **Discovery:** User searches for a video via API Gateway -> ElasticSearch.
2.  **Handshake:** Player requests video metadata and the signed manifest URL.
3.  **ABR Engine:** The client-side player (ExoPlayer/hls.js) fetches the master manifest.
4.  **Segment Fetching:** The player selects the highest bitrate possible and fetches 2-4 second segments from the nearest **CDN Edge**.

---

## 6. Scalability & Performance Deep-Dive

### 6.1 CDN Strategy
*   **Push vs Pull:** High-traffic videos are **pushed** to the edge. Niche videos are **pulled** on-demand and cached via LRU (Least Recently Used) policy.
*   **Netflix Open Connect:** Placing storage appliances directly inside ISP data centers to eliminate backbone transit costs.

### 6.2 Caching Strategy
*   **L1 Cache (Client):** Player-side buffering of the next 3 segments.
*   **L2 Cache (Edge):** CDN caching of video segments.
*   **L3 Cache (Server):** Redis caching for `video_metadata` and `trending_list`.

### 6.3 Hot-Key Management
Videos that go viral (e.g., Super Bowl trailers) create "Hot Keys".
*   **Solution:** Multi-level replication. Instead of one node holding the metadata for a viral video, the metadata is replicated across `K` nodes in the cluster.

---

## 7. Special Problem: Adaptive Bitrate (ABR) Logic

**The Problem:** Network conditions fluctuate constantly.
**The Technical Solution:**
*   **Throughput Estimation:** The client calculates `bits_received / time_taken` for the last few segments.
*   **Switching Logic:** 
    *   If `Available Bandwidth > Current Bitrate * 1.5`, switch UP.
    *   If `Buffer Occupancy < 5 seconds`, switch DOWN immediately.
*   **Codecs:** Using **AV1** for 4K video can reduce bandwidth by 30% compared to H.264, though it requires significantly more compute (CPU) for encoding.

---

## 8. Reliability & Fault Tolerance

*   **Region Evacuation:** If an entire AWS region (e.g., us-east-1) fails, DNS (Route 53) fails over to the next healthy region.
*   **Circuit Breaking:** Using Envoy's outlier detection to stop sending traffic to failing service instances.
*   **Backpressure:** If the Transcoding Cluster is 90% full, the Ingestion Service rejects new uploads with a `503 Service Unavailable` or places them in a "Delayed" queue.

---

## 9. Security & DRM

*   **DRM (Digital Rights Management):** Integration with Widevine (Google), FairPlay (Apple), and PlayReady (Microsoft) using the Common Encryption (CENC) standard.
*   **Signed URLs:** HMAC-signed tokens included in chunk requests to prevent URL sharing/leaking.
*   **Content Authenticity:** Using Merkle Tree hashes for video segments to ensure they haven't been tampered with at the Edge.

---

## 10. Monitoring & Observability

*   **QoE (Quality of Experience) Metrics:**
    *   **Vre (Video Rebuffering Event):** Count and duration.
    *   **Vsf (Video Start Failure).**
    *   **EBVS (Exist Before Video Start).**
*   **Distributed Tracing:** Jaeger/Zipkin to trace a request from the UI through the 20+ microservices.
*   **Log Aggregation:** ELK stack (Elasticsearch, Logstash, Kibana) for real-time error analysis.

---

## 11. Evolution & Future Optimizations

*   **Edge Transcoding:** Using WASM (WebAssembly) to perform light-weight transcoding or watermarking at the CDN Edge.
*   **AI-Based Bitrate Ladder:** Instead of fixed resolutions (720p, 1080p), use machine learning to determine the optimal bitrate for a specific video's complexity (e.g., an action movie needs more bits than a static interview).
*   **P2P Mesh Streaming:** For massive live events, allow clients to share segments with nearby peers to reduce CDN costs.
