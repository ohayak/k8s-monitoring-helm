<!--
(NOTE: Do not edit README.md directly. It is a generated file!)
(      To make changes, please modify README.md.gotmpl and run `helm-docs`)
-->

# k8s-monitoring-feature-auto-instrumentation

![Version: 1.0.0](https://img.shields.io/badge/Version-1.0.0-informational?style=flat-square) ![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=flat-square)
Gathers telemetry data via automatic instrumentation

The annotation-based autodiscovery feature makes it easy to add scrape targets. With this feature enabled, any
Kubernetes Pods or Services with the `k8s.grafana.com/scrape` annotation set to `true` will be automatically discovered
and scraped by the collector. There are several other annotations that can be used to customize the behavior of the
scrape configuration, such as:

*   `k8s.grafana.com/job`: The value to use for the `job` label.
*   `k8s.grafana.com/instance`: The value to use for the `instance` label.
*   `k8s.grafana.com/metrics.path`: The path to scrape for metrics. Defaults to `/metrics`.
*   `k8s.grafana.com/metrics.portNumber`: The port on the Pod or Service to scrape for metrics. This is used to target a specific port by its number, rather than all ports.
*   `k8s.grafana.com/metrics.portName`: The named port on the Pod or Service to scrape for metrics. This is used to target a specific port by its name, rather than all ports.
*   `k8s.grafana.com/metrics.scheme`: The scheme to use when scraping metrics. Defaults to `http`.
*   `k8s.grafana.com/metrics.scrapeInterval`: The scrape interval to use when scraping metrics. Defaults to `60s`.

## Testing

This chart contains unit tests to verify the generated configuration. A hidden value, `deployAsConfigMap`, will render
the generated configuration into a ConfigMap object. This ConfigMap is not used during regular operation, but it is
useful for showing the outcome of a given values file.

The unit tests use this to create an object with the configuration that can be asserted against. To run the tests, use
`helm test`.

Actual integration testing in a live environment should be done in the main [k8s-monitoring](../k8s-monitoring) chart.

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| petewall | <pete.wall@grafana.com> |  |
<!-- markdownlint-disable no-bare-urls -->
<!-- markdownlint-disable list-marker-space -->
## Source Code

* <https://github.com/grafana/k8s-monitoring-helm/tree/main/charts/feature-annotation-autodiscovery>
<!-- markdownlint-enable list-marker-space -->
<!-- markdownlint-enable no-bare-urls -->
## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://grafana.github.io/helm-charts | beyla | 1.4.3 |
## Values

### Deployment: Beyla

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| beyla.config.data | object | `{"attributes":{"kubernetes":{"enable":true}},"internal_metrics":{"prometheus":{"path":"/internal/metrics","port":9090}},"prometheus_export":{"features":["application","network","application_service_graph","application_span"],"path":"/metrics","port":9090}}` | The Configuration for Beyla |

### Beyla

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| beyla.extraDiscoveryRules | string | `""` | Rule blocks to be added to the discovery.relabel component for Beyla. These relabeling rules are applied pre-scrape against the targets from service discovery. Before the scrape, any remaining target labels that start with __ (i.e. __meta_kubernetes*) are dropped. ([docs](https://grafana.com/docs/alloy/latest/reference/components/discovery.relabel/#rule-block)) |
| beyla.extraMetricProcessingRules | string | `""` | Rule blocks to be added to the prometheus.relabel component for Beyla. ([docs](https://grafana.com/docs/alloy/latest/reference/components/prometheus.relabel/#rule-block)) These relabeling rules are applied post-scrape against the metrics returned from the scraped target, no __meta* labels are present. |
| beyla.labelMatchers | object | `{"app.kubernetes.io/name":"beyla"}` | Label matchers used to select the Beyla pods |
| beyla.maxCacheSize | string | 100000 | Sets the max_cache_size for the prometheus.relabel component for Beyla. This should be at least 2x-5x your largest scrape target or samples appended rate. ([docs](https://grafana.com/docs/alloy/latest/reference/components/prometheus.relabel/#arguments)) Overrides metrics.maxCacheSize |
| beyla.metricsTuning.excludeMetrics | list | `[]` | Metrics to drop. Can use regular expressions. |
| beyla.metricsTuning.includeMetrics | list | `[]` | Metrics to keep. Can use regular expressions. |
| beyla.scrapeInterval | string | 60s | How frequently to scrape metrics from Beyla. Overrides metrics.scrapeInterval |

### General settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| fullnameOverride | string | `""` | Full name override |
| nameOverride | string | `""` | Name override |

### Global Settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| global.maxCacheSize | int | `100000` | Sets the max_cache_size for every prometheus.relabel component. ([docs](https://grafana.com/docs/alloy/latest/reference/components/prometheus.relabel/#arguments)) This should be at least 2x-5x your largest scrape target or samples appended rate. |
| global.scrapeInterval | string | `"60s"` | How frequently to scrape metrics. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| beyla.traces.endpoint | string | `""` |  |
