---
title: "Monitoring and Alerting using InfluxDB — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring/monitor-and-alert-using-influxdb.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring/monitor-and-alert-using-influxdb"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Monitoring and Alerting using InfluxDB — SILO (MinIO Object Storage)"
---
# Monitoring and Alerting using InfluxDB

MinIO publishes cluster and node metrics using the [Prometheus Data Model](https://prometheus.io/docs/concepts/data_model/).
[InfluxDB](https://www.influxdata.com/?ref=minio) supports scraping MinIO metrics data for monitoring and alerting.

The procedure on this page documents the following:

Configuring an InfluxDB service to scrape and display metrics from a MinIO deployment

Configuring an Alert on a MinIO metric

Prerequisites

This procedure requires the following:

An existing InfluxDB deployment configured with one or more [notification endpoints](https://docs.influxdata.com/influxdb/v2.4/notification-endpoints/)

An existing MinIO deployment with network access to the InfluxDB deployment

An [[reference/minio-mc#command-mc|`mc`]] installation on your local host configured to [[reference/minio-mc/mc-alias-set#alias|access]] the MinIO deployment

These instructions use [[operations/monitoring/metrics-v2#minio-metrics-v2|version 2 metrics.]]
For more about metrics API versions, see [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|Metrics and alerts.]]

For MinIO Deployments on Kubernetes, this procedure assumes all necessary network control components, such as Ingress or Load Balancers, to facilitate access between the MinIO Tenant and the InfluxDB service.

## Configure InfluxDB to Collect and Alert using MinIO Metrics

Important

This procedure specifically uses the InfluxDB UI to create a scraping endpoint.

The InfluxDB UI does not provide the same level of configuration as using [Telegraf](https://docs.influxdata.com/telegraf/v1.24/) and the corresponding [Prometheus plugin](https://github.com/influxdata/telegraf/blob/release-1.24/plugins/inputs/prometheus/README.md).
Specifically:

You cannot enable authenticated access to the MinIO metrics endpoint via the InfluxDB UI

You cannot set a tag for collected metrics (e.g. `url_tag`) for uniquely identifying the metrics for a given MinIO deployment

The Telegraf Prometheus plugin also supports Kubernetes-specific features, such as scraping the `minio` service for a given MinIO Tenant.

Configuring Telegraf is out of scope for this procedure.
You can use this procedure as general guidance for configuring Telegraf to scrape MinIO metrics.

Configure Public Access to MinIO Metrics

Set the [[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_PROMETHEUS_AUTH_TYPE|`MINIO_PROMETHEUS_AUTH_TYPE`]] environment variable to `"public"` for all nodes in the MinIO deployment.
You can then restart the deployment to allow public access to MinIO metrics.

You can validate the change by attempting to `curl` the metrics endpoint:

```
curl https://HOSTNAME/minio/v2/metrics/cluster
```

Replace `HOSTNAME` with the URL of the load balancer or reverse proxy through which you access the MinIO deployment.
You can alternatively specify any single node as `HOSTNAME:PORT`, specifying the MinIO server API port in addition to the node hostname.

The response body should include a list of collected MinIO metrics.

Log into the InfluxDB UI and Create a Bucket

Select the [Organization](https://docs.influxdata.com/influxdb/v2.4/organizations/view-orgs/) under which you want to store MinIO metrics.

Create a [New Bucket](https://docs.influxdata.com/influxdb/v2.4/organizations/buckets/create-bucket/) in which to store metrics for the MinIO deployment.

Create a new Scraping Source

Create a [new InfluxDB Scraper](https://docs.influxdata.com/influxdb/v2.4/write-data/no-code/scrape-data/manage-scrapers/create-a-scraper/).

Specify the full URL to the MinIO deployment, including the metrics endpoint:

```
https://HOSTNAME/minio/v2/metrics/cluster
```

Replace `HOSTNAME` with the URL of the load balancer or reverse proxy through which you access the MinIO deployment.
You can alternatively specify any single node as `HOSTNAME:PORT`, specifying the MinIO server API port in addition to the node hostname.

Validate the Data

Use the [DataExplorer](https://docs.influxdata.com/influxdb/v2.4/query-data/execute-queries/data-explorer/) to visualize the collected MinIO data.

For example, you can set a filter on `minio_cluster_capacity_usable_total_bytes` and `minio_cluster_capacity_usable_free_bytes` to compare the total usable against total free space on the MinIO deployment.

Configure a Check

Create a [new Check](https://docs.influxdata.com/influxdb/v2.4/https://docs.influxdata.com/influxdb/v2.4/monitor-alert/checks/create/) on a MinIO metric.

The following example check rules provide a baseline of alerts for a MinIO deployment.
You can modify or otherwise use these examples for guidance in building your own checks.

Create a Threshold Check named `MINIO_NODE_DOWN`.

Set the filter for the `minio_cluster_nodes_offline_total` key.

Set the Thresholds to WARN when the value is greater than 1

Create a Threshold Check named `MINIO_QUORUM_WARNING`.

Set the filter for the `minio_cluster_drive_offline_total` key.

Set the Thresholds to CRITICAL when the value is one less than your configured [[operations/concepts/erasure-coding#minio-erasure-coding|Erasure Code Parity]] setting.

For example, a deployment using EC:4 should set this value to `3`.

Configure your [Notification endpoints](https://docs.influxdata.com/influxdb/v2.4/monitor-alert/notification-endpoints/) and [Notification rules](https://docs.influxdata.com/influxdb/v2.4/monitor-alert/notification-rules/) such that checks of each type trigger an appropriate response.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).