# Runbook: Node Exporter Target Down

## Incident summary

- **Alert:** `MonitoringTargetDown`
- **Severity:** Critical
- **Environment:** Development
- **Affected target:** `192.168.100.87:9100`
- **Detected:** November 9, 2026
- **Recovery verified:** November 10, 2026
- **Status:** Resolved

## Detection

Prometheus detected a failed scrape of the Node Exporter target.

Alert expression:

`up == 0`

Alert threshold:

`for: 3m`

After the condition remained true, Prometheus triggered the `MonitoringTargetDown` alert. Alertmanager successfully received the firing alert.

## Root cause

The virtual machine `k8s.worker1` (`192.168.100.87`) was powered off.

This made Node Exporter unreachable from Prometheus.

## Troubleshooting

1. Check the target status in Prometheus.
2. Verify network connectivity to port `9100`.
3. Check whether the virtual machine is powered on.
4. Verify that Node Exporter is running.
5. Check firewall and proxy configuration if connectivity fails.

A direct HTTP request using `curl --noproxy '*'` was successful after the VM started.

An additional host-side proxy issue was identified: HTTP requests without proxy bypass could be routed through `192.168.100.86:10809` instead of connecting directly to the target.

## Resolution

The virtual machine was powered on again, restoring access to Node Exporter.

## Verification

- Direct Node Exporter HTTP check returned `200 OK`.
- Prometheus query `up{job="node-exporter",instance="192.168.100.87:9100"}` returned `1`.
- Alertmanager's active-alerts endpoint returned an empty list.

The recovery was verified through both the metrics endpoint and the alerting system.

## Lessons learned

- A failed Prometheus scrape does not necessarily indicate an application failure; the underlying VM or network may be unavailable.
- Proxy configuration must be considered when troubleshooting internal network connectivity.
- Both alert triggering and alert resolution should be validated.
- External notification delivery requires a separately configured receiver and has not yet been tested.
