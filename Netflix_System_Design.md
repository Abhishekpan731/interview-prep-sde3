# System Design: Netflix / YouTube (Large-Scale Video Streaming)

*Designed by a Principal Engineer for High-Scale Distributed Systems Interviews.*

---

## 1. Requirements

### Functional Requirements
1.  **Video Upload**: Users can upload high-resolution videos.
2.  **Streaming**: Users can watch videos on various devices (Web, Mobile, Smart TV) with minimal buffering.
3.  **Search**: Users can search for video titles.
4.  **Analytics**: Track views, watch time, and user preferences.
5.  **Quality Selection**: Automatic or manual adjustment of video quality (360p, 720p, 1080p, 4K).

### Non-Functional Requirements
1.  **Low Latency (Startup)**: Video should start playing nearly instantly (**< 1 second**).
2.  **High Reliability**: Zero downtime during peak hours (new show releases).
3.  **High Availability**: Near 100% availability for playback.
4.  **Scalability**: Handle billions of views per day and millions of concurrent streamers.
5.  **Quality of Experience (QoE)**: Smooth playback despite fluctuating network conditions (Adaptive Bitrate).

---

## 2. Capacity Estimation & Scale

### Assumptions
*   **DAU**: 200 Million.
*   **Average Watch Time**: 60 minutes/day.
*   **Uploads**: 100k videos/day.
*   **View vs. Upload Ratio**: 1000:1 (Extremely Read-heavy).

### Storage Estimation
*   **Original Video**: 1 hour of 1080p $\approx$ 1.5 GB.
*   **Transcoded Versions**: A single video is converted into multiple formats (MPEG-DASH, HLS) and resolutions (144p to 4K). Total storage needed $\approx$ **5x the original size**.
*   **Total Daily Storage**: $100k \times 1.5 \text{ GB} \times 5 \approx 750 \text{ TB/day}$.

### Bandwidth Estimation
*   **Egress (Streaming)**: $200M \times 60 \text{ mins} \times 10 \text{ MB/min (avg bitrate)} \approx 120$ Petabytes/day.
*   **Peak Egress**: ~10 Terabits/sec. This is why a global **CDN** is mandatory.

---

## 3. High-Level Architecture

![Netflix Architecture](netflix_system_architecture_1767127235034.png)

### Key Components
1.  **Ingestion Service**: Handles file uploads and saves raw video to **AWS S3 (Raw Bucket)**.
2.  **Transcoding Workflow**: An asynchronous pipeline that chunks video and encodes it into various bitrates.
3.  **Metadata Service**: Stores video info (Title, tags, duration, CDN URLs) in a NoSQL DB (Cassandra/DynamoDB).
4.  **CDN (Content Delivery Network)**: Distributed edge servers that cache transcoded video segments close to the user.
5.  **Client Player**: Intelligent logic that measures network speed and requests the appropriate quality segment.

---

## 4. Deep Dive: Video Transcoding Pipeline

Broadcasting a single raw file is inefficient. We need to convert it into formats compatible with different devices and bandwidths.

### Steps in the Pipeline:
1.  **Inspiration**: Use an **Event-Driven Microservices** architecture.
2.  **Chunking**: The raw video is split into small "chunks" (e.g., 2-4 seconds each).
    *   *Why?* Allows parallel transcoding. If one chunk fails, only that chunk is retried.
3.  **Parallel Encoding**: Workers pull chunks from a **DAG (Directed Acyclic Graph)** scheduler.
    *   **Resolution**: 4K, 1080p, 720p, 480p, etc.
    *   **Codec**: H.264, H.265 (HEVC), VP9, AV1.
4.  **Manifest Generation**: Once all chunks are ready, a **Manifest file** (`.m3u8` for HLS or `.mpd` for DASH) is created. This file acts as a "map" for the client player.
5.  **Storage**: Processed chunks and manifest are moved to **AWS S3 (Production Bucket)**.

---

## 5. Deep Dive: Adaptive Bitrate Streaming (HLS/DASH)

Adaptive Bitrate (ABR) is the magic behind "no buffering".

### How it works:
1.  The client app requests the **Manifest file**.
2.  The manifest contains a list of segments for **every resolution**.
3.  The **Client Player** acts as an intelligent agent:
    *   It measures the current download speed (e.g., 10 Mbps).
    *   It chooses the highest bitrate that fits (e.g., 1080p).
    *   It downloads the next few seconds of video.
4.  **The Shift**: If the user's Wi-Fi drops to 2 Mbps, the player detects this and requests the next chunk in 480p. The user sees a slight drop in quality instead of a "Loading..." spinner.

---

## 6. Deep Dive: CDN Distribution & Edge Computing

For a global scale, serving from a central AWS region is too slow.

### The CDN Strategy:
1.  **Push vs Pull**:
    *   **Popular Content**: Pushed to all edge nodes (pre-caching).
    *   **Long-tail Content**: Pulled on-demand and cached locally.
2.  **Netflix Open Connect (OCA)**: Netflix builds its own hardware (Open Connect Appliances) and places them directly inside **ISPs** (Internet Service Providers).
    *   *Result*: When you watch Netflix, the data often travels only a few miles from your ISP's data center to your home, avoiding the public internet backbone entirely.
3.  **Geo-Routing**: Use **Anycast IP** or Geo-DNS to route the user to the closest available edge node.

---

## 7. Data Model & Storage

| Table | Storage Choice | Partition Key | Notes |
| :--- | :--- | :--- | :--- |
| **Video Metadata** | Cassandra | `video_id` | Highly available, low latency for lookups. |
| **User History** | Cassandra | `user_id` | Optimized for "Continue Watching" row. |
| **Search Index** | Elasticsearch | - | Full-text search on titles/tags. |
| **Video Chunks** | AWS S3 | - | Immutable object storage. |

---

## 8. Reliability & Performance

1.  **Multiple CDNs**: If one CDN (like Akamai) goes down, switch to another (like CloudFront).
2.  **Graceful Degradation**: If the playback service is slow, the app might disable "Thumbnail Previews" to prioritize actual video start.
3.  **Caching**: Use **Redis** for hot metadata (video titles/links) to reduce DB load.

---

## 9. Security & Abuse Prevention

1.  **DRM (Digital Rights Management)**: Use systems like Widewine (Google), FairPlay (Apple), or PlayReady (Microsoft) to prevent piracy. Segments are encrypted; keys are served only to authenticated users.
2.  **Signed URLs**: S3/CDN links expire in 4 hours to prevent link sharing.
3.  **Rate Limiting**: Throttling search and metadata requests to prevent scraping.

---

## 10. Summary

| Feature | Design Decision |
| :--- | :--- |
| **Ingestion** | S3 + Lambda/EC2 Workers |
| **Encoding** | Chunk-based parallel transcoding (DAG) |
| **Streaming** | HLS / MPEG-DASH (Adaptive Bitrate) |
| **Distribution** | Geographically distributed CDN / ISP Edge nodes |
| **Metadata** | Cassandra for scale |

---
*Created by Antigravity for Principal Engineer System Design Interviews.*
