# Day 6 – Resource Management & Autoscaling (HPA)

## 🎯 Objective
Understand how Kubernetes manages resources and automatically scales applications using the Horizontal Pod Autoscaler (HPA).

This day focuses on:
- CPU & memory requests vs limits
- Metrics Server
- CPU-based autoscaling
- Observing Kubernetes control loops in action

---

## 🧰 Tools & Platform
- Kubernetes (AKS)
- kubectl
- Metrics Server
- Horizontal Pod Autoscaler (HPA)
- Git Bash (Windows)

---

## 🧠 Key Concepts

### Requests vs Limits
- **Requests**: minimum guaranteed resources used by the scheduler
- **Limits**: maximum allowed usage enforced at runtime

Rules:
- CPU limit breach → throttling
- Memory limit breach → OOMKill
- Requests must be ≤ limits

---

## 🧪 Hands-on Summary

### 1. Deployed an app without resources
Observed that Kubernetes had no information for scheduling or autoscaling.

### 2. Added requests & limits
Defined:
- CPU request: 100m
- CPU limit: 200m
- Memory request: 128Mi
- Memory limit: 256Mi

This enabled accurate scheduling and autoscaling.

---

## 🔧 Metrics Server

Metrics Server was installed and configured to work on AKS using:
- `--kubelet-insecure-tls`
- `--kubelet-preferred-address-types=InternalIP`
- Reduced resource usage

Verified metrics using:
```bash
kubectl top pods
kubectl top nodes
```
📈 Horizontal Pod Autoscaler (HPA)

Created HPA with:

Target CPU utilization: 50%

Min replicas: 1

Max replicas: 5

Autoscaling behavior observed under CPU load.

🧠 How HPA Works

CPU utilization is calculated as:
(current CPU usage) / (CPU request)
HPA does not use CPU limits.

🔍 Verification

Metrics visible via kubectl top
HPA created successfully
Pods scaled up and down automatically based on load

🧠 Key Learnings

Autoscaling depends on CPU requests, not limits
Metrics Server is mandatory for HPA
AKS requires explicit metrics-server configuration
Kubernetes reacts automatically to real load
