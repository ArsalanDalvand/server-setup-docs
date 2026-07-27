---
title: "mc batch — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch — SILO (MinIO Object Storage)"
---
# mc batch

New in version mc: RELEASE.2023-03-20T17-17-53Z

Added the ability to cancel jobs with the [[reference/minio-mc/mc-batch-cancel#command-mc.batch.cancel|`mc batch cancel`]] command.

## Description

The [`mc batch`](#command-mc.batch) commands allow you to run one or more job tasks on a MinIO deployment.

## Subcommands

[`mc batch`](#command-mc.batch) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-batch-cancel#command-mc.batch.cancel|`cancel`]]

The [[reference/minio-mc/mc-batch-cancel#command-mc.batch.cancel|`mc batch cancel`]] stops an ongoing batch job.

[[reference/minio-mc/mc-batch-describe#command-mc.batch.describe|`describe`]]

The [[reference/minio-mc/mc-batch-describe#command-mc.batch.describe|`mc batch describe`]] command outputs the job definition for a specified job ID.

[[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`generate`]]

The [[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]] command creates a basic YAML-formatted template file for the specified job type.

[[reference/minio-mc/mc-batch-list#command-mc.batch.list|`list`]]

The [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]] command outputs a list of the batch jobs currently in progress on a deployment.

[[reference/minio-mc/mc-batch-start#command-mc.batch.start|`start`]]

The [[reference/minio-mc/mc-batch-start#command-mc.batch.start|`mc batch start`]] command launches a batch job from a job batch YAML file.

[[reference/minio-mc/mc-batch-status#command-mc.batch.status|`status`]]

The [[reference/minio-mc/mc-batch-status#command-mc.batch.status|`mc batch status`]] command outputs summaries of job events on a MinIO server.

Changed in version mc: RELEASE.2024-07-03T20-17-25Z

Batch status displays summaries for active, in-progress jobs or any batch job completed in the previous three (3) days.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).