# Postgres

Connections, transactions, block cache hit ratio and query time.

Needs postgres-exporter running beside the database, plus a `postgres_query_duration_seconds` histogram from the application if the query-time panels are kept.

Builds the ConfigMap `vedavid-dashboards-postgres`. Add it to the connector's
`dashboards.sources` to mount it.
