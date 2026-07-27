---
title: "mc admin policy rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-remove.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-remove"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy rm — SILO (MinIO Object Storage)"
---
# mc admin policy rm

## Syntax

Removes an IAM policy from the target MinIO deployment.

The [`mc admin policy remove`](#command-mc.admin.policy.remove) command has equivalent functionality to [`mc admin policy rm`](#command-mc.admin.policy.rm).

EXAMPLE

The following command removes the policy names `writeonly` from the `myminio` MinIO deployment:

```
mc admin policy rm myminio writeonly
```

SYNTAX

The command has the following syntax:

```
mc admin policy rm TARGET POLICYNAME
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

The [`mc admin policy rm`](#command-mc.admin.policy.rm) command accepts the following arguments:

TARGET[](#mc.admin.policy.rm.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which to remove the policy.

POLICYNAME[](#mc.admin.policy.rm.POLICYNAME)

The name of the policy to remove.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

Remove a policy called `listbuckets`.

```
mc admin policy rm myminio listbuckets
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).