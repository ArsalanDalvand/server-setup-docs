---
title: "mc idp ldap add — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-add.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-add"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap add — SILO (MinIO Object Storage)"
---
# mc idp ldap add

## Description

The [`mc idp ldap add`](#command-mc.idp.ldap.add) command creates an AD/LDAP IDP server configuration.

MinIO supports no more than one (1) AD/LDAP provider per deployment.

EXAMPLE

The following example sets the AD/LDAP configuration settings for the `myminio` deployment.

```
mc idp ldap add                                                            \
            myminio                                                        \
            server_addr=myldapserver:636                                   \
            lookup_bind_dn=cn=admin,dc=min,dc=io                           \
            lookup_bind_password=somesecret                                \
            user_dn_search_base_dn=dc=min,dc=io                            \
            user_dn_search_filter="(uid=%s)"                               \
            group_search_base_dn=ou=swengg,dc=min,dc=io                    \
            group_search_filter="(&(objectclass=groupofnames)(member=%d))"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap add               \
                          ALIAS             \
                          [CFG_PARAM1]      \
                          [CFG_PARAM2]...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to create for AD/LDAP integration.

Replace the `[CFG_PARAM#]` with each of the [[reference/minio-server/settings/iam/ldap#minio-ldap-config-settings|configuration setting]] key-value pairs in the format of `PARAMETER="value"`.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.add.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment on which to add an AD/LDAP integration.

For example:

```
mc idp ldap add myminio                               \
                server_addr=myldapserver:636          \
                lookup_bind_dn=cn=admin,dc=min,dc=io  \
                lookup_bind_password=somesecret       \
                user_dn_search_base_dn=dc=min,dc=io   \
                user_dn_search_filter="(uid=%s)"      \
```

server_addr[](#mc.idp.ldap.add.server_addr)
Required

Specify the hostname for the Active Directory / LDAP server. For example:

```
ldapserver.com:636
```

[`srv_record_name`](#mc.idp.ldap.add.srv_record_name) automatically identifies the port

If your AD/LDAP server uses [`DNS SRV Records`](#mc.idp.ldap.add.srv_record_name), do not append the port number to your [`server_addr`](#mc.idp.ldap.add.server_addr) value.
SRV requests automatically include port numbers when returning the list of available servers.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_SERVER_ADDR|`MINIO_IDENTITY_LDAP_SERVER_ADDR`]] environment variable.

lookup_bind_dn[](#mc.idp.ldap.add.lookup_bind_dn)
Required

Specify the Distinguished Name (DN) for an AD/LDAP account MinIO uses when
querying the AD/LDAP server. Enables [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|Lookup-Bind]] authentication to the AD/LDAP server.

The DN account should be a read-only access keys with sufficient
privileges to support querying performing user and group lookups.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN|`MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN`]] environment variable.

lookup_bind_password[](#mc.idp.ldap.add.lookup_bind_password)
Required

Specify the password for the [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|Lookup-Bind]] user account.

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD|`MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD`]] environment variable.

user_dn_attributes[](#mc.idp.ldap.add.user_dn_attributes)
Optional

New in version RELEASE.2024-06-06T09-36-42Z.

Comma-separated list of user DN attributes.

Some valid values include, `uid,cn,mail,sshPublicKey`.

To enable public authentication for LDAP users, pass `sshPublicKey` as a DN attribute.
The user can then use the passed SSH Public Key to log in to SFTP servers.

```
mc idp ldap update ALIAS user_dn_attributes=sshPublicKey
```

user_dn_search_base_dn[](#mc.idp.ldap.add.user_dn_search_base_dn)
Required

Specify the base Distinguished Name (DN) MinIO uses when querying for
user credentials matching those provided by an authenticating client.

Separate multiple DNs with a semicolon (`;`).

For example:

```
cn=miniousers,dc=myldapserver,dc=net;ou=swengg,dc=min,dc=io
```

Supports [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|Lookup-Bind]] mode.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN|`MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN`]] environment variable.

user_dn_search_filter[](#mc.idp.ldap.add.user_dn_search_filter)
Required

Specify the AD/LDAP search filter MinIO uses when querying for user credentials
matching those provided by an authenticating client.

Use the `%s` substitution character to insert the client-specified
username into the search string. For example:

```
(userPrincipalName=%s)
```

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER|`MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER`]] environment variable.

comment[](#mc.idp.ldap.add.comment)
Optional

Specify a comment to associate to the AD/LDAP configuration.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_COMMENT|`MINIO_IDENTITY_LDAP_COMMENT`]] environment variable.

