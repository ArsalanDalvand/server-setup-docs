---
title: "mc support callhome — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-callhome.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-callhome"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support callhome — SILO (MinIO Object Storage)"
---
# mc support callhome

## Description

The [`mc support callhome`](#command-mc.support.callhome) command allows the enabling or disabling of diagnostic information from a deployment to [MinIO SUBNET](https://min.io/pricing?jmp=docs).

All `mc support` commands require an active SUBNET subscription.

When enabled, MinIO sends diagnostic information to SUBNET.

MinIO disables this functionality by default, regardless of registration status.
You must explicitly enable the `callhome` function to begin information upload.

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Syntax

mc support callhome enable[](#mc.support.callhome.enable)

Begin sending a deployment’s diagnostics, logs, or both to SUBNET.

```
mc support callhome enable    \
                    ALIAS     \
                    [--logs]  \
                    [--diag]
```

Note

The `--logs` and `--diag` flags are no longer supported in SUBNET and will be removed in a future release.

mc support callhome disable[](#mc.support.callhome.disable)

Stop sending a deployment’s diagnostics, logs, or both to SUBNET.

```
mc support callhome disable  \
                    ALIAS    \
                    [--logs] \
                    [--diag]
```

Note

The `--logs` and `--diag` flags are no longer supported in SUBNET and will be removed in a future release.

mc support callhome status[](#mc.support.callhome.status)

Output whether a deployment currently sends diagnostics, logs, or both to SUBNET.

```
mc support callhome status   \
                    ALIAS    \
                    [--diag]
```

Note

The `--diag` flag is no longer supported in SUBNET and will be removed in a future release.

### Parameters

ALIAS[](#mc.support.callhome.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

--diag[](#mc.support.callhome.-diag)
Optional

Note

This option is no longer supported in SUBNET and will be removed in a future release.

Send or stop sending deployment diagnostic information to SUBNET every 24 hours.

## Examples

### Enable callhome reporting

Enable sending diagnostic information to SUBNET for a deployment registered to SUBNET with an [[reference/minio-mc/mc-alias-set#alias|alias]] of `minio1`.

```
mc support callhome enable minio1
```

### Disable callhome reporting

Disable sending diagnostic information to SUBNET for a deployment registered to SUBNET with an [[reference/minio-mc/mc-alias-set#alias|alias]] of `minio1`.

```
mc support callhome disable minio1
```

### Display Current callhome settings

Display whether a deployment with the alias `minio1` sends information to SUBNET.

```
mc support callhome status minio1
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).