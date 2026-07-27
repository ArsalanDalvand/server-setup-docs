---
title: "mc support — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support — SILO (MinIO Object Storage)"
---
# mc support

## Description

The MinIO Client [`mc support`](#command-mc.support) commands provides tools for analyzing deployment health or performance and for running diagnostics.
You can also upload generated health reports for further analysis by MinIO engineering.

Important

The `mc support` commands require an active [MinIO SUBNET](https://min.io/pricing?jmp=docs) registration.

[[reference/minio-mc/mc-support-proxy#mc.support.proxy.set|`mc support proxy set`]] and [[reference/minio-mc/mc-support-proxy#mc.support.proxy.remove|`mc support proxy remove`]] are exceptions, as you may need to set up a proxy to complete the deployment registration.

## Subcommands

[`mc support`](#command-mc.support) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-support-callhome#command-mc.support.callhome|`callhome`]]

The [[reference/minio-mc/mc-support-callhome#command-mc.support.callhome|`mc support callhome`]] command allows the enabling or disabling of diagnostic information from a deployment to [MinIO SUBNET](https://min.io/pricing?jmp=docs).

[[reference/minio-mc/mc-support-diag#command-mc.support.diag|`diag`]]

The [[reference/minio-mc/mc-support-diag#command-mc.support.diag|`mc support diag`]] command generates a health report for a MinIO deployment.

[[reference/minio-mc/mc-support-inspect#command-mc.support.inspect|`inspect`]]

The [[reference/minio-mc/mc-support-inspect#command-mc.support.inspect|`mc support inspect`]] command collects the data and metadata associated to objects at the specified path.

[[reference/minio-mc/mc-support-perf#command-mc.support.perf|`perf`]]

Use the [[reference/minio-mc/mc-support-perf#command-mc.support.perf|`mc support perf`]] command to review the performance of the S3 API (read/write), network IO, and storage (drive read/write).

[[reference/minio-mc/mc-support-profile#command-mc.support.profile|`profile`]]

[[reference/minio-mc/mc-support-profile#command-mc.support.profile|`mc support profile`]] runs a system profile for your deployment.
The results of the profile can provide insight into the MinIO server process running on a given node.

[[reference/minio-mc/mc-support-proxy#command-mc.support.proxy|`proxy`]]

Use the [[reference/minio-mc/mc-support-proxy#command-mc.support.proxy|`mc support proxy`]] command to configure a proxy to use to communicate with [MinIO SUBNET](https://min.io/pricing?jmp=docs).

[[reference/minio-mc/mc-support-top#command-mc.support.top|`top`]]

The [[reference/minio-mc/mc-support-top#command-mc.support.top|`mc support top`]] command returns statistics for distributed
MinIO deployments, similar to the output of the `top` command in a shell.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).