Metrics

## Q: Why can CPU metrics show 0 even if the pod is running?
A: Metrics depend on workload. No traffic = no usage.

Logging

## ## Q: Why does {} return logs but {namespace="default"} doesn’t?
A: Either the label doesn’t exist or the time range is too narrow.

Tracing

## Q: Difference between logs and traces?
A:

Logs → discrete events

Traces → request lifecycle across services

## Q: What does resource.service.name represent?
A: The OpenTelemetry service identity, not Kubernetes app name.

Q: Why didn’t service.name=hotrod work?
A: HotROD emits multiple services (frontend, driver, etc.).

## Q: How do you debug missing traces?

{} query

Check OTel Collector logs

Verify Tempo ingestion

Fix TraceQL filters

## Q: Why Grafana instead of Jaeger UI?
A: Grafana unifies metrics, logs, traces and enables correlation.