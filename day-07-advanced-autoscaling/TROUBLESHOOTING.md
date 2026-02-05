# Day 7 – Troubleshooting: Advanced Autoscaling

This document captures common autoscaling misconceptions and real-world pitfalls.

---

## Issue 1 – HPA Did Not Scale Up

### Possible Causes
- CPU requests not defined
- Short CPU spikes
- Metrics delay
- Workload not CPU-bound

### Resolution
Verify requests and observe metrics over time.

---

## Issue 2 – HPA Did Not Scale Down

### Observation
Pods remained running even after load dropped.

### Explanation
Kubernetes uses a stabilization window to:
- Avoid oscillation
- Protect caches
- Preserve active connections

This is expected behavior.

---

## Issue 3 – Memory Autoscaling Caused Instability

### Root Cause
Memory is not compressible.
Scaling increased total memory usage, leading to node pressure.

### Best Practice
Avoid memory-based HPA in most cases.
Use right-sizing or VPA instead.

---

## Issue 4 – Autoscaling Didn’t Improve Performance

### Cause
Autoscaling increases capacity but does not fix:
- Slow databases
- Network bottlenecks
- Application inefficiencies

Autoscaling is not a performance silver bullet.

---

## Useful Debug Commands

```bash
kubectl describe hpa
kubectl get hpa
kubectl top pods
kubectl get events
```