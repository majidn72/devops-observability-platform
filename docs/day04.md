# Day 04 — Grafana and Digikala Dashboard

## Objective

Deploy Grafana, provision Prometheus as a data source, secure access with authentication, and create a version-controlled dashboard for Digikala synthetic monitoring.

## Implemented Components

- Grafana OSS
- Persistent Grafana storage
- Grafana authentication
- Disabled anonymous access
- Disabled public user registration
- Provisioned Prometheus data source
- Provisioned Digikala dashboard
- Docker Compose secrets

## Dashboard Panels

- Current probe status
- HTTP status code
- 24-hour availability
- Current probe duration
- Probe duration history
- HTTP phase duration

## Data Flow

```text
Digikala
    ↑
    │ HTTP synthetic probe
    │
Blackbox Exporter
    ↓
Prometheus
    ↓
Grafana
```

## Authentication

Grafana requires authentication before users can access dashboards.

The following settings are enabled:

- Anonymous access is disabled.
- Public user registration is disabled.
- The administrator password is loaded from a Docker Compose secret.
- The Grafana internal secret key is loaded from a Docker Compose secret.

## Data Source Provisioning

Prometheus is provisioned automatically as Grafana's default data source.

Grafana communicates with Prometheus through the internal Docker network:

```text
http://prometheus:9090
```

## Dashboard Provisioning

The Digikala synthetic monitoring dashboard is stored as a JSON file and loaded automatically by Grafana.

This allows the dashboard to be:

- Version controlled
- Reviewed through Git
- Reproduced on other environments
- Deployed later to the production VPS environment

## Persistent Storage

Grafana data is stored in the following named Docker volume:

```text
devops-observability-grafana-data
```

This volume preserves internal Grafana data such as:

- Users
- Sessions
- Organizations
- Internal settings
- Grafana database

## Security

Grafana credentials and the internal secret key are stored outside Git in the local `secrets/` directory.

The `secrets/` directory is excluded through `.gitignore`.

No password or secret value is stored in the public repository.

## Validation

- Grafana health API returns `database: ok`.
- Grafana requires a valid username and password.
- Prometheus data source is provisioned automatically.
- Digikala dashboard is loaded automatically.
- Dashboard panels query Digikala metrics from Prometheus.
- Grafana data is stored in a persistent Docker volume.
- Secret files are ignored by Git.
