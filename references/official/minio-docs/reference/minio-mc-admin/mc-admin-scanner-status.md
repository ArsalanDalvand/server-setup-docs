---
title: "mc admin scanner status — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-scanner-status.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-scanner-status"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin scanner status — SILO (MinIO Object Storage)"
---
# mc admin scanner status

## Description

The [`mc admin scanner status`](#command-mc.admin.scanner.status) command displays a real-time summary of [[operations/concepts/scanner#minio-concepts-scanner|scanner]] information for a MinIO Server.

This command has an alias of `mc admin scanner info`.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

EXAMPLE

The following example returns information about the current state of the scanner process.

```
mc admin scanner status myminio
```

The command returns results similar to the following:

```
Overall Statistics
------------------
Last full scan time:   0d0h15m; Estimated 2879.79/month
Current cycle:         (between cycles)
Active drives: 0

Last Minute Statistics
----------------------
Objects Scanned:       3 objects; Avg: 67.611µs; Rate: 4320/day
Versions Scanned:      3 versions; Avg: 2.506µs; Rate: 4320/day
Versions Heal Checked: 0 versions; Avg: 0ms
Read Metadata:         3 objects; Avg: 40.817µs, Size: 395 bytes/obj
ILM checks:            3 versions; Avg: 714ns
Check Replication:     3 versions; Avg: 892ns
Verify Deleted:        0 folders; Avg: 0ms
Yield:                 18ms total; Avg: 6ms/obj
```

SYNTAX

The command has the following syntax:

```
mc admin scanner status ALIAS
                       [--bucket <string>]     \
                       [--interval <value>]   \
                       [--max-paths <value>]  \
                       [-n <integer>]         \
                       [--nodes <string>]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.scanner.status.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment for which to display [[operations/concepts/scanner#minio-concepts-scanner|scanner]] API operations.

--bucket[](#mc.admin.scanner.status.-bucket)
Optional

Filter scanner statistics to the specified bucket.

--interval[](#mc.admin.scanner.status.-interval)
Optional

The number of seconds to wait between status request refreshes.
If not specified, the status refreshes every 3 seconds.

--max-paths[](#mc.admin.scanner.status.-max-paths)
Optional

The maximum number of active paths to show.
Use `-1` for an unlimited number of paths.

Limiting the number of paths shown can reduce the scrolling of the console window when there are a large number of drives being scanned.

If not specified, the results return for an unlimited number of active paths.

-n[](#mc.admin.scanner.status.-n)
Optional

The number of status requests to return before automatically exiting.
Use `0` to return an unlimited number of status results.

If not specified, the results continuously refresh at the specified interval until manually exited.

--nodes[](#mc.admin.scanner.status.-nodes)
Optional

Returns scanner status information for the specified node(s).
Specify multiple nodes as a comma-separated list.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).