# ATC TaskMesh — Distributed ATC Server & Task Scheduling Monitor

A high-fidelity Distributed Computing (DC) project demonstrating **High Availability Failover (Experiment 6)** and **Distributed Task Scheduling / Load Balancing (Experiment 7)** across simulated Air Traffic Control (ATC) server clusters.

---

## 🛫 Overview

In a distributed air traffic control network, flight telemetry, route schedules, and tower handovers are routed through centralized coordinator nodes. **ATC TaskMesh** models this multi-tower environment with two resilient distributed server architectures:

1. **Experiment 6: Primary-Secondary Hot Standby with Recovery Synchronization**
   - **Normal Operation:** Primary Server (Server 1) accepts all incoming telemetry and asynchronously replicates records to Server 2 with sub-second replication delay.
   - **Failover Trigger:** Simulating a Server 1 failure instantly switches the ingestion mesh to Server 2 with zero lost updates.
   - **Recovery Sync:** Bringing Server 1 back online initiates automatic delta backfill from Server 2 before restoring Server 1 as the primary coordinator.

2. **Experiment 7: Task Scheduling & Dynamic Load Balancing**
   - **Active-Active Routing:** Incoming flight processing tasks are distributed dynamically between Server 1 and Server 2.
   - **Supported Schedulers:**
     - **Weighted Round Robin (WRR):** Configurable ratio (e.g. `3:1` or `5:1`), converging smoothly over continuous traffic.
     - **Standard Round Robin (RR):** Equal `1:1` interleaved task distribution.
   - **Node Health Failover:** Crashing an individual active node automatically redirects 100% of tasks to surviving nodes without breaking the mesh.

---

## 🌟 Key Features

- **Live Flight Strip Rack:** High-density ATC flight progress strips rendering origin/destination, flight number, category (`domestic`, `international`, `cargo`), landing ATC tower, sent timestamps, and storage server tags.
- **Side-by-Side Server Replication Tables:** Real-time visibility into Server 1 and Server 2 record queues, storage timestamps, and ingestion provenance (`direct`, `replica +XX ms`, `synced back`, `weighted RR`, `round robin`).
- **Telemetry Filters:** Instant filtering by ATC tower code, source city, destination city, and flight type without affecting server replication fidelity.
- **Audit Logging:** Monotonic event log with millisecond-precision timestamps tracking cluster status changes, sync triggers, and scheduler mode switches.
- **Zero-Dependency Architecture:** Standalone, browser-ready single-file delivery using modern web standards.

---

## 🛠️ Tech Stack & Architecture

- **Frontend:** Pure HTML5, Modern CSS (CSS custom properties, CSS Grid, prefers-color-scheme responsive dark/light theme), Vanilla ES6+ JavaScript.
- **Architecture Pattern:** Decoupled Event-Bus (`flight_created`, `record_stored`, `server_status`, `sync_progress`, `mode_changed`, `balance_config`).
- **Plug-and-Play Backend:** The simulation module emits standardized bus events. To connect a live backend (Go, Python, Node.js, WebSockets, Kafka, gRPC), simply replace the simulator emitter with WebSocket event listeners.

---

## 🚀 Quick Start

### Option 1: Open Directly in Browser
Double-click `index.html` or open it with any web browser (Chrome, Edge, Firefox, Safari).

### Option 2: Run via Node Local Server
```bash
npx serve .
# Or open http://localhost:3000
```

---

## 📊 Distributed Computing Experiments Guide

| Experiment | Mode | Description | How to Demo |
| :--- | :--- | :--- | :--- |
| **Exp 6** | **Failover & Replication** | Primary/Secondary hot standby, replication lag, and recovery sync | 1. Select `6 — Failover`<br>2. Observe flights replicate S1 → S2<br>3. Click **Crash Server 1** (traffic shifts to S2)<br>4. Click **Restore Server 1** (missed records sync back) |
| **Exp 7** | **Load Balancing** | Smooth Weighted Round Robin & standard Round Robin task scheduling | 1. Select `7 — Load balancing`<br>2. Choose **Weighted round robin** with `3:1`<br>3. Observe ratio converge to ~75% S1 and ~25% S2<br>4. Change weights or switch to plain Round Robin (`50% / 50%`) |

---

## 📜 License
MIT License.
