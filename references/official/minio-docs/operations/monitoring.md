---
title: "Monitoring and Alerts — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Monitoring and Alerts — SILO (MinIO Object Storage)"
---
# Monitoring and Alerts

[Monitoring with MinIO and Prometheus: Overview](https://youtu.be/A3vCDaFWNNs?ref=docs)

[Monitoring with MinIO and Prometheus: Lab](https://youtu.be/Oix9iXndSUY?ref=docs)

## Metrics and Alerts

MinIO publishes point-in-time metrics using the [Prometheus Data Model](https://prometheus.io/docs/concepts/data_model/).
You can use any scraping tool which supports that data model to pull those metrics into a database for populating historical views, performing query/analysis of metrics data, or creating alerts on preferred data points.

The following table lists tutorials for integrating MinIO metrics with select third-party monitoring software.

[[operations/monitoring/collect-minio-metrics-using-prometheus#minio-metrics-collect-using-prometheus|Monitoring and Alerting using Prometheus]]

Configure Prometheus to Monitor and Alert for a MinIO deployment

[[operations/monitoring/monitor-and-alert-using-influxdb#minio-metrics-influxdb|Monitoring and Alerting using InfluxDB]]

Configure InfluxDB to Monitor and Alert for a MinIO deployment.

Other metrics and analytics software suites which support the Prometheus data model may work regardless of their inclusion on the above list.

## Logging

MinIO publishes all [[reference/minio-server/minio-server#command-minio.server|`minio server`]] operations to the system console.
MinIO also supports publishing server logs and audit logs to an HTTP webhook.

[[operations/monitoring/minio-logging#minio-logging-publish-server-logs|Server logs]] contain the same [[reference/minio-server/minio-server#command-minio.server|`minio server`]] operations logged to the system console.
Server logs support general monitoring and troubleshooting of operations.

[[operations/monitoring/minio-logging#minio-logging-publish-audit-logs|Audit logs]] are more granular descriptions of each operation on the MinIO deployment.
Audit logging supports security standards and regulations which require detailed tracking of operations.

MinIO publishes logs as a JSON document as a `PUT` request to each configured endpoint.
The endpoint server is responsible for processing each JSON document.
MinIO requires explicit configuration of each webhook endpoint and does not publish logs to a webhook by default.

See [[operations/monitoring/minio-logging#minio-logging|Publish Server or Audit Logs to an External Service]] for more complete documentation.

## Healthchecks

MinIO exposes unauthenticated endpoints for probing node uptime and  cluster [[operations/concepts/erasure-coding#minio-ec-parity|high availability]] for simple healthchecks.
These endpoints return only an HTTP status code.
See [[operations/monitoring/healthcheck-probe#minio-healthcheck-api|Healthcheck API]] for more information.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).