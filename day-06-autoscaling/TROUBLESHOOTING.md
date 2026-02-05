
### Day 6 – Metrics & HPA Debugging

```md
# Day 6 – Troubleshooting: Metrics Server & HPA

This document captures real issues faced while enabling autoscaling on AKS.

---

## Issue 1 – Metrics Server Deployment Invalid

### Error
**requests.memory must be <= limits.memory**

### Cause
Upstream Metrics Server manifest had incompatible resource values.

### Fix
Patched deployment with valid requests & limits.

---

## Issue 2 – Metrics Server CrashLoopBackOff on AKS

### Symptoms
- Pods repeatedly restarting
- HPA showing `<unknown>` metrics

### Root Cause
Metrics Server could not communicate with kubelets due to:
- Self-signed certificates
- Hostname resolution issues

---

### Fix (AKS-specific)
Configured Metrics Server with:
- `--kubelet-insecure-tls`
- `--kubelet-preferred-address-types=InternalIP`

Restarted deployment to apply changes.

---

## Issue 3 – Multiple Metrics Server Pods in Mixed States

### Cause
Patching created multiple ReplicaSets.

### Fix
- Restarted deployment
- Verified at least one `2/2 Running` pod
- Confirmed metrics via `kubectl top`

---

## Issue 4 – HPA CPU Flag Deprecation Warning

### Message
**--cpu-percent is deprecated**

### Explanation
Kubernetes now prefers explicit syntax:
```bash
--cpu=50%
```
Autoscaling still worked correctly.

**Key Debug Commands**
kubectl top pods
kubectl top nodes
kubectl get hpa
kubectl describe hpa
kubectl logs -n kube-system deployment/metrics-server

Final Takeaways

Metrics Server is not plug-and-play on AKS
CrashLoopBackOff often indicates configuration issues
HPA depends strictly on requests + metrics
Real-world autoscaling requires platform-specific tuning