---
title: "mc batch list — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch list — SILO (MinIO Object Storage)"
---
# mc batch list

Changed in version MinIO: RELEASE.2022-10-08T20-11-00Z or later

## Syntax

The [`mc batch list`](#command-mc.batch.list) command outputs a list of the batch jobs currently in progress on a deployment.

EXAMPLE

The following command outputs a list of all jobs currently in progress on the `myminio` alias.

```
mc batch list myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] batch list TARGET           \
                            --type "string"
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.batch.list.TARGET)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment for which you want to list jobs in progress.

--type[](#mc.batch.list.-type)
Optional

List batch jobs only of a certain type.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### List all replicate type batch jobs

The following command lists the `replicate`` type job(s) on the deployment at [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] `myminio`:

```
mc batch list myminio --type "replicate"
```

Replace `myminio` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment that should run the job.

Replace `replicate` with the job type to output.

Currently, [[reference/minio-mc/mc-batch#command-mc.batch|`mc batch`]] only supports the `replicate` job type.

The output of the above command is similar to the following:

```
ID                      TYPE            USER            STARTED
E24HH4nNMcgY5taynaPfxu  replicate       minioadmin      1 minute ago
```

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Permissions

You must have the [[administration/identity-access-management/policy-based-access-control#policy-action.admin-ListBatchJobs|`admin:ListBatchJobs`]] permission to list jobs on the deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).