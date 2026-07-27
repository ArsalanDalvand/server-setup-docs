---
title: "Healthcheck API — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring/healthcheck-probe.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring/healthcheck-probe"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Healthcheck API — SILO (MinIO Object Storage)"
---
# Healthcheck API

MinIO exposes unauthenticated endpoints for probing node uptime and cluster
[[operations/concepts/erasure-coding#minio-ec-parity|high availability]] for simple healthchecks. These
endpoints return an HTTP status code indicating whether the underlying
resource is healthy or satisfies read/write quorum. MinIO exposes no other data
through these endpoints.

## Node Liveness

Use the following endpoint to test if a MinIO server is online:

```
curl -I https://minio.example.net:9000/minio/health/live
```

Replace `https://minio.example.net:9000` with the DNS hostname of the
MinIO server to check.

A response code of `200 OK` indicates the MinIO server is
online and functional. Any other HTTP codes indicate an issue with reaching
the server, such as a transient network issue or potential downtime.

The healthcheck probe alone cannot determine if a MinIO server is offline.
Instead, the probe determines whether the current host machine can reach the server.
Consider configuring a Prometheus [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|alert]] using `minio_cluster_health_nodes_offline_count` for [[operations/monitoring/metrics-and-alerts#minio-available-v3-cluster-metrics|metrics v3]] or `minio_cluster_nodes_offline_total` for [[operations/monitoring/metrics-v2#minio-available-cluster-metrics|metrics v2]] to detect whether one or more MinIO nodes are offline.

## Cluster Write Quorum

Use the following endpoint to test if a MinIO cluster has
[[operations/concepts/erasure-coding#minio-ec-parity|write quorum]]:

```
curl -I https://minio.example.net:9000/minio/health/cluster
```

Replace `https://minio.example.net:9000` with the DNS hostname of a node
in the MinIO cluster to check. For clusters using a load balancer to manage
incoming connections, specify the hostname for the load balancer.

A response code of `200 OK` indicates that the MinIO cluster has
sufficient MinIO servers online to meet write quorum. A response code of
`503 Service Unavailable` indicates the cluster does not currently have
write quorum.

The healthcheck probe alone cannot determine if a MinIO server is offline or
processing write operations normally - only whether enough MinIO servers are
online to meet write quorum  requirements based on the configured
[[operations/concepts/erasure-coding#minio-ec-parity|erasure code parity]]. Consider configuring a Prometheus
[[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|alert]] using one of the following
metrics to detect potential issues or errors on the MinIO cluster:

`minio_cluster_nodes_offline_total` to alert if one or more
MinIO nodes are offline.

`minio_node_drive_free_bytes` to alert if the cluster is running
low on free drive space.

## Cluster Read Quorum

Use the following endpoint to test if a MinIO cluster has
[[operations/concepts/erasure-coding#minio-ec-parity|read quorum]]:

```
curl -I https://minio.example.net:9000/minio/health/cluster/read
```

Replace `https://minio.example.net:9000` with the DNS hostname of a node
in the MinIO cluster to check. For clusters using a load balancer to manage
incoming connections, specify the hostname for the load balancer.

A response code of `200 OK` indicates that the MinIO cluster has
sufficient MinIO servers online to meet read quorum. A response code of
`503 Service Unavailable` indicates the cluster does not currently have
read quorum.

The healthcheck probe alone cannot determine if a MinIO server is offline or
processing read operations normally - only whether enough MinIO servers are
online to meet read quorum requirements based on the configured
[[operations/concepts/erasure-coding#minio-ec-parity|erasure code parity]]. Consider configuring a Prometheus
[[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|alert]] using the
`minio_cluster_nodes_offline_total` metric to detect whether one or more
MinIO nodes are offline.

## Cluster Maintenance Check

Use the following endpoint to test if the MinIO cluster can maintain
both [[operations/concepts/erasure-coding#minio-ec-parity|read]] and [[operations/concepts/erasure-coding#minio-ec-parity|write]]
if the specified MinIO server is taken down for maintenance:

```
curl -I https://minio.example.net:9000/minio/health/cluster?maintenance=true
```

Replace `https://minio.example.net:9000` with the DNS hostname of a node
in the MinIO cluster to check. For clusters using a load balancer to manage
incoming connections, specify the hostname for the load balancer.

A response code of `200 OK` indicates that the MinIO cluster has
sufficient MinIO servers online to meet write quorum. A response code of
`412 Precondition Failed` indicates the cluster will lose quorum if the
MinIO server goes offline.

The healthcheck probe alone cannot determine if a MinIO server is offline - only
whether enough MinIO servers will be online after taking the node down for
maintenance to meet read and write quorum requirements based on the configured
[[operations/concepts/erasure-coding#minio-ec-parity|erasure code parity]]. Consider configuring a Prometheus
[[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|alert]] using the `minio_cluster_nodes_offline_total` metric to detect whether one or more
MinIO nodes are offline.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).