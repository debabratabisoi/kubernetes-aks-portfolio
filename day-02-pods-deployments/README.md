# Day 2 – Pods & Deployments (Core Kubernetes Workloads)

## 🎯 Objective
Understand the difference between Pods and Deployments by:

- Creating a Pod directly
- Observing failure behavior
- Creating a Deployment
- Testing self-healing
- Scaling replicas
- Performing rolling updates

This lab demonstrates why Deployments are the production standard in Kubernetes.

---

# 🛠 Tools Used

- kubectl
- Azure Kubernetes Service (AKS)
- NGINX container image

Cluster created on Day 1 was reused.

---

# 📚 Concepts Learned

## What is a Pod?
A Pod is the smallest deployable unit in Kubernetes.

It contains:
- One or more containers
- Shared network
- Shared storage

### Limitations of Pods
❌ No self-healing  
❌ No scaling  
❌ No rolling updates  
❌ Not production safe  

Pods are ephemeral and should not be used directly for real applications.

---

## What is a Deployment?

A Deployment manages Pods using:
- ReplicaSets
- Controllers

### Benefits
✔ Self-healing  
✔ Scaling  
✔ Rolling updates  
✔ Zero downtime  
✔ Production ready  

Most real-world workloads use Deployments.

---

# ⚙️ Steps Performed

---

## Step 1 – Verify cluster

```bash
kubectl get nodes
```
Confirmed worker nodes are Ready.

🔴 Part A – Pod (Wrong Way)
Create Pod
use create_pod_bad_way.yaml

```bash
kubectl apply -f create_pod_bad_way.yaml
kubectl get pods
```
Delete Pod
```bash
kubectl delete pod nginx-pod
kubectl get pods
```
## Observation

Pod disappears permanently. Kubernetes does NOT recreate it

## Learning

Pods are not managed automatically.
They should not be used directly for applications.

🟢 Part B – Deployment (Correct Way)
Create Pod
use create_pod_good_way.yaml

```bash
kubectl apply -f create_pod_godd_way.yaml
kubectl get pods
```
🔁 Self-Healing Test

Delete one Pod:
```bash
kubectl delete pod <pod-name>
kubectl get pods -w
```
## Observation
Kubernetes immediately creates a new Pod.

## Learning

Deployment ensures desired state (replicas always maintained).

📈 Scaling Test

# Scale up:

kubectl scale deployment nginx-deploy --replicas=4

# Scale down:

kubectl scale deployment nginx-deploy --replicas=1

# Learning

Scaling is instant and declarative.

🚀 Rolling Update Test

# Update image:

kubectl set image deployment/nginx-deploy nginx=nginx:1.25
kubectl rollout status deployment/nginx-deploy

# Observation

Pods updated gradually with no downtime.

# Learning

Deployments support safe production updates.