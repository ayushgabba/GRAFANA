Monitoring Stack with Node Exporter, Prometheus, and Grafana 📊
Overview 🌍
This project sets up a monitoring stack using Node Exporter, Prometheus, and Grafana to collect and visualize system metrics from a Linux machine (or WSL) using Docker Compose.

Technologies Used: 🛠️
Node Exporter: Collects system metrics. 📈
Prometheus: Scrapes and stores metrics. 🧑‍💻
Grafana: Visualizes metrics in dashboards. 📉
Prerequisites ⚙️
Docker and Docker Compose installed.
If not, follow the Docker installation guide. 🐋
WSL 2 (Windows Subsystem for Linux) if running on Windows with Ubuntu.
WSL Installation Guide. 🖥️
Basic knowledge of Docker, Prometheus, and Grafana. 📚
Setup Instructions 🛠️
1. Install Node Exporter on Linux/WSL 🔧
Node Exporter collects system metrics and exposes them for Prometheus.

Download and Extract Node Exporter:

wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.9.1.linux-amd64.tar.gz
cd node_exporter-1.9.1.linux-amd64
IMG IMG IMG

Run Node Exporter:

./node_exporter
Node Exporter will be running on http://localhost:9100. 🌐
IMG
Verify Metrics:

Open http://localhost:9100/metrics in your browser. 🌐
Or use curl to verify:
curl http://localhost:9100/metrics
IMG
Note the WSL IP (if needed): If Prometheus is outside WSL, get the WSL IP using:

hostname -I
Replace localhost with this IP in prometheus.yml later.

2. Set Up Prometheus & Grafana with Docker Compose 🚢
Prometheus scrapes metrics from Node Exporter and Grafana visualizes them.

Create docker-compose.yml: In your project directory, create the file docker-compose.yml with the following content:

version: '3'
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
Configure prometheus.yml: Create the file prometheus.yml with the following content:

global:
  scrape_interval: 15s
scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['<WSL_IP>:9100']  # Replace with WSL IP if Prometheus is outside WSL
Start Services: Run the following command to start Prometheus and Grafana with Docker Compose:

docker-compose up -d
IMG 4. Verify Prometheus:

Open http://localhost:9090 in your browser. 🌐
Check that Node Exporter is UP under Status → Targets. ✅
3. Set Up Grafana Dashboard 📊
Grafana provides the visualization layer for metrics collected by Prometheus.

Access Grafana: Open http://localhost:3000 in your browser. 🌐

Default login: admin / admin IMG
Add Prometheus Data Source:

Go to Configuration → Data Sources → Add Prometheus.
Set the URL to http://prometheus:9090 (or use http://<Prometheus_IP>:9090 if Prometheus is not running in Docker).
IMG
Import Node Exporter Dashboard:

Go to Create (+) → Import → Dashboard ID 1860 IMG (or download the Node Exporter Full Dashboard).
Select Prometheus as the data source and click Import.
View Metrics: The imported dashboard will display CPU, memory, disk, and network metrics. 📉 IMG

Troubleshooting ⚠️
Docker Integration with WSL: If running on Windows with WSL, make sure Docker Desktop is integrated with your WSL distro. Follow the Docker WSL2 guide for setup. 🖥️
If you run Prometheus outside WSL, make sure to replace localhost with the WSL IP in the prometheus.yml file. 📡
