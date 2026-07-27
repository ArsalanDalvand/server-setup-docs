---
title: "mc batch cancel — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch-cancel.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch-cancel"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch cancel — SILO (MinIO Object Storage)"
---
# mc batch cancel

New in version mc: RELEASE.2023-03-20T17-17-53Z

## Syntax

The [`mc batch cancel`](#command-mc.batch.cancel) stops an ongoing batch job.

You must specify the job ID.
To find the job ID, use [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]].

EXAMPLE

The following command outputs the job definition for the job identified as `KwSysDpxcBU9FNhGkn2dCf`.

```
mc batch cancel myminio KwSysDpxcBU9FNhGkn2dCf
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] batch cancel ALIAS JOBID
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.batch.cancel.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] for the MinIO deployment on which the job is currently running.

JOBID[](#mc.batch.cancel.JOBID)
Required

The unique identifier of the batch job to cancel.
To find the ID of a job, use [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]].

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Cancel an ongoing batch job

The following command cancels the job with ID `KwSysDpxcBU9FNhGkn2dCf` on the deployment at alias `myminio`:

```
mc batch cancel myminio KwSysDpxcBU9FNhGkn2dCf
```

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).