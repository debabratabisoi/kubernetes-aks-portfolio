### Day 9 – Monitoring & Dashboard Interpretation

```md
# Day 9 – Troubleshooting: Prometheus & Grafana

This document captures common questions and confusions when working with Kubernetes monitoring.

---

## Issue 1 – Grafana Login Password Did Not Work

### Cause
Default passwords may differ depending on chart version.

### Resolution
Retrieved credentials from Kubernetes secret:
```bash
kubectl -n monitoring get secret monitoring-grafana \
  -o jsonpath="{.data.admin-password}" | base64 -d
```

## Issue 2 – Only CPU and Memory Metrics Visible
Explanation

CPU and memory metrics always exist due to cgroup tracking.
Other metrics (network, requests, ingress) require real traffic.
This behavior is expected in small or idle clusters.

## Issue 3 – Network Panels Showed No Data
Cause

No sustained Service or ingress traffic
Very low internal request volume

Resolution

Understood as normal behavior, not a monitoring issue.

## Issue 4 – Load Generator Pod Not Visible in Dashboards
Cause

Load generator was created as a short-lived pod.
Prometheus scrapes at intervals and may miss ephemeral workloads.

## Issue 5 – Empty Panels Interpreted as Errors
Clarification

Empty panels indicate no activity, not misconfiguration.
This is correct observability behavior.

Key Debug Commands
-------------------
```bash
kubectl get pods -n monitoring
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
kubectl top pods
kubectl describe pod <pod-name>
```

Final Takeaways
----------------
Monitoring shows reality, not expectations
Low traffic equals quiet dashboards
CPU & memory are foundational metrics
Correct interpretation matters more than tool installation

## 🧩 Issue – Port Forwarding Blocked Further Commands
Symptom

While accessing Grafana using port-forwarding, the terminal became occupied and did not allow running additional commands such as generating traffic from a load generator pod.
```bash
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
```

This command runs in the foreground and blocks the terminal session.

Root Cause

kubectl port-forward is a long-running foreground process.
As long as it is active:

The terminal cannot be reused

Stopping it breaks Grafana access on localhost

This is expected behavior and not an error.

Resolution

Opened a separate Git Bash terminal to perform additional Kubernetes operations while keeping the port-forward session alive.

Terminal 1 (Grafana Access)
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80


Purpose:

Maintain uninterrupted access to Grafana dashboards

Terminal 2 (Traffic Generation)

Used a second terminal to run a load generator:
```bash
kubectl run load-generator \
  --image=busybox \
  -it --rm \
  --restart=Never \
  -- /bin/sh
```

Inside the container:
```bash
while true; do wget -q -O- http://cpu-app; done
```

## Purpose:

Generate traffic

Populate Grafana CPU and network metrics
Observe monitoring behavior under load

## Key Learning

In real-world Kubernetes operations:
Multiple terminals are commonly used
One terminal is reserved for port-forwarding or log streaming
Other terminals are used for load testing, debugging, or monitoring
This workflow prevents accidental disruption of critical access paths such as dashboards.

## Takeaway

Port-forwarding should be treated like a persistent tunnel.
Always use a separate terminal for operational tasks to avoid interrupting active connections.