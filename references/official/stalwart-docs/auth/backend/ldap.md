---
title: "LDAP Server"
source: "https://stalw.art/docs/auth/backend/ldap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/backend/ldap"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "LDAP Server"
---
# LDAP Server

      

LDAP (Lightweight Directory Access Protocol) is an open, vendor-neutral protocol used to access and manage directory information services. LDAP directories such as OpenLDAP and Microsoft Active Directory store user credentials and metadata in a hierarchical form that can be queried efficiently, and are commonly used to centralise authentication across multiple systems.

Stalwart integrates with LDAP directories for account authentication and lookup. With an LDAP directory configured, the server can reuse existing account records and group policies. Stalwart is compatible with LDAP directories that follow RFC 4511, including OpenLDAP and Active Directory.

An LDAP integration is represented by the LDAP variant of the [[ref/object/directory|Directory]] object (found in the WebUI under  Settings ›  Authentication › Directories).

## Connection details

[Section titled “Connection details”](#connection-details)

The minimum required configuration for LDAP is the URL of the directory server. The URL is set through [[ref/object/directory#url|`url`]]; a typical value is `"ldap://localhost:389"` for an unencrypted connection to a server on the same host.

Related connection fields on the LDAP variant of the Directory object are:

[[ref/object/directory#url|`url`]]: URL of the LDAP server.
[[ref/object/directory#usetls|`useTls`]]: whether to negotiate TLS on the connection. Default `false`.
[[ref/object/directory#allowinvalidcerts|`allowInvalidCerts`]]: whether to accept invalid TLS certificates. Default `false`.
[[ref/object/directory#timeout|`timeout`]]: connection timeout. Default `"30s"`.

For example:

```
{  "@type": "Ldap",  "description": "Corporate LDAP",  "url": "ldap://localhost:389",  "timeout": "30s",  "useTls": false,  "allowInvalidCerts": false}
```

## Default bind credentials

[Section titled “Default bind credentials”](#default-bind-credentials)

Binding is the operation by which an LDAP client authenticates to the directory. To query the directory and validate account entries, Stalwart needs to bind using a service account that holds sufficient privileges to search the directory tree.

Service-account credentials are configured through:

[[ref/object/directory#binddn|`bindDn`]]: the distinguished name of the account used to connect to the directory.
[[ref/object/directory#bindsecret|`bindSecret`]]: the secret associated with `bindDn`. Accepts a direct value, an environment-variable reference, or a file reference.

For example:

```
{  "@type": "Ldap",  "bindDn": "cn=admin,dc=example,dc=org",  "bindSecret": {    "@type": "Value",    "secret": "password"  }}
```

If `bindDn` is left unset, the server attempts anonymous queries. Some directories allow that for basic lookups, but most require an authenticated bind. Without valid credentials, authentication or address validation may fail.

## Authentication methods

[Section titled “Authentication methods”](#authentication-methods)

Stalwart supports two authentication modes against an LDAP directory, selected by the [[ref/object/directory#bindauthentication|`bindAuthentication`]] boolean on the Directory object.

In some LDAP environments, particularly hardened OpenLDAP setups and Active Directory, password hashes are not returned even to a privileged service account. In those environments Stalwart cannot compare a stored hash against the password supplied by the user, so it must authenticate by attempting to bind directly as the user. If the bind succeeds, the credentials are considered valid.

### Service-account bind (hash comparison)

[Section titled “Service-account bind (hash comparison)”](#service-account-bind-hash-comparison)

When [[ref/object/directory#bindauthentication|`bindAuthentication`]] is set to `false`, Stalwart uses the service account (`bindDn` / `bindSecret`) to search for the user entry, reads the password hash from the attribute configured by [[ref/object/directory#attrsecret|`attrSecret`]], and compares it against the password supplied at login. This mode only works if the LDAP server exposes password hashes in a format that Stalwart [[auth/authentication/password|recognises]].

For example:

```
{  "@type": "Ldap",  "bindAuthentication": false,  "attrSecret": ["userPassword"]}
```

### Bind authentication

[Section titled “Bind authentication”](#bind-authentication)

When [[ref/object/directory#bindauthentication|`bindAuthentication`]] is set to `true` (the default), Stalwart authenticates users by attempting to bind as the user themselves, using the password supplied at login. If the bind succeeds, the credentials are accepted. No password hash is read from the directory.

In this mode, the user’s distinguished name is located by running the [[ref/object/directory#filterlogin|`filterLogin`]] search using the service account; the `?` placeholder in the filter is replaced with the login value. Once the DN is known, Stalwart attempts the user bind. Because the password hash is not available, Stalwart relies on the [[ref/object/directory#attrsecretchanged|`attrSecretChanged`]] attribute (by default `pwdChangeTime`) to detect password changes and invalidate cached OAuth tokens.

For example:

```
{  "@type": "Ldap",  "bindAuthentication": true,  "filterLogin": "(&(objectClass=inetOrgPerson)(mail=?))",  "attrSecretChanged": ["pwdChangeTime"]}
```

## Lookup filters

[Section titled “Lookup filters”](#lookup-filters)

Two LDAP filters drive account resolution: a login filter used at authentication time, and a mailbox filter used during email delivery.

[[ref/object/directory#filterlogin|`filterLogin`]]: locates the LDAP entry that matches the login value supplied by the user. Default `"(&(objectClass=inetOrgPerson)(mail=?))"`.
[[ref/object/directory#filtermailbox|`filterMailbox`]]: locates a user or group entry that matches a recipient email address or alias. Default `"(|(&(objectClass=inetOrgPerson)(|(mail=?)(mailAlias=?)))(&(objectClass=groupOfNames)(|(mail=?)(mailAlias=?))))"`.
[[ref/object/directory#filtermemberof|`filterMemberOf`]]: locates the groups an account belongs to when group membership is not carried on the account entry. The `?` is replaced with the account DN. Default `"(&(objectClass=groupOfNames)(member=?))"`.

Each filter contains a `?` placeholder that the server replaces at runtime: the login value for `filterLogin`, the recipient address for `filterMailbox`, or the account DN for `filterMemberOf`.

For example:

```
{  "@type": "Ldap",  "filterLogin": "(&(|(objectClass=posixAccount)(objectClass=posixGroup))(uid=?))",  "filterMailbox": "(&(|(objectClass=posixAccount)(objectClass=posixGroup))(|(mail=?)(mailAlias=?)))"}
```

## Base DN

[Section titled “Base DN”](#base-dn)

Searches start from a base distinguished name configured through [[ref/object/directory#basedn|`baseDn`]]. The base DN sets the scope of every query and should be narrow enough to limit the search surface but broad enough to cover all relevant accounts.

For example, `"dc=example,dc=org"` restricts lookups to entries under the `example.org` organisation.

## Object attributes

[Section titled “Object attributes”](#object-attributes)

LDAP schemas vary between servers, so Stalwart has to be told which attributes to read for each piece of account information. The mapping is carried on the LDAP variant of the Directory object through the following fields:

[[ref/object/directory#attrclass|`attrClass`]]: attribute(s) carrying the account’s object class. Defaults to `["objectClass"]`. An account entry is treated as an individual unless its class matches [[ref/object/directory#groupclass|`groupClass`]], which defaults to `"groupOfNames"`.
[[ref/object/directory#attrdescription|`attrDescription`]]: attribute(s) used as the account description. Default `["description"]`.
[[ref/object/directory#attrsecret|`attrSecret`]]: attribute carrying the password hash, used only when `bindAuthentication` is `false`. Default `["userPassword"]`.
[[ref/object/directory#attrsecretchanged|`attrSecretChanged`]]: attribute carrying the last password-change timestamp (or version value), used to invalidate OAuth tokens in bind-authentication mode. Default `["pwdChangeTime"]`.
[[ref/object/directory#attrmemberof|`attrMemberOf`]]: attribute(s) listing the groups an account belongs to. Default `["memberOf"]`.
[[ref/object/directory#attremail|`attrEmail`]]: attribute carrying the primary email address. Default `["mail"]`.
[[ref/object/directory#attremailalias|`attrEmailAlias`]]: attribute carrying email aliases. Default `["mailAlias"]`.

There is no attribute mapping for the account’s login name or for a per-account disk quota: the login name is resolved from the entry returned by [[ref/object/directory#filterlogin|`filterLogin`]], and disk quotas are held on the [[ref/object/account|Account]] and [[ref/object/tenant|Tenant]] objects rather than read from the directory (see [[auth/authorization/quotas|Quotas]]).

For example:

```
{  "@type": "Ldap",  "attrClass": ["objectClass"],  "attrDescription": ["principalName", "description"],  "attrSecret": ["userPassword"],  "attrMemberOf": ["memberOf", "otherGroups"],  "attrEmail": ["mail"],  "attrEmailAlias": ["mailAlias"]}
```