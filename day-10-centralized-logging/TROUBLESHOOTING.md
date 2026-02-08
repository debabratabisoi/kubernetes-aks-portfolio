Day 10 – Loki & Grafana
## Issue 1: Grafana shows “Failed to fetch”

Cause
Port-forward session dropped
Grafana pod restarted

Fix

kubectl port-forward -n logging svc/grafana 3000:80
Refresh browser.

## Issue 2: “Unable to connect with Loki” in Data Source

Cause
Grafana UI test is stricter than runtime connectivity

Fix
Verify from inside Grafana pod:
wget -qO- http://loki:3100/ready

If ready, Loki is healthy.

## Issue 3: Logs visible by namespace but not by pod

Cause
Assumed labels don’t exist in Loki
Promtail defines labels dynamically

Fix

Use Grafana → Label Browser
Query using actual labels:

{pod="cpu-app-xxxxx"}

## Issue 4: {} or {namespace="default"} shows “Logs volume error”

Cause
Loki single-binary doesn’t support log volume APIs

Fix
Ignore “Logs volume”

Use pipeline queries:
```bash
{namespace="default"} |= ""
```
## Issue 5: No logs shown initially

Cause
Promtail only ships logs after it starts

Fix
Increase time range to Last 3 hour
Generate fresh logs

Debug Commands Cheat Sheet
```bash
kubectl get pods -n logging
kubectl logs -n logging -l app.kubernetes.io/name=promtail
kubectl logs -l app=cpu-app
```