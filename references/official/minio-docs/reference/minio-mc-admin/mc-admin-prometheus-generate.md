---
title: "mc admin prometheus generate — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-prometheus-generate.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-prometheus-generate"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin prometheus generate — SILO (MinIO Object Storage)"
---
# mc admin prometheus generate

## Description

The [`mc admin prometheus generate`](#command-mc.admin.prometheus.generate) command generates a metrics scraping configuration file for use with [Prometheus](https://prometheus.io/).

For more complete documentation on using MinIO with Prometheus, see [[operations/monitoring/collect-minio-metrics-using-prometheus#minio-metrics-collect-using-prometheus|How to monitor MinIO server with Prometheus]]

Starting with MinIO Server [RELEASE.2024-07-15T19-02-30Z](https://github.com/minio/minio/releases/tag/RELEASE.2024-07-15T19-02-30Z) and MinIO Client [RELEASE.2024-07-11T18-01-28Z](https://github.com/minio/mc/releases/tag/RELEASE.2024-07-11T18-01-28Z), [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|metrics version 3 (v3)]] provides additional endpoints and metrics.
To generate a v3 scrape configuration use the `--api_version v3` option.

MinIO recommends new deployments use [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|version 3 (v3)]].
Existing deployments can continue to use [[operations/monitoring/metrics-v2#minio-metrics-v2|metrics version 2]]

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

EXAMPLE

The following command generates a Prometheus scrape configuration that collects version 2 bucket metrics from the deployment at [[glossary#term-alias|alias]] `myminio`:

```
mc admin prometheus generate myminio bucket
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin prometheus generate                                        \
                                  ALIAS                                           \
                                  [TYPE]                                          \
                                  [--api_version v3]                              \
                                  [--bucket <bucket name>]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.prometheus.generate.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment for which the command generates a Prometheus-compatible configuration file.

--api-version[](#mc.admin.prometheus.generate.-api-version)
Optional

To generate a scrape configuration for [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|v3 metrics]], include  an `--api-version v3` parameter.
`v3` is the only accepted value.

Omit `--api-version` to generate a [[operations/monitoring/metrics-v2#minio-metrics-v2|v2 metrics]] configuration.

--bucket[](#mc.admin.prometheus.generate.-bucket)
Optional

Only valid for v3 metrics.

For v3 metric types that return bucket-level metrics, specify a bucket name.
Requires [`--api-version`](#mc.admin.prometheus.generate.-api-version).

`--bucket` works for the following v3 metric types:

`api`

`replication`

The following example generates a configuration for API metrics from the bucket `mybucket`:

```
mc admin prometheus generate ALIAS api --bucket mybucket --api-version v3
```

TYPE[](#mc.admin.prometheus.generate.TYPE)
Optional

The type of metrics to scrape.

Valid values for metrics version 3 are:

`api`

`audit`

`cluster`

`debug`

`ilm`

`logger`

`notification`

`replication`

`scanner`

`system`

If not specified, a `v3` command returns all metrics.

Valid values for metrics version 2 are:

`bucket`

`cluster`

`node`

`resource`

If not specified, a `v2` command returns cluster metrics.
Cluster metrics include rollups of certain node metrics.

### Global flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Generate a default metrics v3 config

Use [`mc admin prometheus generate --api-version v3`](#mc.admin.prometheus.generate.-api-version) to generate a scrape configuration that collects all v3 metrics for a MinIO deployment:

```
mc admin prometheus generate ALIAS --api-version v3
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The output resembles the following:

```
scrape_configs:
- job_name: minio-job
  bearer_token: [auth token]
  metrics_path: /minio/metrics/v3
  scheme: http
  static_configs:
  - targets: ['localhost:9000']
```

### Generate a v3 metrics config for another type

To generate a configuration for another metric type, specify the type.
The following generates a scrape configuration for v3 cluster metrics:

```
mc admin prometheus generate ALIAS cluster --api-version v3
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The output resembles the following:

```
scrape_configs:
- job_name: minio-job-cluster
  bearer_token: [auth token]
  metrics_path: /minio/metrics/v3/cluster
  scheme: http
  static_configs:
  - targets: ['localhost:9000']
```

To generate a configuration for a [`different metric type`](#mc.admin.prometheus.generate.TYPE), replace `cluster` with the desired type.

### Generate a v3 bucket replication metrics config

The following example generates a scrape configuration for v3 replication metrics of bucket `mybucket`:

```
mc admin prometheus generate ALIAS replication --bucket mybucket --api-version v3
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The output resembles the following:

```
scrape_configs:
- job_name: minio-job-replication
  bearer_token: [auth token]
  metrics_path: /minio/metrics/v3/bucket/replication/mybucket
  scheme: https
  static_configs:
  - targets: [`localhost:9000`]
```

### Generate a v3 config for bucket API metrics

The following example generates a scrape configuration for v3 API metrics for bucket `mybucket`:

```
mc admin prometheus generate ALIAS api --bucket mybucket --api-version v3
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The output resembles the following:

```
scrape_configs:
- job_name: minio-job-api
  bearer_token: [auth token]
  metrics_path: /minio/metrics/v3/bucket/api/mybucket
  scheme: https
  static_configs:
  - targets: [`localhost:9000`]
```

### Generate a default metrics v2 config

By default, [`mc admin prometheus generate`](#command-mc.admin.prometheus.generate) generates a scrape configuration for v2 cluster metrics:

```
mc admin prometheus generate ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The output resembles the following:

```
scrape_configs:
- job_name: minio-job
  bearer_token: [auth token]
  metrics_path: /minio/v2/metrics
  scheme: http
  static_configs:
  - targets: ['localhost:9000']
```

### Generate a v2 config for other metric types

To generate a configuration for another metric type, specify the type.
The following generates a scrape configuration for v2 bucket metrics:

```
mc admin prometheus generate ALIAS bucket
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).