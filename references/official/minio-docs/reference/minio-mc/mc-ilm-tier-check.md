---
title: "mc ilm tier check — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-tier-check.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-tier-check"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm tier check — SILO (MinIO Object Storage)"
---
# mc ilm tier check

## Description

The [`mc ilm tier check`](#command-mc.ilm.tier.check) command displays the configuration for remote tier on a deployment.

## Syntax

The command has the following syntax:

EXAMPLE

The following example displays the configuration for an existing remote tier called `WARM-TIER` on the `myminio` deployment.

```
mc ilm tier check myminio WARM-TIER
```

SYNTAX

The command has the following syntax:

```
mc ilm tier add TARGET TIER_NAME
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

The command accepts the following arguments:

TARGET[](#mc.ilm.tier.check.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which the desired tier exists.

TIER_NAME[](#mc.ilm.tier.check.TIER_NAME)
Required

The name of an existing remote tier to display.

You must specify the tier in all-caps, e.g. `WARM_TIER`.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Display the Configuration for an Existing Tier

The following example displays the configuration of the tier `WARM-TIER` on the `myminio` deployment.

```
mc ilm tier check myminio WARM-TIER
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