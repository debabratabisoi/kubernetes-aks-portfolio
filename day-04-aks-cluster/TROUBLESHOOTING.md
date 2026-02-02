
---

# 📄 File 2: `TROUBLESHOOTING.md` (Day 4 Issues & Fixes)

```md
# Day 4 – Troubleshooting (ConfigMaps & Secrets)

This document captures real issues encountered while working with ConfigMaps and Secrets and how they were resolved.

---

## 🧩 Issue 1 – grep Not Working in PowerShell

### Problem
PowerShell does not support `grep` by default.

### Fix
- Used `Select-String` in PowerShell
- Installed Git Bash for Linux-style commands

Example:
```bash
kubectl exec <pod> -- env | grep APP_ENV
```

🧩 Issue 2 – kubectl panic / nil pointer error with Git Bash
Error
panic: runtime error: invalid memory address or nil pointer dereference

Cause

Using kubectl exec -it with piped commands (| grep) in Git Bash causes TTY issues on Windows.

Fix

Do NOT use -it when piping output:
```bash
kubectl exec <pod> -- env | grep APP_ENV
```
Learning

If piping kubectl output, never use -it.

🧩 Issue 3 – Secret Environment Variables Not Visible
Observation

Running:

kubectl exec nginx-config-demo-pod -- env | grep DB_


returned no output.

Cause

Secrets were injected into a different Pod (nginx-secret-demo).

Fix

Run command against the correct Pod:
```bash
kubectl exec nginx-secret-demo-pod -- env | grep DB_
```
Learning

Secrets and ConfigMaps are injected per Pod, not globally.

🧩 Issue 4 – CreateContainerConfigError
Error

Pod stuck in:

CreateContainerConfigError

Event Message
Error: secret "db-secret" not found

Cause

The Deployment referenced a Secret that did not exist in the namespace.

Fix

Recreate the Secret:
```bash
kubectl create secret generic db-secret \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASSWORD=supersecret
```

Restart Deployment:
```bash
kubectl rollout restart deployment nginx-secret-demo
```
🧠 Key Debugging Commands
kubectl describe pod <pod-name>
kubectl get secrets
kubectl describe secret <secret-name>
kubectl get pods
kubectl logs <pod-name>

🔥 Key Takeaways

CreateContainerConfigError usually indicates missing ConfigMaps or Secrets
Always inspect Pod events first
Secrets are namespace-scoped
Kubernetes fails fast to prevent misconfigured containers from starting
Debugging configuration issues is a core DevOps skill

✅ Outcome

ConfigMaps and Secrets working correctly
Pods running successfully
Configuration properly decoupled from images