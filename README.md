# DevOps Observability Platform

A production-like observability platform for monitoring infrastructure, application performance, logs, traces, and external website availability.

The primary synthetic monitoring target of this project is the Digikala website.

## Project Objective

This project is designed to provide centralized visibility into:

* Infrastructure health
* Application availability
* HTTP response time
* External endpoint availability
* DNS, TCP, TLS, and HTTP performance
* Application metrics
* Logs
* Distributed traces
* Alerts and incidents

## Current Architecture

```text
Application VM
192.168.100.87
│
└── Node Exporter
        │
        │ Host Metrics
        ▼
Observability VM
192.168.100.88
│
├── Prometheus
├── Blackbox Exporter
├── Node Exporter
├── Grafana
├── Persistent Prometheus Storage
└── Persistent Grafana Storage
```

## Current Data Flow

### Infrastructure Monitoring

```text
Application VM Node Exporter
              │
              │ Host Metrics
              ▼
          Prometheus
              │
              │ PromQL Queries
              ▼
            Grafana
```

### Digikala Synthetic Monitoring

```text
Digikala Website
        ↑
        │ HTTP Synthetic Probe
        │
Blackbox Exporter
        │
        │ Probe Metrics
        ▼
    Prometheus
        │
        │ PromQL Queries
        ▼
      Grafana
```

## Current Components

* Prometheus
* Node Exporter
* Blackbox Exporter
* Grafana
* Docker Compose
* Docker Compose Secrets
* Persistent Docker Volumes
* Provisioned Prometheus data source
* Provisioned Digikala dashboard

## Planned Components

* Alertmanager
* Demo Application
* OpenTelemetry SDK
* OpenTelemetry Collector
* Grafana Tempo
* Elastic Agent
* Logstash
* Elasticsearch
* Kibana
* Nginx Reverse Proxy
* HTTPS
* Role-based access
* CI/CD Pipeline
* VPS Deployment

## Synthetic Monitoring

The platform performs scheduled HTTP synthetic checks against external endpoints.

### Primary Target

```text
https://www.digikala.com
```

### Control Target

```text
https://example.com
```

Digikala is the primary monitoring target of this project.

The `example.com` endpoint is used only as a control target to validate the Prometheus and Blackbox Exporter configuration.

## Current Metrics

* `up`
* `probe_success`
* `probe_duration_seconds`
* `probe_http_status_code`
* `probe_http_duration_seconds`

## Grafana Dashboard

The version-controlled Digikala dashboard currently displays:

* Current probe status
* HTTP status code
* 24-hour availability
* Current probe duration
* Probe duration history
* HTTP phase duration

Grafana authentication is enabled.

Anonymous access and public user registration are disabled.

## Development Environment

### Application Server

```text
Host: application-vm-01
Private IP: 192.168.100.87
Role: Application host
```

### Observability Server

```text
Host: observability-vm-01
Private IP: 192.168.100.88
Role: Monitoring and observability host
```

The listed IP addresses belong only to the local VMware development environment.

Production deployment will use private VPS networking or internal DNS names.

## Current Implementation Status

### Day 01

* Created the initial project structure
* Deployed Node Exporter on the application host
* Initialized the Git repository

### Day 02

* Deployed Prometheus
* Configured the first Node Exporter scrape target
* Connected the local repository to GitHub

### Day 03

* Migrated observability services to Docker Compose
* Added persistent storage for Prometheus
* Added Node Exporter to the observability server
* Added Blackbox Exporter
* Added HTTP synthetic monitoring
* Configured Digikala as the primary synthetic target
* Added `example.com` as a control target
* Pinned container image versions

### Day 04

* Deployed Grafana with Docker Compose
* Added persistent Grafana storage
* Enabled authenticated access
* Disabled anonymous access and public registration
* Provisioned Prometheus as the default data source
* Added a version-controlled Digikala dashboard
* Added Docker Compose secrets for Grafana

## Repository Structure

```text
.
├── blackbox/
│   └── blackbox.yml
├── deploy/
│   └── observability/
│       └── compose.yaml
├── docs/
│   ├── day03.md
│   └── day04.md
├── grafana/
│   ├── dashboards/
│   │   └── digikala-synthetic.json
│   └── provisioning/
│       ├── dashboards/
│       │   └── dashboards.yml
│       └── datasources/
│           └── prometheus.yml
├── prometheus/
│   └── prometheus.yml
├── .gitignore
└── README.md
```

The local `secrets/` directory is intentionally excluded from Git.

## Security

The current development environment includes the following security controls:

* Grafana authentication enabled
* Anonymous access disabled
* Public user registration disabled
* Grafana credentials stored outside Git
* Grafana internal secret key stored outside Git
* Secret files excluded through `.gitignore`
* Provisioned configuration files mounted as read-only
* Services deployed with pinned container image versions

In the production VPS environment, Grafana will be placed behind an Nginx reverse proxy with HTTPS.

Internal services such as Prometheus, Node Exporter, Elasticsearch, Tempo, and the OpenTelemetry Collector will not be exposed directly to the public internet.

## Project Status

```text
Active Development
```

The project is currently developed and tested on two VMware virtual machines.

After all monitoring, logging, tracing, alerting, security, and CI/CD components are completed, the platform will be deployed to a secured, production-like VPS environment.
