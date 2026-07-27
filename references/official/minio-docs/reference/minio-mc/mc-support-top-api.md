---
title: "mc support top api — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-top-api.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-top-api"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support top api — SILO (MinIO Object Storage)"
---
# mc support top api

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Syntax

The [`mc support top api`](#command-mc.support.top.api) command summarizes the real-time API events on a MinIO deployment server.

EXAMPLE

The following command displays the current in-progress S3 API calls on the [[glossary#term-alias|alias]] `myminio`.

```
mc support top api myminio/
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] support top api    \
                 TARGET             \
                 [--name "string"]  \
                 [--path "string"]  \
                 [--node "string"]  \
                 [--errors, -e]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.support.top.api.TARGET)
Required

The full path to the alias, prefix, or object where the command should run.
The path must include at least an [[reference/minio-mc/mc-alias-set#minio-mc-alias|ALIAS]].

--name[](#mc.support.top.api.-name)
Optional

Outputs a summary of current API calls matching the entered string.

--path[](#mc.support.top.api.-path)
Optional

Outputs a summary of current API calls for a specified path.

--node[](#mc.support.top.api.-node)
Optional

Outputs a summary of the current API calls on matching servers.

--errors, -e[](#mc.support.top.api.-errors)
Optional

Outputs a summary of current API calls returning errors.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Display All Current In-progress S3 API Calls

The following command displays all in-progress S3 calls for the `myminio` deployment:

```
mc support top api myminio/
```

### Display Current, In-progress s3.PutObject Calls

The following command displays all in-progress `s3.PutObject` calls for the `myminio` deployment:

```
mc support top api --name s3.PutObject myminio/
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).