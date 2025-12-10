# 🚀 GenLayer Validator Ultimate Dashboard

This Grafana dashboard provides deep insights into your GenLayer validator node's performance. It covers consensus participation, transaction throughput (TPS), synchronization status, system resource usage (Go runtime, CPU, RAM), and **long-term historical trends**. It is designed to help node operators maintain high uptime and troubleshoot performance bottlenecks effectively.

## ✨ Key Features

* **Real-time Health Check**: Instant visual feedback on Validator status (Online/Offline).

* **Consensus Deep Dive**: Track `Commit` and `Reveal` rates to ensure you are actively participating in consensus rounds.

* **Transaction Metrics**: Granular TPS breakdown (Accepted into Mempool vs. Activated/Executed).

* **Network I/O**: Visual bar gauges for real-time Upload and Download bandwidth usage.

* **Runtime Internals**: Advanced monitoring of Go Goroutines, Heap Memory, and Garbage Collection (GC) latency.

* **📈 Historical Trend Analysis**: New charts for Hour/Day/Week analysis to identify memory leaks, daily traffic patterns, and block growth.

  ![](imgs\img1.png)

-----

## 🛠️ Requirements

Before importing this dashboard, ensure you have the following components running:

1. **GenLayer Node**: A running validator node exposing metrics on port `9153` (default).
2. **Prometheus**: Configured to scrape the GenLayer node.
3. **Grafana**: v10.0 or higher is recommended for the best visualization experience.

### Datasources

* **Prometheus** (Required): The primary source for all metrics (`up`, `genlayer_*`, `go_*`, `process_*`).

-----

## ⚙️ Installation & Setup

### 1\. Configure Prometheus

To ensure the dashboard displays data correctly, your `prometheus.yml` **MUST** use the exact job name: `genlayer-validator`.

Add the following snippet to your `scrape_configs` in `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'genlayer-validator'
    static_configs:
      - targets: ['TODO:YOUR_IP:9153'] # Replace with your node's actual IP:Port
        labels:
          instance: 'My-Validator'
    scrape_interval: 5s # Recommended for real-time updates
```

> **Note:** If running Prometheus inside Docker on Windows/Mac, use `host.docker.internal:9153`. On Linux, use the host IP.

### 2\. Import Dashboard to Grafana

1. Open Grafana and navigate to **Dashboards** -\> **New** -\> **Import**.
2. Upload the `dashboard.json` file (or paste the JSON content).
3. Select your **Prometheus** datasource from the dropdown menu.
4. Click **Import**.

-----

## 📊 Panels Explained

### 🟢 Node Overview & Health

* **Status**: Node uptime status (Green = Online, Red = Disconnected).
* **Block Height**: The current block number the node has processed.
* **Sync Lag**: The difference between the network head and your node (Should be 0).
* **CPU/RAM Gauge**: Real-time resource usage with color-coded thresholds (Green -\> Orange -\> Red).

### 🗳️ Consensus & Transactions

* **Participation Rate**: Tracks the rate of `Validator Commits` and `Validator Reveals`. A drop in these lines indicates missed voting rounds.
* **Throughput**:
  * **Accepted**: Rate of transactions entering the Mempool.
  * **Activated**: Rate of transactions successfully executing.

### 🛠️ Network & Runtime

* **Network Bandwidth**: Horizontal bars showing Inbound (Download) and Outbound (Upload) traffic.
* **Go Heap Memory**: Visualizes memory allocation trends. Constant growth without drops may indicate a memory leak.
* **Goroutines & GC**: Monitors active lightweight threads and Garbage Collection duration. High GC spikes (\>1s) can cause the node to miss blocks.

### 📈 Historical Trends (Hour / Day / Week)

* **Last 1 Hour (Detailed TPS)**: Zooms in on recent TPS fluctuations to spot short-term anomalies.
* **Last 24 Hours (Consensus)**: Compares consensus activity against block growth over the last day to ensure stability.
* **Last 7 Days (System Resources)**: Long-term view of CPU and Memory usage. Essential for detecting slow memory leaks.

-----
