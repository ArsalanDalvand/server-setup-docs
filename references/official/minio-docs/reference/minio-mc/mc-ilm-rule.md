---
title: "mc ilm rule — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-rule.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-rule"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm rule — SILO (MinIO Object Storage)"
---
# mc ilm rule

Changed in version RELEASE.2022-12-24T15-21-38Z: The following commands have moved to subcommands under [`mc ilm rule`](#command-mc.ilm.rule):

[[reference/deprecated/mc-ilm-add#command-mc.ilm.add|`mc ilm add`]]

[[reference/deprecated/mc-ilm-edit#command-mc.ilm.edit|`mc ilm edit`]]

[[reference/deprecated/mc-ilm-export#command-mc.ilm.export|`mc ilm export`]]

[[reference/deprecated/mc-ilm-import#command-mc.ilm.import|`mc ilm import`]]

[[reference/deprecated/mc-ilm-ls#command-mc.ilm.ls|`mc ilm ls`]]

[[reference/deprecated/mc-ilm-rm#command-mc.ilm.rm|`mc ilm rm`]]

## Description

The [`mc ilm rule`](#command-mc.ilm.rule) command and its subcommands configure the rules used to transition objects between storage tiers in MinIO’s Lifecycle Management.

Before creating rules with this command, use [[reference/minio-mc/mc-ilm-tier#command-mc.ilm.tier|`mc ilm tier`]] and its subcommands to create the tier or tiers of other object storage locations where objects move.

For more information, see the overview of [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|lifecycle management]].

## Subcommands

[`mc ilm rule`](#command-mc.ilm.rule) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`add`]]

The [[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`mc ilm rule add`]] command adds an object lifecycle management rule to a bucket.

[[reference/minio-mc/mc-ilm-rule-edit#command-mc.ilm.rule.edit|`edit`]]

The [[reference/minio-mc/mc-ilm-rule-edit#command-mc.ilm.rule.edit|`mc ilm rule edit`]] command modifies an existing object lifecycle management
rule on a MinIO bucket.

[[reference/minio-mc/mc-ilm-rule-export#command-mc.ilm.rule.export|`export`]]

The [[reference/minio-mc/mc-ilm-rule-export#command-mc.ilm.rule.export|`mc ilm rule export`]] command exports the object lifecycle management configuration for a MinIO bucket.

[[reference/minio-mc/mc-ilm-rule-import#command-mc.ilm.rule.import|`import`]]

The [[reference/minio-mc/mc-ilm-rule-import#command-mc.ilm.rule.import|`mc ilm rule import`]] command imports an object lifecycle management
configuration and applies it to a MinIO bucket.

[[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`ls`]]

The [[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]] command summarizes all configured object lifecycle management rules on a MinIO bucket in a tabular format.

[[reference/minio-mc/mc-ilm-rule-rm#command-mc.ilm.rule.rm|`rm`]]

The [[reference/minio-mc/mc-ilm-rule-rm#command-mc.ilm.rule.rm|`mc ilm rule rm`]] command removes an object lifecycle management rule from a MinIO Bucket.

## Permissions

MinIO requires the following permissions scoped to to the bucket or buckets for which you create lifecycle management rules.

[[administration/identity-access-management/policy-based-access-control#policy-action.s3-PutLifecycleConfiguration|`s3:PutLifecycleConfiguration`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.s3-GetLifecycleConfiguration|`s3:GetLifecycleConfiguration`]]

For example, the following policy provides permission for configuring object
transition lifecycle management rules on any bucket in the cluster:.

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

Object transition lifecycle management rules require additional permissions
on the remote storage tier. Specifically, MinIO requires the remote
tier credentials provide read, write, list, and delete permissions.

For example, if the remote storage tier implements AWS IAM policy-based
access control, the following policy provides the necessary permission
for transitioning objects into and out of the remote tier:

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

Defer to the documentation for the supported tiering targets for more complete
information on configuring users and permissions to support MinIO tiering:

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