---
title: "Batch Framework — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/batch-framework.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/batch-framework"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Batch Framework — SILO (MinIO Object Storage)"
---
# Batch Framework

## Overview

The MinIO Batch Framework allows you to create, manage, monitor, and execute jobs using a YAML-formatted job definition file (a “batch file”).
The batch jobs run directly on the MinIO deployment to take advantage of the server-side processing power without constraints of the local machine where you run the [[reference/minio-mc#minio-client|MinIO Client]].

A batch file defines one job task.

Once started, MinIO starts processing the job.
Time to completion depends on the resources available to the deployment.

If any portion of the job fails, MinIO retries the job up to the number of times defined in the job definition.

The MinIO Batch Framework supports the following job types:

Job Type

Description

[[administration/batch-framework-job-replicate#minio-batch-framework-replicate-job|replicate]]

Perform a one-time replication procedure from one MinIO location to another MinIO location.

[[administration/batch-framework-job-keyrotate#minio-batch-framework-keyrotate-job|keyrotate]]

Perform a one-time process to cycle the [[operations/server-side-encryption#minio-sse-data-encryption|sse-s3 or sse-kms]] cryptographic keys on objects.

[[administration/batch-framework-job-expire#minio-batch-framework-expire-job|expire]]

Perform a one-time immediate expiration of objects in a bucket.

## MinIO Batch CLI

Install the [[reference/minio-mc#minio-client|MinIO Client]]

Define an [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`alias`]] for the MinIO deployment

The [[reference/minio-mc/mc-batch#command-mc.batch|`mc batch`]] commands include

[[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]]

The [[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]] command creates a basic YAML-formatted template file for the specified job type.

[[reference/minio-mc/mc-batch-start#command-mc.batch.start|`mc batch start`]]

The [[reference/minio-mc/mc-batch-start#command-mc.batch.start|`mc batch start`]] command launches a batch job from a job batch YAML file.

[[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]]

The [[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]] command outputs a list of the batch jobs currently in progress on a deployment.

[[reference/minio-mc/mc-batch-status#command-mc.batch.status|`mc batch status`]]

The [[reference/minio-mc/mc-batch-status#command-mc.batch.status|`mc batch status`]] command outputs summaries of job events on a MinIO server.

Changed in version mc: RELEASE.2024-07-03T20-17-25Z

Batch status displays summaries for active, in-progress jobs or any batch job completed in the previous three (3) days.

[[reference/minio-mc/mc-batch-describe#command-mc.batch.describe|`mc batch describe`]]

The [[reference/minio-mc/mc-batch-describe#command-mc.batch.describe|`mc batch describe`]] command outputs the job definition for a specified job ID.

[[reference/minio-mc/mc-batch-cancel#command-mc.batch.cancel|`mc batch cancel`]]

The [[reference/minio-mc/mc-batch-cancel#command-mc.batch.cancel|`mc batch cancel`]] stops an ongoing batch job.

## Access to mc batch

Each batch job executes using the credentials specified in the batch definition.
The success of a given batch job depends on those credentials having the appropriate [[administration/identity-access-management/policy-based-access-control#minio-policy|permissions]] to perform all requested actions.

The user executing the batch job must have the following permissions.
You can alternatively restrict users from accessing these functions by blocking or limiting access to these actions:

`admin:ListBatchJobs`

Grants the user the ability to see batch jobs currently in process.

`admin:DescribeBatchJobs`

Grants the user the ability to see the definition details of batch job currently in process.

`admin:StartBatchJob`

Grants the user the ability to start a batch job.
The job may be further restricted by the credentials the job uses to access either the source or target deployments.

`admin:CancelBatchJob`

Allows the user to stop a batch job currently in progress.

You can assign any of these actions to users independently or in any combination.

The built-in `ConsoleAdmin` policy includes sufficient access to perform all of these types of batch job actions.

## Local Deployment

You run a batch job against a particular deployment by passing an `alias` to the [[reference/minio-mc/mc-batch#command-mc.batch|`mc batch`]] command.
The deployment you specify in the command becomes the `local` deployment within the context of that batch job.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).