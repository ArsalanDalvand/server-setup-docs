---
title: "Core Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/core.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/core"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Core Settings — SILO (MinIO Object Storage)"
---
# Core Settings

This page covers settings that control core behavior of the MinIO process.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## MinIO Server CLI Options

Environment Variable

MINIO_OPTS[](#envvar.MINIO_OPTS)

Configuration Setting

There is no configuration setting for this variable, as these settings apply at server startup.

Optional

Set a string of [[reference/minio-server/minio-server#minio-server-parameters|parameters]] to use when starting the MinIO Server.

For Unix-like systems using the recommended MinIO `systemd` service, use the `/etc/default/minio` file and create an environment variable `MINIO_OPTS` for specifying parameters to append to the `minio` systemd process:

```
# Editing /etc/default/minio

MINIO_OPTS=' --console-address=":9001" --ftp="address=:8021" --ftp="passive-port-range=30000-40000" '
```

For systems running `minio` on the command line, `MINIO_OPTS` is optional.
To use it, declare the environment variable using standard shell semantics, then reference the environment variable when starting up the MinIO Server:

```
export MINIO_OPTS=' --console-address=":9001" --ftp="address=:8021" --ftp="passive-port-range=30000-40000" '

minio server $MINIO_OPTS ...

# The above is equivalent to running the following:
# minio server --console-address=":9001" \
#              --ftp="address=:8021"     \
#              --ftp="passive-port-range=30000-40000"
```

Important

The `minio server` command does not read `$MINIO_OPTS` directly.
The variable only functions if used as described above.

## Storage Volumes

Environment Variable

MINIO_VOLUMES[](#envvar.MINIO_VOLUMES)

The directories or drives the [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process uses as the storage backend.

Functionally equivalent to setting [[reference/minio-server/minio-server#minio.server.DIRECTORIES|`minio server DIRECTORIES`]].
Use this value when configuring MinIO to run using an environment file.

Configuration Setting

This setting does not have a configuration setting option.

## Environment Variable File Path

Environment Variable

MINIO_CONFIG_ENV_FILE[](#envvar.MINIO_CONFIG_ENV_FILE)

Specifies the full path to the file the MinIO server process uses for loading environment variables.

For `systemd`-managed files, set this value to the path of the environment file (`/etc/default/minio`) to direct MinIO to reload changes to that file when using [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] to restart the deployment.

Configuration Setting

This setting does not have a configuration setting option.

## Workers for Expiration

Environment Variable

MINIO_ILM_EXPIRY_WORKERS[](#envvar.MINIO_ILM_EXPIRY_WORKERS)

Specifies the number of workers to make available to expire objects configured with ILM rules for expiration.
When not set, MinIO defaults to using up to half of the available processing cores available.

Configuration Setting

This setting does not have a configuration setting option.

## Domain

Environment Variable

MINIO_DOMAIN[](#envvar.MINIO_DOMAIN)

Enables Virtual Host-style requests to the MinIO deployment.
Set the value to the Fully Qualified Domain Name (FQDN) for MinIO to accept incoming virtual host requests.

Omitting this setting directs MinIO to only accept the default path-style requests.

For example, consider a MinIO deployment with an assigned FQDN of `minio.example.net`.

With path-style lookups, applications can access the bucket using its full path as `minio.example.net/mybucket`.

With virtual-host lookups, application can access the bucket as a virtual host as `mybucket.minio.example.net/`.

Important

If you configure `MINIO_DOMAIN`, you must consider all subdomains of the specified FQDN as exclusively assigned for use as bucket names.
Any MinIO services which conflict with those domains, such as replication targets, may exhibit unexpected or undesired behavior as a result of the collision.

For example, if setting `MINIO_DOMAIN=minio.example.net`, you cannot assign any subdomains of `minio.example.net` (in the form of `*.minio.example.net`) to any MinIO service or target.
This includes hostnames for use with [[administration/bucket-replication#minio-bucket-replication|bucket]], [[administration/batch-framework-job-replicate#minio-batch-framework-replicate-job|batch]], or [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]].

Configuration Setting

This setting does not have a configuration setting option.

## Scanner Speed

Environment Variable

MINIO_SCANNER_SPEED[](#envvar.MINIO_SCANNER_SPEED)

Configuration Setting

scanner speed[](#mc-conf.scanner.speed)

Manage the maximum wait period for the [[operations/concepts/scanner#minio-concepts-scanner|scanner]] when balancing MinIO read/write performance to scanner processes.

MinIO utilizes the [[operations/concepts/scanner#minio-concepts-scanner|scanner]] for [[administration/bucket-replication#minio-bucket-replication|bucket replication]], [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]], [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|lifecycle management]], and [[operations/concepts/healing#minio-concepts-healing|healing]] tasks.

Valid values include:

`fastest`

Removes scanner wait on read/write latency, allowing the scanner to operate at full-speed and IOPS consumption.
This setting may result in reduced read and write performance.

`fast`

Sets a short scanner wait time on read/write latency, allowing the scanner to operate at a higher speed and IOPS consumption.
This setting may result in reduced read and write performance.

`default`

Sets a moderate scanner wait time on read/write latency, allowing the scanner to operate at a balanced speed and IOPS consumption.
This setting seeks to maintain read and write performance while allowing ongoing scanner activity.

`slow`

Sets a medium scanner wait time on read/write latency, where the scanner operates at a reduced speed and IOPS consumption.
This setting allows better read and write performance while reducing scanner performance.

May impact scanner-dependent features, such as lifecycle management and replication.

`slowest`

Sets a large scanner wait time on read/write latency, where the scanner operates at a substantially lower speed and IOPS consumption.
This setting prioritizes read and write operations at the potential cost of scanner operations.

May impact scanner-dependent features, such as lifecycle management and replication.

## Batch Replication

Configuration Setting

This setting does not have a configuration setting option.

## Data Compression

The following section documents settings for enabling data compression for objects.
See [[administration/object-management/data-compression#minio-data-compression|Data Compression]] for tutorials on using these configuration settings.

All of the settings in this section fall under the following top-level key:

compression[](#mc-conf.compression)

### Enable Compression

Environment Variable

MINIO_COMPRESSION_ENABLE[](#envvar.MINIO_COMPRESSION_ENABLE)

Configuration Setting

compression enable[](#mc-conf.compression.enable)

Optional

Set to `on` to enable data compression for new objects.
Defaults to `off`.

Enabling or disabling data compression does not change existing objects.

### Allow Encryption

Environment Variable

MINIO_COMPRESSION_ALLOW_ENCRYPTION[](#envvar.MINIO_COMPRESSION_ALLOW_ENCRYPTION)

Configuration Setting

compression allow_encryption[](#mc-conf.compression.allow_encryption)

Optional

Set to `on` to encrypt objects after compressing them.
Defaults to `off`.

Encrypting compressed objects may compromise security

MinIO strongly recommends against encrypting compressed objects.
If you require encryption, carefully evaluate the risk of potentially leaking information about the contents of encrypted objects.

### Compression Extensions

Environment Variable

MINIO_COMPRESSION_EXTENSIONS[](#envvar.MINIO_COMPRESSION_EXTENSIONS)

Configuration Setting

compression extensions[](#mc-conf.compression.extensions)

Optional

Comma-separated list of the file extensions to compress.
Setting a new list of file extensions replaces the previously configured list.
Defaults to `".txt, .log, .csv, .json, .tar, .xml, .bin"`.

Changed in version RELEASE.2024-03-15T01-07-19Z: Specify `"*"` to direct MinIO to compress all supported file types.

MinIO does not support compressing file types on the [[administration/object-management/data-compression#minio-data-compression-excluded-types|Excluded File Types]] list, even if explicitly specified in this argument.

### Compression MIME Types

Environment Variable

MINIO_COMPRESSION_MIME_TYPES[](#envvar.MINIO_COMPRESSION_MIME_TYPES)

Configuration Variable

compression mime_types[](#mc-conf.compression.mime_types)

Optional

Comma-separated list of the MIME types to compress.
Setting a new list of types replaces the previously configured list.
Defaults to `"text/*, application/json, application/xml, binary/octet-stream"`.

Default excluded files

Some types of files cannot be significantly reduced in size.
MinIO will not compress these, even if specified in an [`mime_types`](#mc-conf.compression.mime_types) argument.
See [[administration/object-management/data-compression#minio-data-compression-excluded-types|Excluded types]] for details.

### Comments

Environment Variable

This setting does not have an environment variable option.
Use the configuration setting instead.

Configuration Setting

compression comment[](#envvar.compression.comment)

Optional

Specify a comment to associate with the data compression configuration.

## Erasure Stripe Size

Environment Variable

MINIO_ERASURE_SET_DRIVE_COUNT[](#envvar.MINIO_ERASURE_SET_DRIVE_COUNT)

Configuration Variable

This setting does not have a configuration setting option.

Optional

The [[operations/concepts/erasure-coding#minio-ec-basics|erasure set size]] to apply for all drives in a given [[glossary#term-server-pool|server pool]].

If you set this value, you must do so before you initialize the cluster
The selected stripe size is immutable after the cluster has been initialized and affects any future server pools added to the cluster.

[MinIO SUBNET](https://min.io/pricing?jmp=docs) users should log in and open an issue to discuss stripe size settings prior to implementing them in any environment.

Warning

Do not change the stripe size setting unless directed to by MinIO engineering.

Changes to stripe size have significant impact to deployment functionality, availability, performance, and behavior.
MinIO’s stripe selection algorithms set appropriate defaults for the majority of workloads.
Changing the stripe size from this default is unusual and generally not necessary or advised.

## Maximum Object Versions

Environment Variable

MINIO_API_OBJECT_MAX_VERSIONS[](#envvar.MINIO_API_OBJECT_MAX_VERSIONS)

Configuration Setting

api object_max_versions[](#mc-conf.api.object_max_versions)

Optional

Defines the default maximum versions to allow per object.

By default, MinIO allows up to the maximum value of an Int64 versions per object, or over 9.2 quintillion.

Note

MinIO versions from `RELEASE.2023-08-04T17-40-21Z``to ``RELEASE.2024-03-26T22-10-45Z` had a default limit of 10,000 object versions.
This setting can be used to override that limit to another value.

Arbitrarily high versions per objects may cause performance degradation on some operations, such as `LIST`.
This is especially true on systems running budget hardware or spinning drives (HDD).
Applications or workloads which produce thousands or more versions per object may require design or architecture review to mitigate potential performance degradations.

Setting a limit of no more than `100` should provide enough versions for most typical use cases.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).