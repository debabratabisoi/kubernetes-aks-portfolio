Kubernetes Observability Lab (AKS)

Metrics • Logs • Traces

Stack Used

Kubernetes (AKS, 2 nodes)

Grafana

Loki (logs)

Tempo (traces)

OpenTelemetry Collector

HotROD demo app

Day-wise Progress
✅ Day 9 – Metrics

CPU-bound demo app deployed

Grafana dashboards for:

CPU usage

Requests vs limits

Throttling

Learned:

Metrics require load generation

Requests ≠ limits

Metrics visibility ≠ application traffic

✅ Day 10 – Centralized Logging

Loki + Promtail deployed

Grafana Loki datasource configured

Logs explored by:

namespace

pod

container

Learned:

{} returns logs, filters must match actual labels

Time range matters (logs ≠ metrics latency)

✅ Day 11 – Distributed Tracing

Tempo deployed (local backend)

OpenTelemetry Collector deployed

HotROD app deployed and emitting traces

Grafana Tempo datasource configured

Traces visible via TraceQL

Architecture (Logical)
HotROD
  └── OpenTelemetry SDK
        └── OTel Collector
              ├── Tempo (traces)
              └── (future) Prometheus / Metrics backend
Grafana
  ├── Loki (logs)
  └── Tempo (traces)

Key Learnings

Observability pillars are independent

Traces ≠ logs ≠ metrics

Correct labels / attributes matter more than tooling

Debugging observability is itself an observability skill

## NOTE: use the 'new' version files from the file section. Instead of trace-app-hotrod we used the hotrod-* files for our exercise, check the screenshots for details.