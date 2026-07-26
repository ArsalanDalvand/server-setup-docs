---
title: "Transition cluster manager alerts to Prometheus alerts"
source: "https://redis.io/docs/latest/operate/rs/monitoring/metrics_stream_engine/alerts-v1-to-v2/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/metrics_stream_engine/alerts-v1-to-v2"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Transition cluster manager alerts to Prometheus alerts"
---
# Transition cluster manager alerts to Prometheus alerts

Transition from internal cluster manager alerts to external monitoring alerts using Prometheus.

            Redis Software
          

As Redis Software transitions from the [[operate/rs/monitoring/v1_monitoring|deprecated monitoring system]] to the [[operate/rs/monitoring/metrics_stream_engine|new metrics stream engine]], some internal cluster manager alerts were deprecated in favor of external monitoring solutions.

You can use the following table to transition from the deprecated alerts and set up equivalent alerts in Prometheus with [PromQL (Prometheus Query Language)](https://prometheus.io/docs/prometheus/latest/querying/basics/):

Cluster manager alert
Equivalent PromQL
Description

BdbSizeAlert
`sum by(db, cluster) (redis_server_used_memory) / sum by(db, cluster) (redis_server_maxmemory) > 0.8`
Redis server memory usage exceeds 80%

NodeMemoryAlert
`(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes > 0.7`
Node memory usage exceeds 70%

NodeFreeFlashAlert
`(node_available_flash_bytes - node_bigstore_free_bytes) / node_available_flash_bytes > 0.7`
Node flash storage usage exceeds 70%

NodeEphemeralStorageAlert
`(node_ephemeral_storage_avail_bytes - node_ephemeral_storage_free_bytes) / node_ephemeral_storage_avail_bytes > 0.7`
Node ephemeral storage usage exceeds 70%

NodePersistentStorageAlert
`(node_persistent_storage_avail_bytes - node_persistent_storage_free_bytes) / node_persistent_storage_avail_bytes > 0.7`
Node persistent storage usage exceeds 70%

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)