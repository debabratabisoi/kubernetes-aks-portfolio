Day 10 – Centralized Logging with Loki & Grafana
🎯 Objective

Implement centralized logging in AKS using Grafana Loki and Promtail, and visualize application logs in Grafana without using Elasticsearch.

## 🧠 Architecture Overview
[ Application Pods ]
        |
        | (stdout/stderr)
        v
[ Promtail (DaemonSet) ]
        |
        | (LogQL over HTTP)
        v
[ Loki (Log Store) ]
        |
        v
[ Grafana (Explore UI) ]

## 🛠 Tools Used

Azure Kubernetes Service (AKS)
Helm
Grafana
Loki (single-binary)
Promtail
kubectl

## 📦 Setup Steps
1️⃣ Create logging namespace
kubectl create namespace logging

2️⃣ Add Grafana Helm repo
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

3️⃣ Install Loki Stack (without bundled Grafana)
helm install loki grafana/loki-stack \
  --namespace logging \
  --set promtail.enabled=true \
  --set grafana.enabled=false

4️⃣ Install Grafana separately
helm install grafana grafana/grafana \
  --namespace logging

5️⃣ Access Grafana (port-forward)
kubectl port-forward -n logging svc/grafana 3000:80


Access:
👉 http://localhost:3000

6️⃣ Add Loki as Grafana Data Source

URL

http://loki.logging.svc.cluster.local:3100

7️⃣ Verify Loki health (inside cluster)
kubectl exec -n logging deploy/grafana -it -- sh
wget -qO- http://loki:3100/ready


Expected output:

ready

🔎 Example LogQL Queries
Namespace level
{namespace="default"}

Application level
{app="cpu-app"}

Pod-specific (label discovered via Grafana)
{pod="cpu-app-xxxxx"}

## ✅ Outcome

Centralized logging implemented
Logs searchable by namespace, app, pod
Loki + Grafana successfully integrated
No Elasticsearch dependency

## 📌 Key Learnings

Loki indexes labels, not full text
Labels depend on Promtail configuration
Grafana UI warnings ≠ backend failure