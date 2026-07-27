---
title: "mc batch describe — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch-describe.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch-describe"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch describe — SILO (MinIO Object Storage)"
---
# mc batch describe

Changed in version MinIO: RELEASE.2022-10-08T20-11-00Z or later

## Syntax

The [`mc batch describe`](#command-mc.batch.describe) command outputs the job definition for a specified job ID.

You must specify the job ID.
To find the job ID, use [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]].

EXAMPLE

The following command outputs the job definition for the job identified as `KwSysDpxcBU9FNhGkn2dCf`.

```
mc batch describe myminio KwSysDpxcBU9FNhGkn2dCf
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] batch describe TARGET           \
                                JOBID
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.batch.describe.TARGET)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] for the MinIO deployment to look for the Job ID.

JOBID[](#mc.batch.describe.JOBID)
Required

The unique identifier of a job to describe.
To find the ID of a job, use [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]].

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Show the Definition of an In Progress Batch Job

The following command provides the full job definition of a specific job at [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] `myminio`:

```
mc batch describe myminio KwSysDpxcBU9FNhGkn2dCf
```

Replace `myminio` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment that should run the job.

Replace `KwSysDpxcBU9FNhGkn2dCf` with the ID of the job to define.

The output of the above command is similar to the following:

```
mc batch describe myminio KwSysDpxcBU9FNhGkn2dCf
replicate:
  apiVersion: v1
...
```

Note, this example is truncated.
The output is the full job definition for the specified job.

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Permissions

You must have the `admin:DescribeBatchJobs` permission to describe jobs on the deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).