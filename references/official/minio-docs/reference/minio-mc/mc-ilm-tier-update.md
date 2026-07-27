---
title: "mc ilm tier update — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-tier-update.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-tier-update"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm tier update — SILO (MinIO Object Storage)"
---
# mc ilm tier update

Changed in version RELEASE.2022-12-24T15-21-38Z: [`mc ilm tier update`](#command-mc.ilm.tier.update) replaces `mc admin tier edit`.

## Description

The [`mc ilm tier update`](#command-mc.ilm.tier.update) command modifies an existing configured remote tier.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

### Supported S3 Services

[[reference/minio-mc/mc-ilm-tier#command-mc.ilm.tier|`mc ilm tier`]] supports only the following S3-compatible services as a remote target for object tiering:

MinIO

Amazon S3

Google Cloud Storage

Azure Blob Storage

### Required Permissions

MinIO requires the following permissions scoped to to the bucket or buckets
for which you are creating lifecycle management rules.

[[administration/identity-access-management/policy-based-access-control#policy-action.s3-PutLifecycleConfiguration|`s3:PutLifecycleConfiguration`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.s3-GetLifecycleConfiguration|`s3:GetLifecycleConfiguration`]]

MinIO also requires the following administrative permissions on the cluster
in which you are creating remote tiers for object transition lifecycle
management rules:

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-SetTier|`admin:SetTier`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-ListTier|`admin:ListTier`]]

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

#### Transition Permissions

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

## Syntax

EXAMPLE

The following example updates the credentials for an existing remote tier called `S3TIER` on the `myminio` deployment.

```
mc ilm tier update myminio S3TIER --access-key ACCESS_KEY --secret-key SECRET_KEY
```

After running this command, lifecycle management rules on the `myminio` deployment use the tier’s new credentials to transition objects into the remote location.
Options not modified in the command maintain their existing configurations.

SYNTAX

The command has the following syntax:

```
mc ilm tier update TARGET                         \
                   TIER_NAME                      \
                   [--account-key value]          \
                   [--access-key value]           \
                   [--az-sp-tenant-id value]      \
                   [--az-sp-client-id value]      \
                   [--az-sp-client-secret value]  \
                   [--secret-key value]           \
                   [--use-aws-role]               \
                   [--credentials-file value]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

The command accepts the following arguments:

TARGET[](#mc.ilm.tier.update.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

TIER_NAME[](#mc.ilm.tier.update.TIER_NAME)
Required

The name of the remote tier the command modifies.
The value corresponds to the [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_NAME|`mc ilm tier add TIER_NAME`]] specified when creating the remote tier.

--access-key[](#mc.ilm.tier.update.-access-key)
Optional

The access key for a user on the remote S3 or MinIO tier.
The user must have permission to perform read/write/list/delete operations on the remote bucket or bucket prefix.

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `s3` or `minio`.
This option has no effect for any other `TIER_TYPE`.

--secret-key[](#mc.ilm.tier.update.-secret-key)
Optional

The secret key for a user on the remote `s3` or `minio` tier.

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `s3` or `minio`.
This option has no effect for any other `TIER_TYPE`.

--use-aws-role[](#mc.ilm.tier.update.-use-aws-role)
Optional

Use the access permission for the locally configured [AWS Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html).

This option only applies if [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `s3` or `minio`.
This option has no effect for any other value of `TIER_TYPE`.

--account-key[](#mc.ilm.tier.update.-account-key)
Optional

The account key for a user on a remote Azure tier.

Required for Azure tier types.

Use this option to rotate the credentials for the [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-account-name|`--account-name`]] associated to the remote tier.

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `azure`.
This option has no effect for any other type of login.

--az-sp-tenant-id[](#mc.ilm.tier.update.-az-sp-tenant-id)
Optional

New in version mc: RELEASE.2024-07-03T20-17-25Z

Directory ID for the Azure service principal account.

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `azure`.
This option has no effect for any other type of login.

--az-sp-client-id[](#mc.ilm.tier.update.-az-sp-client-id)
Optional

New in version mc: RELEASE.2024-07-03T20-17-25Z

Client ID of the Azure service principal account.

Requires [`--az-sp-client-secret`](#mc.ilm.tier.update.-az-sp-client-secret).

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `azure`.
This option has no effect for any other type of login.

--az-sp-client-secret[](#mc.ilm.tier.update.-az-sp-client-secret)
Optional

New in version mc: RELEASE.2024-07-03T20-17-25Z

The secret for the Azure service principal account.

Requires [`--az-sp-client-id`](#mc.ilm.tier.update.-az-sp-client-id).

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `azure`.
This option has no effect for any other type of login.

--credentials-file[](#mc.ilm.tier.update.-credentials-file)
Optional

Required for Google Cloud Storage tier types.

The credential file for a user on the remote GCS tier.
The user must have permission to perform read/write/list/delete operations on the remote bucket or bucket prefix.

This option only applies to remote storage tiers with [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_TYPE|`TIER_TYPE`]] is `gcs`.
This option has no effect for any other type of login.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Rotate Credentials for an S3 Remote Tier

The following example updates the credentials for an S3 remote tier called `S3TIER` on the `myminio` deployment.

```
mc ilm tier update myminio S3TIER --access-key ACCESS_KEY --secret-key SECRET_KEY
```

Replace `S3TIER` with the name for your Amazon Simple Storage Solution tier.

Replace `ACCESS_KEY` with the updated access key for your S3 storage.

Replace `SECRET_KEY` with the updated secret key for the access key provided.

### Rotate Credentials for an Azure Blob Storage Remote Tier

The following example updates the credentials for an Azure remote tier called `AXTIER` on the `myminio` deployment.

```
mc ilm tier update myminio AZTIER --account-key ACCOUNT-KEY
```

Replace `AZTIER` with the name for your Azure tier.

Replace `ACCOUNT-KEY` with the updated key for your Azure storage.

### Rotate Credentials for a Google Cloud Storage Remote Tier

The following example updates the credentials for a Google Cloud Storage remote tier called `GCSTIER` on the `myminio` deployment.

```
mc ilm tier update myminio GCSTIER --credentials-file /path/to/credentials.json
```

Replace `GCSTIER` with the name for your Google Cloud Storage tier.

Replace `/path/to/credentials.json` with the path of the updated credential file to use to access the remote storage.

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Required Permissions

For permissions required to modify a tier, refer to the [[reference/minio-mc/mc-ilm-tier#minio-mc-ilm-tier-permissions|required permissions]] on the parent command.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).