# Express.js API Monitoring Stack: Prometheus, Node Exporter & Grafana

## Project Overview

This project demonstrates a comprehensive monitoring solution for containerized applications using industry-standard observability tools. The implementation showcases how to instrument an Express.js API with system-level monitoring through Prometheus metrics collection, Node Exporter for hardware statistics, and Grafana for visualization, all orchestrated through Docker Compose for seamless deployment and scaling.

## Learning Objectives

### Primary Goal

Implement production-ready monitoring practices using the Prometheus ecosystem to achieve comprehensive observability for containerized applications.

### Secondary Goals

- **Container Orchestration**: Master Docker Compose configurations with service separation and dependency management
- **Metrics Visualization**: Create meaningful dashboards in Grafana to translate raw metrics into actionable insights

## Technical Prerequisites

Ensure your development environment includes:

- **Node.js** (v16 or higher) with npm package manager
- **PostgreSQL Database** (Cloud-hosted recommended - Neon, Supabase, or AWS RDS)
- **Docker Engine** and **Docker Compose** (v2.0+)
- **Linux Environment**
- **Basic Understanding** of containerization concepts and YAML configuration

## Quick Start Guide

### 1. Repository Setup

```bash
# Clone the project repository
git clone https://github.com/noruwa03/express-js-nginx-lb
cd express-js-nginx-lb
```

### 2. Application Configuration

```bash
# Navigate to application directory
cd app

# Install Node.js dependencies
npm install
```

### 3. Environment Configuration

Create a `.env` file in the `app/` directory with the following variables:

```env
# Application Settings
PORT=8080
NODE_ENV=production

# Database Configuration for Neon DB or AWS RDS
DB_CONN_LINK=postgresql://username:password@host:port/database
```

### 4. Local Development

```bash
# Start development server with hot reload to test
npm run dev
```

### 5. Production Deployment

```bash
# Return to project root
cd ..

# Change ports of nginx service in compose.yml from 3000:80 to 4000:80, port 3000 will be used by Grafana service
nginx:
   ports:
      - 4000:80

# Launch complete monitoring stack
docker compose up
```

## Architecture Overview

### Application Structure (Before Monitoring)

```
express-js-nginx-lb/
├── app/                            # Express.js application
│   ├── src/
│   │   ├── controllers/            # Route handlers and business logic
│   │   │   ├── create-post.ts
│   │   │   ├── delete-post.ts
│   │   │   ├── get-post-by-id.ts
│   │   │   ├── get-posts.ts
│   │   │   └── update-post.ts
│   │   ├── db/                     # Database configuration
│   │   │   ├── express-test-db.sql
│   │   │   └── index.ts
│   │   ├── middlewares/            # Request processing middleware
│   │   │   └── post-validation.ts
│   │   ├── routes/                 # API route definitions
│   │   │   └── index.ts
│   │   └── app.ts                  # Application entry point
│   ├── .dockerignore
│   ├── package.json
│   └── tsconfig.json
├── nginx/
│   └── nginx.conf                  # Load balancer configuration
├── compose.yml                     # Main service orchestration
└── README.md
```

### Enhanced Structure (With Monitoring Stack)

```
express-js-nginx-lb/
├── app/                          # .env file added with PORT and DB_CONN_LINK
├── nginx/                        # [Unchanged] Load balancer config
├── monitoring/                   # [New] Monitoring configurations
│   └── prometheus.yml            # Metrics collection rules
├── compose.monitoring.yml        # Monitoring stack services
├── compose.yml                   # Core application services
└── README.md
```

## Monitoring Stack Components

### Prometheus (Time-Series Database)

- **Purpose**: Collects and stores metrics with timestamps
- **Configuration**: Scrapes metrics from Node Exporter and application endpoints
- **Retention**: Configurable data retention policies for historical analysis

### Node Exporter (System Metrics Collector)

- **Purpose**: Exposes hardware and OS-level metrics
- **Metrics Include**: CPU usage, memory consumption, disk I/O, network statistics
- **Endpoint**: Accessible at `http://localhost:9100/metrics`

### Grafana (Visualization Platform)

- **Purpose**: Creates dashboards and alerts from Prometheus data
- **Features**: Custom dashboards, alerting rules, user management
- **Access**: Web interface at `http://localhost:3000`

## Key Metrics to Monitor

### System-Level Metrics

- **CPU Utilization**: Identify performance bottlenecks
- **Memory Usage**: Prevent out-of-memory conditions
- **Disk I/O**: Monitor storage performance
- **Network Traffic**: Track bandwidth usage

## Future Enhancements

### Advanced Monitoring

Implement container-specific monitoring using **cAdvisor** for granular insights into Docker container resource usage, complementing the current system-level monitoring approach.

### Centralized Logging

Integrate **OpenTelemetry** or **Promtail** as log collectors, **Loki** for log aggregation, and enhance Grafana dashboards with log correlation capabilities for comprehensive observability.

### Distributed Tracing

Add **Jaeger** or **Zipkin** integration to trace request flows through the microservice architecture, enabling detailed performance analysis and bottleneck identification across service boundaries.

### Production Deployment

Implement **GitHub Actions** CI/CD pipeline for automated testing, building, and deployment to cloud infrastructure (AWS ECS, Google Cloud Run, or Azure Container Instances).

## Images

### Prometheus Targets

![Prometheus](./assets/prom-targets.PNG)

### Node Exporter

![Node Exporter](./assets/node-exporter.PNG)

### Grafana

![Grafana](./assets/gf-dashboard.PNG)

### Metrics Dashboard

![Prometheus](./assets/node-exporter-dashboard-full.PNG)
