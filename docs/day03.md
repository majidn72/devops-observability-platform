# Day 03 — Compose and Synthetic Monitoring

## Objective

Convert the observability services from standalone Docker containers to Docker Compose and introduce HTTP synthetic monitoring.

## Implemented components

- Prometheus
- Blackbox Exporter
- Node Exporter on the observability host
- Persistent Prometheus storage
- Dedicated Docker network
- HTTP probes executed every 60 seconds

## Monitored hosts

- Application VM: `192.168.100.87`
- Observability VM: `192.168.100.88`

## Synthetic targets

- Primary target: `https://www.digikala.com`
- Control target: `https://example.com`

The primary target of this project is Digikala. The `example.com` endpoint is used only as a control target to verify the health of the Prometheus and Blackbox Exporter configuration.

## Key metrics

- `up`
- `probe_success`
- `probe_duration_seconds`
- `probe_http_status_code`
- `probe_http_duration_seconds`

## Architecture

```text
Digikala Website
       ↑
       │ HTTP Probe every 60 seconds
       │
Blackbox Exporter
       ↑
       │ /probe
       │
Prometheus
       ↓
Persistent Time-Series Storage
```

## Validation

- Prometheus is running through Docker Compose.
- Prometheus monitors its own internal metrics.
- Both Node Exporter targets are visible in Prometheus.
- Blackbox Exporter exposes its internal metrics.
- HTTP probes run every 60 seconds.
- Digikala is configured as the primary synthetic target.
- Prometheus data is stored in a persistent Docker volume.
