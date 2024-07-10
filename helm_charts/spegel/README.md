# Spegel

Stateless cluster local OCI registry mirror.

## Installation

Make sure that you have read the [compatibility guide](../../docs/COMPATIBILITY.md) before proceeding the with the installation.

### CLI

Delpoy Spegel with the Helm CLI.

```sh
helm upgrade --create-namespace --namespace spegel --install --version v0.0.23 spegel oci://ghcr.io/spegel-org/helm-charts/spegel
```

### Flux

Deploy Spegel with Flux.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: spegel
---
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: spegel
  namespace: spegel
spec:
  type: "oci"
  interval: 5m0s
  url: oci://ghcr.io/spegel-org/helm-charts
---
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: spegel
  namespace: spegel
spec:
  interval: 1m
  chart:
    spec:
      chart: spegel
      version: "v0.0.23"
      interval: 5m
      sourceRef:
        kind: HelmRepository
        name: spegel
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity settings for pod assignment. |
| fullnameOverride | string | `""` | Overrides the full name of the chart. |
| grafanaDashboard.annotations | object | `{}` | Annotations that ConfigMaps can have to get configured in Grafana, See: sidecar.dashboards.folderAnnotation for specifying the dashboard folder. https://github.com/grafana/helm-charts/tree/main/charts/grafana |
| grafanaDashboard.enabled | bool | `false` | If true creates a Grafana dashboard. |
| grafanaDashboard.sidecarLabel | string | `"grafana_dashboard"` | Label that ConfigMaps should have to be loaded as dashboards. |
| grafanaDashboard.sidecarLabelValue | string | `"1"` | Label value that ConfigMaps should have to be loaded as dashboards. |
| image.digest | string | `""` | Image digest. |
| image.pullPolicy | string | `"IfNotPresent"` | Image Pull Policy. |
| image.repository | string | `"ghcr.io/spegel-org/spegel"` | Image repository. |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | Image Pull Secrets |
| nameOverride | string | `""` | Overrides the name of the chart. |
| namespaceOverride | string | `""` | Overrides the namespace where spegel resources are installed. |
| nodeSelector | object | `{"kubernetes.io/os":"linux"}` | Node selector for pod assignment. |
| podAnnotations | object | `{}` | Annotations to add to the pod. |
| podSecurityContext | object | `{}` | Security context for the pod. |
| priorityClassName | string | `"system-node-critical"` | Priority class name to use for the pod. |
| resources | object | `{}` | Resource requests and limits for the Spegel container. |
| securityContext | object | `{}` | Security context for the Spegel container. |
| service.metrics.port | int | `9090` | Port to expose the metrics via the service. |
| service.registry.hostPort | int | `30020` | Local host port to expose the registry. |
| service.registry.nodePort | int | `30021` | Node port to expose the registry via the service. |
| service.registry.port | int | `5000` | Port to expose the registry via the service. |
| service.registry.topologyAwareHintsEnabled | bool | `true` | If true adds topology aware hints annotation to node port service. |
| service.router.port | int | `5001` | Port to expose the router via the service. |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template. |
| serviceMonitor.enabled | bool | `false` | If true creates a Prometheus Service Monitor. |
| serviceMonitor.interval | string | `"60s"` | Prometheus scrape interval. |
| serviceMonitor.labels | object | `{}` | Service monitor specific labels for prometheus to discover servicemonitor. |
| serviceMonitor.metricRelabelings | list | `[]` | List of relabeling rules to apply to the samples before ingestion. |
| serviceMonitor.relabelings | list | `[]` | List of relabeling rules to apply the target’s metadata labels. |
| serviceMonitor.scrapeTimeout | string | `"30s"` | Prometheus scrape interval timeout. |
| spegel.additionalMirrorRegistries | list | `[]` | Additional target mirror registries other than Spegel. |
| spegel.appendMirrors | bool | `false` | When true existing mirror configuration will be appended to instead of replaced. |
| spegel.blobSpeed | string | `""` | Maximum write speed per request when serving blob layers. Should be an integer followed by unit Bps, KBps, MBps, GBps, or TBps. |
| spegel.containerdContentPath | string | `"/var/lib/containerd/io.containerd.content.v1.content"` | Path to Containerd content store.. |
| spegel.containerdMirrorAdd | bool | `true` | If true Spegel will add mirror configuration to the node. |
| spegel.containerdNamespace | string | `"k8s.io"` | Containerd namespace where images are stored. |
| spegel.containerdRegistryConfigPath | string | `"/etc/containerd/certs.d"` | Path to Containerd mirror configuration. |
| spegel.containerdSock | string | `"/run/containerd/containerd.sock"` | Path to Containerd socket. |
| spegel.kubeconfigPath | string | `""` | Path to Kubeconfig credentials, should only be set if Spegel is run in an environment without RBAC. |
| spegel.logLevel | string | `"INFO"` | Minimum log level to output. Value should be DEBUG, INFO, WARN, or ERROR. |
| spegel.mirrorResolveRetries | int | `3` | Max ammount of mirrors to attempt. |
| spegel.mirrorResolveTimeout | string | `"20ms"` | Max duration spent finding a mirror. |
| spegel.registries | list | `["https://cgr.dev","https://docker.io","https://ghcr.io","https://quay.io","https://mcr.microsoft.com","https://public.ecr.aws","https://gcr.io","https://registry.k8s.io","https://k8s.gcr.io","https://lscr.io"]` | Registries for which mirror configuration will be created. |
| spegel.resolveLatestTag | bool | `true` | When true latest tags will be resolved to digests. |
| spegel.resolveTags | bool | `true` | When true Spegel will resolve tags to digests. |
| tolerations | list | `[{"key":"CriticalAddonsOnly","operator":"Exists"},{"effect":"NoExecute","operator":"Exists"},{"effect":"NoSchedule","operator":"Exists"}]` | Tolerations for pod assignment. |
| updateStrategy | object | `{}` | An update strategy to replace existing pods with new pods. |
