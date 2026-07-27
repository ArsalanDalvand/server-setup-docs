---
title: "mc support profile — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-profile.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-profile"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support profile — SILO (MinIO Object Storage)"
---
# mc support profile

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Description

[`mc support profile`](#command-mc.support.profile) runs a system profile for your deployment.
The results of the profile can provide insight into the MinIO server process running on a given node.

The resulting report is intended for use by MinIO Engineering.
You can upload the report to [MinIO SUBNET](https://min.io/pricing?jmp=docs).
Independent or third-party use of these profiles for diagnostics and remediation is done at your own risk.

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Examples

### Fetch CPU Profiling

This command retrieves the CPU profile on a MinIO deployment with the alias `minio1`.
The profile runs for the default of 10 seconds.

```
mc support profile --type cpu minio1/
```

### Fetch CPU, Memory, and Block Profiling Concurrently

This command fetches the profile of the CPU, memory, and block usage on the alias `minio2`.
The profile runs for the default of 10 seconds.

```
mc support profile --type cpu,mem,block minio2/
```

### Fetch CPU, Memory, and Block Profiling Concurrently for 10 Minutes

This command fetches the profile of the CPU, memory, and block  on the alias `minio3`.
The profile runs for 10 minutes (600 seconds).

```
mc support profile --type cpu,mem,block --duration 600 minio3/
```

## Syntax

The [`mc support profile`](#command-mc.support.profile) command has the following syntax:

```
mc [GLOBALFLAGS] support profile       \
                         COMMAND       \
                         [--type]      \
                         [--airgap]    \
                         [--duration]  \
                         ALIAS
```

### Parameters

--duration[](#mc.support.profile.-duration)
Optional

Run profiling for the specified duration in seconds.

Use `--type <value>` where `<value>` is the number of seconds for the profile to run.

If not specified, the command collects data for 10 seconds.

--type[](#mc.support.profile.-type)
Optional

Specify the profile(s) to gather data for.

Use `--type <value>` where `<value>` is one or more comma-separated types of data to collect.

Valid types are:

`cpu`

`cpuio`

`mem`

`block`

`mutex`

`trace`

`threads`

`goroutines`

If not specified, the command collects data for CPU, memory, block, mutex, threads, and goroutines.

Important

Do not use the `cpuio` or `trace` data types unless directed to by MinIO Support.
These profiles require significant resources and may degrade cluster performance if used without guidance.

--airgap[](#mc.support.profile.-airgap)
Optional

Use in environments without network access to SUBNET (for example, airgapped, firewalled, or similar configuration).

If the deployment is airgapped, but the local device where you are using the [[reference/minio-mc#minio-client|minio client]] has network access, you do not need to use the `--airgap` flag.

ALIAS[](#mc.support.profile.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).