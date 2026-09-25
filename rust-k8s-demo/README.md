# rust-k8s-demo

Request rate, error rate and latency of the demo application, hop by hop from the HTTP frontend through gRPC to the database.

Needs the metrics rust-k8s-demo's services expose: `http_requests_total`, `http_request_duration_seconds`, `grpc_requests_total`, `grpc_request_duration_seconds`, `postgres_query_duration_seconds` and `cache_requests_total`.

Builds the ConfigMap `vedavid-dashboards-rust-k8s-demo`. Add it to the connector's
`dashboards.sources` to mount it.

`quotation-service.yaml` looks inside the quotation service, where `service-latency.yaml` stops: the Redis cache, the Postgres connection pool, gRPC outcomes, and the process's memory, CPU and open files.
