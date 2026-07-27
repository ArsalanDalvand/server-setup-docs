---
title: "Metrics and alerts — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring/metrics-and-alerts.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring/metrics-and-alerts"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Metrics and alerts — SILO (MinIO Object Storage)"
---
# Metrics and alerts

MinIO publishes metrics using the [Prometheus Data Model](https://prometheus.io/docs/concepts/data_model/).
You can use any scraping tool to pull metrics data from MinIO for further analysis and alerting.

Starting with MinIO Server [RELEASE.2024-07-15T19-02-30Z](https://github.com/minio/minio/releases/tag/RELEASE.2024-07-15T19-02-30Z) and MinIO Client [RELEASE.2024-07-11T18-01-28Z](https://github.com/minio/mc/releases/tag/RELEASE.2024-07-11T18-01-28Z), metrics version 3 provides additional endpoints.
MinIO recommends version 3 for new deployments.

Metrics version 2

Existing deployments can continue to use version 2 [[operations/monitoring/metrics-v2#minio-metrics-v2|metrics]] and [[operations/monitoring/grafana#minio-grafana|Grafana dashboards]].

## Version 3 Endpoints

For metrics version 3, all metrics are available under the base `/minio/metrics/v3` endpoint.
You can scrape the base endpoint to collect all metrics in a single operation, or append an optional path to return a specific category.

Important

The V3 metrics on this page may have gaps, inaccuracies, or incorrect information.
Reference the [minio/minio](https://github.com/minio/minio) repository and review the source code for the most accurate representation of metrics as available.

For example, the following endpoint returns audit metrics:

```
http://HOSTNAME:PORT/minio/metrics/v3/audit
```

Replace `HOSTNAME:PORT` with the FQDN and port of the MinIO deployment.
For deployments with a load balancer managing connections between MinIO nodes, specify the address of the load balancer.

By default, MinIO requires authentication to scrape the metrics endpoints.
To generate the needed bearer tokens, use [[reference/minio-mc-admin/mc-admin-prometheus-generate#command-mc.admin.prometheus.generate|`mc admin prometheus generate`]].
You can also disable metrics endpoint authentication by setting [[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_PROMETHEUS_AUTH_TYPE|`MINIO_PROMETHEUS_AUTH_TYPE`]] to `public`.

MinIO provides the following scraping endpoints, relative to the base URL:

Category

Path

API

`/api/requests`

`/bucket/api`

Audit

`/audit`

Cluster

`/cluster/config`

`/cluster/erasure-set`

`/cluster/health`

`/cluster/iam`

`/cluster/usage/buckets`

`/cluster/usage/objects`

Debug

`/debug/go`

ILM

`/ilm`

Logger webhook

`/logger/webhook`

Notification

`/notification`

Replication

`/replication`

`/bucket/replication`

Scanner

`/scanner`

System

`/system/drive`

`/system/memory`

`/system/cpu`

`/system/network/internode`

`/system/process`

For a complete list of metrics for each endpoint, see [Available version 3 metrics](#minio-metrics-and-alerts-available-metrics).

To enable historical data visualization in MinIO Console, set the following environment variables on each node in the MinIO deployment:

Set [[reference/minio-server/settings/console#envvar.MINIO_PROMETHEUS_URL|`MINIO_PROMETHEUS_URL`]] to the URL of the Prometheus service

Set [[reference/minio-server/settings/console#envvar.MINIO_PROMETHEUS_JOB_ID|`MINIO_PROMETHEUS_JOB_ID`]] to the unique job ID assigned to the collected metrics

## Available version 3 metrics

MinIO publishes a number of metrics for clusters, API requests, buckets, and other aspects of the MinIO service:

[API Metrics](#minio-available-v3-api-metrics)

[Audit Metrics](#minio-available-v3-audit-metrics)

[Cluster Metrics](#minio-available-v3-cluster-metrics)

[Debug Metrics](#minio-available-v3-debug-metrics)

[ILM Metrics](#minio-available-v3-ilm-metrics)

[Logger webhook Metrics](#minio-available-v3-logger-webhook-metrics)

[Notification Metrics](#minio-available-v3-notification-metrics)

[Replication Metrics](#minio-available-v3-replication-metrics)

[Scanner Metrics](#minio-available-v3-scanner-metrics)

[System Metrics](#minio-available-v3-system-metrics)

Many metrics include labels identifying the resource which generated that metric and other relevant details.

### API metrics

Metrics about requests served by the current node.

Path

Description

`/api/requests`

Metrics over all requests.

`/bucket/api`

Metrics over all requests for a given bucket.

#### /api/requests

Name

Description

Labels

`minio_api_requests_rejected_auth_total`

Total number of requests rejected for auth failure. Type: counter

`type`, `pool_index`, `server`

`minio_api_requests_rejected_header_total`

Total number of requests rejected for invalid header. Type: counter

`type`, `pool_index`, `server`

`minio_api_requests_rejected_timestamp_total`

Total number of requests rejected for invalid timestamp. Type: counter

`type`, `pool_index`, `server`

`minio_api_requests_rejected_invalid_total`

Total number of invalid requests. Type: counter

`type`, `pool_index`, `server`

`minio_api_requests_waiting_total`

Total number of requests in the waiting queue. Type: gauge

`type`, `pool_index`, `server`

`minio_api_requests_incoming_total`

Total number of incoming requests. Type: gauge

`type`, `pool_index`, `server`

`minio_api_requests_inflight_total`

Total number of requests currently in flight. Type: gauge

`name`, `type`, `pool_index`, `server`

`minio_api_requests_total`

Total number of requests. Type: counter

`name`, `type`, `pool_index`, `server`

`minio_api_requests_errors_total`

Total number of requests with 4xx or 5xx errors. Type: counter

`name`, `type`, `pool_index`, `server`

`minio_api_requests_5xx_errors_total`

Total number of requests with 5xx errors. Type: counter

`name`, `type`, `pool_index`, `server`

`minio_api_requests_4xx_errors_total`

Total number of requests with 4xx errors. Type: counter

`name`, `type`, `pool_index`, `server`

`minio_api_requests_canceled_total`

Total number of requests canceled by the client. Type: counter

`name`, `type`, `pool_index`, `server`

`minio_api_requests_ttfb_seconds_distribution`

Distribution of time to first byte across API calls. Type: counter

`name`, `type`, `le`, `pool_index`, `server`

`minio_api_requests_traffic_sent_bytes`

Total number of bytes sent. Type: counter

`type`, `pool_index`, `server`

`minio_api_requests_traffic_received_bytes`

Total number of bytes received. Type: counter

`type`, `pool_index`, `server`

#### /bucket/api

Name

Description

Labels

`minio_bucket_api_traffic_received_bytes`

Total number of bytes sent for a bucket. Type: counter

`bucket`, `type`, `server`, `pool_index`

`minio_bucket_api_traffic_sent_bytes`

Total number of bytes received for a bucket. Type: counter

`bucket`, `type`, `server`, `pool_index`

`minio_bucket_api_inflight_total`

Total number of requests currently in flight for a bucket. Type: gauge

`bucket`, `name`, `type`, `server`, `pool_index`

`minio_bucket_api_total`

Total number of requests for a bucket. Type: counter

`bucket`, `name`, `type`, `server`, `pool_index`

`minio_bucket_api_canceled_total`

Total number of requests canceled by the client for a bucket. Type: counter

`bucket`, `name`, `type`, `server`, `pool_index`

`minio_bucket_api_4xx_errors_total`

Total number of requests with 4xx errors for a bucket. Type: counter

`bucket`, `name`, `type`, `server`, `pool_index`

`minio_bucket_api_5xx_errors_total`

Total number of requests with 5xx errors for a bucket. Type: counter

`bucket`, `name`, `type`, `server`, `pool_index`

`minio_bucket_api_ttfb_seconds_distribution`

Distribution of time to first byte across API calls for a bucket. Type: counter

`bucket`, `name`, `le`, `type`, `server`, `pool_index`

### Audit metrics

Metrics about the MinIO audit functionality.

Path

Description

`/audit`

Metrics related to audit functionality.

#### /audit

Name

Description

Labels

`minio_audit_failed_messages`

Total number of messages that failed to send since start. Type: counter

`target_id`, `server`

`minio_audit_target_queue_length`

Number of unsent messages in queue for target. Type: gauge

`target_id`, `server`

`minio_audit_total_messages`

Total number of messages sent since start. Type: counter

`target_id`, `server`

### Cluster metrics

Metrics about an entire MinIO cluster.

Path

Description

`/cluster/config`

Cluster configuration metrics.

`/cluster/erasure-set`

Erasure set metrics.

`/cluster/health`

Cluster health metrics.

`/cluster/iam`

Cluster iam metrics.

`/cluster/usage/buckets`

Object statistics by bucket.

`/cluster/usage/objects`

Object statistics.

#### /cluster/config

Name

Description

Labels

`minio_cluster_config_rrs_parity`

Reduced redundancy storage class parity. Type: gauge

`minio_cluster_config_standard_parity`

Standard storage class parity. Type: gauge

#### /cluster/erasure-set

Name

Description

Labels

`minio_cluster_erasure_set_overall_write_quorum`

Overall write quorum across pools and sets. Type: gauge

`minio_cluster_erasure_set_overall_health`

Overall health across pools and sets (1=healthy, 0=unhealthy). Type: gauge

`minio_cluster_erasure_set_read_quorum`

Read quorum for the erasure set in a pool. Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_write_quorum`

Write quorum for the erasure set in a pool. Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_online_drives_count`

Count of online drives in the erasure set in a pool. Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_healing_drives_count`

Count of healing drives in the erasure set in a pool. Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_health`

Health of the erasure set in a pool (1=healthy, 0=unhealthy). Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_read_tolerance`

Number of drive failures that can be tolerated without disrupting read operations. Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_write_tolerance`

Number of drive failures that can be tolerated without disrupting write operations. Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_read_health`

Health of the erasure set in a pool for read operations (1=healthy, 0=unhealthy). Type: gauge

`pool_id`, `set_id`

`minio_cluster_erasure_set_write_health`

Health of the erasure set in a pool for write operations (1=healthy, 0=unhealthy). Type: gauge

`pool_id`, `set_id`

#### /cluster/health

Name

Description

Labels

`minio_cluster_health_drives_offline_count`

Count of offline drives in the cluster. Type: gauge

`minio_cluster_health_drives_online_count`

Count of online drives in the cluster. Type: gauge

`minio_cluster_health_drives_count`

Count of all drives in the cluster. Type: gauge

`minio_cluster_health_nodes_offline_count`

Count of offline nodes in the cluster. Type: gauge

`minio_cluster_health_nodes_online_count`

Count of online nodes in the cluster. Type: gauge

`minio_cluster_health_capacity_raw_total_bytes`

Total cluster raw storage capacity in bytes. Type: gauge

`minio_cluster_health_capacity_raw_free_bytes`

Total cluster raw storage free in bytes. Type: gauge

`minio_cluster_health_capacity_usable_total_bytes`

Total cluster usable storage capacity in bytes. Type: gauge

`minio_cluster_health_capacity_usable_free_bytes`

Total cluster usable storage free in bytes. Type: gauge

#### /cluster/iam

Name

Description

Labels

`minio_cluster_iam_last_sync_duration_millis`

Last successful IAM data sync duration in milliseconds. Type: counter

`minio_cluster_iam_plugin_authn_service_failed_requests_minute`

When plugin authentication is configured, returns failed requests count in the last full minute. Type: counter

`minio_cluster_iam_plugin_authn_service_last_fail_seconds`

When plugin authentication is configured, returns time (in seconds) since the last failed request to the service. Type: counter

`minio_cluster_iam_plugin_authn_service_last_succ_seconds`

When plugin authentication is configured, returns time (in seconds) since the last successful request to the service. Type: counter

`minio_cluster_iam_plugin_authn_service_succ_avg_rtt_ms_minute`

When plugin authentication is configured, returns average round-trip time of successful requests in the last full minute. Type: counter

`minio_cluster_iam_plugin_authn_service_succ_max_rtt_ms_minute`

When plugin authentication is configured, returns maximum round-trip time of successful requests in the last full minute. Type: counter

`minio_cluster_iam_plugin_authn_service_total_requests_minute`

When plugin authentication is configured, returns total requests count in the last full minute. Type: counter

`minio_cluster_iam_since_last_sync_millis`

Time (in milliseconds) since last successful IAM data sync. Type: counter

`minio_cluster_iam_sync_failures`

Number of failed IAM data syncs since server start. Type: counter

`minio_cluster_iam_sync_successes`

Number of successful IAM data syncs since server start. Type: counter

#### /cluster/usage/buckets

Name

Description

Labels

`minio_cluster_usage_buckets_since_last_update_seconds`

Time since last update of usage metrics in seconds. Type: gauge

`minio_cluster_usage_buckets_total_bytes`

Total bucket size in bytes. Type: gauge

`bucket`

`minio_cluster_usage_buckets_objects_count`

Total object count in bucket. Type: gauge

`bucket`

`minio_cluster_usage_buckets_versions_count`

Total object versions count in bucket, including delete markers. Type: gauge

`bucket`

`minio_cluster_usage_buckets_delete_markers_count`

Total delete markers count in bucket. Type: gauge

`bucket`

`minio_cluster_usage_buckets_quota_total_bytes`

Total bucket quota in bytes. Type: gauge

`bucket`

`minio_cluster_usage_buckets_object_size_distribution`

Bucket object size distribution. Type: gauge

`range`, `bucket`

`minio_cluster_usage_buckets_object_version_count_distribution`

Bucket object version count distribution. Type: gauge

`range`, `bucket`

#### /cluster/usage/objects

Name

Description

Labels

`minio_cluster_usage_objects_since_last_update_seconds`

Time since last update of usage metrics in seconds. Type: gauge

`minio_cluster_usage_objects_total_bytes`

Total cluster usage in bytes. Type: gauge

`minio_cluster_usage_objects_count`

Total cluster objects count. Type: gauge

`minio_cluster_usage_objects_versions_count`

Total cluster object versions count, including delete markers. Type: gauge

`minio_cluster_usage_objects_delete_markers_count`

Total cluster delete markers count. Type: gauge

`minio_cluster_usage_objects_buckets_count`

Total cluster buckets count. Type: gauge

`minio_cluster_usage_objects_size_distribution`

Cluster object size distribution. Type: gauge

`range`

`minio_cluster_usage_objects_version_count_distribution`

Cluster object version count distribution. Type: gauge

`range`

### Debug metrics

Standard Go runtime metrics from the [Prometheus Go Client base collector](https://github.com/prometheus/client_golang).

Path

Description

`/debug/go`

Go runtime metrics.

### ILM metrics

Metrics about the MinIO ILM functionality.

Path

Description

`/ilm`

Metrics related to ILM functionality.

#### /ilm

Name

Description

Labels

`minio_cluster_ilm_expiry_pending_tasks`

Number of pending ILM expiry tasks in the queue. Type: gauge

`server`

`minio_cluster_ilm_transition_active_tasks`

Number of active ILM transition tasks. Type: gauge

`server`

`minio_cluster_ilm_transition_pending_tasks`

Number of pending ILM transition tasks in the queue. Type: gauge

`server`

`minio_cluster_ilm_transition_missed_immediate_tasks`

Number of missed immediate ILM transition tasks. Type: counter

`server`

`minio_cluster_ilm_versions_scanned`

Total number of object versions checked for ILM actions since server start. Type: counter

`server`

### Logger webhook metrics

Metrics about MinIO logger webhooks.

Path

Description

`/logger/webhook`

Metrics related to logger webhooks.

#### /logger/webhook

Name

Description

Labels

`minio_logger_webhook_failed_messages`

Number of messages that failed to send. Type: counter

`server`, `name`, `endpoint`

`minio_logger_webhook_queue_length`

Webhook queue length. Type: gauge

`server`, `name`, `endpoint`

`minio_logger_webhook_total_message`

Total number of messages sent to this target. Type: counter

`server`, `name`, `endpoint`

### Notification metrics

Metrics about the MinIO notification functionality.

Path

Description

`/notification`

Metrics related to notification functionality.

#### /notification

Name

Description

Labels

`minio_notification_current_send_in_progress`

Number of concurrent async Send calls active to all targets. Type: counter

`server`

`minio_notification_events_errors_total`

Total number of events that failed to send to the targets. Type: counter

`server`

`minio_notification_events_sent_total`

Total number of events sent to the targets. Type: counter

`server`

`minio_notification_events_skipped_total`

Number of events not sent to the targets due to the in-memory queue being full. Type: counter

`server`

### Replication metrics

Metrics about MinIO site and bucket replication.

Path

Description

`/bucket/replication`

Metrics related to bucket replication.

`/replication`

Metrics related to site replication.

#### /replication

Name

Description

Labels

`minio_replication_average_active_workers`

Average number of active replication workers. Type: gauge

`server`

`minio_replication_average_queued_bytes`

Average number of bytes queued for replication since server start. Type: gauge

`server`

`minio_replication_average_queued_count`

Average number of objects queued for replication since server start. Type: gauge

`server`

`minio_replication_average_data_transfer_rate`

Average replication data transfer rate in bytes/sec. Type: gauge

`server`

`minio_replication_current_active_workers`

Total number of active replication workers. Type: gauge

`server`

`minio_replication_current_data_transfer_rate`

Current replication data transfer rate in bytes/sec. Type: gauge

`server`

`minio_replication_last_minute_queued_bytes`

Number of bytes queued for replication in the last full minute. Type: gauge

`server`

`minio_replication_last_minute_queued_count`

Number of objects queued for replication in the last full minute. Type: gauge

`server`

`minio_replication_max_active_workers`

Maximum number of active replication workers seen since server start. Type: gauge

`server`

`minio_replication_max_queued_bytes`

Maximum number of bytes queued for replication since server start. Type: gauge

`server`

`minio_replication_max_queued_count`

Maximum number of objects queued for replication since server start. Type: gauge

`server`

`minio_replication_max_data_transfer_rate`

Maximum replication data transfer rate in bytes/sec since server start. Type: gauge

`server`

`minio_replication_recent_backlog_count`

Total number of objects seen in replication backlog in the last 5 minutes Type: gauge

`server`

#### /bucket/replication

Name

Description

Labels

`minio_bucket_replication_last_hour_failed_bytes`

Total number of bytes on a bucket which failed to replicate at least once in the last hour. Type: gauge

`bucket`, `server`

`minio_bucket_replication_last_hour_failed_count`

Total number of objects on a bucket which failed to replicate in the last hour. Type: gauge

`bucket`, `server`

`minio_bucket_replication_last_minute_failed_bytes`

Total number of bytes on a bucket which failed at least once in the last full minute. Type: gauge

`bucket`, `server`

`minio_bucket_replication_last_minute_failed_count`

Total number of objects on a bucket which failed to replicate in the last full minute. Type: gauge

`bucket`, `server`

`minio_bucket_replication_latency_ms`

Replication latency on a bucket in milliseconds. Type: gauge

`bucket`, `operation`, `range`, `targetArn`, `server`

`minio_bucket_replication_proxied_delete_tagging_requests_total`

Number of DELETE tagging requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_get_requests_failures`

Number of failures in GET requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_get_requests_total`

Number of GET requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_get_tagging_requests_failures`

Number of failures in GET tagging requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_get_tagging_requests_total`

Number of GET tagging requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_head_requests_failures`

Number of failures in HEAD requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_head_requests_total`

Number of HEAD requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_put_tagging_requests_failures`

Number of failures in PUT tagging requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_put_tagging_requests_total`

Number of PUT tagging requests proxied to replication target. Type: counter

`bucket`, `server`

`minio_bucket_replication_sent_bytes`

Total number of bytes replicated to the target. Type: counter

`bucket`, `server`

`minio_bucket_replication_sent_count`

Total number of objects replicated to the target. Type: counter

`bucket`, `server`

`minio_bucket_replication_total_failed_bytes`

Total number of bytes failed to replicate at least once since server start. Type: counter

`bucket`, `server`

`minio_bucket_replication_total_failed_count`

Total number of objects that failed to replicate since server start. Type: counter

`bucket`, `server`

`minio_bucket_replication_proxied_delete_tagging_requests_failures`

Number of failures in DELETE tagging requests proxied to replication target. Type: counter

`bucket`, `server`

### Scanner metrics

Metrics about the MinIO scanner.

Path

Description

`/scanner`

Metrics related to the MinIO scanner.

#### /scanner

Name

Description

Labels

`minio_scanner_bucket_scans_finished`

Total number of bucket scans completed since server start. Type: counter

`server`

`minio_scanner_bucket_scans_started`

Total number of bucket scans started since server start. Type: counter

`server`

`minio_scanner_directories_scanned`

Total number of directories scanned since server start. Type: counter

`server`

`minio_scanner_last_activity_seconds`

Time elapsed (in seconds) since last scan activity. Type: gauge

`server`

`minio_scanner_objects_scanned`

Total number of unique objects scanned since server start. Type: counter

`server`

`minio_scanner_versions_scanned`

Total number of object versions scanned since server start. Type: counter

`server`

### System metrics

Metrics about the MinIO process and the node.

Path

Description

`/system/cpu`

Metrics about CPUs on the system.

`/system/drive`

Metrics about drives on the system.

`/system/network/internode`

Metrics about internode requests made by the node.

`/system/memory`

Metrics about memory on the system.

`/system/process`

Standard process metrics.

#### /system/drive

Name

Description

Labels

`minio_system_drive_used_bytes`

Total storage used on a drive in bytes. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_free_bytes`

Total storage free on a drive in bytes. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_total_bytes`

Total storage available on a drive in bytes. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_used_inodes`

Total used inodes on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_free_inodes`

Total free inodes on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_total_inodes`

Total inodes available on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_timeout_errors_total`

Total timeout errors on a drive. Type: counter

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_io_errors_total`

Total I/O errors on a drive. Type: counter

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_availability_errors_total`

Total availability errors (I/O errors, timeouts) on a drive. Type: counter

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_waiting_io`

Total waiting I/O operations on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_api_latency_micros`

Average last minute latency in µs for drive API storage operations. Type: gauge

`drive`, `api`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_offline_count`

Count of offline drives. Type: gauge

`pool_index`, `server`

`minio_system_drive_online_count`

Count of online drives. Type: gauge

`pool_index`, `server`

`minio_system_drive_count`

Count of all drives. Type: gauge

`pool_index`, `server`

`minio_system_drive_health`

Drive health (0 = offline, 1 = healthy, 2 = healing). Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_reads_per_sec`

Reads per second on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_reads_kb_per_sec`

Kilobytes read per second on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_reads_await`

Average time for read requests served on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_writes_per_sec`

Writes per second on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_writes_kb_per_sec`

Kilobytes written per second on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_writes_await`

Average time for write requests served on a drive. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

`minio_system_drive_perc_util`

Percentage of time the disk was busy. Type: gauge

`drive`, `set_index`, `drive_index`, `pool_index`, `server`

#### /system/memory

Name

Description

Labels

`minio_system_memory_used`

Used memory on the node. Type: gauge

`server`

`minio_system_memory_used_perc`

Used memory percentage on the node. Type: gauge

`server`

`minio_system_memory_free`

Free memory on the node. Type: gauge

`server`

`minio_system_memory_total`

Total memory on the node. Type: gauge

`server`

`minio_system_memory_buffers`

Buffers memory on the node. Type: gauge

`server`

`minio_system_memory_cache`

Cache memory on the node. Type: gauge

`server`

`minio_system_memory_shared`

Shared memory on the node. Type: gauge

`server`

`minio_system_memory_available`

Available memory on the node. Type: gauge

`server`

#### /system/cpu

Name

Description

Labels

`minio_system_cpu_avg_idle`

Average CPU idle time. Type: gauge

`server`

`minio_system_cpu_avg_iowait`

Average CPU IOWait time. Type: gauge

`server`

`minio_system_cpu_load`

CPU load average 1min. Type: gauge

`server`

`minio_system_cpu_load_perc`

CPU load average 1min (percentage). Type: gauge

`server`

`minio_system_cpu_nice`

CPU nice time. Type: gauge

`server`

`minio_system_cpu_steal`

CPU steal time. Type: gauge

`server`

`minio_system_cpu_system`

CPU system time. Type: gauge

`server`

`minio_system_cpu_user`

CPU user time. Type: gauge

`server`

#### /system/network/internode

Name

Description

Labels

`minio_system_network_internode_errors_total`

Total number of failed internode calls. Type: counter

`server`, `pool_index`

`minio_system_network_internode_dial_errors_total`

Total number of internode TCP dial timeouts and errors. Type: counter

`server`, `pool_index`

`minio_system_network_internode_dial_avg_time_nanos`

Average dial time of internodes TCP calls in nanoseconds. Type: gauge

`server`, `pool_index`

`minio_system_network_internode_sent_bytes_total`

Total number of bytes sent to other peer nodes. Type: counter

`server`, `pool_index`

`minio_system_network_internode_recv_bytes_total`

Total number of bytes received from other peer nodes. Type: counter

`server`, `pool_index`

#### /system/process

Name

Description

Labels

`minio_system_process_locks_read_total`

Number of current READ locks on this peer. Type: gauge

`server`

`minio_system_process_locks_write_total`

Number of current WRITE locks on this peer. Type: gauge

`server`

`minio_system_process_cpu_total_seconds`

Total user and system CPU time spent in seconds. Type: counter

`server`

`minio_system_process_go_routine_total`

Total number of go routines running. Type: gauge

`server`

`minio_system_process_io_rchar_bytes`

Total bytes read by the process from the underlying storage system including cache, /proc/[pid]/io rchar. Type: counter

`server`

`minio_system_process_io_read_bytes`

Total bytes read by the process from the underlying storage system, /proc/[pid]/io read_bytes. Type: counter

`server`

`minio_system_process_io_wchar_bytes`

Total bytes written by the process to the underlying storage system including page cache, /proc/[pid]/io wchar. Type: counter

`server`

`minio_system_process_io_write_bytes`

Total bytes written by the process to the underlying storage system, /proc/[pid]/io write_bytes. Type: counter

`server`

`minio_system_process_start_time_seconds`

Start time for MinIO process in seconds since Unix epoch. Type: gauge

`server`

`minio_system_process_uptime_seconds`

Uptime for MinIO process in seconds. Type: gauge

`server`

`minio_system_process_file_descriptor_limit_total`

Limit on total number of open file descriptors for the MinIO Server process. Type: gauge

`server`

`minio_system_process_file_descriptor_open_total`

Total number of open file descriptors by the MinIO Server process. Type: gauge

`server`

`minio_system_process_syscall_read_total`

Total read SysCalls to the kernel. /proc/[pid]/io syscr. Type: counter

`server`

`minio_system_process_syscall_write_total`

Total write SysCalls to the kernel. /proc/[pid]/io syscw. Type: counter

`server`

`minio_system_process_resident_memory_bytes`

Resident memory size in bytes. Type: gauge

`server`

`minio_system_process_virtual_memory_bytes`

Virtual memory size in bytes. Type: gauge

`server`

`minio_system_process_virtual_memory_max_bytes`

Maximum virtual memory size in bytes. Type: gauge

`server`

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).