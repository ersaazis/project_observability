# Full-Stack Observability (LGTM Stack)

> [!NOTE]
> **Consultation & Implementation Services**
> If you are interested in consulting or professional implementation for your company, please feel free to reach out:
> - **Name**: Ersa Azis Mansyur
> - **Email**: [eam24maret@gmail.com](mailto:eam24maret@gmail.com)
> - **LinkedIn**: [linkedin.com/in/ersaazis](https://www.linkedin.com/in/ersaazis/)

---

## 📌 Description
This repository contains a centralized observability architecture leveraging the **LGTM stack (Loki, Grafana, Tempo, Mimir)**. 
The system is managed and built with the assistance of the **Antigravity (Gemini)** AI Agent, ensuring consistency, high-performance configuration, and rapid iteration.
The stack scrapes metrics, logs, and traces using **Grafana Alloy** agents distributed across multi-VPCs (Control, Application, and Database). Dynamic, intelligent alerts are routed via Grafana Alerting to specific Telegram groups.

## 🏛️ Service Architecture (Docker Compose)
Below is the list of core services currently running in `docker-compose.yml`:
- **Grafana (Port 3000)**: The central brain for visualization, alerting, and unified dashboards.
- **Loki (Port 3100)**: Log aggregator grabbing logs from Nginx, Docker, OS, and distributed applications.
- **Mimir (Port 9009)**: High-availability metrics database that is 100% Prometheus-compatible.
- **Tempo (Port 3200, 4317, 4318)**: Distributed Tracing engine with OpenTelemetry (OTLP) receiver capabilities.
- **Pyroscope (Port 4040)**: Continuous Profiling system for application CPU/Memory performance.

## 📂 Configuration Structure
All configurations are stored inside the `config/` directory spanning several key components:
```text
config/
├── blackbox/     # Blackbox Exporter config and Alloy endpoints for Uptime/Health checks
├── grafana/      # Alert Rules YAML, Datasource Provisioning (Loki, Tempo, Mimir), & Analytics Dashboards
├── loki/         # Log retention rules, Mimir log schema
├── mimir/        # Metrics retention rules, TSDB block storage config
├── tempo/        # OTLP / gRPC distributed tracing receiver settings
```

## 🚨 Intelligent Alert Routing (Telegram)
Alerts are not dumped into a single channel. Instead, they are intelligently routed based on 3 distinct domains of responsibility:
1. **🖥️ Alerts - Server (`domain=server`)**: High Load notifications, Disk Full, High I/O wait, Out Of Memory, or Node Exporter Down.
2. **📱 Alerts - Application (`domain=application`)**: HTTP 5xx errors on Nginx/Traefik, OTel error traces, Lambda HTTP Response latency, Blackbox Uptime failures, or impending SSL certificate expirations.
3. **🗄️ Alerts - Database (`domain=database`)**: Database Deadlocks, Slow Query Latencies, Connection Drops, and specific DB partition Disk space < 20%.

Grafana analyzes metrics labels and routes the Telegram message to the appropriate group via the `contact_points.yaml` and `alert_rules_*.yml` integrations.

## ⚠️ Pre-Run Configuration Requirements
Before you can run the stack, you **MUST** configure the following credentials and domains.

1. **Environment Variables**:
   - Rename `.env.grafana.example` to `.env.grafana` and set your desired Grafana admin password.
2. **Internal Gateway (Nginx)**:
   - This stack is intended to be accessed via the Nginx reverse proxy configured in `config/nginx/`.
   - Ensure local SSL certificates for `homelab.local` are placed in `config/nginx/certs/`.

---

## ⚙️ Quick Start Guide

Once the pre-run requirements above are completed, you can build and launch the stack:

1. Bring up the entire stack:
   ```bash
   docker compose up -d
   ```
2. Verify service availability:
   - Access Grafana locally at `http://localhost:3000` or via the gateway.
3. Connect your other nodes (Application VM & Database VM) using the **Grafana Alloy** Ansible agents so metrics begin flowing into your new LGTM server.

---

## ⚖️ License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
