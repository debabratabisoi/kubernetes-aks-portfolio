# Day 8 – Observability (Logs, Metrics & Tracing)

## 🎯 Objective
Understand observability in Kubernetes and cloud-native systems by learning:
- The three pillars of observability
- When to use logs, metrics, and traces
- How observability enables debugging, autoscaling, and reliability

This day focuses on **thinking like an SRE**, not tool installation.

---

## 🧠 What Is Observability?

Observability is the ability to understand the internal state of a system by examining its outputs.

In modern distributed systems, observability answers:
- What happened?
- How often did it happen?
- Where did it happen?

---

## 🧱 The Three Pillars of Observability

### 1️⃣ Logs – *What happened?*
Logs are discrete, timestamped events.

Examples:
- Error messages
- Stack traces
- Request logs

Best used for:
- Debugging failures
- Root cause analysis
- Investigating specific incidents

---

### 2️⃣ Metrics – *How bad / how often?*
Metrics are numerical values collected over time.

Examples:
- CPU and memory usage
- Request rate
- Error rate
- Latency percentiles

Best used for:
- Monitoring system health
- Alerting
- Dashboards
- Autoscaling decisions

---

### 3️⃣ Traces – *Where is the time going?*
Traces show the lifecycle of a request across services.

Examples:
- API → backend → database
- Latency breakdown across components

Best used for:
- Debugging latency
- Understanding microservice dependencies

---

## 🔥 Golden Rule of Observability

> Logs tell you **what** happened, metrics tell you **when and how often**, traces tell you **where**.

---

## 🧠 Logs vs Metrics vs Traces (Comparison)

| Signal | Best For | Cost | Scope |
|-----|--------|------|------|
| Logs | Debugging | High | Narrow |
| Metrics | Monitoring | Low | Broad |
| Traces | Latency analysis | Medium | Request-level |

---

## 🧠 Kubernetes Perspective

- Logs are per-pod and ephemeral
- Metrics drive HPA and alerts
- Traces require explicit instrumentation

Observability must be **designed**, not added later.

---

## 🧠 Key Learnings

- Metrics-first approach scales better than logs-first
- Logs without context are noisy
- Traces are powerful but not free
- Observability enables reliability, not just visibility


