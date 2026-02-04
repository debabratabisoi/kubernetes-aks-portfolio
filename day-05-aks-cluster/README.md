# Day 5 – Ingress & Traffic Routing (AKS)

## 🎯 Objective
Understand how Kubernetes Ingress works and how multiple applications can be exposed using a single public IP on Azure Kubernetes Service (AKS).

This day focuses on **Layer 7 routing**, Ingress controllers, and real-world cloud behavior.

---

## 🧰 Tools & Platform
- Azure Kubernetes Service (AKS)
- kubectl
- Helm
- NGINX Ingress Controller
- Git Bash (Windows)

---

## 🧠 Key Concepts Covered

### 1. Why Ingress Exists
Using `Service type=LoadBalancer` for every application:
- Creates multiple public IPs
- Increases cost
- Complicates TLS and routing

Ingress solves this by:
- Using **one LoadBalancer**
- Routing traffic internally based on HTTP rules

---

### 2. Ingress vs Ingress Controller
- **Ingress**: declarative routing rules (paths / hosts)
- **Ingress Controller**: actual component that processes traffic

In this lab:
- Controller: NGINX Ingress Controller
- Platform: AKS

---

### 3. Architecture Used

Internet
↓
Azure LoadBalancer (1 Public IP)
↓
NGINX Ingress Controller
↓
Ingress Rules
↓
ClusterIP Services
↓
Pods


---

## 🧪 What Was Implemented

### Applications
- app1 (nginx)
- app2 (nginx)

Both exposed internally via `ClusterIP` services.

---

### Ingress Routing
Path-based routing configured:

- `/app1` → app1 service
- `/app2` → app2 service

Ingress used:
- `IngressClass: nginx`
- Regex-based paths
- Rewrite rules to forward traffic correctly to backend services

---

## 🔍 Verification Steps

- Pods running and healthy
- Services reachable via `kubectl port-forward`
- Ingress rules accepted by controller
- Ingress controller pods running
- Azure LoadBalancer public IP assigned

---

## ⚠️ Important AKS Observation

Although Ingress configuration was correct, **browser access via external IP timed out**.

This was due to **single-node AKS + Azure LoadBalancer behavior**, not a Kubernetes YAML issue.

This is a **known AKS-specific limitation** when using ingress-nginx on a single-node cluster.

---

## 🧠 Key Learnings (Most Important)

- Ingress routing can be logically correct but still fail due to cloud provider behavior
- Azure LoadBalancer operates at **node level**, not pod level
- Health probes and node distribution matter
- Port-forward success ≠ Ingress success
- Real-world Kubernetes debugging goes beyond YAML

---

## 📌 Production Notes

In real environments, this issue is resolved by:
- Running AKS with multiple worker nodes
- Running ingress controller with multiple replicas
- Or using managed ingress (Azure Application Gateway Ingress Controller)

---

