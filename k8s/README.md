# Kubernetes cluster health

Node and pod health, CPU, memory and disk, restarts, and what needs attention.

Needs kube-state-metrics and node-exporter, as the kube-prometheus and prometheus community charts install them.

Builds the ConfigMap `vedavid-dashboards-k8s`. Add it to the connector's
`dashboards.sources` to mount it.
