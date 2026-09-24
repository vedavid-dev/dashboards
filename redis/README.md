# Redis

Cache hit rate, keys, memory, clients and commands per second.

Needs redis_exporter running beside Redis, plus a `cache_requests_total{result}` counter from the application if the hit-rate panels are kept.

Builds the ConfigMap `vedavid-dashboards-redis`. Add it to the connector's
`dashboards.sources` to mount it.
