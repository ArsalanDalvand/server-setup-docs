---
title: "mc support top net — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-top-net.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-top-net"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support top net — SILO (MinIO Object Storage)"
---
# mc support top net

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Syntax

The [`mc support top net`](#command-mc.support.top.net) command displays realtime network metrics.

EXAMPLE

The following command displays the current realtime network metrics for the [[glossary#term-alias|alias]] `myminio` deployment.

```
mc support top net myminio/
```

The output returns information such as the server URL, network interface, receive rate, transmit rate, and system messages.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] support top disk                \
                             [--interval value]  \
                             TARGET
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.support.top.net.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or [[glossary#term-prefix|prefix]] where the command should run.

--interval[](#mc.support.top.net.-interval)
Optional

The interval in seconds between metric requests.

By default, the command requests metrics every second.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).