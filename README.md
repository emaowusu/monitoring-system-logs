## Monitoring Stack with Prometheus, Grafana, and Loki

This project sets up a complete **monitoring and observability stack** using **Docker Compose**.  
It includes:
- **Prometheus** for metrics collection
- **Grafana** for visualization
- **Loki** for log aggregation
- **Promtail** for log forwarding

---

## 🧰 Prerequisites

- Docker installed → [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose installed → [Install Docker Compose](https://docs.docker.com/compose/install/)
- Free ports: `3000`, `9090`, `3100`

---

### 🏗️ Project Structure



monitoring-stack/
│
├── docker-compose.yml
├── prometheus/
│   └── prometheus.yml
├── loki/
│   └── local-config.yaml
├── promtail/
│   └── promtail.yaml
└── README.md



---

## ⚙️ How to Run

1. **Clone this project**
   
   ```bash
   git clone https://github.com/emaowusu/monitoring-system-logs.git
   cd monitoring-system-logs
   ```

2. **Start the services**


```bash
   docker-compose up -d
```

3. **Check containers**

   ```bash
   docker ps
   ```

4. **Access the services**

   * Grafana → [http://localhost:3000](http://localhost:3000)
     Username: `admin`
     Password: `admin`
   * Prometheus → [http://localhost:9090](http://localhost:9090)
   * Loki API → [http://localhost:3100](http://localhost:3100)

---

### 🔗 Configure Grafana Data Sources

1. Open Grafana.
2. Go to **Settings → Data Sources → Add data source**.
3. Add **Prometheus**:

   * URL: `http://prometheus:9090`
4. Add **Loki**:

   * URL: `http://loki:3100`
5. Click **Save & Test** for both.

---

## 📊 Create Dashboards

### Prometheus (Metrics)

* Click **+ → Dashboard → Add new panel**
* Example query:

  ```
  up
  ```

  This shows if services are up.

### Loki (Logs)

* Click **Explore → Select Loki**
* Example query:

  ```
  {job="varlogs"}
  ```

  Shows logs from `/var/log/*.log` on the host system.

---

## 🧹 Stop and Clean Up

To stop all containers:

```bash
docker-compose down
```

To remove volumes as well:

```bash
docker-compose down -v
```

---

## 🧠 How It Works (Architecture Overview)

1. **Promtail** reads system logs from `/var/log` and sends them to **Loki**.
2. **Loki** stores and indexes logs efficiently.
3. **Prometheus** scrapes metrics from services (like itself or other apps).
4. **Grafana** visualizes data from both Prometheus (metrics) and Loki (logs).

---

## 🧩 Tech Stack

| Component  | Purpose            | Port | Docker Image     |
| ---------- | ------------------ | ---- | ---------------- |
| Prometheus | Metrics Collection | 9090 | prom/prometheus  |
| Grafana    | Visualization      | 3000 | grafana/grafana  |
| Loki       | Log Aggregation    | 3100 | grafana/loki     |
| Promtail   | Log Shipping Agent | —    | grafana/promtail |

---

### 🛠️ Troubleshooting

If Grafana cannot connect to Prometheus or Loki:

* Check container names with `docker ps`
* Ensure network is `monitoring`
* Try restarting:

  ```bash
  docker-compose restart
  ```

---

### 🏁 Result

Once completed, you’ll have:

* Real-time system metrics (CPU, memory, uptime)
* Centralized logs from all containers
* Visual dashboards accessible via Grafana
