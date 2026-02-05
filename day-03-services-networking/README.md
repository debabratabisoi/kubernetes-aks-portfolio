# Day 3 – Services & Networking in Kubernetes (AKS)

## 🎯 Objective
Understand Kubernetes networking by exposing an application using different Service types and accessing it from the internet using Azure Load Balancer.

---

## 🛠 Tools Used
- Azure Kubernetes Service (AKS)
- kubectl
- Azure Load Balancer
- NGINX

---

## 📚 Concepts Learned

### Why Services are Needed
Pods in Kubernetes have dynamic IPs and can be recreated at any time.
A Service provides a stable network endpoint to access Pods.

---

## Types of Kubernetes Services

### 1. ClusterIP (Internal)
- Default service type
- Accessible only inside the cluster
- Used for backend and internal services

---

### 2. NodePort
- Exposes service on a static port on each node
- Mostly used for testing
- Not recommended for production

---

### 3. LoadBalancer (Production)
- Creates a cloud provider load balancer
- Assigns a public IP
- Used for exposing applications to the internet

---

## ⚙️ Steps Performed

### 1. Created AKS Cluster
Recreated a 2-node AKS cluster using Standard_B2s VM size.

---

### 2. Deployed Application
Created an NGINX Deployment with 2 replicas.

---

### 3. Exposed Application

#### ClusterIP Service
Internal-only access within cluster.

#### NodePort Service
Exposed application via node IP and static port.

#### LoadBalancer Service
Created Azure Load Balancer and public IP automatically.

---

## 🌐 Public Access Test

Accessed the application using the assigned public IP:

http://<EXTERNAL-IP>
Successfully loaded the NGINX welcome page.

---

## 🧠 Traffic Flow
Client Browser
↓
Azure Load Balancer
↓
Kubernetes Service
↓
NGINX Pod


---

## ✅ Result
Application successfully exposed to the internet using Kubernetes Service type LoadBalancer.

---

## 💡 Key Learnings
- Services abstract Pod IP changes
- LoadBalancer integrates Kubernetes with Azure networking
- AKS automatically manages cloud load balancer lifecycle
- LoadBalancer is the standard way to expose apps publicly

---


