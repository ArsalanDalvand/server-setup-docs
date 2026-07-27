---
title: "mc admin profile — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-profile.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-profile"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin profile — SILO (MinIO Object Storage)"
---
# mc admin profile

Note

This command has been replaced by [[reference/minio-mc/mc-support-profile#command-mc.support.profile|`mc support profile`]] as of mc RELEASE.2023-04-06T16-51-10Z.

## Description

The [`mc admin profile`](#command-mc.admin.profile) command generates profiling data for debugging
purposes.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

### Profile Data Format

[`mc admin profile`](#command-mc.admin.profile) produces a `ZIP` archive `profile.zip` that
contains one or more `.pprof` files. Use the
[pprof](https://github.com/google/pprof) `go` utility to read the
profile data.

## Examples

### Profile Data for Single Resource

Use [`mc admin profile start`](#mc.admin.profile.start) with the
[`type`](#mc.admin.profile.start.type) flag to start profiling the
resource:

```
mc admin profile start --type "TYPE" ALIAS
```

Replace [`ALIAS`](#mc.admin.profile.start.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO host.

Replace [`TYPE`](#mc.admin.profile.start.type) with the resource to
profile.

Use [`mc admin profile stop`](#mc.admin.profile.stop) to stop profiling data from the specified
resource and output the results:

```
mc admin profile stop
```

The command outputs the profiled data as `profile.zip`.

### Profile Data for Multiple Resources

Use [`mc admin profile start`](#mc.admin.profile.start) with the
[`type`](#mc.admin.profile.start.type) flag to start profiling the
resources:

```
mc admin profile start --type "TYPE,[TYPE...]" ALIAS
```

Replace [`ALIAS`](#mc.admin.profile.start.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO host.

Replace [`TYPE`](#mc.admin.profile.start.type) with the resources to
profile. Specify multiple resources as a comma-separated list.

Use [`mc admin profile stop`](#mc.admin.profile.stop) to stop profiling data from the specified
resources and output the results:

```
mc admin profile stop
```

The command outputs the profiled data as `profile.zip`.

## Syntax

[`mc admin profile`](#command-mc.admin.profile) has the following syntax:

```
mc admin profile SUBCOMMAND
```

[`mc admin profile`](#command-mc.admin.profile) supports the following subcommands:

mc admin profile start[](#mc.admin.profile.start)

Starts collecting profiling data on the target MinIO deployment. The
command has the following syntax:

```
mc admin profile start [FLAGS] TARGET
```

[`mc admin profile start`](#mc.admin.profile.start) supports the following arguments:

TARGET[](#mc.admin.profile.start.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from
which the command collects profiling data.

type[](#mc.admin.profile.start.type)

The type(s) of profiling data to collect from the
[`TARGET`](#mc.admin.profile.start.TARGET) MinIO deployment.

Specify one or more of the following supported types as a comma-separated
list:

`cpu`

`mem`

`block`

`mutex`

`trace`

`threads`

`goroutines`

Defaults to `cpu,mem,block` if omitted.

mc admin profile stop[](#mc.admin.profile.stop)

Stops the profiling process and returns the collected data as
`profile.zip`. The `zip` file contains one or more
`.pprof` files which are readable with programs like the `go`
[pprof](https://github.com/google/pprof) utility.

The command has the following syntax:

```
mc admin profile stop TARGET
```

The command supports the following arguments:

TARGET[](#mc.admin.profile.stop.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from
which the command returns available profiling data.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).