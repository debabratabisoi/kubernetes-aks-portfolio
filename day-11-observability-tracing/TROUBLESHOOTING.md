This section intentionally documents real failures, not ideal paths.

## Day 9 – Metrics Troubleshooting
❌ No CPU usage visible

Cause
App running but no load

Fix
kubectl exec load-generator -- sh
while true; do wget -q -O- http://cpu-app; done


Lesson
Metrics reflect activity, not deployment success.

❌ Load generator CrashLoopBackOff

Cause
Using /bin/sh on Windows Git Bash path (C:\Program Files\Git\usr\bin\sh)

Fix
Use BusyBox image

Avoid interactive shell on Windows-hosted clusters

## Day 10 – Logging (Loki) Troubleshooting
❌ Loki datasource “Unable to connect”

Cause
Wrong service name or namespace
Grafana running outside cluster

Fix
http://loki.logging.svc.cluster.local:3100

❌ {namespace="default"} returns nothing

Cause
Logs exist, but time range too narrow

Fix
Expand to Last 1 hour / 3 hours

❌ Logs visible only with {} query

Cause
Misunderstanding Loki labels

Fix
Use Label Browser

Query only existing labels

## Day 11 – Tracing (Tempo) Troubleshooting
❌ Tempo CrashLoopBackOff: mkdir C:: permission denied

Cause
Helm value interpreted incorrectly on Windows shell
Path parsed as C: instead of /tmp/tempo

Fix
--set-string tempo.storage.trace.local.path="/tmp/tempo"

❌ Tempo error: unknown backend memory

Cause
Invalid storage backend name

Fix
--set tempo.storage.trace.backend=local

❌ Tempo error: invalid keys: grpc, http

Cause
Wrong Helm values for receivers

Fix
--set tempo.receivers.otlp.grpc.enabled=true
--set tempo.receivers.otlp.http.enabled=true

❌ No traces visible in Grafana

Root Cause
Wrong TraceQL queries
Wrong
{service.name="hotrod"}


Correct
{}
{ resource.service.name="frontend" }
{ resource.service.name="driver" }


Lesson
HotROD is multi-service, not one service.

❌ “open trace” link not working

Cause
Jaeger UI not deployed / port-forwarded

Fix
Use Grafana Explore → Tempo
Grafana is the trace UI

❌ TraceQL parse errors

Cause
Mixing label syntax with TraceQL

Rule
TraceQL uses attributes, not labels

Golden Rule (Interview-worthy)

If {} works, ingestion is fine.
If filters fail, the problem is query semantics, not data flow.