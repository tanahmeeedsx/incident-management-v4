 <div align="center">

<img src="https://cdn.simpleicons.org/grafana/F46800" width="110" alt="Grafana Logo" />

# Incident Command Center

**Incident Management & Observability Platform**

A Dockerized Full-Stack Application with Centralized Monitoring

[![Docker](https://img.shields.io/badge/Docker-Containerization-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Orchestration-2496ED?logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![React](https://img.shields.io/badge/React-Frontend-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-Backend-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-Observability-F46800?logo=grafana&logoColor=white)](https://grafana.com/)
[![Loki](https://img.shields.io/badge/Loki-Log%20Aggregation-F46800?logo=grafana&logoColor=white)](https://grafana.com/oss/loki/)

</div>

---

## Overview

Incident Command Center is a full-stack Incident Management application enhanced with a complete observability stack.

The application uses React, Node.js, Express, and PostgreSQL. Docker Compose manages the application services, while Prometheus, Grafana, Loki, Grafana Alloy, Node Exporter, and cAdvisor provide monitoring and observability.

The project demonstrates how to deploy an application, collect infrastructure and application metrics, centralize container logs, and investigate application errors through a unified monitoring platform.

## Features

**Application**

- React frontend served through Nginx.
- Node.js and Express backend.
- PostgreSQL database.
- Incident creation and management.
- Incident severity and status tracking.
- REST API integration.
- Application health endpoint.
- Prometheus-compatible backend metrics.

**Containerization**

- Multi-stage Docker builds.
- Separate frontend, backend, and database containers.
- Docker Compose orchestration.
- Internal Docker networking.
- Persistent PostgreSQL storage.
- Container health checks.
- Environment-based configuration.

**Monitoring & Observability**

- Prometheus metrics collection.
- Grafana monitoring dashboards.
- Loki log aggregation.
- Grafana Alloy log collection.
- Node Exporter host metrics.
- cAdvisor container metrics.
- Backend request and response monitoring.
- Live traffic and error investigation.

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React, Vite |
| Backend | Node.js, Express.js |
| Database | PostgreSQL 16 |
| Web Server | Nginx |
| Containerization | Docker |
| Orchestration | Docker Compose |
| Metrics Collection | Prometheus |
| Visualization | Grafana |
| Log Aggregation | Loki |
| Log Collection | Grafana Alloy |
| Host Monitoring | Node Exporter |
| Container Monitoring | cAdvisor |
| Development Environment | Linux |

## Architecture

The project uses two independent Docker Compose stacks connected through a shared external Docker network.

### Application Architecture

```text
                  User / Browser
                        |
                        v
               Frontend Container
                   Nginx :80
                        |
                    /api Requests
                        |
                        v
               Backend Container
                 Express :5000
                   /        \
                  /          \
                 v            v
          PostgreSQL       /metrics
             :5432             |
                               |
                               v
                         Prometheus
```

### Monitoring Architecture

```text
Backend Metrics ------------------\
                                   \
Node Exporter ---------------------> Prometheus
                                    /     |
cAdvisor -------------------------/      |
                                          v
                                       Grafana


Docker Container Logs
         |
         v
    Grafana Alloy
         |
         v
        Loki
         |
         v
      Grafana
```

### Complete Observability Flow

```text
Application Traffic
        |
        v
   Backend API
        |
        +--------------------+
        |                    |
        v                    v
    /metrics             Container Logs
        |                    |
        v                    v
   Prometheus             Grafana Alloy
        |                    |
        v                    v
      Grafana  <----------  Loki
```

## Project Structure

```text
incident-management-v4/
│
├── backend/
│   ├── src/
│   │   ├── db.js
│   │   └── server.js
│   └── Dockerfile
│
├── frontend/
│   ├── src/
│   │   ├── main.jsx
│   │   └── styles.css
│   ├── index.html
│   ├── nginx.conf
│   └── Dockerfile
│
├── monitoring/
│   ├── alloy/
│   │   └── config.alloy
│   │
│   ├── prometheus/
│   │   └── prometheus.yml
│   │
│   ├── loki/
│   │   └── loki-config.yml
│   │
│   ├── grafana/
│   │   ├── dashboards/
│   │   │   └── incident-observability.json
│   │   └── provisioning/
│   │       ├── dashboards/
│   │       └── datasources/
│   │
│   ├── compose.monitoring.yaml
│   ├── compose.monitoring.mac.yaml
│   └── .env.monitoring.example
│
├── screenshots/
│   ├── Screenshot From 2026-11-13 19-25-30.png
│   ├── Screenshot From 2026-11-13 19-25-36.png
│   ├── Screenshot From 2026-11-13 19-25-47.png
│   ├── Screenshot From 2026-11-13 19-25-54.png
│   ├── Screenshot From 2026-11-13 19-26-05.png
│   ├── Screenshot From 2026-11-13 19-26-26.png
│   ├── Screenshot From 2026-11-13 19-26-48.png
│   ├── Screenshot From 2026-11-13 19-27-03.png
│   └── Screenshot From 2026-11-13 19-27-40.png
│
├── compose.yaml
├── .env.example
├── .gitignore
└── README.md
```

## Getting Started

### Prerequisites

- Docker Engine or Docker Desktop.
- Docker Compose V2.
- Git.
- curl.
- jq (for convenient Prometheus target inspection).

Verify the installations:

```bash
docker --version
docker compose version
git --version
```

## Application Deployment

### 1. Clone the Repository

```bash
git clone https://github.com/tanahmeeedsx/incident-management-v4.git

cd incident-management-v4
```

### 2. Configure Environment Variables

Create the application environment file:

```bash
cp .env.example .env
```

Example configuration:

```env
POSTGRES_DB=incidentdb
POSTGRES_USER=incident_user
POSTGRES_PASSWORD=CHANGE_ME_TO_A_STRONG_PASSWORD
```

Replace the example password with a strong private password.

Never commit the actual `.env` file.

### 3. Create the Shared Docker Network

Both application and monitoring stacks communicate through the external network:

```bash
docker network inspect incident-observability >/dev/null 2>&1 || \
docker network create incident-observability
```

### 4. Validate Docker Compose

```bash
docker compose config --quiet
```

### 5. Build and Start the Application

```bash
docker compose up -d --build
```

This starts:

- PostgreSQL
- Backend
- Frontend

### 6. Verify Application Containers

```bash
docker compose ps
```

Check application logs:

```bash
docker compose logs
```

Check backend logs:

```bash
docker compose logs -f backend
```

### 7. Access the Application

| Service | URL |
|---|---|
| Frontend | http://localhost:8081 |
| Application Health | http://localhost:8081/health |
| Incidents API | http://localhost:8081/api/incidents |

The frontend uses Nginx to forward API requests to the backend over the internal Docker network.

Port `8081` is used on the host to avoid conflicts with an existing host-level Nginx service.

## Monitoring Stack Deployment

The monitoring services are managed independently from the application.

### 1. Configure Grafana Credentials

Create the monitoring environment file:

```bash
cp monitoring/.env.monitoring.example monitoring/.env.monitoring
```

Configure:

```env
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=CHANGE_ME_TO_A_STRONG_PASSWORD
```

Use a strong private password. Do not commit `.env.monitoring`.

### 2. Enter the Monitoring Directory

```bash
cd monitoring
```

### 3. Validate Monitoring Configuration

```bash
docker compose \
  --env-file .env.monitoring \
  -f compose.monitoring.yaml \
  config --quiet
```

### 4. Start Monitoring Services

```bash
docker compose \
  --env-file .env.monitoring \
  -f compose.monitoring.yaml \
  up -d
```

### 5. Verify Monitoring Containers

```bash
docker compose \
  --env-file .env.monitoring \
  -f compose.monitoring.yaml \
  ps
```

The monitoring stack includes six services:

| Service | Responsibility |
|---|---|
| Prometheus | Collects and stores metrics |
| Grafana | Monitoring dashboards and visualization |
| Loki | Log aggregation and querying |
| Grafana Alloy | Collects container logs |
| Node Exporter | Host-level metrics |
| cAdvisor | Container resource metrics |

## Monitoring Access

| Service | URL |
|---|---|
| Grafana | http://localhost:3000 |
| Prometheus | http://localhost:9090 |
| Prometheus Targets | http://localhost:9090/targets |
| Loki | Access through Grafana Explore |
| Alloy UI | http://localhost:12345 |

Grafana login uses the credentials configured in `monitoring/.env.monitoring`.

Prometheus and Loki are bound to localhost on the host. Their ports are not intended to be exposed publicly.

## Prometheus Metrics

The backend exposes a Prometheus-compatible metrics endpoint:

```text
/metrics
```

Test it from the backend container:

```bash
docker compose exec backend node -e 'fetch("http://127.0.0.1:5000/metrics").then(async r => { console.log("HTTP", r.status); console.log(await r.text()); })'
```

Expected response:

```text
HTTP 200
```

Prometheus scrapes the following targets:

- `incident-backend`
- `node-exporter`
- `cadvisor`
- `prometheus`

### Verify Prometheus Targets

Open:

```text
http://localhost:9090/targets
```

Check that all monitoring targets are `UP`.

### Useful Prometheus Queries

Check target availability:

```promql
up
```

Check HTTP request count:

```promql
incident_http_requests_total
```

Check request rate:

```promql
sum(rate(incident_http_requests_total[1m]))
```

These queries help inspect application request activity and target health.

## Grafana Dashboards

Grafana provides a centralized view of application and infrastructure metrics.

Open:

```text
http://localhost:3000
```

The project includes a provisioned observability dashboard and data sources.

The dashboard helps inspect:

- Application request activity.
- Backend performance.
- CPU utilization.
- Memory utilization.
- Container resource consumption.
- Host metrics.
- Monitoring system health.

## Log Monitoring with Loki

Grafana Alloy collects Docker container logs and forwards them to Loki.

To inspect logs:

1. Open Grafana.
2. Navigate to Explore.
3. Select Loki as the data source.
4. Execute LogQL queries.

Example queries:

All application logs:

```logql
{compose_project="incident-app"}
```

Backend logs:

```logql
{compose_project="incident-app", compose_service="backend"}
```

Filter error responses:

```logql
{compose_project="incident-app", compose_service="backend"} |= "404"
```

This makes it possible to investigate application activity and HTTP errors without manually inspecting every container log.

## Live Traffic and Error Testing

Generate application traffic to observe changes in metrics and logs.

### Generate Successful Requests

```bash
for i in {1..500}; do
  curl -sS -o /dev/null \
    http://localhost:8081/api/incidents
  sleep 0.03
done
```

### Generate Intentional HTTP 404 Responses

```bash
for i in {1..100}; do
  curl -sS -o /dev/null \
    http://localhost:8081/api/monitoring-demo-missing
  sleep 0.03
done
```

These requests generate normal API activity and intentional missing-route responses.

Observe the results in:

- Prometheus request metrics.
- Grafana performance dashboards.
- Loki application logs.

## Useful Docker Commands

View application containers:

```bash
docker compose ps
```

View all running containers:

```bash
docker ps
```

View application logs:

```bash
docker compose logs -f
```

Rebuild the application:

```bash
docker compose up -d --build
```

Stop the application:

```bash
docker compose down
```

From the `monitoring` directory, view monitoring logs:

```bash
docker compose \
  --env-file .env.monitoring \
  -f compose.monitoring.yaml \
  logs -f
```

Stop the monitoring stack:

```bash
docker compose \
  --env-file .env.monitoring \
  -f compose.monitoring.yaml \
  down
```

**Important:** Avoid `docker compose down -v` unless you intentionally want to delete persistent database and monitoring data.

## Troubleshooting

### Application Containers Are Not Starting

Inspect their status and logs:

```bash
docker compose ps
docker compose logs
```

### PostgreSQL Connection Error

Check database health:

```bash
docker compose ps db
docker compose logs db
```

### Backend Metrics Are Unavailable

Test the endpoint from inside the backend container:

```bash
docker compose exec backend node -e 'fetch("http://127.0.0.1:5000/metrics").then(r => console.log(r.status))'
```

### Prometheus Targets Are Down

Open:

```text
http://localhost:9090/targets
```

Check the failing target and inspect Prometheus logs.

### Grafana Login Problem

The initial admin password is stored in Grafana's persistent data volume. Changing the environment variable after initialization does not automatically change the stored password.

Use the Grafana admin CLI to reset it when necessary.

## What I Learned

- Building and deploying full-stack Docker applications.
- Managing application services with Docker Compose.
- Configuring Docker networks and service discovery.
- Managing PostgreSQL persistence and application configuration.
- Exposing application metrics through Prometheus.
- Collecting host and container metrics.
- Building centralized Grafana dashboards.
- Aggregating Docker logs using Grafana Alloy and Loki.
- Monitoring API traffic and investigating HTTP errors.
- Understanding the relationship between metrics, logs, and observability.
- Troubleshooting application and container health.

## Project Evolution

This project is the fourth stage of my Incident Management DevOps learning series.

| Version | Focus |
|---|---|
| V1 | Full-Stack Application and Manual Deployment |
| V2 | Automated Deployment with Bash and Nginx |
| V3 | Docker Containerization |
| V4 | Monitoring and Observability |

## Project Screenshots

### 1. Application Dashboard

![Incident Management Application](screenshots/Screenshot%20From%202026-11-13%2019-25-30.png)

### 2. Docker Containers

![Docker Containers](screenshots/Screenshot%20From%202026-11-13%2019-25-36.png)

### 3. Prometheus Targets

![Prometheus Targets](screenshots/Screenshot%20From%202026-11-13%2019-25-47.png)

### 4. Prometheus Metrics

![Prometheus Metrics](screenshots/Screenshot%20From%202026-11-13%2019-25-54.png)

### 5. Grafana Overview

![Grafana Dashboard](screenshots/Screenshot%20From%202026-11-13%2019-26-05.png)

### 6. Resource Monitoring

![CPU and Memory Monitoring](screenshots/Screenshot%20From%202026-11-13%2019-26-26.png)

### 7. Loki Application Logs

![Loki Logs](screenshots/Screenshot%20From%202026-11-13%2019-26-48.png)

### 8. Live Traffic Generation

![Traffic Generation](screenshots/Screenshot%20From%202026-11-13%2019-27-03.png)

### 9. Additional Monitoring View

![Additional Monitoring Screenshot](screenshots/Screenshot%20From%202026-11-13%2019-27-40.png)

## Author

**Tanjim Ahmed (Tomal)**

DevOps Engineer | Linux | Docker | CI/CD | AWS | Kubernetes | Observability

- GitHub: [@tanahmeeedsx](https://github.com/tanahmeeedsx)
- LinkedIn: [Tanjim Ahmed](https://linkedin.com/in/tanahmedd/)

---

<div align="center">

Built as a hands-on project to explore Docker, monitoring, logging, and modern DevOps observability workflows.

</div>
