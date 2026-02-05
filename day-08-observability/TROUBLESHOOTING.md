# Day 8 – Troubleshooting: Observability

This document captures common observability mistakes and misconceptions in Kubernetes environments.

---

## Issue 1 – Relying Only on Logs

### Problem
Debugging solely using logs becomes slow and expensive in distributed systems.

### Resolution
Use metrics to identify patterns and logs for detailed investigation.

---

## Issue 2 – Too Many Logs, No Signal

### Problem
Logging everything increases cost and noise while hiding real issues.

### Resolution
Log meaningful events and rely on metrics for system-wide health.

---

## Issue 3 – No Correlation Between Signals

### Problem
Metrics, logs, and traces exist but are not correlated.

### Resolution
Use consistent identifiers (request IDs, trace IDs) across signals.

---

## Issue 4 – No Alerts, Only Dashboards

### Problem
Issues are noticed only after users complain.

### Resolution
Define alerts based on metrics and SLO thresholds.

---

## Issue 5 – Assuming Autoscaling Fixes Performance

### Problem
Scaling does not improve performance if the bottleneck is external (DB, API).

### Resolution
Use metrics and traces to identify real bottlenecks.

---

## Key Debugging Commands (Kubernetes)

```bash
kubectl logs <pod>
kubectl describe pod <pod>
kubectl top pods
kubectl get events
```