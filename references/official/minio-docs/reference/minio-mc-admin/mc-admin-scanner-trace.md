---
title: "mc admin scanner trace — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-scanner-trace.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-scanner-trace"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin scanner trace — SILO (MinIO Object Storage)"
---
# mc admin scanner trace

## Description

The [`mc admin scanner trace`](#command-mc.admin.scanner.trace) command displays [[operations/concepts/scanner#minio-concepts-scanner|scanner]]-specific API operations occurring on the target MinIO deployment.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

EXAMPLE

The following example returns a list of API operations related to the scanner on the `myminio` deployment.

```
mc admin scanner trace myminio
```

SYNTAX

The command has the following syntax:

```
mc admin scanner trace ALIAS
                       [--filter-request]            \
                       [--filter-response]           \
                       [--filter-size <value>]       \
                       [--funcname <value>]          \
                       [--node <value>]              \
                       [--path <value>]              \
                       [--response-duration <value>] \
                       [--verbose, -v]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.scanner.trace.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment for which to display [[operations/concepts/scanner#minio-concepts-scanner|scanner]] API operations.

--filter-request[](#mc.admin.scanner.trace.-filter-request)
Optional

Trace scanner operations or calls with request size greater than the specified [`--filter-size`](#mc.admin.scanner.trace.-filter-size) value.

Must be used with [`--filter-size`](#mc.admin.scanner.trace.-filter-size) flag.

--filter-response[](#mc.admin.scanner.trace.-filter-response)
Optional

Trace scanner operations or calls with response size greater than the specified [`--filter-size`](#mc.admin.scanner.trace.-filter-size) value.

Must be used with [`--filter-size`](#mc.admin.scanner.trace.-filter-size) flag.

--filter-size[](#mc.admin.scanner.trace.-filter-size)
Optional

Filter output to request sizes or response sizes greater than the specified size.

Must be used with either [`--filter-request`](#mc.admin.scanner.trace.-filter-request) or [`--filter-response`](#mc.admin.scanner.trace.-filter-response) flag.

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

--funcname[](#mc.admin.scanner.trace.-funcname)
Optional

Returns calls for the entered function name.

--node[](#mc.admin.scanner.trace.-node)
Optional

Returns calls for the specified server.

--path[](#mc.admin.scanner.trace.-path)
Optional

Returns calls for the specified path.

--response-duration[](#mc.admin.scanner.trace.-response-duration)
Optional

Trace calls with response duration greater than the specified value.

--verbose, -v[](#mc.admin.scanner.trace.-verbose)
Optional

Returns verbose output.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Monitor all scanner API operations

Use [`mc admin scanner trace`](#command-mc.admin.scanner.trace) to monitor [[operations/concepts/scanner#minio-concepts-scanner|scanner]] API operations on the MinIO deployment at the alias `myminio`:

```
mc admin scanner trace myminio
```

### Show scanner trace for a specific path

Use [`mc admin scanner trace`](#command-mc.admin.scanner.trace) to monitor API operations for a the path `my-bucket/my-prefix/*` on the deployment at the `myminio` alias:

```
mc admin scanner trace --path my-bucket/my-prefix/* myminio
```

### Show scanner API operations for the scanObject function

Monitor scanner activity for the `scanObject function` on the `myminio` deployment:

```
mc admin scanner trace --funcname=scanner.ScanObject myminio
```

### Show scanner operation requests greater than 1MB in size

Use [`mc admin scanner trace`](#command-mc.admin.scanner.trace) to monitor requests larger than a `1MB` on the `myminio` deployment:

```
mc admin scanner trace --filter-request --filter-size 1MB myminio
```

### Show scanner operation responses greater than 1MB in size

Use [`mc admin scanner trace`](#command-mc.admin.scanner.trace) to monitor large response sizes:

```
mc admin scanner trace --filter-response --filter-size 1MB myminio
```

### Show scanner operations that last longer than five milliseconds

Use [`mc admin scanner trace`](#command-mc.admin.scanner.trace) to monitor long operations:

```
mc admin scanner trace --response-duration 5ms myminio
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).