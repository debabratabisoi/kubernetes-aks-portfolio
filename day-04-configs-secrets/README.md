# Day 4 – ConfigMaps & Secrets in Kubernetes

## 🎯 Objective
Understand how Kubernetes separates configuration from application code using:
- ConfigMaps (non-sensitive configuration)
- Secrets (sensitive configuration)

This lab demonstrates how to inject configuration into Pods without rebuilding container images.

---

## 🛠 Tools Used
- Kubernetes
- kubectl
- Azure Kubernetes Service (AKS)
- Git Bash (for Linux-style commands)

---

## 📚 Concepts Learned

### Why Configuration Should Not Be in Images
Hardcoding configuration inside container images causes:
- Image rebuilds for config changes
- Security risks (credentials baked into images)
- Poor reusability across environments

Kubernetes follows the principle:
> **Build once, configure many times**

---

## 🔹 ConfigMaps

### What is a ConfigMap?
A ConfigMap stores **non-sensitive configuration data**, such as:
- Environment name
- Log level
- Feature flags

Example values:
- `APP_ENV=dev`
- `LOG_LEVEL=debug`

---

### ConfigMap Creation

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=dev \
  --from-literal=LOG_LEVEL=debug
```
Using ConfigMap in a Deployment

The Deployment injects ConfigMap values as environment variables.

Verified inside Pod using:
```bash
kubectl exec <pod-name> -- env | grep APP_
```
🔹 Secrets
What is a Secret?

A Secret stores sensitive data, such as:
Database usernames
Passwords
API tokens

Secrets are base64 encoded (not encrypted by default).

Secret Creation
```bash
kubectl create secret generic db-secret \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASSWORD=supersecret
```
Using Secret in a Deployment
The Deployment references the Secret and injects values as environment variables.
Verified inside Pod using:
```bash
kubectl exec <pod-name> -- env | grep DB_
```
✅ Results

ConfigMaps successfully injected into Pods
Secrets successfully injected into Pods
Same container image reused without modification
Kubernetes prevented Pod startup when Secret was missing (fail-fast behavior)

🧠 Key Learnings

Configuration should be externalized from container images
ConfigMaps are for non-sensitive data
Secrets are for sensitive data
Pods only receive configs they explicitly reference
Kubernetes fails fast when required configuration is missing

