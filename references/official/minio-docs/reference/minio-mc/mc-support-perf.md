---
title: "mc support perf — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-perf.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-perf"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support perf — SILO (MinIO Object Storage)"
---
# mc support perf

Changed in version RELEASE.2022-07-24T02-25-13Z: `mc support perf` replaces the `mc admin speedtest` command.

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Description

Use the [`mc support perf`](#command-mc.support.perf) command to review the performance of the S3 API (read/write), network IO, and storage (drive read/write).

The resulting tests can provide general guidance of deployment performance under S3 `GET` and `PUT` requests and identify any potential bottlenecks.
For more complete performance testing, consider using a combination of load-testing using your staging application environments and the MinIO [WARP](https://github.com/minio/warp) S3 benchmarking tool.

[`mc support perf`](#command-mc.support.perf) has the following subcommands

[`drive`](#mc.support.perf.drive)

Measure the speed of drives in a MinIO deployment.

[`mc support perf drive`](#mc.support.perf.drive) temporarily suspends S3 API calls during the test.
Incoming requests are held in a queue while the command runs.
When the command completes or ends, MinIO processes the queued requests and resumes normal operations.

[`object`](#mc.support.perf.object)

Measure the speed of reading and writing objects in a cluster.

[`net`](#mc.support.perf.net)

Measure the network throughput of all nodes.

[`mc support perf net`](#mc.support.perf.net) temporarily suspends S3 API calls during the test.
Incoming requests are held in a queue while the command runs.
When the command completes or ends, MinIO processes the queued requests and resumes normal operations.

[`client`](#mc.support.perf.client)

Measure the network throughput to a client.

[`site-replication`](#mc.support.perf.site-replication)

Measure the speed of site replication operations.

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Examples

### Measure Speed of an Object

Measure the performance of S3 read/write of an object on the alias `minio1`.
MinIO autotunes concurrency to obtain maximum throughput and IOPS (Input/Output Per Second).

```
mc support perf object minio1
```

### Measure Speed of an Object of a Specific Size for a Specific Duration

Run object the S3 read/write performance of an object for 20 seconds with object size of 128MiB on alias `minio1`.
MinIO autotunes concurrency to obtain maximum throughput.

```
mc support perf object minio1 --duration 20s --size 128MiB
```

### Test Speed of All Drives on All Nodes with Default Specifications

Run drive read/write performance measurements on all drive on all nodes for a cluster with alias `minio1`.
The command does not specify the blocksize, so the default of 4MiB is used.

```
mc support perf drive minio1
```

### Test Drive Speed Measurements with Custom Specifications

Run drive read/write performance measurements on a cluster with alias `minio1` specifying a blocksize of 64KiB and data read/written from each drive of 2GiB.

```
mc support perf drive minio1 --blocksize 64KiB --filesize 2GiB
```

### Test Network Throughput

Run a network throughput test on a cluster with alias `minio1`.

```
mc support perf net minio1
```

### Test Site Replication Speed

Run a test on the speed of site replication operations from the `minio1` site to other configured peers.

```
mc support perf site-replication minio1
```

## Syntax

mc support perf drive[](#mc.support.perf.drive)

Measure the read/write speed of the drives in a cluster.

```
mc [GLOBAL FLAGS] support perf drive   \
                [--concurrent]         \
                [--verbose, -v]        \
                [--filesize]           \
                [--blocksize]          \
                [--serial]             \
                [--airgap]             \
                ALIAS
```

mc support perf object[](#mc.support.perf.object)

Measure the S3 performance of reading and writing objects in a cluster.

```
mc [GLOBAL FLAGS] support perf object  \
                [--size]               \
                [--concurrent]         \
                [--verbose, -v]        \
                [--airgap]             \
                ALIAS
```

mc support perf net[](#mc.support.perf.net)

Measure the network throughput of all nodes in a cluster.

```
mc [GLOBAL FLAGS] support perf net  \
                [--concurrent]      \
                [--verbose, -v]     \
                [--serial]          \
                [--airgap]          \
                ALIAS
```

mc support perf client[](#mc.support.perf.client)

Measure the network throughput from the local device running the MinIO Client to the server.

```
mc [GLOBAL FLAGS] support perf client  \
                --duration             \
                [--verbose, -v]        \
                [--airgap]             \
                ALIAS
```

mc support perf site-replication[](#mc.support.perf.site-replication)

Measure the speed of site replication operations from the specified `ALIAS` to other configured peers.

```
mc [GLOBAL FLAGS] support perf site-replication \
                  --duration                    \
                  [--verbose, -v]               \
                  ALIAS
```

### Parameters

--airgap[](#mc.support.perf.-airgap)
Optional

Use in environments without network access to SUBNET (for example, airgapped, firewalled, or similar configuration).

If the deployment is airgapped, but the local device where you are using the [[reference/minio-mc#minio-client|minio client]] has network access, you do not need to use the `--airgap` flag.

--size[](#mc.support.perf.-size)
Optional

Applies to the [`object`](#mc.support.perf.object) command.

Specify the size of the object to use for upload and download performance test.

If not specified, the default value is `64MiB`.

Use `--size <value>` where `<value>` is a number and the storage unit, `KiB`, `MiB`, or `GiB`.

--concurrent[](#mc.support.perf.-concurrent)
Optional

Applies to the [`drive`](#mc.support.perf.drive), [`object`](#mc.support.perf.object), and [`net`](#mc.support.perf.net) commands.

Specify the number of concurrent requests to test per server.

If not specified, the default value is `32`.

Use `--concurrent <value>` where `<value>` is a number.

--verbose, -v[](#mc.support.perf.-verbose)
Optional

Applies to the [`drive`](#mc.support.perf.drive), [`object`](#mc.support.perf.object), and [`net`](#mc.support.perf.net) commands.

Show per-server stats in the output.

--filesize[](#mc.support.perf.-filesize)
Optional

Applies to the [`drive`](#mc.support.perf.drive) command.

Specify the total size of data to read or write to each drive.

If not specified, the default value is `1GiB`.

Use `--filesize <value>` where `<value>` is a number and storage unit, `KiB`, `MiB`, or `GiB`.

--blocksize[](#mc.support.perf.-blocksize)
Optional

Applies to the [`drive`](#mc.support.perf.drive) command.

Specify the read/write block size.

If not specified, the default value is `4MiB`.

Use `--filesize <value>` where `<value>` is a number and a storage unit, using standard storage unit abbreviations.

--serial[](#mc.support.perf.-serial)
Optional

Applies to the [`drive`](#mc.support.perf.drive) and [`net`](#mc.support.perf.net) commands.

Run performance tests on drive(s) one by one.

ALIAS[](#mc.support.perf.ALIAS)
Required

Applies to the [`drive`](#mc.support.perf.drive), [`object`](#mc.support.perf.object), [`net`](#mc.support.perf.net), and [`client`](#mc.support.perf.client) commands.

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

--duration[](#mc.support.perf.-duration)
Required

Applies to the [`client`](#mc.support.perf.client) command.

Length of time in seconds to perform the test.
Time cannot be 0 or negative.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).