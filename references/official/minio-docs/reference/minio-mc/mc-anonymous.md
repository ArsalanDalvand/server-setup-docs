---
title: "mc anonymous — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-anonymous.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-anonymous"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc anonymous — SILO (MinIO Object Storage)"
---
# mc anonymous

## Description

The [`mc anonymous`](#command-mc.anonymous) command supports setting or removing anonymous [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to a bucket and its contents.
Buckets with anonymous policies allow public access where clients can perform any action granted by the policy without [[administration/identity-access-management#minio-authentication-and-identity-management|authentication]].

## Subcommands

[`mc anonymous`](#command-mc.anonymous) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-anonymous-get#command-mc.anonymous.get|`get`]]

The [[reference/minio-mc/mc-anonymous-get#command-mc.anonymous.get|`mc anonymous get`]] command gets the anonymous (i.e. unauthenticated or
public) access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket.

[[reference/minio-mc/mc-anonymous-get-json#command-mc.anonymous.get-json|`get-json`]]

The [[reference/minio-mc/mc-anonymous-get-json#command-mc.anonymous.get-json|`mc anonymous get-json`]] command gets anonymous (i.e. unauthenticated or
public) access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket.

[[reference/minio-mc/mc-anonymous-links#command-mc.anonymous.links|`links`]]

The [[reference/minio-mc/mc-anonymous-links#command-mc.anonymous.links|`mc anonymous links`]] retrieves the HTTP URL for anonymous (i.e.
unauthenticated or public) access to a bucket.

[[reference/minio-mc/mc-anonymous-list#command-mc.anonymous.list|`list`]]

The [[reference/minio-mc/mc-anonymous-list#command-mc.anonymous.list|`mc anonymous list`]] retrieves all anonymous (i.e. unauthenticated or
public) access policies for a bucket.

[[reference/minio-mc/mc-anonymous-set#command-mc.anonymous.set|`set`]]

The [[reference/minio-mc/mc-anonymous-set#command-mc.anonymous.set|`mc anonymous set`]] command sets anonymous (i.e. unauthenticated or public)
access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket.

[[reference/minio-mc/mc-anonymous-set-json#command-mc.anonymous.set-json|`set-json`]]

The [[reference/minio-mc/mc-anonymous-set-json#command-mc.anonymous.set-json|`mc anonymous set-json`]] command sets anonymous (i.e. unauthenticated or
public) access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket using using an IAM
[JSON policy document](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-iam-policies).

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).