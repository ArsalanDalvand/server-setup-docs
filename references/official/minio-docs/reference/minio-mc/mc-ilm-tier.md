---
title: "mc ilm tier — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-tier.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-tier"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm tier — SILO (MinIO Object Storage)"
---
# mc ilm tier

Changed in version RELEASE.2022-12-24T15-21-38Z: [`mc ilm tier`](#command-mc.ilm.tier) replaces `mc admin tier`.

## Description

The [`mc ilm tier`](#command-mc.ilm.tier) command and its subcommands configure a remote supported S3-compatible service for MinIO [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|Lifecycle Management: Object Transition (“Tiering”)]].

After creating one or more tiers with this command, use [[reference/minio-mc/mc-ilm-rule#command-mc.ilm.rule|`mc ilm rule`]] and its subcommands to create the rules that move objects to other storage.

For more information, see the overview of [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|lifecycle management]].

## Subcommands

[`mc ilm tier`](#command-mc.ilm.tier) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-ilm-tier-add#command-mc.ilm.tier.add|`add`]]

The [[reference/minio-mc/mc-ilm-tier-add#command-mc.ilm.tier.add|`mc ilm tier add`]] command creates a new remote storage tier to a supported storage services.

[[reference/minio-mc/mc-ilm-tier-check#command-mc.ilm.tier.check|`check`]]

The [[reference/minio-mc/mc-ilm-tier-check#command-mc.ilm.tier.check|`mc ilm tier check`]] command displays the configuration for remote tier on a deployment.

[[reference/minio-mc/mc-ilm-tier-info#command-mc.ilm.tier.info|`info`]]

The [[reference/minio-mc/mc-ilm-tier-info#command-mc.ilm.tier.info|`mc ilm tier info`]] command outputs statistics about a tier or all tiers for a deployment.

[[reference/minio-mc/mc-ilm-tier-ls#command-mc.ilm.tier.ls|`ls`]]

The [[reference/minio-mc/mc-ilm-tier-ls#command-mc.ilm.tier.ls|`mc ilm tier ls`]] command shows the remote tiers configured on a deployment.

[[reference/minio-mc/mc-ilm-tier-rm#command-mc.ilm.tier.rm|`rm`]]

The [[reference/minio-mc/mc-ilm-tier-rm#command-mc.ilm.tier.rm|`mc ilm tier rm`]] command removes an remote tier that has not been used to transition any objects.

[[reference/minio-mc/mc-ilm-tier-update#command-mc.ilm.tier.update|`update`]]

The [[reference/minio-mc/mc-ilm-tier-update#command-mc.ilm.tier.update|`mc ilm tier update`]] command modifies an existing configured remote tier.

## Required Permissions

To create tiers for object transition, MinIO requires the following administrative permissions on the cluster:

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-SetTier|`admin:SetTier`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-ListTier|`admin:ListTier`]]

For example, the following policy provides sufficient permissions for configuring object transition lifecycle management rules on any bucket in the cluster:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
            "Action": [
               "admin:SetTier",
               "admin:ListTier"
            ],
            "Effect": "Allow",
            "Sid": "EnableRemoteTierManagement"
      },
      {
            "Action": [
               "s3:PutLifecycleConfiguration",
               "s3:GetLifecycleConfiguration"
            ],
            "Resource": [
                        "arn:aws:s3:::*"
            ],
            "Effect": "Allow",
            "Sid": "EnableLifecycleManagementRules"
      }
   ]
}
```

### Transition Permissions

Object transition lifecycle management rules require additional permissions on the remote storage tier.
Specifically, MinIO requires the remote tier credentials provide read, write, list, and delete permissions.

For example, if the remote storage tier implements AWS IAM policy-based access control, the following policy provides the necessary permissions for transitioning objects into and out of the remote tier:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
            "Action": [
               "s3:ListBucket"
            ],
            "Effect": "Allow",
            "Resource": [
               "arn:aws:s3:::MyDestinationBucket"
            ],
            "Sid": ""
      },
      {
            "Action": [
               "s3:GetObject",
               "s3:PutObject",
               "s3:DeleteObject"
            ],
            "Effect": "Allow",
            "Resource": [
               "arn:aws:s3:::MyDestinationBucket/*"
            ],
            "Sid": ""
      }
   ]
}
```

Modify the `Resource` for the bucket into which MinIO tiers objects.

Avoid enabling versioning in the remote tier

MinIO strongly recommends against enabling bucket versioning for remote tiers.
If the remote tier bucket is versioned, each source object version is transitioned to a unique object in the remote tier.

If your environment requires versioning for the remote tier, you must also allow the `s3:DeleteObjectVersion` permission.

Defer to the documentation for the supported tiering targets for more complete information on configuring users and permissions to support MinIO tiering:

[Amazon S3 Permissions](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazons3.html#amazons3-actions-as-permissions)

[Google Cloud Storage Access Control](https://cloud.google.com/storage/docs/access-control)

[Authorizing access to data in Azure storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-auth?toc=/azure/storage/blobs/toc.json)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).