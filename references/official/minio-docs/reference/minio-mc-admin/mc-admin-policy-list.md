---
title: "mc admin policy ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy ls — SILO (MinIO Object Storage)"
---
# mc admin policy ls

## Syntax

Lists all policies on the target MinIO deployment.

The [`mc admin policy list`](#command-mc.admin.policy.list) command has equivalent functionality to [`mc admin policy ls`](#command-mc.admin.policy.ls).

EXAMPLE

The following command displays a list of the policies currently current on the [[glossary#term-alias|alias]] `play`.

```
mc admin policy ls play
```

SYNTAX

The command has the following syntax:

```
mc admin policy ls TARGET
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

The [`mc admin policy ls`](#command-mc.admin.policy.ls) command accepts the following arguments:

TARGET[](#mc.admin.policy.list.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which the command lists the available policies.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

List the policies that exist on the deployment at alias `myminio`.

```
mc admin policy ls myminio
```

### Output

The command returns output that resembles the following:

```
readwrite
writeonly
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).