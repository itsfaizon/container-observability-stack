# Multi-Cloud Observability & Security Stack
### AWS | GCP | Azure | Linux VPS

A production-ready monitoring and security environment built to provide real-time visibility across a hybrid-cloud infrastructure. This project demonstrates the deployment of a containerized observability pipeline secured by a private VPN tunnel.

---

## 🏗️ Architecture Overview


The environment consists of a central **Monitoring Hub** (Linux VPS) and distributed **Target Nodes** (AWS EC2, GCP Compute Engine). 
- **Central Hub**: Runs Dockerized Prometheus, Grafana, and Nginx.
- **Edge Nodes**: Run Node Exporter to relay system metrics (CPU, RAM, Disk) back to the Hub.
- **Security Layer**: Management traffic is restricted to a private **WireGuard VPN** subnet.

## 🚀 Tech Stack & Flags
* **Orchestration**: Docker Compose (`restart: always` policy for high availability).
* **Observability**: Prometheus (Time Series Data) & Grafana (Visualization).
* **Web Server**: Nginx (Reverse Proxy & Sub-path routing).
* **Security**: WireGuard (VPN), UFW (Firewall), Kali Linux (Vulnerability Scanning).
* **Cloud Platforms**: AWS (EC2), GCP (Compute Engine), Linux VPS.

## 🛠️ Key Features & Implementation
### 1. Multi-Cloud Scraping
Prometheus is configured to monitor targets across different cloud providers using static configurations:
- `job_name: 'aws-node'`: Scrapes EC2 instance metrics.
- `job_name: 'gcp-node'`: Scrapes Google Cloud VM metrics.

### 2. Secure Reverse Proxy
Nginx serves the Grafana dashboard on a custom sub-path (`/grafana/`) with specific proxy headers to prevent redirect loops:
- `proxy_set_header Host $http_host;`
- `proxy_pass http://grafana:3000;`

### 3. Environment Security
- **Credential Masking**: Utilizes `.env` files and `.gitignore` to ensure secrets (passwords/API keys) never touch version control.
- **Network Isolation**: The dashboard is only accessible via the internal VPN IP `10.97.51.1`.

## 🚦 Getting Started
1. **Clone the repo**: `git clone https://github.com/itsfaizon/monitoring-stack.git`
2. **Configure Secrets**: Create a `.env` file with your `GRAFANA_PASSWORD`.
3. **Launch Stack**: `sudo docker compose up -d`
4. **Access Dashboard**: Connect to WireGuard and visit `http://10.97.51.1/grafana/`.

---
*Created by Jonathan Faizon Xavier Porter as part of a Cloud Infrastructure transition lab.*
