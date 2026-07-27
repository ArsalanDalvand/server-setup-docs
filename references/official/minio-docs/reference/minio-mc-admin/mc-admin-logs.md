---
title: "mc admin logs — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-logs.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-logs"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin logs — SILO (MinIO Object Storage)"
---
# mc admin logs

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

Changed in version RELEASE.2022-12-02T23-48-47Z: `mc support logs` moved to `mc admin logs` and provide a simpler command interface for displaying server logs for the MinIO deployment.

The output is similar to what is available via `journalctl -uf minio` for systemd-controlled deployments.

## Description

Use the [`mc admin logs`](#command-mc.admin.logs) command to show MinIO server logs.

The uploading feature remains disabled by default until explicitly enabled for a deployment on an opt-in only basis.
If enabled, you can disable the feature at any time with [[reference/minio-mc/mc-support-callhome#mc.support.callhome.disable|`mc support callhome disable`]].

## Examples

### Show Logs for a Deployment

The following command displays the most recent ten server logs of any type for the alias `minio1`.

```
mc admin logs minio1
```

### Show Last 5 Log Entries for a Node

The following command shows the most recent five log entries for a `node1` on the deployment with alias `minio1`.

```
mc admin logs --last 5 myminio node1
```

### Show Application Type Log Entires for a Deployment

The following command shows log entries of the type `application` for all nodes on the deployment with alias `minio1`.

```
mc admin logs --type application minio1
```

## Syntax

The command has the following syntax:

```
mc admin logs [GLOBAL FLAGS]     \
              [--last, -l value] \
              [--type, -t value] \
              ALIAS              \
              [NODE]
```

### Parameters

ALIAS[](#mc.admin.logs.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

--last, -l[](#mc.admin.logs.-last)
Optional

Show only the most recent specified number of log entries.

If this flag is not included, up to the last 10 log entries show.

--type, --type[](#mc.admin.logs.-type)
Optional

List log entries of a specified type.
Valid types are `minio`, `application`, or `all`.

If not specified, all log entry types show.

NODE[](#mc.admin.logs.NODE)
Optional

In distributed deployments, specify which node’s logs to show by entering the node’s name.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).