---
title: "mc ilm tier info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-tier-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-tier-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm tier info — SILO (MinIO Object Storage)"
---
# mc ilm tier info

## Description

The [`mc ilm tier info`](#command-mc.ilm.tier.info) command outputs statistics about a tier or all tiers for a deployment.

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

## Syntax

The command has the following syntax:

EXAMPLE

The following example outputs the configuration for an existing remote tier called `WARM-TIER` on the `myminio` deployment.

```
mc ilm tier info myminio WARM-TIER
```

SYNTAX

The command has the following syntax:

```
mc ilm tier info TARGET TIER_NAME
```

### Parameters

The command accepts the following arguments:

TARGET[](#mc.ilm.tier.info.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which the desired tier exists.

TIER_NAME[](#mc.ilm.tier.info.TIER_NAME)
Optional

The name of an existing remote tier to display.

You must specify the tier in all-caps, e.g. `WARM_TIER`.

If not specified, MinIO lists statistics for all existing tiers on the deployment.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Display the Statistics for an Existing Tier

The following example displays the statistics of the tier `WARM-TIER` on the `myminio` deployment.

```
mc ilm tier info myminio WARM-TIER
```

### Display the Statistics for all Existing Tiers on a Deployment

The following example displays the statistics of all existing tiers on the `myminio` deployment.

```
mc ilm tier info myminio
```

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Required Permissions

For permissions required to review a tier, refer to the [[reference/minio-mc/mc-ilm-tier#minio-mc-ilm-tier-permissions|required permissions]] on the parent command.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).