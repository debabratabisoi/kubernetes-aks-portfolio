**Centralized Logging – Kubernetes (Loki)**

## Q1. Why Loki instead of ELK?

Answer:
Loki stores logs as compressed streams indexed only by labels, making it cheaper, simpler, and more Kubernetes-native than Elasticsearch.

## Q2. How does Loki index logs?

Answer:
Loki does not index log content. It indexes only labels (namespace, app, pod, etc.) provided by Promtail.

## Q3. What is Promtail?

Answer:
Promtail is a log shipping agent that:
Runs as a DaemonSet
Reads pod logs from /var/log/pods
Adds Kubernetes labels
Pushes logs to Loki

## Q4. Why can logs appear by namespace but not by pod?

Answer:
Because Loki can only query labels that Promtail explicitly adds. Pod labels vary based on Promtail configuration.

## Q5. Why does Grafana show “Failed to fetch” even when Loki is healthy?

Answer:
This commonly happens due to:
Port-forward interruptions
UI session issues
Actual connectivity must be validated from inside the cluster.

## Q6. Does Loki support full-text search?

Answer:
No. Loki supports label-based filtering and line filtering (|=), not full-text indexing.

## Q7. How is Loki different from Prometheus?

Answer:

Prometheus	Loki
Metrics	Logs
Time-series DB	Log store
Pull model	Push model
Numerical data	Text data

## Q8. What happens if a pod restarts?

Answer:
Old logs remain in Loki
New logs start streaming immediately
No data loss if Promtail was running

## Q9. Production considerations for Loki?

Answer:

Object storage (S3 / Azure Blob)
Retention policies
Multi-tenant mode
Separate read/write components

## Q10. One-line summary for interview?

Answer:

Loki provides cost-efficient, Kubernetes-native centralized logging by indexing logs using labels instead of full text.