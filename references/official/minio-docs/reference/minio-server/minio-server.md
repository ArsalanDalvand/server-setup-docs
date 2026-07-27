---
title: "MinIO Server — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/minio-server.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/minio-server"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Server — SILO (MinIO Object Storage)"
---
# MinIO Server

## MinIO Server

The [`minio server`](#command-minio.server) command starts the MinIO server process:

```
minio server /mnt/disk{1...4}
```

For examples of deploying [`minio server`](#command-minio.server) on a bare metal environment, see [[operations/deployments/installation#minio-installation|Installation and Management]].

For examples of deploying [`minio server`](#command-minio.server) on a Kubernetes environment, see [[operations/deployments/k8s-deploy-minio-tenant-on-kubernetes#minio-k8s-deploy-minio-tenant|Deploying a MinIO Tenant]].

### Syntax

Starts the `minio` server process.

The command has the following syntax:

```
minio server [FLAGS] HOSTNAME/DIRECTORIES [HOSTNAME/DIRECTORIES..]
```

The command accepts the following arguments:

HOSTNAME[](#minio.server.HOSTNAME)

The hostname of a [`minio server`](#command-minio.server) process.

For standalone deployments, this field is optional.
You can start a standalone [`server`](#command-minio.server) process with only the [`DIRECTORIES`](#minio.server.DIRECTORIES) argument.

For distributed deployments, specify the hostname of each [`minio server`](#command-minio.server) in the deployment.
The group of [`minio server`](#command-minio.server) processes represent a single [[operations/concepts#minio-intro-server-pool|Server Pool]].

[`HOSTNAME`](#minio.server.HOSTNAME) supports MinIO expansion notation `{x...y}` to denote a sequential series of hostnames.
MinIO requires sequential hostnames to identify each [`minio server`](#command-minio.server) process in the set.

For example, `https://minio{1...4}.example.net` expands to:

`https://minio1.example.net`

`https://minio2.example.net`

`https://minio3.example.net`

`https://minio4.example.net`

You must run the [`minio server`](#command-minio.server) command with the same combination of [`HOSTNAME`](#minio.server.HOSTNAME) and [`DIRECTORIES`](#minio.server.DIRECTORIES) on   each host in the Server Pool.

Each additional `HOSTNAME/DIRECTORIES` pair denotes an additional Server Set for the purpose of horizontal expansion of the MinIO deployment.
For more information on Server Pools, see [[operations/concepts#minio-intro-server-pool|Server Pool]].

DIRECTORIES[](#minio.server.DIRECTORIES)
Required

The directories or drives the [`minio server`](#command-minio.server) process uses as the storage backend.

[`DIRECTORIES`](#minio.server.DIRECTORIES) supports MinIO expansion notation `{x...y}` to denote a sequential series of folders or drives.
For example, `/mnt/disk{1...4}` expands to:

`/mnt/disk1`

`/mnt/disk2`

`/mnt/disk3`

`/mnt/disk4`

The [`DIRECTORIES`](#minio.server.DIRECTORIES) path(s) must be empty when first starting the [`minio`](#command-minio.server) process.

The [`minio server`](#command-minio.server) process requires at least 4 drives or directories to enable [[operations/concepts/erasure-coding#minio-erasure-coding|erasure coding]].

Important

MinIO recommends locally-attached drives, where the [`DIRECTORIES`](#minio.server.DIRECTORIES) path points to each drive on the host machine.
MinIO recommends against using network-attached storage, as network latency reduces performance of those drives compared to locally-attached storage.

For development or evaluation, you can specify multiple logical directories or partitions on a single physical volume to enable erasure coding on the deployment.

For production environments, MinIO does not recommend using multiple logical directories or partitions on a single physical disk.
While MinIO supports those configurations, the potential cost savings come at the risk of decreased reliability.

--address[](#minio.server.-address)
Optional

Binds the [`minio`](#command-minio.server) server process to a specific network address and port number.
Specify the address and port as `ADDRESS:PORT`, where `ADDRESS` is an IP address or hostname and `PORT` is a valid and open port on the host system.
MinIO supports both IPv4 and IPv6 addressing, provided that the specified addresses are routable and resolveable.

To change the port number for all IP addresses or hostnames configured on the host machine, specify only `:PORT` where `PORT` is a valid and open port on the host.

Changed in version RELEASE.2023-01-02T09-40-09Z: You can configure your hosts file to have MinIO only listen on specific IPs.
For example, if the machine’s /etc/hosts file contains the following:

```
127.0.1.1       minioip
127.0.1.2       minioip
```

A command like the following would listen for API calls on port `9000` on both configured IP addresses.

```
minio server --address "minioip:9000" ~/miniodirectory
```

If omitted, [`minio`](#command-minio.server) binds to port `9000` on all configured IPv4 addresses, IPv6 addresses, and hostnames on the host machine.

--console-address[](#minio.server.-console-address)
Optional

Specifies a static port for the embedded MinIO Console.

Omit to direct MinIO to generate a dynamic port at server startup.
The MinIO server outputs the port to the system log.

--ftp[](#minio.server.-ftp)
Optional

Enable and configure a File Transfer Protocol (`FTP`) or File Transfer Protocol over SSL/TLS (`FTPS`) server.
Use this flag multiple times to specify an address port, a passive port range of addresses, or a TLS certificate and key as key-value pairs.

Valid keys:

`address`, which takes a single port to use for the server, typically `8021`

(Optional) `passive-port-range`, which restricts the range of potential ports the server can use to transfer data, such as when tight firewall rules limit the port the FTP server can request for the connection

(Optional) `tls-private-key`, which takes the path to the user’s private key for accessing the MinIO deployment by TLS

Use with `tls-public-cert`.

(Optional) `tls-public-cert`, which takes the path to the certificate for accessing the MinIO deployment by TLS

Use with `tls-private-key`.

For MinIO deployments with TLS enabled, omit `tls-private-key` and `tls-public-key` to direct MinIO to use the default TLS keys for the MinIO deployment.
See [[operations/network-encryption#minio-tls|Network Encryption (TLS)]] for more information.
You only need to specify a certificate and private key to a different set of TLS certificate and key than the MinIO default (for example, to use a different domain).

For example:

```
minio server http://server{1...4}/disk{1...4} \
--ftp="address=:8021"                         \
--ftp="passive-port-range=30000-40000"        \
--ftp="tls-private-key=path/to/private.key"   \
--ftp="tls-public-cert=path/to/public.crt"    \
...
```

--sftp[](#minio.server.-sftp)
Optional

Enable and configure a SSH File Transfer Protocol (`SFTP`) server.
Use multiple times to specify each desired key-value pair.

The following table lists valid keys.

Key

Description

Valid values

`address`

Port to use for connecting to SFTP.

Any valid port number, typically `8022`.

`ssh-private-key`

Path to the user’s private key file.

Absolute path or relative path from current location to the key file to use.

`trusted-user-ca-key`

Specifies a file containing public key of a certificate authority that is trusted to sign user certificates for authentication.
The file must contain a [user principals list](https://man.openbsd.org/ssh-keygen#CERTIFICATES), and the list must include the user(s) that can authenticate with the key.

Absolute path or relative path from current location to the user’s trusted certificate authority public key file.

`pub-key-algos`

Comma-separated list of the public key algorithms to support.

```
ssh-ed25519
[email protected]
[email protected]
ecdsa-sha2-nistp256
ecdsa-sha2-nistp384
ecdsa-sha2-nistp521
rsa-sha2-256
rsa-sha2-512
ssh-rsa
ssh-dss
```

`kex-algos`

Comma-separated list in priority order of the key-exchange algorithms to support.

```
curve25519-sha256
[email protected]
ecdh-sha2-nistp256
ecdh-sha2-nistp384
ecdh-sha2-nistp521
diffie-hellman-group14-sha256
diffie-hellman-group16-sha512
diffie-hellman-group14-sha1
diffie-hellman-group1-sha1
```

`cipher-algos`

Comma-separated list of cipher algorithms to support

```
aes128-ctr
aes192-ctr
aes256-ctr
[email protected]
[email protected]
[email protected]
arcfour256
arcfour128
arcfour
aes128-cbc
3des-cbc
```

`mac-algos`

Comma-separated list in preference order of MAC algorithms to support.
Based on [RFC 4253 section 6.4](https://www.rfc-editor.org/rfc/rfc4253) with the exception of `hmac-md5` variants, which are end of life.

```
[email protected]
[email protected]
hmac-sha2-256
hmac-sha2-512
hmac-sha1
hmac-sha1-96
```

`disable-password-auth`

Disable password authentication.

`true`

For example:

```
minio server http://server{1...4}/disk{1...4}                                 \
--sftp="address=:8022" --sftp="ssh-private-key=/home/miniouser/.ssh/id_rsa"   \
--sftp="kex-algos=diffie-hellman-group14-sha256,[email protected]" \
...
```

--certs-dir, -S[](#minio.server.-certs-dir)
Optional

Specifies the path to the folder containing certificates the [`minio`](#command-minio) process uses for configuring TLS/SSL connectivity.

The contents of the specified folder must follow that of the [[operations/network-encryption#minio-tls-user-generated|default path structure]].
For example, the path contents of `--certs-dir /etc/minio` should resemble the following:

```
/etc/minio
  private.key
  public.crt
  domain.tld/
    private.key
    public.crt
  CAs/
    full-chain-ca.crt
```

Omit to use the default directory paths:

Linux/macOS: `${HOME}/.minio/certs`

Windows: `%%USERPROFILE%%\.minio\certs`.

See [[operations/network-encryption#minio-tls|Network Encryption (TLS)]] for more information on TLS/SSL connectivity.

Important

[MinIO Server RELEASE.2023-12-09T18-17-51Z](https://github.com/minio/minio/releases/tag/RELEASE.2023-12-09T18-17-51Z) removes the deprecated `--config-dir | -C` parameter.
Deployments using this flag may start without TLS enabled.
Replace those parameters with `--certs-dir | -S` and restart to re-enable TLS.

--quiet[](#minio.server.-quiet)
Optional

Disables startup information.

--anonymous[](#minio.server.-anonymous)
Optional

Hides sensitive information from logging.

--json[](#minio.server.-json)
Optional

Outputs server logs and startup information in `JSON` format.

Note

You can define any of the `minio` parameters above by setting them in the [[reference/minio-server/settings/core#envvar.MINIO_OPTS|`MINIO_OPTS`]] environment variable.
This variable takes as its value a single string that contains any of the above parameters and their values that you want to set when starting the MinIO Server.

## Settings

You can perform other customizations to the MinIO Server process by defining additional [[reference/minio-server/settings#minio-server-configuration-options|Configuration Values]] or [[reference/minio-server/settings#minio-server-environment-variables|Environment Variables]].

Many configuration values and environment variables define the same value.
If you set both a configuration value and the matching environment variable, MinIO uses the value from the environment variable.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).