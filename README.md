# dashboards

Dashboards for [vedavid](https://vedavid.dev), written in the
[dashboard DSL](https://github.com/vedavid-dev/dashboard-dsl). One directory
per subject; take the ones your cluster has metrics for.

| Directory | Dashboard | Needs |
| --- | --- | --- |
| [`k8s/`](k8s/) | Cluster health, and Prometheus itself | kube-state-metrics, node-exporter, Prometheus's self-scrape |
| [`postgres/`](postgres/) | Postgres | postgres-exporter |
| [`redis/`](redis/) | Redis | redis_exporter |
| [`rust-k8s-demo/`](rust-k8s-demo/) | Service latency hop by hop, and the quotation service's internals | the demo application's own metrics |

Each directory builds one ConfigMap, `vedavid-dashboards-<directory>`, in the
`vedavid` namespace. The connector chart mounts any number of them:

```yaml
dashboards:
  sources:
    - configMap: vedavid-dashboards-k8s
    - configMap: vedavid-dashboards-postgres
```

## Using them

Apply one directory from a release:

```sh
kubectl apply -k https://github.com/vedavid-dev/dashboards//postgres?ref=v0.1.0
```

Or let Flux follow releases, one Kustomization per directory you want:

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: dashboards
  namespace: flux-system
spec:
  interval: 10m
  url: https://github.com/vedavid-dev/dashboards
  ref:
    semver: ">=0.1.0"
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: dashboards-postgres
  namespace: flux-system
spec:
  interval: 10m
  path: ./postgres
  prune: true
  sourceRef:
    kind: GitRepository
    name: dashboards
```

The root `kustomization.yaml` builds every directory, for a cluster that has
all of the exporters.

The connector reloads its directory on its own; a new release reaches the app
within a minute of being applied.

## Releasing

Tag `main` with the next `v*` version. Consumers pin a tag or a semver range.

## Checking a change

```sh
cargo install --git https://github.com/vedavid-dev/dashboard-dsl vedavid-dash
vedavid-dash lint */*.yaml
kubectl kustomize .
```
