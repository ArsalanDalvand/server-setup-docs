---
title: "Monitor a MinIO Server with Grafana — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring/grafana.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring/grafana"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Monitor a MinIO Server with Grafana — SILO (MinIO Object Storage)"
---
# Monitor a MinIO Server with Grafana

[Grafana](https://grafana.com/) allows you to query, visualize, alert on and understand your metrics no matter where they are stored.

## Prerequisites

An existing [Prometheus deployment](https://prometheus.io/docs/prometheus/latest/installation/) with backing [Alert Manager](https://prometheus.io/docs/alerting/latest/overview/)

An existing MinIO deployment with network access to the Prometheus deployment

[Grafana installed](https://grafana.com/grafana/download)

Grafana dashboards use metrics version 2

The MinIO Grafana dashboards use [[operations/monitoring/metrics-v2#minio-metrics-v2|metrics version 2]].
For more about metrics API versions, see [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|Metrics and alerts.]]

Version 3 metrics require creating your own dashboard.
For more information about dashboards, see [the Grafana documentation.](https://grafana.com/docs/grafana/latest/dashboards/)

## MinIO Grafana Dashboard

MinIO provides several official Grafana Dashboards you can download from the Grafana Dashboard portal.

[MinIO Server metrics](#minio-server-grafana-metrics)

[MinIO Bucket metrics](#minio-buckets-grafana-metrics)

[MinIO Replication metrics](#minio-replication-grafana-metrics)

To track changes to the Grafana dashboard, introspect the JSON files for the [server](https://github.com/minio/minio/blob/master/docs/metrics/prometheus/grafana/minio-dashboard.json) or [bucket](https://github.com/minio/minio/blob/master/docs/metrics/prometheus/grafana/minio-bucket.json) dashboards in the MinIO Server GitHub repository.

### MinIO Server Metrics Dashboard

Visualize MinIO metrics with the official MinIO Grafana dashboard for the MinIO Server available on the [Grafana dashboard portal](https://grafana.com/grafana/dashboards/13502-minio-dashboard/).

MinIO provides a Grafana Dashboard for MinIO Server metrics.
For specifics on the dashboard’s configuration, see the [JSON file on GitHub](https://raw.githubusercontent.com/minio/minio/master/docs/metrics/prometheus/grafana/minio-dashboard.json).

For MinIO Deployments running with [[operations/server-side-encryption#minio-sse-data-encryption|Server-Side Encryption]] (SSE-KMS or SSE-S3), the dashboard includes metrics for the KMS.
These metrics include status, request error rates, and request success rates.

[](../../_images/grafana-minio1.png)

### MinIO Bucket Metrics Dashboard

Visualize MinIO bucket metrics with the official MinIO Grafana dashboard for buckets available on the [Grafana dashboard portal](https://grafana.com/grafana/dashboards/19237-minio-bucket-dashboard/).

Bucket metrics can be viewed in the Grafana dashboard using the [bucket JSON file on GitHub](https://raw.githubusercontent.com/minio/minio/master/docs/metrics/prometheus/grafana/bucket/minio-bucket.json).

[](../../_images/grafana-bucket1.png)

### MinIO Node Metrics Dashboard

Node metrics can be viewed in the Grafana dashboard using the [node JSON file on GitHub](https://raw.githubusercontent.com/minio/minio/master/docs/metrics/prometheus/grafana/node/minio-node.json).

[](../../_images/grafana-node1.png)

### MinIO Replication Metrics Dashboard

Visualize MinIO bucket metrics with the official MinIO Grafana dashboard for replication available on the [Grafana dashboard portal](https://grafana.com/grafana/dashboards/15305-minio-replication-dashboard/).

Cluster replication metrics can be viewed in the Grafana dashboard using the [cluster replication JSON file on GitHub](https://raw.githubusercontent.com/minio/minio/master/docs/metrics/prometheus/grafana/replication/minio-replication-cluster.json).

[](../../_images/grafana-replication1.png)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).