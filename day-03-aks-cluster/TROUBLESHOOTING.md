🧪 Simulating a Broken Service (Deep Dive)

🔥 Scenario 1: Service Exists, But Traffic Fails (Label Mismatch)
🎯 Goal: Understand how Services find Pods.

## Step 1 — Working state (mental model)

# Normally, this works:
```yaml
Service selector:
  app: nginx

Pod label:
  app: nginx
```

# Traffic flow:
```**nginx**
Service → matching Pods → response
```
## Step 2 — Break it (change selector)

Edit your Service and intentionally mismatch labels:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-lb
spec:
  type: LoadBalancer
  selector:
    app: wronglabel
  ports:
    - port: 80
      targetPort: 80
```

Apply:
```bash
kubectl apply -f service-loadbalancer.yaml
```
## Step 3 — Observe failure
```bash
kubectl get endpoints nginx-lb
```

Output:

<none>

Browser:
❌ hangs or times out

🧠 Why this broke ?

*Services do NOT talk to Deployments
Services ONLY care about Pod labels
No matching Pods → no endpoints → black hole*

## Step 4 — Fix it

Restore selector:
```yaml
selector:
  app: nginx
```

Reapply:
```bash
kubectl apply -f service-loadbalancer.yaml
```

Endpoints reappear:
```bash
kubectl get endpoints nginx-lb
```

Browser:
✅ works again

💡 Lesson 1

If Service is broken → check labels first

*This is the #1 real-world Service bug.*

🔥 Scenario 2: Pods Are Running, But App Is Down (Port Mismatch)
🎯 Goal: Understand targetPort vs containerPort

## Step 1 — Break the Service port

Change Service:
```yaml
ports:
  - port: 80
    targetPort: 9999
```

But container runs on:
```yaml
containerPort: 80
```

Apply:
```bash
kubectl apply -f service-loadbalancer.yaml
```
## Step 2 — Observe
```bash
kubectl get pods
```

Pods are:

Running


But browser:
❌ connection refused / timeout

🧠 Why this broke ?

Traffic flow:

LoadBalancer → Service → Pod:9999 ❌


Nothing listening on port 9999.

## Step 3 — Fix it
targetPort: 80


Reapply → browser works.

💡 Lesson 2

Kubernetes won’t validate ports for you
Running Pods ≠ Working Service

🔥 Scenario 3: Service Is Fine, Pods Keep Restarting
🎯 Goal: Understand app-level vs service-level failures

## Step 1 — Break the container

Change Deployment image to something invalid:

kubectl set image deployment/nginx-deploy nginx=nginx:doesnotexist

## Step 2 — Observe
kubectl get pods


You’ll see:

ImagePullBackOff
CrashLoopBackOff


Service still exists:
```bash
kubectl get svc
```

But browser:
❌ fails

🧠 Why this broke ?

Service is healthy

But no healthy Pods

Kubernetes refuses to route traffic

Check:
```bash
kubectl get endpoints nginx-lb
```

Empty again.

## Step 3 — Fix it
```bash
kubectl set image deployment/nginx-deploy nginx=nginx
```

Pods recover → endpoints return → browser works.

💡 Lesson 3

Services don’t fix broken apps
They only route to healthy Pods

🧠 Debugging Order (VERY IMPORTANT)

When a Service is broken, always debug in this order:

1️⃣ Pods running?
```bash
kubectl get pods
```

2️⃣ Labels match?
```bash
kubectl describe svc <service>
```

3️⃣ Endpoints exist?
```bash
kubectl get endpoints <service>
```

4️⃣ Ports correct?
targetPort
containerPort

5️⃣ App logs?
```bash
kubectl logs <pod>
```

This order saves hours in real jobs.

**🔥 One Golden Rule (memorize)**

Service problems are almost never Service problems
They are usually label, port, or pod health problems