enabled[](#mc.idp.ldap.add.enabled)
Optional

Set to `false` to disable the AD/LDAP configuration.

If `false`, applications cannot generate STS credentials or otherwise authenticate to MinIO using the configured provider.

Defaults to `true` or “enabled”.

group_search_base_dn[](#mc.idp.ldap.add.group_search_base_dn)
Optional

Specify a semicolon-separated (`;`) list of group search base [Distinguished Names](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/ldap/distinguished-names)
MinIO uses when performing group lookups.

For example:

```
cn=miniogroups,dc=myldapserver,dc=net;ou=swengg,dc=min,dc=io
```

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_GROUP_SEARCH_BASE_DN|`MINIO_IDENTITY_LDAP_GROUP_SEARCH_BASE_DN`]] environment variable.

group_search_filter[](#mc.idp.ldap.add.group_search_filter)
Optional

Specify an AD/LDAP search filter for performing group lookups for the
authenticated user

Use the `%s` substitution character to insert the client-specified username
into the search string. Use the `%d` substitution character to insert the
Distinguished Name of the client-specified username into the search string.

For example:

```
(&(objectclass=groupOfNames)(memberUid=%s))
```

When providing an AD/LDAP group search filter, configure a filter that returns the minimum number of relevant groups for the purpose of supporting authentication.
Filters that return large group assignments increase the size of associated calls and resources.
Functions sensitive to large request or response bodies may exhibit unexpected behaviors as a result.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_GROUP_SEARCH_FILTER|`MINIO_IDENTITY_LDAP_GROUP_SEARCH_FILTER`]] environment variable.

server_insecure[](#mc.idp.ldap.add.server_insecure)
Optional

Specify `on` to allow unsecured (non-TLS encrypted) connections to
the AD/LDAP server.

MinIO sends AD/LDAP user credentials in plain text to the AD/LDAP server, such
that enabling TLS is required to prevent reading credentials over the wire.
Using this option presents a security risk where any user with access to
network traffic can observe the unencrypted plaintext credentials.

Defaults to `off`.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_SERVER_INSECURE|`MINIO_IDENTITY_LDAP_SERVER_INSECURE`]] environment variable.

server_starttls[](#mc.idp.ldap.add.server_starttls)
Optional

Specify `on` to enable `StartTLS` connections to an AD/LDAP server.

Defaults to `off`

For more about `StartTLS`, refer to section 4.14 of the [LDAP RFC 4511 specification](https://docs.ldap.com/specs/rfc4511.txt).

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_SERVER_STARTTLS|`MINIO_IDENTITY_LDAP_SERVER_STARTTLS`]] environment variable.

srv_record_name[](#mc.idp.ldap.add.srv_record_name)
Optional

New in version RELEASE.2022-12-12T19-27-27Z.

Specify the appropriate value to enable MinIO to select an AD/LDAP server using a [DNS SRV record](https://ldap.com/dns-srv-records-for-ldap) request.

When enabled, MinIO selects an AD/LDAP server by:

Constructing the target SRV record name following standard naming conventions.

Requesting a list of available AD/LDAP servers.

Choosing an appropriate target based on priority and weight.

The configuration examples below presume the AD/LDAP server address is set to `example.com` and the SRV record protocol is `_tcp`.

For SRV record names beginning with `_ldap`, specify `ldap`.
The constructed DNS SRV record name resembles the following:

```
_ldap._tcp.example.com
```

For SRV record names with beginning with `_ldaps`, specify `ldaps`.
The constructed DNS SRV record name resembles the following:

```
_ldaps._tcp.example.com
```

If your DNS SRV record name uses alternate service or protocol names, specify `on` and provide the full record name as your LDAP server address.
Example: `_ldapserver._specialtcp.example.com`

For more about DNS SRV records, see [DNS SRV Records for LDAP](https://ldap.com/dns-srv-records-for-ldap).

Server address for DNS SRV record configurations

The specified server name must not include a port number.
This is different from a standard AD/LDAP configuration, where the port number is required.

See [[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap.server_addr|`server_addr`]] or [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_SERVER_ADDR|`MINIO_IDENTITY_LDAP_SERVER_ADDR`]] for more about configuring an AD/LDAP server address.

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_SRV_RECORD_NAME|`MINIO_IDENTITY_LDAP_SRV_RECORD_NAME`]] environment variable.

tls_skip_verify[](#mc.idp.ldap.add.tls_skip_verify)
Optional

Specify `on` to trust the AD/LDAP server TLS certificates without
verification. This option may be required if the AD/LDAP server TLS certificates
are signed by an untrusted Certificate Authority (e.g. self-signed).

Defaults to `off`

This parameter corresponds with the [[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_TLS_SKIP_VERIFY|`MINIO_IDENTITY_LDAP_TLS_SKIP_VERIFY`]] environment variable.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).