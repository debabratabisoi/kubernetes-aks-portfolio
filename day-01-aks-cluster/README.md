# Day 1 – Azure AKS Cluster Setup

## 🎯 Objective
Provision a real Azure Kubernetes cluster and connect using kubectl.

This lab focuses on:
- Installing required tools
- Authenticating with Azure
- Creating AKS cluster
- Connecting kubectl
- Verifying nodes

---

## 🛠 Tools Used

- Azure CLI
- kubectl
- Helm
- Azure Kubernetes Service (AKS)

---

## 📚 Concepts Learned

### What is Kubernetes?
Kubernetes is a container orchestration platform that:
- Deploys applications
- Scales automatically
- Self-heals failed containers
- Manages networking and storage

---

### AKS Architecture (High-Level)

Control Plane (managed by Azure)
- API Server
- Scheduler
- Controller Manager
- etcd

Worker Nodes (managed by me)
- Run Pods and containers
- VM instances where workloads execute

AKS abstracts control plane operations so we only manage workloads and nodes.

---

## ⚙️ Steps Performed

### 1. Install Tools
Verified installations:

```powershell
az version
kubectl version --client
helm version
