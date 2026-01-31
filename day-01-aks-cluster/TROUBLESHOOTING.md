# Day 1 – Troubleshooting Guide

This document captures all real issues faced while setting up Azure Kubernetes Service (AKS) and how they were resolved.

The goal is to build practical debugging skills, not just follow happy-path tutorials.

---

🧩 # Issue 1 – Azure CLI DLL Import Error

## Error
ImportError: DLL load failed while importing win32file

## Cause
- Old 32-bit Azure CLI mixed with new 64-bit version
- Conflicting Python/DLL files

## Fix
Clean reinstall:

```powershell
winget uninstall Microsoft.AzureCLI

Delete leftover folders:
C:\Program Files (x86)\Microsoft SDKs\Azure\
C:\Program Files\Microsoft SDKs\Azure\

Reinstall:
winget install Microsoft.AzureCLI

## Learning
Always perform clean reinstall when CLI upgrades break dependencies.

---

🧩 # Issue 2 – Tenant Blocked Due to Inactivity

## Error
AADSTS5000225: This tenant has been blocked due to inactivity

## Cause
- Old Azure tenant inactive for long time
- Azure automatically disables unused tenants

## Fix
Login directly to active tenant:
az login --tenant <TENANT_ID>

## Learning
Azure CLI may authenticate to wrong tenant by default.
Always specify tenant when multiple directories exist.

---

🧩 # Issue 3 – No Subscriptions Found in CLI

## Error
No subscriptions found

## Cause
- CLI logged into incorrect directory
- Subscription exists in different tenant

## Fix
Clear cache and login again:
az logout
az account clear
az login --tenant <TENANT_ID>
az account list -o table

## Learning
Azure concepts:

Account ≠ Tenant ≠ Subscription
CLI must connect to correct tenant.

---

🧩 # Issue 4 – VM Size Not Allowed

## Error
The VM size Standard_DS2_v2 is not allowed in your subscription

## Cause
- New subscriptions have quota restrictions
- Some VM families unavailable

## Fix
Use smaller burstable VM:
--node-vm-size Standard_B2s

## Learning
For dev/test:
Always start with smallest VM
Saves cost
Avoids quota limits

---

🧩 # Issue 5 – kubectl Connecting to localhost:8080

## Error
couldn not get current server API group list
Get http://localhost:8080
connection refused

## Cause
- kubeconfig not configured
- kubectl does not know AKS endpoint

## Fix
Download credentials:
az aks get-credentials -g aks-learning-rg -n aks-learning

## Learning
kubectl requires:
kubeconfig → API server endpoint → authentication
Without this, it defaults to localhost.

---

🧩 # Issue 6 – Multiple Azure Tokens Cached

## Error
CLI repeatedly logs into wrong tenant even after logout.

## Cause
- kubeconfig not configured
- kubectl does not know AKS endpoint

## Fix
Download credentials:
az aks get-credentials -g aks-learning-rg -n aks-learning

## Learning
kubectl requires:
kubeconfig → API server endpoint → authentication
Without this, it defaults to localhost.

🔥 # Debugging Commands Cheat Sheet
# Azure
az account show
az account list -o table
az login --tenant <id>
az logout
az account clear

# Kubernetes
kubectl config get-contexts
kubectl config current-context
kubectl get nodes
kubectl get pods -A
kubectl cluster-info

🧠 # Key Takeaways
- Always verify tenant/subscription first
- Use smallest VM sizes for labs
- Clean reinstall tools when corrupted
- kubectl requires kubeconfig credentials
- Troubleshooting is part of real DevOps work

✅ Outcome

After resolving all issues:
- AKS cluster successfully created
- 2 nodes running
- kubectl connected properly
- Environment stable for further labs
