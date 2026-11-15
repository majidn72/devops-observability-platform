# Runbook: BIND DNS Probe Failure

## Alert

- Rule: `DNSResolutionFailed`
- Target: `192.168.100.76:53`
- Query: `example.com` (`A` record)
- Transports: UDP and TCP, tracked separately by `dns_transport`
- Trigger: `probe_success == 0` for two minutes

## Triage

1. Check which transport failed in Prometheus or Alertmanager.
2. Check `up` for the `blackbox-dns` job. If it is `0`, inspect the scrape
   error before concluding that BIND is unavailable.
3. If `up` is `1` but `probe_success` is `0`, query BIND directly from the
   monitoring host:

   ```bash
   dig @192.168.100.76 example.com A +time=2 +tries=1
   dig +tcp @192.168.100.76 example.com A +time=2 +tries=1
   ```

4. If both queries fail, check whether the DNS VM is powered on, BIND is
   running, and the monitoring host can reach port 53. If only one fails,
   check the listener and firewall for that transport.

## Recovery verification

- Both transport series return `probe_success = 1`.
- Neither DNS alert instance remains active in Alertmanager.
- The Digikala HTTP probe is checked separately. A DNS timeout can prevent
  the HTTP probe from reaching the site and does not establish a site outage.

## Lab validation

During a controlled DNS VM shutdown, both alert instances transitioned from
Pending to Firing in Prometheus and appeared in Alertmanager. The rule's
failure and recovery behavior was also verified with `promtool test rules`.
External notification delivery was not tested.