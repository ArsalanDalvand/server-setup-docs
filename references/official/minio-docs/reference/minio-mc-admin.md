---
title: "MinIO Admin Client — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Admin Client — SILO (MinIO Object Storage)"
---
# MinIO Admin Client

The MinIO Client [[reference/minio-mc#command-mc|`mc`]] command line tool provides the [`mc admin`](#command-mc.admin)
command for performing administrative tasks on your MinIO deployments.

While [[reference/minio-mc#command-mc|`mc`]] supports any S3-compatible service,
[`mc admin`](#command-mc.admin) only supports MinIO deployments.

[`mc admin`](#command-mc.admin) has the following syntax:

```
mc admin [FLAGS] COMMAND [ARGUMENTS]
```

## Command Quick reference

The following table lists [`mc admin`](#command-mc.admin) commands:

Command

Description

[[reference/minio-mc-admin/mc-admin-accesskey#command-mc.admin.accesskey|`mc admin accesskey`]]

The [[reference/minio-mc-admin/mc-admin-accesskey#command-mc.admin.accesskey|`mc admin accesskey`]] command and its subcommands create and manage [[administration/identity-access-management/minio-user-management#minio-idp-service-account|Access Keys]] for internally managed users on a MinIO deployment.

[[reference/minio-mc-admin/mc-admin-cluster-bucket#command-mc.admin.cluster.bucket|`mc admin cluster bucket`]]

The [[reference/minio-mc-admin/mc-admin-cluster-bucket#command-mc.admin.cluster.bucket|`mc admin cluster bucket`]] command and its subcommands provide tools for manually importing and exporting MinIO bucket metadata.

[[reference/minio-mc-admin/mc-admin-cluster-iam#command-mc.admin.cluster.iam|`mc admin cluster iam`]]

The [[reference/minio-mc-admin/mc-admin-cluster-iam#command-mc.admin.cluster.iam|`mc admin cluster iam`]] command and its subcommands provide tools for manually importing and exporting MinIO [[administration/identity-access-management#minio-authentication-and-identity-management|identity and access management (IAM)]] metadata.

[[reference/minio-mc-admin/mc-admin-decommission#command-mc.admin.decommission|`mc admin decommission`]]

The [[reference/minio-mc-admin/mc-admin-decommission#command-mc.admin.decommission|`mc admin decommission`]] command starts the decommissioning process for a
MinIO [[operations/concepts#minio-intro-server-pool|server pools]]. Decommissioning is designed
for removing an older server pool whose hardware is no longer sufficient or
performant compared to the pools in the deployment. MinIO automatically migrates
data from the decommissioned pool to the remaining pools in the deployment based
on the ratio of free space available in each pool.

[[reference/minio-mc-admin/mc-admin-group#command-mc.admin.group|`mc admin group`]]

The [[reference/minio-mc-admin/mc-admin-group#command-mc.admin.group|`mc admin group`]] command manages groups on a MinIO deployment.

[[reference/minio-mc-admin/mc-admin-heal#command-mc.admin.heal|`mc admin heal`]]

The [[reference/minio-mc-admin/mc-admin-heal#command-mc.admin.heal|`mc admin heal`]] command scans for objects that are damaged or corrupted and heals those objects.

[[reference/minio-mc-admin/mc-admin-info#command-mc.admin.info|`mc admin info`]]

The [[reference/minio-mc-admin/mc-admin-info#command-mc.admin.info|`mc admin info`]] command displays information on a MinIO server.
For distributed MinIO deployments, [[reference/minio-mc-admin/mc-admin-info#command-mc.admin.info|`mc admin info`]] displays information
for each MinIO server in the deployment.

[[reference/minio-mc-admin/mc-admin-kms-key#command-mc.admin.kms.key|`mc admin kms key`]]

The [[reference/minio-mc-admin/mc-admin-kms-key#command-mc.admin.kms.key|`mc admin kms key`]] command performs cryptographic key management
operations through the MinIO Key Encryption Service (KES).

[[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]]

Use the [[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]] command to show MinIO server logs.

[[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]]

The [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] commands manage policies for use with [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy-Based Access Control]] (PBAC).
MinIO PBAC uses IAM-compatible policy JSON documents to define rules for accessing resources on a MinIO server.

[[reference/minio-mc-admin/mc-admin-prometheus#command-mc.admin.prometheus|`mc admin prometheus`]]

The [[reference/minio-mc-admin/mc-admin-prometheus#command-mc.admin.prometheus|`mc admin prometheus`]] command and its subcommands provide access to MinIO Prometheus metrics.

[[reference/minio-mc-admin/mc-admin-rebalance#command-mc.admin.rebalance|`mc admin rebalance`]]

The [[reference/minio-mc-admin/mc-admin-rebalance#command-mc.admin.rebalance|`mc admin rebalance`]] command allows starts, monitors, or stops a rebalancing operation on a MinIO deployment.
Rebalancing redistributes objects across all pools in the deployment.

[[reference/minio-mc-admin/mc-admin-replicate#command-mc.admin.replicate|`mc admin replicate`]]

The [[reference/minio-mc-admin/mc-admin-replicate#command-mc.admin.replicate|`mc admin replicate`]] command creates and manages [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]] for a set of MinIO peer sites.

Site replication mimics an active-active bucket replication, but for multiple MinIO deployments.
Wherever a change occurs to IAM settings, buckets, or objects across the set of sites, the change replicates across all sites in the site replication group.

[[reference/minio-mc-admin/mc-admin-scanner#command-mc.admin.scanner|`mc admin scanner`]]

The [[reference/minio-mc-admin/mc-admin-scanner#command-mc.admin.scanner|`mc admin scanner`]] commands provide information about the [[operations/concepts/scanner#minio-concepts-scanner|scanner]] process.

[[reference/minio-mc-admin/mc-admin-service#command-mc.admin.service|`mc admin service`]]

The [[reference/minio-mc-admin/mc-admin-service#command-mc.admin.service|`mc admin service`]] command can restart or unfreeze MinIO servers.

[[reference/minio-mc-admin/mc-admin-trace#command-mc.admin.trace|`mc admin trace`]]

The [[reference/minio-mc-admin/mc-admin-trace#command-mc.admin.trace|`mc admin trace`]] command displays API operations occurring on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-update#command-mc.admin.update|`mc admin update`]]

The [[reference/minio-mc-admin/mc-admin-update#command-mc.admin.update|`mc admin update`]] command updates all MinIO servers in the deployment.
The command also supports using a private mirror server for environments where the deployment does not have public internet access.

[[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]]

The [[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]] command and its subcommands manage [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO users]].

## Installation

Linux

The following commands add a temporary extension to your system
PATH for running the `mc` utility. Defer to your operating system
instructions for making permanent modifications to your system PATH.

Alternatively, execute `mc` by navigating to the parent folder and
running `./mc --help`

64-bit Intel

```
curl https://dl.min.io/client/mc/release/linux-amd64/mc \
  --create-dirs \
  -o $HOME/minio-binaries/mc

chmod +x $HOME/minio-binaries/mc
export PATH=$PATH:$HOME/minio-binaries/

mc --help
```

64-bit PPC

```
curl https://dl.min.io/client/mc/release/linux-ppc64le/mc \
  --create-dirs \
  -o ~/minio-binaries/mc

chmod +x $HOME/minio-binaries/mc
export PATH=$PATH:$HOME/minio-binaries/

mc --help
```

ARM64

```
curl https://dl.min.io/client/mc/release/linux-arm64/mc \
  --create-dirs \
  -o ~/minio-binaries/mc

chmod +x $HOME/minio-binaries/mc
export PATH=$PATH:$HOME/minio-binaries/

mc --help
```

Install from the MinIO Download Page

MinIO does not officially publish its binaries to common Linux repositories or package managers (Ubuntu, RHEL, Archlinux/AUR).
The only official source of MinIO binaries is the [MinIO Download Page](https://dl.min.io/client/mc/release/).

MinIO does not recommend installation through a package manager, as upstream repositories may install the incorrect package or a renamed package.

All documentation assumes the installation of the official `mc` client binary through the download page only, with no changes to binary naming.

macOS

```
brew install minio/stable/mc
mc --help
```

Windows

Open the following file in a browser:

[https://dl.min.io/client/mc/release/windows-amd64/mc.exe](https://dl.min.io/client/mc/release/windows-amd64/mc.exe)

Execute the file by double clicking on it, or by running the
following in the command prompt or powershell:

```
\path\to\mc.exe --help
```

Source

Installation from source is intended for developers and advanced users
and requires a working Golang environment. See
[How to install Golang](https://golang.org/doc/install).

Run the following commands in a terminal environment to install `mc`
from source:

```
go install github.com/minio/mc@latest
```

[[reference/minio-mc/mc-update#command-mc.update|`mc update`]] does not support source-based installations.

## Quickstart

Ensure that the host machine has [[reference/minio-mc#command-mc|`mc`]]
[installed](#mc-admin-install) prior to starting this procedure.

Important

The following example temporarily disables the bash history to mitigate the
risk of authentication credentials leaking in plain text. This is a basic
security measure and does not mitigate all possible attack vectors. Defer to
security best practices for your operating system for inputting sensitive
information on the command line.

Use the [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command to add the
deployment to the mc configuration.

```
bash +o history
mc config host add <ALIAS> <ENDPOINT> ACCESS_KEY SECRET_KEY
bash -o history
```

Replace each argument with the required values. Specifying only the
`mc config host add` command starts an input prompt for entering the
required values.

Use the [[reference/minio-mc-admin/mc-admin-info#command-mc.admin.info|`mc admin info`]] command to test the connection to
the newly added MinIO deployment:

```
mc admin info <ALIAS>
```

## Global Options

[`mc admin`](#command-mc.admin) supports the same global options as [[reference/minio-mc#command-mc|`mc`]].
See [[reference/minio-mc#minio-mc-global-options|Global Options]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).