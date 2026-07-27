---
title: "mc admin trace — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-trace.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-trace"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin trace — SILO (MinIO Object Storage)"
---
# mc admin trace

## Description

The [`mc admin trace`](#command-mc.admin.trace) command displays API operations occurring on the target MinIO deployment.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Examples

### Monitor All API operations

Use [`mc admin trace`](#command-mc.admin.trace) to monitor API operations on a MinIO deployment:

```
mc admin trace -a ALIAS
```

Replace [`ALIAS`](#mc.admin.trace.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

### See Calls that Return 503 Errors

Use [`mc admin trace`](#command-mc.admin.trace) to monitor API operations that return a service unavailable 503 error:

```
mc admin trace -v --status-code 503 ALIAS
```

Replace [`ALIAS`](#mc.admin.trace.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

### See Console Trace for a Path

Use [`mc admin trace`](#command-mc.admin.trace) to monitor activity for a specific path:

```
mc admin trace --path my-bucket/my-prefix/* ALIAS
```

Replace [`ALIAS`](#mc.admin.trace.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace `my-bucket/my-prefix/*` with the bucket, prefix, and object name or wildcard you wish to trace.

### See Console Trace for a Response Size Greater than 1Mb

Use [`mc admin trace`](#command-mc.admin.trace) to monitor responses over a specific size:

```
mc admin trace --filter-response --filter-size 1Mb ALIAS
```

Replace [`ALIAS`](#mc.admin.trace.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace `1Mb` with the desired response size.

### See Console Trace for a Request Operation Durations Greater than 5ms

Use [`mc admin trace`](#command-mc.admin.trace) to monitor long operations:

```
mc admin trace --filter-duration --filter-size 5ms ALIAS
```

Replace [`ALIAS`](#mc.admin.trace.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

## Syntax

[`mc admin trace`](#command-mc.admin.trace) has the following syntax:

```
mc admin trace [FLAGS] TARGET
```

[`mc admin trace`](#command-mc.admin.trace) supports the following argument:

TARGET[](#mc.admin.trace.TARGET)

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment for which to monitor API operations.

--all, a[](#mc.admin.trace.-all)

Returns all traffic on the MinIO deployment, including internode traffic between MinIO servers.

--call[](#mc.admin.trace.-call)

Traces only matching client operation or call types.
For example, the following command only traces operations of the type `scanner`.

```
mc admin trace --call scanner TARGET
```

Valid call types include:

`batch-keyrotation`

`batch-replication`

`bootstrap`

`decommission`

`ftp`

`healing`

`ilm`

`internal`

`os`

`rebalance`

`replication-resync`

`s3`

`scanner`

`storage`

If not specified, MinIO returns call types of `s3`.

--errors, e[](#mc.admin.trace.-errors)

Returns failed API operations only.

--filter-request[](#mc.admin.trace.-filter-request)

Trace client operations or calls with request size greater than the specified [`--filter-size`](#mc.admin.trace.-filter-size) value.

Must be used with [`--filter-size`](#mc.admin.trace.-filter-size) flag.

--filter-response[](#mc.admin.trace.-filter-response)

Trace client operations or calls with response size greater than the specified [`--filter-size`](#mc.admin.trace.-filter-size) value.

Must be used with [`--filter-size`](#mc.admin.trace.-filter-size) flag.

--filter-size[](#mc.admin.trace.-filter-size)

Size limit of a filtered client operation or call.

Must be used with either [`--filter-request`](#mc.admin.trace.-filter-request) or [`--filter-response`](#mc.admin.trace.-filter-response) flag.

Valid units include:

Suffix

Unit Size

`k`

KB (Kilobyte, 1000 Bytes)

`m`

MB (Megabyte, 1000 Kilobytes)

`g`

GB (Gigabyte, 1000 Megabytes)

`t`

TB (Terrabyte, 1000 Gigabytes)

`ki`

KiB (Kibibyte, 1024 Bites)

`mi`

MiB (Mebibyte, 1024 Kibibytes)

`gi`

GiB (Gibibyte, 1024 Mebibytes)

`ti`

TiB (Tebibyte, 1024 Gibibytes)

--funcname[](#mc.admin.trace.-funcname)

Returns calls for the entered function name.

--method[](#mc.admin.trace.-method)

Returns call of the specified HTTP method.

--node[](#mc.admin.trace.-node)

Returns calls for the specified server.

--path[](#mc.admin.trace.-path)

Returns calls for the specified path.

--request-header[](#mc.admin.trace.-request-header)

Returns calls matching the supplied request header.

--request-query[](#mc.admin.trace.-request-query)

Returns calls matching the supplied request query parameter.
This debug option should only be used at the direction of MinIO Support.

--response-duration[](#mc.admin.trace.-response-duration)

Trace calls with response duration greater than the specified value.

--response-threshold[](#mc.admin.trace.-response-threshold)

Takes a time string as a value, such as `5ms`.
Returns only calls with a response time greater than the supplied threshold.

If not specified, MinIO returns calls with a response time greater than 5ms.

--status-code[](#mc.admin.trace.-status-code)

Returns calls of the specified HTTP status code.

--stats[](#mc.admin.trace.-stats)

Accumulates aggregated statistics for each traced function call during the current trace session.

The output table includes the following columns.

Call

The name of the captured client operation or function.

Count

The number of times the client operation or call occurred.

RPM

The Rate Per Minute (RPM) of the client operation or call.

Avg Time

The average time required for the client operation or call to complete.

Min Time

The minimum time spent for the client operation or call to complete.

Max Time

The maximum time spent for the client operation or call to complete.

Avg TTFB

New in version RELEASE.2023-11-15T22-45-58Z.

The average Time To First Byte (TTFB) for the client operation or call response.

Max TTFB

New in version RELEASE.2023-11-15T22-45-58Z.

The maximum Time To First Byte for the client operation or call response.

Avg Size

Average size of client operation or call responses.

Errors

The number of client operations or calls that failed with an error.

RX Avg

The average number of Bytes Received (RX) for the client operation or call.
This stat only displays if not zero (0).

TX AVG

The average number of Bytes Sent (TX) for the client operation or call.
This stat only displays if not zero (0).

Accumulate stats, such as name, count, duration, min time, max time, time to first byte, or errors.
Accumulates up to 15 stat entries.

--verbose[](#mc.admin.trace.-verbose)

Returns verbose output.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).