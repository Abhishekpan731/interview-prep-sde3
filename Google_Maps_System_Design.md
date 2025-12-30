# High-Level Design: Google Maps / Yelp (Proximity Service)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Location Search:** Find "Restaurants near me" or "Gas stations within 10km".
*   **Rendering:** Display map tiles at different zoom levels.
*   **Routing:** Calculate shortest path between A and B (Navigation).
*   **Geocoding:** Convert Address <-> Coordinates.

### 1.2 Non-Functional Requirements
*   **Read-Heavy:** 99% reads, 1% writes (Adding new businesses).
*   **Latency:** Search results < 200ms. Tile load < 100ms.
*   **Scalability:** 1 Billion DAU.
*   **Accuracy:** Location data must be reasonably precise.

---

## 2. Capacity Estimation & Scale

### 2.1 Data Volume
*   **Places (POI):** 200 Million Points of Interest globally.
*   **Road Segments:** ~500 Million road segments (Graph edges).
*   **Tiles:** Pre-rendered images.
    *   20 Zoom levels.
    *   Total Tiles $\approx 300 \text{ TB}$ (mostly empty oceans, but dense cities).

### 2.2 Traffic
*   **DAU:** 100 Million.
*   **Requests:** 50 searches/day/user -> 5 Billion/day.
*   **QPS:** ~60,000 QPS. Peak 200,000 QPS.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **Tile Service (Static Rendering):** Serves map images/vector data.
2.  **Location Search Service (Geospatial):** Finds POIs.
3.  **Navigation Service (Routing):** Pathfinding algorithms.
4.  **Contributor Service (Writes):** Users adding reviews or new places.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client([User Mobile / Web])
    
    subgraph "Edge / CDN"
        CDN_Tiles[Tile CDN]
        LB[Load Balancer]
    end
    
    subgraph "Services"
        SearchSvc[Location Search Service]
        NavSvc[Navigation Service]
        TileSvc[Tile Service]
        ReviewSvc[Review/Write Service]
    end
    
    subgraph "Geospatial Data"
        GeoIndex[(Geo Index / QuadTree)]
        PlaceDB[(Place DB: Cassandra)]
        RouteGraph[(Graph DB / RAM)]
        VectorTiles[(Vector Tile Storage)]
    end

    Client -->|1. Get Map Tiles| CDN_Tiles
    CDN_Tiles -.->|Miss| TileSvc
    TileSvc --> VectorTiles
    
    Client -->|2. Search "Pizza"| LB
    LB --> SearchSvc
    SearchSvc -->|Geo Query| GeoIndex
    SearchSvc -->|Details| PlaceDB
    
    Client -->|3. Route A->B| LB
    LB --> NavSvc
    NavSvc -->|A* Pathfinding| RouteGraph
```

---

## 4. Geospatial Indexing (Search)

**Problem:** How to execute `SELECT * FROM POI WHERE dist(me, poi) < 5km` fast? Scanning 200M rows is $O(N)$.

### 4.1 Approach 1: GeoHash (String Prefix)
*   **Concept:** Encode Lat/Long into a string. `9q8yy` is an area in San Francisco.
*   **Search:** Convert user location to `9q8yy`. Query database `WHERE geohash LIKE '9q8yy%'`.
*   **Edge Case:** Boundary issue. The closest pizza place might be in `9q8yz` (neighbor cell). You must search center + 8 neighbors.

### 4.2 Approach 2: Google S2 (Space Filling Curve)
*   **Concept:** Wraps the sphere (Earth) onto a Cube (Hilbert Curve).
*   **ID:** 64-bit Integer.
*   **Pros:** Better math properties than GeoHash. Supports "Covering" (Can I cover this zip code with 5 S2 cells?).
*   **Storage:** Store `Cell_ID` in a B-Tree Key-Value store. Range scans are efficient.

### 4.3 Approach 3: QuadTree (Adaptive)
*   **Concept:** Divide map into 4 quadrants. Recursively sub-divide if a node has > 100 POIs.
*   **Pros:** Dense in cities, sparse in deserts.
*   **Cons:** Hard to implement on Disk (it's a pointer-based tree). Best for in-memory index.

**Preferred Solution:** **Google S2 (Int64)**. It maps 2D coordinates to a 1D integer space, allowing standardized B-Tree indexing (SQL/NoSQL).

---

## 5. Map Rendering: Tiling System

### 5.1 The Logic of Tiles
We don't send one giant 20TB image of Earth. We slice it.
*   **Zoom Level 0:** 1 tile (Whole world).
*   **Zoom Level 1:** 4 tiles.
*   **Zoom Level N:** $4^N$ tiles.

### 5.2 Raster vs Vector Tiles
*   **Raster (Old way):** Server sends PNG images.
    *   *Cons:* High bandwidth. Blurry text on rotation. Hard to style.
*   **Vector (Modern - WebGL):** Server sends geometric data (Protobuf/JSON) like "Draw a polygon here, color it green".
    *   *Pros:* Client GPU renders it. Small file size. Infinite zoom clarity.

### 5.3 Fetching Workflow
1.  Client viewport calculation: "I am at Zoom 15, Lat X, Long Y".
2.  Math: Calculate required Tile IDs `(z, x, y)`. e.g., `(15, 1200, 4500)`.
3.  Request: `GET /tiles/15/1200/4500.pbf`.
4.  CDN: Returns cached file.

---

## 6. Navigation (Routing Service)

**Problem:** Shortest path in a graph of 500M edges (roads).
Dijkstra is $O(E + V \log V)$. Too slow for continental routing.

### 6.1 Contraction Hierarchies
*   **Preprocessing:** We don't route on the raw map. We route on a "Hierarchy".
    *   Small roads (neighborhoods) are "contracted" (bypassed) when creating shortcuts for highways.
*   **Query:** Bidirectional Search.
    *   Forward search from Start (Uses only roads leading UP in importance).
    *   Backward search from End (Uses only roads leading UP).
    *   They meet at a "Hub" (Highway junction).
*   **Speed:** Reduces search space by 1000x. Query takes ~10ms.

### 6.2 ETA Calculation (Real-time)
*   **Graph Weight:** The edge weight is not `Distance`. It is `Time`.
*   **Traffic:** `Time = Distance / (SpeedLimit * TrafficFactor)`.
*   **Traffic Data:** Collected from millions of Android/iOS users passively sending GPS velocity.

---

## 7. Data Storage Model

### 7.1 Place Database
*   **Sharding:** Shard by `S2 Cell ID` (Location).
*   **Why?** Users usually query features in the same city. Keeps hot data on the same shard.
*   **Schema (Cassandra/BigTable):**
    *   RowKey: `S2_Cell_ID`
    *   Columns: List of POIs `{id, lat, long, type}`.

### 7.2 Cache Strategy
*   **Tile Cache:** Immutable. Infinite TTL. Invalidation only when map data updates (rare).
*   **Search Cache:** "Restaurants in San Francisco" can be cached for 10 mins.

---

## 8. Summary

1.  **Indexing:** Google S2 (64-bit integers) is superior to GeoHash for database indexing.
2.  **Rendering:** Vector Tiles (Protobuf) reduce bandwidth and enable client-side GPU rendering.
3.  **Routing:** Cannot run raw Dijkstra. Must use **Contraction Hierarchies** or **A* with Landmarks (ALT)** on a pre-processed graph.
