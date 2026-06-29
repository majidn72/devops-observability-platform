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
└── Persistent Prometheus Storage
```

## Current Components

* Prometheus
* Node Exporter
* Blackbox Exporter
* Docker Compose
* Persistent Docker Volumes

## Planned Components

* Grafana
* Alertmanager
* OpenTelemetry SDK
* OpenTelemetry Collector
* Grafana Tempo
* Elastic Agent
* Logstash
* Elasticsearch
* Kibana
* Nginx Reverse Proxy
* HTTPS and Authentication
* CI/CD Pipeline

## Synthetic Monitoring

The platform performs HTTP synthetic checks against external endpoints.

### Primary Target

```text
https://www.digikala.com
```

### Control Target

```text
https://example.com
```

The control target is used only to validate the Prometheus and Blackbox Exporter configuration.

## Current Metrics

* `up`
* `probe_success`
* `probe_duration_seconds`
* `probe_http_status_code`
* `probe_http_duration_seconds`

## Environment

### Application Server

```text
IP: 192.168.100.87
Role: Application Host
```

### Observability Server

```text
IP: 192.168.100.88
Role: Monitoring and Observability Host
```

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

## Repository Structure

```text
.
├── blackbox/
│   └── blackbox.yml
├── deploy/
│   └── observability/
│       └── compose.yaml
├── docs/
│   └── day03.md
├── prometheus/
│   └── prometheus.yml
├── .gitignore
└── README.md
```

## Project Status

```text
Active Development
```

This project is initially developed and tested on VMware virtual machines and will later be deployed to a secured, production-like VPS environment.

### Day 04

- Deployed Grafana with Docker Compose
- Added persistent Grafana storage
- Enabled authenticated access
- Disabled anonymous access and public registration
- Provisioned Prometheus as the default data source
- Added a version-controlled Digikala dashboard
- Added Docker Compose secrets for Grafana
