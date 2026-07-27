---
title: "Active Directory / LDAP Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/iam/ldap.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/iam/ldap"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Active Directory / LDAP Settings — SILO (MinIO Object Storage)"
---
# Active Directory / LDAP Settings

This page documents settings for enabling external identity management using an Active Directory or LDAP service.
See [[operations/external-iam/configure-ad-ldap-external-identity-management#minio-authenticate-using-ad-ldap-generic|Configure MinIO for Authentication using Active Directory / LDAP]] for a tutorial on using these settings.

Important

New in version `RELEASE.2023-05-26T23-31-54Z`:

[[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] commands are preferred over using configuration settings to configure MinIO to use Active Directory or LDAP for identity management.

MinIO recommends using the [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] commands for LDAP management operations.
These commands offer better validation and additional features, while providing the same settings as the `identity_ldap` configuration key.
See [[operations/external-iam/configure-ad-ldap-external-identity-management#minio-authenticate-using-ad-ldap-generic|Configure MinIO for Authentication using Active Directory / LDAP]] for a tutorial on using [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]].

The `identity_ldap` configuration settings remains available for existing scripts and other tools.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Examples

Environment Variable

```
MINIO_IDENTITY_LDAP_SERVER_ADDR="ldapserver.com:636"
```

Note

`srv_record_name` automatically identifies the port.

If your AD/LDAP server uses `DNS SRV Records`, do not append the port number to your `server_addr` value.
SRV requests automatically include port numbers when returning the list of available servers.

Configuration Setting

identity_ldap[](#mc-conf.identity_ldap)

The following settings are required when defining LDAP using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]]:

`enabled`

`server_addr`

`lookup_bind_dn`

`lookup_bind_dn_password`

`user_dn_search_base_dn`

`user_dn_search_filter`

```
mc admin config set identity_ldap                        \
   enabled="true"                                        \
   server_addr="ad-ldap.example.net/"                    \
   lookup_bind_dn="cn=miniolookupuser,dc=example,dc=net" \
   lookup_bind_dn_password="userpassword"                \
   user_dn_search_base_dn="dc=example,dc=net"            \
   user_dn_search_filter="(&(objectCategory=user)(sAMAccountName=%s))"
```

## Settings

### Server Address

Required

Environment Variable

MINIO_IDENTITY_LDAP_SERVER_ADDR[](#envvar.MINIO_IDENTITY_LDAP_SERVER_ADDR)

Specify the hostname for the Active Directory / LDAP server. For example:

```
ldapserver.com:636
```

[[reference/minio-mc/mc-idp-ldap-add#mc.idp.ldap.add.srv_record_name|`srv_record_name`]] automatically identifies the port

If your AD/LDAP server uses [[reference/minio-mc/mc-idp-ldap-add#mc.idp.ldap.add.srv_record_name|`DNS SRV Records`]], do not append the port number to your [[reference/minio-mc/mc-idp-ldap-add#mc.idp.ldap.add.server_addr|`server_addr`]] value.
SRV requests automatically include port numbers when returning the list of available servers.

Configuration Setting

identity_ldap server_addr[](#mc-conf.identity_ldap.server_addr)

Specify the hostname for the Active Directory / LDAP server. For example:

```
ldapserver.com:636
```

[[reference/minio-mc/mc-idp-ldap-add#mc.idp.ldap.add.srv_record_name|`srv_record_name`]] automatically identifies the port

If your AD/LDAP server uses [[reference/minio-mc/mc-idp-ldap-add#mc.idp.ldap.add.srv_record_name|`DNS SRV Records`]], do not append the port number to your [[reference/minio-mc/mc-idp-ldap-add#mc.idp.ldap.add.server_addr|`server_addr`]] value.
SRV requests automatically include port numbers when returning the list of available servers.

### Lookup Bind DN

Required

Environment Variable

MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN[](#envvar.MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN)

Configuration Setting

identity_ldap lookup_bind_dn[](#mc-conf.identity_ldap.lookup_bind_dn)

Specify the Distinguished Name (DN) for an AD/LDAP account MinIO uses when
querying the AD/LDAP server. Enables [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|Lookup-Bind]] authentication to the AD/LDAP server.

The DN account should be a read-only access keys with sufficient
privileges to support querying performing user and group lookups.

### Lookup Bind Password

Required

Environment Variable

MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD[](#envvar.MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD)

Configuration Setting

identity_ldap lookup_bind_password[](#mc-conf.identity_ldap.lookup_bind_password)

Specify the password for the [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|Lookup-Bind]] user account.

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

### User DN Search Base DN

Required

Environment Variable

MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN[](#envvar.MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN)

Configuration Setting

identity_ldap user_dn_search_base_dn[](#mc-conf.identity_ldap.user_dn_search_base_dn)

Specify the base Distinguished Name (DN) MinIO uses when querying for
user credentials matching those provided by an authenticating client.

Separate multiple DNs with a semicolon (`;`).

For example:

```
cn=miniousers,dc=myldapserver,dc=net;ou=swengg,dc=min,dc=io
```

Supports [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|Lookup-Bind]] mode.

### User DN Search Filter

Required

Environment Variable

MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER[](#envvar.MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER)

Configuration Setting

identity_ldap user_dn_search_filter[](#mc-conf.identity_ldap.user_dn_search_filter)

Specify the AD/LDAP search filter MinIO uses when querying for user credentials
matching those provided by an authenticating client.

Use the `%s` substitution character to insert the client-specified
username into the search string. For example:

```
(userPrincipalName=%s)
```

### User DN Attributes

Optional

Environment Variable

MINIO_IDENTITY_LDAP_USER_DN_ATTRIBUTES[](#envvar.MINIO_IDENTITY_LDAP_USER_DN_ATTRIBUTES)

Configuration Setting

identity_ldap user_dn_attributes[](#mc-conf.identity_ldap.user_dn_attributes)

New in version RELEASE.2024-06-06T09-36-42Z.

Comma-separated list of user DN attributes.

Some valid values include, `uid,cn,mail,sshPublicKey`.

To enable public authentication for LDAP users, pass `sshPublicKey` as a DN attribute.
The user can then use the passed SSH Public Key to log in to SFTP servers.

```
mc idp ldap update ALIAS user_dn_attributes=sshPublicKey
```

### Enabled

Optional

Environment Variable

This setting does not have an environment variable option.
Use the configuration setting instead.

Configuration Setting

identity_ldap enabled[](#mc-conf.identity_ldap.enabled)

Set to `false` to disable the AD/LDAP configuration.

If `false`, applications cannot generate STS credentials or otherwise authenticate to MinIO using the configured provider.

Defaults to `true` or “enabled”.

### Group Search Filter

Optional

Environment Variable

MINIO_IDENTITY_LDAP_GROUP_SEARCH_FILTER[](#envvar.MINIO_IDENTITY_LDAP_GROUP_SEARCH_FILTER)

Configuration Setting

identity_ldap group_search_filter[](#mc-conf.identity_ldap.group_search_filter)

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

When providing an AD/LDAP group search filter, configure a filter that returns the minimum number of relevant groups for the purpose of supporting authentication.
Filters that return large group assignments increase the size of associated calls and resources.
Functions sensitive to large request or response bodies may exhibit unexpected behaviors as a result.

### Group Search Base DN

Optional

Environment Variable

MINIO_IDENTITY_LDAP_GROUP_SEARCH_BASE_DN[](#envvar.MINIO_IDENTITY_LDAP_GROUP_SEARCH_BASE_DN)

Configuration Setting

identity_ldap group_search_base_dn[](#mc-conf.identity_ldap.group_search_base_dn)

Specify a semicolon-separated (`;`) list of group search base [Distinguished Names](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/ldap/distinguished-names)
MinIO uses when performing group lookups.

For example:

```
cn=miniogroups,dc=myldapserver,dc=net;ou=swengg,dc=min,dc=io
```

### TLS Skip Verify

Optional

Environment Variable

MINIO_IDENTITY_LDAP_TLS_SKIP_VERIFY[](#envvar.MINIO_IDENTITY_LDAP_TLS_SKIP_VERIFY)

Configuration Setting

identity_ldap tls_skip_verify[](#mc-conf.identity_ldap.tls_skip_verify)

Specify `on` to trust the AD/LDAP server TLS certificates without
verification. This option may be required if the AD/LDAP server TLS certificates
are signed by an untrusted Certificate Authority (e.g. self-signed).

Defaults to `off`

### Server Insecure

Optional

Environment Variable

MINIO_IDENTITY_LDAP_SERVER_INSECURE[](#envvar.MINIO_IDENTITY_LDAP_SERVER_INSECURE)

Configuration Setting

identity_ldap server_insecure[](#mc-conf.identity_ldap.server_insecure)

Specify `on` to allow unsecured (non-TLS encrypted) connections to
the AD/LDAP server.

MinIO sends AD/LDAP user credentials in plain text to the AD/LDAP server, such
that enabling TLS is required to prevent reading credentials over the wire.
Using this option presents a security risk where any user with access to
network traffic can observe the unencrypted plaintext credentials.

Defaults to `off`.

### Server Start TLS

Optional

Environment Variable

MINIO_IDENTITY_LDAP_SERVER_STARTTLS[](#envvar.MINIO_IDENTITY_LDAP_SERVER_STARTTLS)

Configuration Setting

identity_ldap server_starttls[](#mc-conf.identity_ldap.server_starttls)

Specify `on` to enable `StartTLS` connections to an AD/LDAP server.

Defaults to `off`

For more about `StartTLS`, refer to section 4.14 of the [LDAP RFC 4511 specification](https://docs.ldap.com/specs/rfc4511.txt).

### SRV Record Name

Optional

New in version RELEASE.2022-12-12T19-27-27Z.

Environment Variable

MINIO_IDENTITY_LDAP_SRV_RECORD_NAME[](#envvar.MINIO_IDENTITY_LDAP_SRV_RECORD_NAME)

Configuration Setting

identity_ldap srv_record_name[](#mc-conf.identity_ldap.srv_record_name)

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

See [`server_addr`](#mc-conf.identity_ldap.server_addr) or [`MINIO_IDENTITY_LDAP_SERVER_ADDR`](#envvar.MINIO_IDENTITY_LDAP_SERVER_ADDR) for more about configuring an AD/LDAP server address.

### Comment

Optional

Environment Variable

MINIO_IDENTITY_LDAP_COMMENT[](#envvar.MINIO_IDENTITY_LDAP_COMMENT)

Configuration Setting

identity_ldap identity_ldap comment[](#mc-conf.identity_ldap.identity_ldap.comment)

Specify a comment to associate to the AD/LDAP configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).