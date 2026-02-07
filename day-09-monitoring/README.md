# Day 9 – Monitoring with Prometheus & Grafana

## 🎯 Objective
Set up a Kubernetes-native monitoring stack and understand how metrics are collected, stored, and visualized using Prometheus and Grafana.

This day focuses on **interpreting dashboards correctly**, not just installing tools.

---

## 🧰 Tools & Stack
- Kubernetes (AKS)
- Helm
- Prometheus
- Grafana
- kube-prometheus-stack

---

## 🧠 Monitoring Architecture

Pods / Nodes
↓
Metrics Endpoints
↓
Prometheus (scrape + store)
↓
Grafana (query + visualize)


Prometheus is responsible for collecting and storing metrics.  
Grafana is responsible for visualizing those metrics.

---

## 🧪 Setup Summary

### 1. Workload Preparation
A sample application (`cpu-app`) was deployed with:
- CPU & memory requests
- CPU & memory limits
- Metrics Server enabled

This ensured meaningful metrics were available.

---

### 2. Monitoring Stack Installation
The `kube-prometheus-stack` Helm chart was installed in a dedicated `monitoring` namespace.

Components included:
- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- kube-state-metrics

---

### 3. Grafana Access
Grafana was accessed via port-forwarding:
```bash
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
```

Credentials were retrieved securely from Kubernetes secrets.

📊 Dashboard Observations
Metrics that showed data

*CPU usage
Memory usage
Requests vs limits*

These metrics always exist because every container consumes CPU and memory.

Metrics that appeared empty

Network traffic
HTTP requests
Ingress metrics
Disk I/O

## This was expected because:

The cluster had no external traffic
Minimal internal traffic was generated
Some pods (like load generators) were short-lived
Empty panels correctly indicated low or no activity, not misconfiguration.

## 🧠 Key Learnings

Dashboards reflect real activity, not configuration
CPU and memory metrics are always present
Network and request metrics appear only under sustained traffic
Short-lived pods may never appear in Prometheus dashboards
Monitoring tools must be interpreted in context

## 🔥 Interview Takeaway

“Grafana dashboards only show metrics when activity exists. In small or idle clusters, CPU and memory are usually the only active signals, while network and request metrics appear under real traffic.”