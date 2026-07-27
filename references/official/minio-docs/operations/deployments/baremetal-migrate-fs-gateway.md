---
title: "Migrate from Gateway or Filesystem Mode — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/deployments/baremetal-migrate-fs-gateway.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/deployments/baremetal-migrate-fs-gateway"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Migrate from Gateway or Filesystem Mode — SILO (MinIO Object Storage)"
---
# Migrate from Gateway or Filesystem Mode

## Background

The MinIO Gateway and the related filesystem mode entered a feature freeze in July 2020.
In February 2022, MinIO announced the [deprecation of the MinIO Gateway](https://blog.min.io/deprecation-of-the-minio-gateway/?ref=docs).
Along with the deprecation announcement, MinIO also announced that the feature would be removed in six months time.

As of [RELEASE.2022-10-29T06-21-33Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-10-29T06-21-33Z), the MinIO Gateway and the related filesystem mode code have been removed.
Deployments still using the standalone or filesystem MinIO modes that upgrade to MinIO Server [RELEASE.2022-10-29T06-21-33Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-10-29T06-21-33Z) or later receive an error when attempting to start MinIO.

## Overview

To upgrade to the [RELEASE.2022-10-29T06-21-33Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-10-29T06-21-33Z) or later release, those who were using the standalone or filesystem deployment modes must create a new [[operations/deployments/installation#minio-snsd|Single-Node Single-Drive]] deployment and migrate settings and content to the new deployment.

This document outlines the steps required to successfully launch and migrate to a new deployment.

Important

Standalone/file system mode continues to work on any release up to and including MinIO Server [RELEASE.2022-10-24T18-35-07Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-10-24T18-35-07Z).
To continue using a standalone deployment, install that MinIO Server release with MinIO Client [RELEASE.2022-10-29T10-09-23Z](https://github.com/minio/mc/releases/tag/RELEASE.2022-10-29T10-09-23Z) or any [earlier release](https://github.com/minio/minio/releases) with its corresponding MinIO Client. Note that the version of the MinIO Client should be newer and as close as possible to the version of the MinIO server.

Filesystem mode deployments must be on at least [RELEASE.2022-06-25T15-50-16Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-06-25T15-50-16Z)  to use the MinIO Client import and export commands.
Filesystem mode deployments up to and including [RELEASE.2022-06-20T23-13-45Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-06-20T23-13-45Z) can be migrated by manually recreating users, policies, buckets, and other resources on the new deployment.

## Procedure

Note

You can set MinIO configuration settings in environment variables and using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].
Depending on your current deployment setup, you may need to retrieve the values for both.

You can examine any runtime settings using `env | grep MINIO_` or, for deployments using MinIO’s systemd service, check the contents of `/etc/default/minio`.

For filesystem mode deployments:

If needed, upgrade the existing deployment.

The oldest acceptable versions are:

MinIO [RELEASE.2022-06-25T15-50-16Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-06-25T15-50-16Z)

MinIO Client [RELEASE.2022-06-26T18-51-48Z](https://github.com/minio/mc/releases/tag/RELEASE.2022-06-26T18-51-48Z)

The newest acceptable versions are:

MinIO [RELEASE.2022-10-24T18-35-07Z](https://github.com/minio/minio/releases/tag/RELEASE.2022-10-24T18-35-07Z)

MinIO Client [RELEASE.2022-10-29T10-09-23Z](https://github.com/minio/mc/releases/tag/RELEASE.2022-10-29T10-09-23Z)

Create a new Single-Node Single-Drive MinIO deployment.

Follow our [[operations/deployments/baremetal-deploy-minio-server#deploy-minio-standalone|installation instructions]] for your OS of choice and configure the installation as a Single-Node Single-Drive (SNSD) topology.

The location of the deployment can be any empty folder on the storage medium of your choice.
A new folder on the same drive can work for the new deployment as long as the existing deployment is not on the root of a drive.
If the existing standalone system points to the root of the drive, you must use a separate drive for the new deployment.

If both old and new deployments are on the same host:

Install the new deployment to a different path from the existing deployment.

Set the new deployment’s Console and API ports to different ports than the existing deployment.

The following commandline options set the ports at startup:

[[reference/minio-server/minio-server#minio.server.-address|`--address`]] to set the API port.

[[reference/minio-server/minio-server#minio.server.-console-address|`--console-address`]] to set the Console port.

For deployments managed by `systemd`:

Duplicate the existing `/etc/default/minio` environment file with a unique name.

In the new deployment’s service file, update `EnvironmentFile` to reference the new environment file.

The steps below use the [[reference/minio-mc#command-mc|`mc`]] command line tool from both deployments.
Existing MinIO Client is [[reference/minio-mc#command-mc|`mc`]] from the old deployment.
New MinIO Client is [[reference/minio-mc#command-mc|`mc`]] from the new deployment.

Add an alias for the deployment created in the previous step using [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] and the new MinIO Client.

```
mc alias set NEWALIAS PATH ACCESSKEY SECRETKEY
```

Use the new MinIO Client.

Replace `NEWALIAS` with the alias to create for the deployment.

Replace `PATH` with the IP address or hostname and port for the new deployment.

Replace `ACCESSKEY` and `SECRETKEY` with the credentials you used when creating the new deployment.

Migrate settings according to the type of deployment:

The MinIO Gateway is a stateless proxy service that provides S3 API compatibility for an array of backend storage systems.

Filesystem mode deployments provide an S3 access layer for a single MinIO server process and single storage volume.

Gateway

Migrate configuration settings:

If your deployment uses [[reference/minio-server/settings#minio-server-environment-variables|environment variables]] for configuration settings, copy the environment variables from the existing deployment’s `/etc/default/minio` file to the same file in the new deployment.
You may omit any `MINIO_CACHE_*` and `MINIO_GATEWAY_SSE` environment variables, as these are no longer used.

If you use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] for configuration settings, duplicate the existing settings for the new deployment using the new MinIO Client.

Filesystem mode

Note

The following Filesystem mode steps presume the existing MinIO Client supports the needed export commands.
If it does not, recreate users, policies, lifecycle rules, and buckets manually on the new deployment using the new MinIO Client.

Export the existing deployment’s configurations.

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.export|`mc admin config export`]] command with the existing MinIO Client to retrieve the configurations defined for the existing standalone MinIO deployment.

```
mc admin config export ALIAS > config.txt
```

Use the existing MinIO Client.

Replace `ALIAS` with the alias used for the existing standalone deployment you are retrieving values from.

Import configurations from the existing standalone deployment to the new deployment with the new MinIO Client.

```
mc admin config import ALIAS < config.txt
```

Use the new MinIO Client.

Replace `ALIAS` with the alias for the new deployment.

If [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.import|`import`]] reports an error for a configuration key, comment it out with `#` at the beginning of the relevant line and try again.
When you are finished migrating the deployment, verify the current syntax for the target MinIO Server version and set any needed keys manually using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

Restart the server for the new deployment with the new MinIO Client.

```
mc admin service restart ALIAS
```

Use the new MinIO Client.

Replace `ALIAS` with the alias for the new deployment.

Export bucket metadata from the existing standalone deployment with the existing MinIO Client.

The following command exports bucket metadata from the existing deployment to a `.zip` file.

The data includes:

bucket targets

lifecycle rules

notifications

quotas

locks

versioning

The export includes the bucket metadata only.
This command does not export objects from the existing deployment.

```
mc admin cluster bucket export ALIAS
```

Use the existing MinIO Client.

Replace `ALIAS` with the alias for your existing deployment.

This command creates a `cluster-metadata.zip` file with metadata for each bucket.

Import bucket metadata to the new deployment with the new MinIO Client.

The following command reads the contents of the exported bucket `.zip` file and creates buckets on the new deployment with the same configurations.

```
mc admin cluster bucket import ALIAS cluster-metadata.zip
```

Use the new MinIO Client.

Replace `ALIAS` with the alias for the new deployment.

The command creates buckets on the new deployment with the same configurations as provided by the metadata in the .zip file from the existing deployment.

Export IAM settings from the existing standalone deployment to new deployment with the existing MinIO Client.

If you are using an external identity and access management provider, recreate those settings in the new deployment along with all associated policies.

Use the following command to export IAM settings from the existing deployment.
This command exports:

Groups and group mappings

STS users and STS user mappings

Policies

Users and user mappings

```
mc admin cluster iam export ALIAS
```

Use the existing MinIO Client.

Replace `ALIAS` with the alias for your existing deployment.

This command creates a `ALIAS-iam-info.zip` file with IAM data.

Import the IAM settings to the new deployment with the new MinIO Client.

Use the exported file to create the IAM setting on the new deployment.

```
mc admin cluster iam import ALIAS alias-iam-info.zip
```

Use the new MinIO Client.

Replace `ALIAS` with the alias for the new deployment.

Replace the name of the zip file with the name for the existing deployment’s file.

Migrate bucket contents with [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].

Use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]] with the [[reference/minio-mc/mc-mirror#mc.mirror.-preserve|`--preserve`]] and [[reference/minio-mc/mc-mirror#mc.mirror.-watch|`--watch`]] flags on the standalone deployment to move objects to the new SNSD deployment with the existing MinIO Client

```
mc mirror --preserve --watch SOURCE/BUCKET TARGET/BUCKET
```

Use the existing MinIO Client.

Replace `SOURCE/BUCKET` with the alias and a bucket for the existing standalone deployment.

Replace `TARGET/BUCKET` with the alias and corresponding bucket for the new deployment.

Stop writes to the standalone deployment from any S3 or POSIX client.

Wait for `mc mirror` to complete for all buckets for any remaining operations.

Stop the server for both deployments.

Restart the new MinIO deployment with the ports used for the previous standalone deployment.

Ensure you apply all environment variables and runtime configuration settings and validate the behavior of the new deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).