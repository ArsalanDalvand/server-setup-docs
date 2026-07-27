---
title: "mc batch status — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch-status.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch-status"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch status — SILO (MinIO Object Storage)"
---
# mc batch status

Changed in version MinIO: RELEASE.2022-10-08T20-11-00Z or later

## Syntax

The [`mc batch status`](#command-mc.batch.status) command outputs summaries of job events on a MinIO server.

Changed in version mc: RELEASE.2024-07-03T20-17-25Z

Batch status displays summaries for active, in-progress jobs or any batch job completed in the previous three (3) days.

EXAMPLE

The following command outputs the status of the specified job with JobID `KwSysDpxcBU9FNhGkn2dCf` currently in progress on the `myminio` alias.

```
mc batch status myminio "KwSysDpxcBU9FNhGkn2dCf"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] batch list TARGET           \
                            ["JOBID"]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.batch.status.TARGET)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] for which to display batch job statuses.

JOBID[](#mc.batch.status.JOBID)
Optional

The unique identifier of a job to summarize.
To find the ID of a job, use [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]].

If not specified, the command returns a summary for the current active batch job.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Summarize the Events of an Active Replicate Job

The following command provides the real-time summary of an active job on the deployment at [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] `myminio`:

```
mc batch status myminio "KwSysDpxcBU9FNhGkn2dCf"
```

Replace `myminio` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment that should run the job.

The output of the above command is similar to the following:

```
●∙∙
JobType:        replicate
Objects:        28766
Versions:       28766
FailedObjects:  0
Transferred:    406 MiB
Elapsed:        2m14.227222868s
CurrObjName:    share/doc/xml-core/examples/foo.xmlcatalogs
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