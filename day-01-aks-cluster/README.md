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

### 2. Login to Azure
az login --tenant <tenant-id>

### 3. Create Resource Group
az group create --name aks-learning-rg --location eastus

### 4. Create AKS Cluster
az aks create \
  --resource-group aks-learning-rg \
  --name aks-learning \
  --node-count 2 \
  --node-vm-size Standard_B2s \
  --enable-managed-identity \
  --generate-ssh-keys

### 5. Connect kubectl to Cluster
az aks get-credentials -g aks-learning-rg -n aks-learning

### 6. Verify Cluster
kubectl get nodes
kubectl get pods -A
kubectl cluster-info

✅ Result

Cluster successfully created with 2 worker nodes:

aks-nodepool1-xxxx   Ready
aks-nodepool1-yyyy   Ready

kubectl can communicate with the API server.