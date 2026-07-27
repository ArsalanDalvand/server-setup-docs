---
title: "Monitoring and Alerting using Prometheus — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring/collect-minio-metrics-using-prometheus.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring/collect-minio-metrics-using-prometheus"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Monitoring and Alerting using Prometheus — SILO (MinIO Object Storage)"
---
# Monitoring and Alerting using Prometheus

[Monitoring with MinIO and Prometheus: Overview](https://youtu.be/A3vCDaFWNNs?ref=docs)

[Monitoring with MinIO and Prometheus: Lab](https://youtu.be/Oix9iXndSUY?ref=docs)

MinIO publishes cluster, node, bucket, and resource metrics using the [Prometheus Data Model](https://prometheus.io/docs/concepts/data_model/#data-model).
The procedure on this page documents the following:

Configuring a Prometheus service to scrape and display metrics from a MinIO deployment

Configuring an Alert Rule on a MinIO Metric to trigger an AlertManager action

These instructions use [[operations/monitoring/metrics-v2#minio-metrics-v2|version 2 metrics.]]
For more about metrics API versions, see [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|Metrics and alerts.]]

Prerequisites

This procedure requires the following:

An existing [Prometheus deployment](https://prometheus.io/docs/prometheus/latest/installation/) with backing [Alert Manager](https://prometheus.io/docs/alerting/latest/overview/)

An existing MinIO deployment with network access to the Prometheus deployment

An [[reference/minio-mc#command-mc|`mc`]] installation on your local host configured to [[reference/minio-mc/mc-alias-set#alias|access]] the MinIO deployment

## Configure Prometheus to Collect and Alert using MinIO Metrics

### 1) Generate the Scrape Configuration

Use the [[reference/minio-mc-admin/mc-admin-prometheus-generate#command-mc.admin.prometheus.generate|`mc admin prometheus generate`]] command to generate the scrape configuration for use by Prometheus in making scraping requests:

MinIO Server

The following command scrapes metrics for the MinIO cluster.

```
mc admin prometheus generate ALIAS
```

Replace [[reference/minio-mc-admin/mc-admin-prometheus-generate#mc.admin.prometheus.generate.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The command returns output similar to the following:

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/cluster
     scheme: https
     static_configs:
     - targets: [minio.example.net]
```

Nodes

The following command scrapes metrics for a node on the MinIO Server.

```
mc admin prometheus generate ALIAS node
```

Replace [[reference/minio-mc-admin/mc-admin-prometheus-generate#mc.admin.prometheus.generate.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job-node
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/node
     scheme: https
     static_configs:
     - targets: [minio-1.example.net, minio-2.example.net, minio-N.example.net]
```

Buckets

The following command scrapes metrics for buckets on the MinIO Server.

```
mc admin prometheus generate ALIAS bucket
```

Replace [[reference/minio-mc-admin/mc-admin-prometheus-generate#mc.admin.prometheus.generate.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job-bucket
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/bucket
     scheme: https
     static_configs:
     - targets: [minio.example.net]
```

Resources

New in version RELEASE.2023-10-07T15-07-38Z.

The following command scrapes metrics for resources on the MinIO Server.

```
mc admin prometheus generate ALIAS resource
```

Replace [[reference/minio-mc-admin/mc-admin-prometheus-generate#mc.admin.prometheus.generate.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job-resource
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/resource
     scheme: https
     static_configs:
     - targets: [minio.example.net]
```

Set an appropriate `scrape_interval` value to ensure each scraping operation completes before the next one begins.
The recommended value is 60 seconds.

Some deployments require a longer scrape interval due to the number of metrics being scraped.
To reduce the load on your MinIO and Prometheus servers, choose the longest interval that meets your monitoring requirements.

Set the `job_name` to a value associated to the MinIO deployment.

Use a unique value to ensure isolation of the deployment metrics from any others collected by that Prometheus service.

MinIO deployments started with [[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_PROMETHEUS_AUTH_TYPE|`MINIO_PROMETHEUS_AUTH_TYPE`]] set to `"public"` can omit the `bearer_token` field.

Set the `scheme` to http for MinIO deployments not using TLS.

Set the `targets` array with a hostname that resolves to the MinIO deployment.

This can be any single node, or a load balancer/proxy which handles connections to the MinIO nodes.

For MinIO Tenants on Kubernetes infrastructure, when using a Prometheus cluster in that same cluster you can specify the service DNS name for the `minio` service.
You can otherwise specify the ingress or load balancer endpoint configured to route connections to and from the MinIO Tenant.

### 2) Restart Prometheus with the Updated Configuration

Append the desired `scrape_configs` job generated in the previous step to the configuration file:

Cluster

Cluster metrics aggregate node-level metrics and, where appropriate, attach labels to metrics for the originating node.

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/cluster
     scheme: https
     static_configs:
     - targets: [minio.example.net]
```

Nodes

Node metrics are specific for node-level monitoring. You need to list all MinIO nodes for this configuration.

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job-node
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/node
     scheme: https
     static_configs:
     - targets: [minio-1.example.net, minio-2.example.net, minio-N.example.net]
```

Bucket

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job-bucket
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/bucket
     scheme: https
     static_configs:
     - targets: [minio.example.net]
```

Resource

```
global:
   scrape_interval: 60s

scrape_configs:
   - job_name: minio-job-resource
     bearer_token: TOKEN
     metrics_path: /minio/v2/metrics/resource
     scheme: https
     static_configs:
     - targets: [minio.example.net]
```

Start the Prometheus cluster using the configuration file:

```
prometheus --config.file=prometheus.yaml
```

### 3) Analyze Collected Metrics

Prometheus includes an [expression browser](https://prometheus.io/docs/prometheus/latest/getting_started/#using-the-expression-browser).
You can execute queries here to analyze the collected metrics.

Examples

The following query examples return metrics collected by Prometheus every five minutes for a scrape job named `minio-job`:

```
minio_node_drive_free_bytes{job="minio-job"}[5m]
minio_node_drive_free_inodes{job="minio-job"}[5m]

minio_node_drive_latency_us{job="minio-job"}[5m]

minio_node_drive_offline_total{job="minio-job"}[5m]
minio_node_drive_online_total{job="minio-job"}[5m]

minio_node_drive_total{job="minio-job"}[5m]

minio_node_drive_total_bytes{job="minio-job"}[5m]
minio_node_drive_used_bytes{job="minio-job"}[5m]

minio_node_drive_errors_timeout{job="minio-job"}[5m]
minio_node_drive_errors_availability{job="minio-job"}[5m]

minio_node_drive_io_waiting{job="minio-job"}[5m]
```

Recommended Metrics

MinIO recommends the following as a basic set of metrics to monitor.

See [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|Metrics and alerts]] for information about all available metrics.

Metric

Description

`minio_node_drive_free_bytes`

Total storage available on a drive.

`minio_node_drive_free_inodes`

Total free inodes.

`minio_node_drive_latency_us`

Average last minute latency in µs for drive API storage operations.

`minio_node_drive_offline_total`

Total drives offline in this node.

`minio_node_drive_online_total`

Total drives online in this node.

`minio_node_drive_total`

Total drives in this node.

`minio_node_drive_total_bytes`

Total storage on a drive.

`minio_node_drive_used_bytes`

Total storage used on a drive.

`minio_node_drive_errors_timeout`

Total number of drive timeout errors since server start.

`minio_node_drive_errors_availability`

Total number of drive I/O errors, permission denied and timeouts since server start.

`minio_node_drive_io_waiting`

Total number of I/O operations waiting on drive.

### 4) Configure an Alert Rule using MinIO Metrics

You must configure [Alert Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/) on the Prometheus deployment to trigger alerts based on collected MinIO metrics.

The following example alert rule files provide a baseline of alerts for a MinIO deployment.
You can modify or otherwise use these examples as guidance in building your own alerts.

```
groups:
- name: minio-alerts
  rules:
  - alert: NodesOffline
    expr: avg_over_time(minio_cluster_nodes_offline_total{job="minio-job"}[5m]) > 0
    for: 10m
    labels:
      severity: warn
    annotations:
      summary: "Node down in MinIO deployment"
      description: "Node(s) in cluster {{ $labels.instance }} offline for more than 5 minutes"

  - alert: DisksOffline
    expr: avg_over_time(minio_cluster_drive_offline_total{job="minio-job"}[5m]) > 0
    for: 10m
    labels:
      severity: warn
    annotations:
      summary: "Disks down in MinIO deployment"
      description: "Disks(s) in cluster {{ $labels.instance }} offline for more than 5 minutes"
```

In the Prometheus configuration, specify the path to the alert file in the `rule_files` key:

```
rule_files:
- minio-alerting.yml
```

Once triggered, Prometheus sends the alert to the configured AlertManager service.

## Dashboards

MinIO provides Grafana Dashboards to display metrics collected by Prometheus.
For more information, see [[operations/monitoring/grafana#minio-grafana|Monitor a MinIO Server with Grafana]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).