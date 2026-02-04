# Day 5 – Troubleshooting: Ingress on AKS

This document captures real issues encountered while setting up Ingress on AKS and the lessons learned from debugging them.

---

## Issue 1 – Ingress IP Timing Out in Browser

### Symptom
ERR_CONNECTION_TIMED_OUT

### Observations
- Ingress resource existed
- IngressClass was present
- Ingress controller pods were running
- Services worked via port-forward
- LoadBalancer had a public IP

### Conclusion
The request never reached the ingress controller. This indicated a cloud networking issue, not an application or YAML problem.

---

## Issue 2 – Path-Based Routing Not Working Initially

### Cause
NGINX default container does not serve subpaths like `/app1`.

### Fix
Used rewrite rules:

```yaml
nginx.ingress.kubernetes.io/use-regex: "true"
nginx.ingress.kubernetes.io/rewrite-target: /$2
And regex paths with:
pathType: ImplementationSpecific
```

## Issue 3 – Validation Error with Regex Paths
Error: cannot be used with pathType Prefix

Cause

Regex paths are not allowed with pathType: Prefix.

Fix

Changed to:
pathType: ImplementationSpecific

## Issue 4 – Ingress Controller Pods Healthy but No Traffic
Investigation:

Service endpoints existed
Ingress controller pod was Ready
Browser requests timed out

Root Cause
AKS cluster had only one worker node.

Azure LoadBalancer:
Distributes traffic at node level
Health probes can fail on single-node setups
Results in silent traffic drops (timeouts)

**Key Diagnostic Commands Used**
kubectl describe ingress
kubectl get svc ingress-nginx-controller
kubectl get endpoints ingress-nginx-controller
kubectl get pods -o wide
kubectl port-forward svc/app1-svc 8080:80

**Final Takeaways**

Ingress failures are often cloud-provider specific
Timeouts usually indicate LoadBalancer or node-level issues
Single-node clusters are not representative of production ingress behavior
Port-forward success proves app and service health
Deep debugging is part of real Kubernetes work

