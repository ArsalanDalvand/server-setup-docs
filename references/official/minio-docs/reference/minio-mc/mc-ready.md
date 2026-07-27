---
title: "mc ready — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ready.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ready"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ready — SILO (MinIO Object Storage)"
---
# mc ready

## Syntax

The [`mc ready`](#command-mc.ready) command checks the status of a cluster and whether the cluster has `read` and `write` quorum.

EXAMPLE

The following sends a `GET` request to the cluster at alias `myminio` and returns its status.

```
mc ready myminio
```

The command sends a `GET` request to the deployment at the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] `myminio`.’
The command repeats the request until it is successful.

The output before the cluster at alias `myminio` is ready resembles the following:

```
The cluster `myminio` is unreachable: Get "http://myminio.example.com:9000/minio/health/cluster": dial tcp 198.51.100.0:9000: connect: connection refused
```

Once the request succeeds in connecting to the `myminio` deployment, the output resembles the following:

```
The cluster `myminio` is ready
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ready            \
                 TARGET           \
                 [--cluster-read] \
                 [--maintenance]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.ready.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or prefix where the command should run.

--cluster-read[](#mc.ready.-cluster-read)
Optional

Checks if the cluster has enough [[glossary#term-read-quorum|quorum]] to serve `READ` requests.

--maintenance[](#mc.ready.-maintenance)
Optional

Checks if the cluster can maintain read and write quorum if the node for the alias is taken down for maintenance.

Use an alias for the specific node you expect to take down for maintenance and not an alias set to a load balancer.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Check if the cluster has read quorum

The following command checks that a deployment has sufficient drives available for read operations.

```
mc read myminio --cluster-read
```

### Check if a cluster is down for maintenance

The following command checks whether the cluster can maintain read and write quorum during maintenance when the node at alias `myminio` is taken down.

```
mc ready myminio --maintenance
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).