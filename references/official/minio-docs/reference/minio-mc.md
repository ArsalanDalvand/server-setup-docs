---
title: "MinIO Client — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Client — SILO (MinIO Object Storage)"
---
# MinIO Client

[Introduction to the MinIO Client (MC) Commands](https://www.youtube.com/watch?v=pukQgDdXfqA)

[Installing and Running MinIO on Linux](https://www.youtube.com/watch?v=74usXkZpNt8&list=PLFOIsHSSYIK1BnzVY66pCL-iJ30Ht9t1o)

The MinIO Client [`mc`](#command-mc) command line tool provides a modern alternative
to UNIX commands like `ls`, `cat`, `cp`, `mirror`, and `diff` with
support for both filesystems and Amazon S3-compatible cloud storage services.

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

[`mc`](#command-mc) has the following syntax:

```
mc [GLOBALFLAGS] COMMAND --help
```

See [Command Quick Reference](#minio-mc-commands) for a list of supported commands.

## Version Alignment with MinIO Server

The MinIO Client releases separately from the MinIO Server.

For best functionality and compatibility, use a MinIO Client version released closely to your MinIO Server version.
For example, a MinIO Client released the same day or later than your MinIO Server version.

You can install a version of the MinIO Client that is more recent than the MinIO Server version.
However, if the MinIO Client version skews too far from the MinIO Server version, you may see increased warnings or errors as a result of the differences.
For example, while core S3 APIs around copying ([[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]]) may remain unchanged, some features or flags may only be available or stable if the client and server versions are aligned.

## Quickstart

### 1) Install mc

Install the mc command line tool onto the host machine. Click
the tab that corresponds to the host machine operating system or environment:

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

### 2) Create an Alias for the S3-Compatible Service

Important

The following example temporarily disables the bash history to mitigate the
risk of authentication credentials leaking in plain text. This is a basic
security measure and does not mitigate all possible attack vectors. Defer to
security best practices for your operating system for inputting sensitive
information on the command line.

Use the [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command to add an Amazon S3-compatible service
to the [`mc`](#command-mc) [configuration](#mc-configuration).

```
bash +o history
mc alias set ALIAS HOSTNAME ACCESS_KEY SECRET_KEY
bash -o history
```

Replace `ALIAS` with a name to associate to the S3 service.
[`mc`](#command-mc) commands typically require `ALIAS` as an argument for
identifying which S3 service to execute against.

Replace `HOSTNAME` with the URL endpoint or IP address of the S3 service.

Replace `ACCESS_KEY` and `SECRET_KEY` with the access and secret
keys for a user on the S3 service.

Replace each argument with the required values.
If you omit the `ACCESS_KEY` and `SECRET_KEY`, the command prompts you to enter those values in the CLI.

Each of the following tabs contains a provider-specific example:

MinIO Server

```
mc alias set myminio https://minioserver.example.net ACCESS_KEY SECRET_KEY
```

AWS S3 Storage

```
mc alias set myS3 https://s3.{your-region-code}.amazonaws.com/endpoint ACCESS_KEY SECRET_KEY
```

Google Cloud Storage

```
mc alias set myGCS https://storage.googleapis.com/endpoint ACCESS_KEY SECRET_KEY
```

### 3) Test the Connection

Use the [[reference/minio-mc-admin/mc-admin-info#command-mc.admin.info|`mc admin info`]] command to test the connection to
the newly added MinIO deployment:

```
mc admin info myminio
```

The command returns information on the S3 service if successful. If
unsuccessful, check each of the following:

The host machine has connectivity to the S3 service URL (i.e. using `ping`
or `traceroute`).

The specified `ACCESSKEY` and `SECRETKEY` correspond to a user on the
S3 service. The user must have permission to perform actions on the
service.

For MinIO deployments, see [[administration/identity-access-management#minio-access-management|Access Management]]
for more information on user access permissions. For other S3-compatible
services, defer to the documentation for that service.

## Command Quick Reference

The following table lists [`mc`](#command-mc) commands:

Note

The MinIO Client also includes an administration extension for managing MinIO deployments.
See [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] for more complete documentation.

The below table does not include those commands.

Command

Description

[[reference/minio-mc/mc-alias-list#command-mc.alias.list|`mc alias list`]]
[[reference/minio-mc/mc-alias-remove#command-mc.alias.remove|`mc alias remove`]]
[[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]]
[[reference/minio-mc/mc-alias-import#command-mc.alias.import|`mc alias import`]]
[[reference/minio-mc/mc-alias-export#command-mc.alias.export|`mc alias export`]]

The [[reference/minio-mc/mc-alias#command-mc.alias|`mc alias`]] commands provide a convenient interface for managing the list of S3-compatible hosts that [`mc`](#command-mc) can connect to and run operations against.

[[reference/minio-mc/mc-anonymous-get#command-mc.anonymous.get|`mc anonymous get`]]
[[reference/minio-mc/mc-anonymous-get-json#command-mc.anonymous.get-json|`mc anonymous get-json`]]
[[reference/minio-mc/mc-anonymous-links#command-mc.anonymous.links|`mc anonymous links`]]
[[reference/minio-mc/mc-anonymous-list#command-mc.anonymous.list|`mc anonymous list`]]
[[reference/minio-mc/mc-anonymous-set#command-mc.anonymous.set|`mc anonymous set`]]
[[reference/minio-mc/mc-anonymous-set-json#command-mc.anonymous.set-json|`mc anonymous set-json`]]

The [[reference/minio-mc/mc-anonymous#command-mc.anonymous|`mc anonymous`]] command supports setting or removing anonymous [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to a bucket and its contents.
Buckets with anonymous policies allow public access where clients can perform any action granted by the policy without [[administration/identity-access-management#minio-authentication-and-identity-management|authentication]].

[[reference/minio-mc/mc-batch-describe#command-mc.batch.describe|`mc batch describe`]]
[[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]]
[[reference/minio-mc/mc-batch-list#command-mc.batch.list|`mc batch list`]]
[[reference/minio-mc/mc-batch-start#command-mc.batch.start|`mc batch start`]]
[[reference/minio-mc/mc-batch-status#command-mc.batch.status|`mc batch status`]]

The [[reference/minio-mc/mc-batch#command-mc.batch|`mc batch`]] commands allow you to run one or more job tasks on a MinIO deployment.

[[reference/minio-mc/mc-cat#command-mc.cat|`mc cat`]]

The [[reference/minio-mc/mc-cat#command-mc.cat|`mc cat`]] command concatenates the contents of a file or
object to another file or object. You can also use the command to
display the contents of the specified file or object to `STDOUT`.
[[reference/minio-mc/mc-cat#command-mc.cat|`cat`]] has similar functionality to `cat`.

[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]]

The [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command copies objects to or from a MinIO deployment, where
the source can MinIO or a local filesystem.

[[reference/minio-mc/mc-diff#command-mc.diff|`mc diff`]]

The [[reference/minio-mc/mc-diff#command-mc.diff|`mc diff`]] mc computes the differences between two filesystem directories
or MinIO buckets. [[reference/minio-mc/mc-diff#command-mc.diff|`mc diff`]] lists only those objects which are missing or
which differ in size. [[reference/minio-mc/mc-diff#command-mc.diff|`mc diff`]] does not compare the contents of
objects.

[[reference/minio-mc/mc-du#command-mc.du|`mc du`]]

The [[reference/minio-mc/mc-du#command-mc.du|`mc du`]] command summarizes the disk usage of buckets and folders.
You can also use [[reference/minio-mc/mc-du#command-mc.du|`du`]] against the local filesystem to produce similar results as the `du` command.

[[reference/minio-mc/mc-encrypt-clear#command-mc.encrypt.clear|`mc encrypt clear`]]
[[reference/minio-mc/mc-encrypt-info#command-mc.encrypt.info|`mc encrypt info`]]
[[reference/minio-mc/mc-encrypt-set#command-mc.encrypt.set|`mc encrypt set`]]

The [[reference/minio-mc/mc-encrypt#command-mc.encrypt|`mc encrypt`]] commands set, update, or disable the default bucket Server-Side Encryption (SSE) mode.
MinIO automatically encrypts objects using the specified SSE mode.

[[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]]
[[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]]
[[reference/minio-mc/mc-event-remove#command-mc.event.rm|`mc event rm`]]

The [[reference/minio-mc/mc-event#command-mc.event|`mc event`]] command supports adding, removing, and listing bucket event notifications.

[[reference/minio-mc/mc-find#command-mc.find|`mc find`]]

The [[reference/minio-mc/mc-find#command-mc.find|`mc find`]] command supports searching for objects on a MinIO deployment.
You can also use the command to search for files on a filesystem.

[[reference/minio-mc/mc-get#command-mc.get|`mc get`]]

The [[reference/minio-mc/mc-get#command-mc.get|`mc get`]] command downloads an object from a target S3 deployment to the local file system.

[[reference/minio-mc/mc-head#command-mc.head|`mc head`]]

The [[reference/minio-mc/mc-head#command-mc.head|`mc head`]] command displays the first `n` lines of an object,
where `n` is an argument specified to the command.

[[reference/minio-mc/mc-idp-ldap-accesskey#command-mc.idp.ldap.accesskey|`mc idp ldap accesskey`]]
[[reference/minio-mc/mc-idp-ldap-accesskey-create-with-login#command-mc.idp.ldap.accesskey.create-with-login|`mc idp ldap accesskey create-with-login`]]
[[reference/minio-mc/mc-idp-ldap-add#command-mc.idp.ldap.add|`mc idp ldap add`]]
[[reference/minio-mc/mc-idp-ldap-disable#command-mc.idp.ldap.disable|`mc idp ldap disable`]]
[[reference/minio-mc/mc-idp-ldap-enable#command-mc.idp.ldap.enable|`mc idp ldap enable`]]
[[reference/minio-mc/mc-idp-ldap-info#command-mc.idp.ldap.info|`mc idp ldap info`]]
[[reference/minio-mc/mc-idp-ldap-ls#command-mc.idp.ldap.ls|`mc idp ldap ls`]]
[[reference/minio-mc/mc-idp-ldap-policy#command-mc.idp.ldap.policy|`mc idp ldap policy`]]
[[reference/minio-mc/mc-idp-ldap-rm#command-mc.idp.ldap.rm|`mc idp ldap rm`]]
[[reference/minio-mc/mc-idp-ldap-update#command-mc.idp.ldap.update|`mc idp ldap update`]]

The [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] commands allow you to manage configurations to 3rd party [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory or LDAP Identity and Access Management (IAM) integrations]].

[[reference/minio-mc/mc-idp-openid#mc.idp.openid.add|`mc idp openid add`]]
[[reference/minio-mc/mc-idp-openid#mc.idp.openid.disable|`mc idp openid disable`]]
[[reference/minio-mc/mc-idp-openid#mc.idp.openid.enable|`mc idp openid enable`]]
[[reference/minio-mc/mc-idp-openid#mc.idp.openid.info|`mc idp openid info`]]
[[reference/minio-mc/mc-idp-openid#mc.idp.openid.ls|`mc idp openid ls`]]
[[reference/minio-mc/mc-idp-openid#mc.idp.openid.rm|`mc idp openid rm`]]
[[reference/minio-mc/mc-idp-openid#mc.idp.openid.update|`mc idp openid update`]]

The [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`mc idp openid`]] commands allow you to manage configurations to 3rd party [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Identity and Access Management (IAM) integrations]].

[[reference/minio-mc/mc-idp-ldap-policy-attach#command-mc.idp.ldap.policy.attach|`mc idp ldap policy attach`]]
[[reference/minio-mc/mc-idp-ldap-policy-detach#command-mc.idp.ldap.policy.detach|`mc idp ldap policy detach`]]
[[reference/minio-mc/mc-idp-ldap-policy-entities#command-mc.idp.ldap.policy.entities|`mc idp ldap policy entities`]]

The [[reference/minio-mc/mc-idp-ldap-policy#command-mc.idp.ldap.policy|`mc idp ldap policy`]] commands show the mapping relationships between policies and the associated groups or users.

[[reference/minio-mc/mc-ilm-restore#command-mc.ilm.restore|`mc ilm restore`]]
[[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`mc ilm rule add`]]
[[reference/minio-mc/mc-ilm-rule-edit#command-mc.ilm.rule.edit|`mc ilm rule edit`]]
[[reference/minio-mc/mc-ilm-rule-export#command-mc.ilm.rule.export|`mc ilm rule export`]]
[[reference/minio-mc/mc-ilm-rule-import#command-mc.ilm.rule.import|`mc ilm rule import`]]
[[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]]
[[reference/minio-mc/mc-ilm-rule-rm#command-mc.ilm.rule.rm|`mc ilm rule rm`]]
[[reference/minio-mc/mc-ilm-tier-add#command-mc.ilm.tier.add|`mc ilm tier add`]]
[[reference/minio-mc/mc-ilm-tier-check#command-mc.ilm.tier.check|`mc ilm tier check`]]
[[reference/minio-mc/mc-ilm-tier-info#command-mc.ilm.tier.info|`mc ilm tier info`]]
[[reference/minio-mc/mc-ilm-tier-ls#command-mc.ilm.tier.ls|`mc ilm tier ls`]]
[[reference/minio-mc/mc-ilm-tier-rm#command-mc.ilm.tier.rm|`mc ilm tier rm`]]
[[reference/minio-mc/mc-ilm-tier-update#command-mc.ilm.tier.update|`mc ilm tier update`]]

The [[reference/minio-mc/mc-ilm#command-mc.ilm|`mc ilm`]] commands manage [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|object lifecycle management rules]] and tiering on a MinIO deployment.

[[reference/minio-mc/mc-legalhold-clear#command-mc.legalhold.clear|`mc legalhold clear`]]
[[reference/minio-mc/mc-legalhold-info#command-mc.legalhold.info|`mc legalhold info`]]
[[reference/minio-mc/mc-legalhold-set#command-mc.legalhold.set|`mc legalhold set`]]

The [[reference/minio-mc/mc-legalhold#command-mc.legalhold|`mc legalhold`]] command sets, removes, or retrieves the [[administration/object-management/object-retention#minio-object-locking-legalhold|object legal hold (WORM)]] settings for object(s).

[[reference/minio-mc/mc-license-info#command-mc.license.info|`mc license info`]]
[[reference/minio-mc/mc-license-register#command-mc.license.register|`mc license register`]]
[[reference/minio-mc/mc-license-update#command-mc.license.update|`mc license update`]]

The [[reference/minio-mc/mc-license#command-mc.license|`mc license`]] commands work with cluster registration for [MinIO SUBNET](https://min.io/pricing?jmp=docs).
Use the commands to register a deployment, display information about the cluster’s current license, or update the license key for a cluster.

[[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]]

The [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] command lists buckets and objects on MinIO or another
S3-compatible service.

[[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]]

The [[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]] command creates a new bucket or directory at the
specified path.

[[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]]

The [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]] command synchronizes content to MinIO deployment, similar to the `rsync` utility.
[[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]] supports filesystems, MinIO deployments, and other S3-compatible hosts as the synchronization source.

[[reference/minio-mc/mc-mv#command-mc.mv|`mc mv`]]

The [[reference/minio-mc/mc-mv#command-mc.mv|`mc mv`]] command moves an object from source to the target, such as
between MinIO deployments or between buckets on the same MinIO deployment.
[[reference/minio-mc/mc-mv#command-mc.mv|`mc mv`]] also supports moving objects between a local filesystem and MinIO.

[[reference/minio-mc/mc-od#command-mc.od|`mc od`]]

The [[reference/minio-mc/mc-od#command-mc.od|`mc od`]] command copies a local file to a remote location in a specified number of parts and part sizes.
The command outputs the time it took to upload the file.

[[reference/minio-mc/mc-ping#command-mc.ping|`mc ping`]]

The [[reference/minio-mc/mc-ping#command-mc.ping|`mc ping`]] command performs a liveness check on a specified target.

[[reference/minio-mc/mc-pipe#command-mc.pipe|`mc pipe`]]

The [[reference/minio-mc/mc-pipe#command-mc.pipe|`mc pipe`]] command streams content from [STDIN](https://www.gnu.org/software/libc/manual/html_node/Standard-Streams.html) to a target object.

[[reference/minio-mc/mc-put#command-mc.put|`mc put`]]

The [[reference/minio-mc/mc-put#command-mc.put|`mc put`]] uploads an object from the local file system to a bucket on a target S3 deployment.

[[reference/minio-mc/mc-rb#command-mc.rb|`mc rb`]]

The [[reference/minio-mc/mc-rb#command-mc.rb|`mc rb`]] command removes one or more buckets on MinIO or
another S3-compatible service.

To remove only the contents of a bucket, use [[reference/minio-mc/mc-rm#command-mc.rm|`mc rm`]] instead.

[[reference/minio-mc/mc-ready#command-mc.ready|`mc ready`]]

The [[reference/minio-mc/mc-ready#command-mc.ready|`mc ready`]] command checks the status of a cluster and whether the cluster has `read` and `write` quorum.

[[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]]
[[reference/minio-mc/mc-replicate-backlog#command-mc.replicate.backlog|`mc replicate backlog`]]
[[reference/minio-mc/mc-replicate-export#command-mc.replicate.export|`mc replicate export`]]
[[reference/minio-mc/mc-replicate-import#command-mc.replicate.import|`mc replicate import`]]
[[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]]
[[reference/minio-mc/mc-replicate-resync#command-mc.replicate.resync|`mc replicate resync`]]
[[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]]
[[reference/minio-mc/mc-replicate-status#command-mc.replicate.status|`mc replicate status`]]
[[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]]

The [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate`]] command configures and manages the [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side Bucket Replication]] for a MinIO deployment, including [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|active-active replication configurations]] and [[administration/bucket-replication#minio-replication-behavior-resync|resynchronization]].

[[reference/minio-mc/mc-retention-clear#command-mc.retention.clear|`mc retention clear`]]
[[reference/minio-mc/mc-retention-info#command-mc.retention.info|`mc retention info`]]
[[reference/minio-mc/mc-retention-set#command-mc.retention.set|`mc retention set`]]

The [[reference/minio-mc/mc-retention#command-mc.retention|`mc retention`]] command configures the [[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM) locking]] settings for an object or object(s) in a bucket.
You can also set the default object lock settings for a bucket, where all objects without explicit object lock settings inherit the bucket default.

[[reference/minio-mc/mc-rm#command-mc.rm|`mc rm`]]

The [[reference/minio-mc/mc-rm#command-mc.rm|`mc rm`]] command [[administration/object-management/object-delete#minio-object-delete|removes objects]] from a bucket on a MinIO deployment.
To completely remove a bucket, use [[reference/minio-mc/mc-rb#command-mc.rb|`mc rb`]] instead.

[[reference/minio-mc/mc-share-download#command-mc.share.download|`mc share download`]]
[[reference/minio-mc/mc-share-list#command-mc.share.ls|`mc share ls`]]
[[reference/minio-mc/mc-share-upload#command-mc.share.upload|`mc share upload`]]

Use the [[reference/minio-mc/mc-share#command-mc.share|`mc share`]] commands to manage presigned URLs for downloading and uploading objects to a MinIO bucket.

[[reference/minio-mc/mc-sql#command-mc.sql|`mc sql`]]

The [[reference/minio-mc/mc-sql#command-mc.sql|`mc sql`]] command provides an S3 Select interface for performing sql queries on objects in the specified MinIO deployment.

[[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]]

The [[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]] command displays information on objects in a MinIO bucket, including object metadata.
You can also use it to retrieve bucket metadata.

[[reference/minio-mc/mc-support-callhome#command-mc.support.callhome|`mc support callhome`]]
[[reference/minio-mc/mc-support-diag#command-mc.support.diag|`mc support diag`]]
[[reference/minio-mc/mc-support-inspect#command-mc.support.inspect|`mc support inspect`]]
[[reference/minio-mc/mc-support-perf#command-mc.support.perf|`mc support perf`]]
[[reference/minio-mc/mc-support-profile#command-mc.support.profile|`mc support profile`]]
[[reference/minio-mc/mc-support-proxy#command-mc.support.proxy|`mc support proxy`]]
[[reference/minio-mc/mc-support-top-api#command-mc.support.top.api|`mc support top api`]]
[[reference/minio-mc/mc-support-top-disk#command-mc.support.top.disk|`mc support top disk`]]
[[reference/minio-mc/mc-support-top-locks#command-mc.support.top.locks|`mc support top locks`]]
[[reference/minio-mc/mc-support-upload#command-mc.support.upload|`mc support upload`]]

The MinIO Client [[reference/minio-mc/mc-support#command-mc.support|`mc support`]] commands provides tools for analyzing deployment health or performance and for running diagnostics.
You can also upload generated health reports for further analysis by MinIO engineering.

[[reference/minio-mc/mc-tag-list#command-mc.tag.list|`mc tag list`]]
[[reference/minio-mc/mc-tag-remove#command-mc.tag.remove|`mc tag remove`]]
[[reference/minio-mc/mc-tag-set#command-mc.tag.set|`mc tag set`]]

The [[reference/minio-mc/mc-tag#command-mc.tag|`mc tag`]] command adds, removes, and lists tags associated to a bucket or object.

[[reference/minio-mc/mc-tree#command-mc.tree|`mc tree`]]

The [[reference/minio-mc/mc-tree#command-mc.tree|`mc tree`]] command lists all prefixes inside a MinIO bucket in a tree
format. The command optionally supports listing all objects inside of bucket
at each prefix, including the bucket root.

[[reference/minio-mc/mc-undo#command-mc.undo|`mc undo`]]

The [[reference/minio-mc/mc-undo#command-mc.undo|`mc undo`]] command reverses changes due to either a `PUT` or `DELETE` operation at a specified path.

[[reference/minio-mc/mc-update#command-mc.update|`mc update`]]

The [[reference/minio-mc/mc-update#command-mc.update|`mc update`]] command automatically updates the mc binary to
the latest stable version.

[[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]]
[[reference/minio-mc/mc-version-info#command-mc.version.info|`mc version info`]]
[[reference/minio-mc/mc-version-suspend#command-mc.version.suspend|`mc version suspend`]]

The [[reference/minio-mc/mc-version#command-mc.version|`mc version`]] commands enable, disable, and retrieve the [[administration/object-management/object-versioning#minio-bucket-versioning|versioning]] status for a MinIO bucket.

[[reference/minio-mc/mc-watch#command-mc.watch|`mc watch`]]

The [[reference/minio-mc/mc-watch#command-mc.watch|`mc watch`]] command watches for events on the specified MinIO bucket or
local filesystem path. For S3 services, use [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] to configure
bucket event notifications on S3-compatible services.

## Configuration File

[`mc`](#command-mc) uses a `JSON` formatted configuration file used for storing
certain kinds of information, such as the [[reference/minio-mc/mc-alias#command-mc.alias|`aliases`]] for
each configured S3-compatible service.

For Linux and macOS, the default configuration file location is
`~/.mc/config.json`.

For Windows, [`mc`](#command-mc) attempts to construct a default file path by trying
specific environment variables. If a variable is unset, [`mc`](#command-mc) moves
to the next variable. If all attempts fail, [`mc`](#command-mc) returns an error.
The following list describes each possible file path location in the order
[`mc`](#command-mc) checks them:

`HOME\.mc\config.json`

`USERPROFILE\.mc\config.json`

`HOMEDRIVE+HOMEPATH\.mc\config.json`

You can use the `--config-dir`

## Certificates

The MinIO Client stores certificates and CAs for deployments to the following paths:

Linux, macOS, and other Unix-like systems:

```
~/.mc/certs/ # certificates
~/.mc/certs/CAs/ # Certificate Authorities
```

Windows systems:

```
C:\Users\[username]\mc\certs\ # certificates
C:\Users\[username]\mc\certs\CAs\ # Certificate Authorities
```

When creating a new [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]], the MinIO Client fetches the peer certificate, computes the public key fingerprint, and asks the user whether to accept the deployment’s certificate.
If you decide to trust the certificate, the MinIO Client adds the certificate to the certificate authority path listed above.

Note

In testing environments, you can bypass the certificate check for selected MinIO Client commands by passing the `--insecure` flag.

## Pattern Matching

Some commands and flags allow for pattern matching.
When enabled, a pattern can include either of these wildcards for character replacement:

`*` to represent a string of characters to match, either in the middle or end.

`?` to represent a single character.

For example, refer to the following examples for wildcard uses and their results.

Pattern

Text

Match Result

`abc*`

ab

Match

`abc*`

abd

Not a match

`abc*c`

abcd

Match

`ab*??d`

abxxc

Match

`ab*??d`

abxc

Match

`ab??d`

abxc

Match

`ab??d`

abc

Match

`ab??d`

abcxdd

Not a match

## Global Options

All [commands](#minio-mc-commands) support the following global options.
You can also define some of these options using [[reference/minio-mc/minio-client-settings#minio-server-envvar-mc|Environment Variables]].

--config-dir[](#cmdoption-mc-config-dir)

The path to a `JSON` formatted configuration file that
mc uses for storing data. See [Configuration File](#mc-configuration) for
more information on how mc uses the configuration file.

Alternatively, set the environment variable [[reference/minio-mc/minio-client-settings#envvar.MC_CONFIG_DIR|`MC_CONFIG_DIR`]].

--debug[](#cmdoption-mc-debug)

Enables verbose output to the console.

For example, the following operation adds verbose output to the
[[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] command:

```
mc --debug ls play
```

Alternatively, set the environment variable [[reference/minio-mc/minio-client-settings#envvar.MC_DEBUG|`MC_DEBUG`]].

--disable-pager, --dp[](#cmdoption-mc-disable-pager)

New in version mc: RELEASE.2024-04-29T09-56-05Z

Disable the pager functionality of the MinIO Client in the CLI.
When used, output prints to raw `STDOUT` instead.

--insecure[](#cmdoption-mc-insecure)

Disables TLS/SSL certificate verification. Allows TLS connectivity to
servers with invalid certificates. Exercise caution when using this
option against untrusted S3 hosts.

Alternatively, set the environment variable [[reference/minio-mc/minio-client-settings#envvar.MC_INSECURE|`MC_INSECURE`]].

--json[](#cmdoption-mc-json)

Enables [JSON lines](http://jsonlines.org/) formatted output to the
console.

For example, the following operation adds JSON Lines output to the
[[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] command:

```
mc --json ls play
```

Alternatively, set the environment variable [[reference/minio-mc/minio-client-settings#envvar.MC_JSON|`MC_JSON`]].

--no-color[](#cmdoption-mc-no-color)

Disables the built-in color theme for console output. Useful for dumb
terminals.

Alternatively, set the environment variable [[reference/minio-mc/minio-client-settings#envvar.MC_NO_COLOR|`MC_NO_COLOR`]].

--quiet[](#cmdoption-mc-quiet)

Suppresses console output.

Alternatively, set the environment variable [[reference/minio-mc/minio-client-settings#envvar.MC_QUIET|`MC_QUIET`]].

--resolve[](#cmdoption-mc-resolve)

New in version mc: RELEASE.2024-08-13T05-33-17Z

Creates a custom DNS mapping to resolve a HOST to a specified IP address.

Use the following syntax:

```
--resolve HOST[:PORT]=IP
```

For example:

```
mc alias set --resolve myminio.example.com:9000=192.168.188.118 'myminio' 'https://myminio.example.com:9000' 'miniouser' 'miniosecret'
```

Repeat the flag multiple times to add additional custom DNS mappings.

--version[](#cmdoption-mc-version)

Displays the current version of [`mc`](#command-mc).

--help[](#mc.-help)
Optional

Displays a summary of command usage on the terminal.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).