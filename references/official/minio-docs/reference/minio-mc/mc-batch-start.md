---
title: "mc batch start — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch-start.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch-start"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch start — SILO (MinIO Object Storage)"
---
# mc batch start

Changed in version MinIO: RELEASE.2022-10-08T20-11-00Z or later

## Syntax

The [`mc batch start`](#command-mc.batch.start) command launches a batch job from a job batch YAML file.

The batch job runs to completion (or up to the number of retries specified in the file) one time.
To run the batch job again after completion, you must start it again.

EXAMPLE

The following command creates a basic YAML file for a replicate job on the `mybucket` bucket of the `myminio` alias.

```
mc batch start myminio jobfile.yaml
```

The output of the above command is something similar to:

```
Successfully start 'replicate' job `B34HHqnNMcg1taynaPfxu` on '2022-10-24 17:19:06.296974771 -0700 PDT'
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] batch start    \
                       ALIAS   \
                       JOBFILE
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.batch.start.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] on which to start the batch job.

For example:

```
mc batch start myminio replicate.yaml
```

JOBFILE[](#mc.batch.start.JOBFILE)
Required

A YAML-defined batch job.
The job may have as many tasks as desired; there is no predefined limit.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Start a Batch Job

The following command starts the batch of job(s) defined in the file `replication.yaml` on the deployment at [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] `myminio`:

```
mc batch start myminio ./replication.yaml
```

Replace `myminio` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment that should run the job.

Replace `./replication.yaml` with the yaml-formatted file that describes the batch job.
Use the file path relative to your current location.

The output of the above command is similar to the following:

```
Successfully start 'replicate' job `E24HH4nNMcgY5taynaPfxu` on '2022-09-26 17:19:06.296974771 -0700 PDT'
```

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Permissions

You must have the [[administration/identity-access-management/policy-based-access-control#policy-action.admin-StartBatchJob|`admin:StartBatchJob`]] permission on the deployment to start jobs.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).