---
title: "Directory"
source: "https://stalw.art/docs/ref/object/directory"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/directory"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Directory"
---
# Directory

      

Defines an external directory for account authentication and lookups.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Authentication › Directories

## Fields

[Section titled “Fields”](#fields)

Directory is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Ldap"

[Section titled “@type: "Ldap"”](#type-ldap)

LDAP Directory

##### description

[Section titled “description”](#description)

Type: `String` · required

Short description of this directory

##### url

[Section titled “url”](#url)

Type: `Uri` · default: `"ldap://localhost:389"`

URL of the LDAP server

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

Connection timeout to the server

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the server

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `false`

Use TLS to connect to the remote server

##### baseDn

[Section titled “baseDn”](#basedn)

Type: `String` · required

The base distinguished name (DN) from where searches should begin

##### bindDn

[Section titled “bindDn”](#binddn)

Type: `String?`

The distinguished name of the account that the server will bind as to connect to the LDAP directory

##### bindSecret

[Section titled “bindSecret”](#bindsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The password or secret for the bind DN account

##### bindAuthentication

[Section titled “bindAuthentication”](#bindauthentication)

Type: `Boolean` · default: `true`

Whether to use bind authentication. When enabled, the server will use the filterLogin to search for the user account and then attempt to bind as that account using the provided password. When disabled, the server will use the bind DN and secret to connect to the LDAP server and obtain the secret from the account entry using the attrSecret attribute.

##### filterLogin

[Section titled “filterLogin”](#filterlogin)

Type: `String` · default: `"(&(objectClass=inetOrgPerson)(mail=?))"`

Searches for user accounts by e-mail address during authentication

##### filterMailbox

[Section titled “filterMailbox”](#filtermailbox)

Type: `String` · default: `"(|(&(objectClass=inetOrgPerson)(|(mail=?)(mailAlias=?)))(&(objectClass=groupOfNames)(|(mail=?)(mailAlias=?))))"`

Searches for users or groups matching a recipient e-mail address or alias

##### filterMemberOf

[Section titled “filterMemberOf”](#filtermemberof)

Type: `String?` · default: `"(&(objectClass=groupOfNames)(member=?))"`

Searches for groups that an account is member of. Use when the group membership is not provided in the account entry. The ? in the filter will be replaced with the account DN.

##### attrClass

[Section titled “attrClass”](#attrclass)

Type: `String[]` · default: `["objectClass"]`

LDAP attribute for the user’s account type, if missing defaults to individual.

##### attrDescription

[Section titled “attrDescription”](#attrdescription)

Type: `String[]` · default: `["description"]`

LDAP attributes used to store the user’s description

##### attrEmail

[Section titled “attrEmail”](#attremail)

Type: `String[]` · default: `["mail"]`

LDAP attribute for the user’s primary email address

##### attrEmailAlias

[Section titled “attrEmailAlias”](#attremailalias)

Type: `String[]` · default: `["mailAlias"]`

LDAP attribute for the user’s email alias(es)

##### attrMemberOf

[Section titled “attrMemberOf”](#attrmemberof)

Type: `String[]` · default: `["memberOf"]`

LDAP attributes for the groups that a user belongs to. Used when filterMemberOf is not configured or when the group membership is also provided in the account entry.

##### attrSecret

[Section titled “attrSecret”](#attrsecret)

Type: `String[]` · default: `["userPassword"]`

LDAP attribute for the user’s password hash. This setting is required when binding as a service user. When using bind authentication, configure the secret-changed attribute instead.

##### attrSecretChanged

[Section titled “attrSecretChanged”](#attrsecretchanged)

Type: `String[]` · default: `["pwdChangeTime"]`

LDAP attribute that provides a password change hash or a timestamp indicating when the password was last changed. When using bind authentication, this attribute is used to determine when to invalidate OAuth tokens.

##### groupClass

[Section titled “groupClass”](#groupclass)

Type: `String` · default: `"groupOfNames"`

LDAP object class used to identify group entries

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections)

Type: `UnsignedInt` · default: `10` · max: 8192

Maximum number of connections that can be maintained simultaneously in the connection pool

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate)

Type: `Duration` · default: `"30s"`

Maximum amount of time that the connection pool will wait for a new connection to be created

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle)

Type: `Duration` · default: `"30s"`

Maximum amount of time that the connection pool manager will wait for a connection to be recycled

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait)

Type: `Duration` · default: `"30s"`

Maximum amount of time that the connection pool will wait for a connection to become available

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this directory belongs to

### @type: "Sql"

[Section titled “@type: "Sql"”](#type-sql)

SQL Database

##### description

[Section titled “description”](#description-1)

Type: `String` · required

Short description of this directory

##### store

[Section titled “store”](#store)

Type: [`SqlAuthStore`](#sqlauthstore) · required

Storage backend where accounts and groups are stored

##### columnEmail

[Section titled “columnEmail”](#columnemail)

Type: `String` · default: `"name"`

Column name for e-mail address. Optional, you can use instead a query to obtain the account’s addresses.

##### columnSecret

[Section titled “columnSecret”](#columnsecret)

Type: `String` · default: `"secret"`

Column name for the account password.

##### columnClass

[Section titled “columnClass”](#columnclass)

Type: `String?` · default: `"type"`

Column name for account type

##### columnDescription

[Section titled “columnDescription”](#columndescription)

Type: `String?` · default: `"description"`

Column name for account full name or description

##### queryLogin

[Section titled “queryLogin”](#querylogin)

Type: `String` · default: `"SELECT name, secret, description, type FROM accounts WHERE name = $1"`

Query to obtain the account details by login e-mail address.

##### queryRecipient

[Section titled “queryRecipient”](#queryrecipient)

Type: `String` · default: `"SELECT name, secret, description, type FROM accounts WHERE name = $1 AND active = true"`

Query to obtain the account details by recipient e-mail address or alias.

##### queryMemberOf

[Section titled “queryMemberOf”](#querymemberof)

Type: `String?` · default: `"SELECT member_of FROM group_members WHERE name = $1"`

Query to obtain the groups an account is member of.

##### queryEmailAliases

[Section titled “queryEmailAliases”](#queryemailaliases)

Type: `String?` · default: `"SELECT address FROM emails WHERE name = $1"`

Query to obtain the e-mail aliases of an account.

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-1)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this directory belongs to

### @type: "Oidc"

[Section titled “@type: "Oidc"”](#type-oidc)

OpenID Connect

##### description

[Section titled “description”](#description-2)

Type: `String` · required

Short description of this directory

##### issuerUrl

[Section titled “issuerUrl”](#issuerurl)

Type: `Uri` · required

The base URL of the OpenID Connect provider (e.g. [https://sso.example.com/realms/myrealm](https://sso.example.com/realms/myrealm)). Stalwart will use this URL to automatically discover the provider’s endpoints, including the token validation and user info endpoints.

##### requireAudience

[Section titled “requireAudience”](#requireaudience)

Type: `String?` · default: `"stalwart"`

If set, Stalwart will reject any token whose aud (audience) claim does not include this value. Set this to the client ID or resource identifier registered for Stalwart in your identity provider to ensure tokens issued for other applications are not accepted.

##### requireScopes

[Section titled “requireScopes”](#requirescopes)

Type: `String[]` · default: `["openid","email"]`

If set, Stalwart will reject any token that does not include all of the specified scopes. Useful for ensuring that only tokens explicitly granted access to the mail server are accepted.

##### claimUsername

[Section titled “claimUsername”](#claimusername)

Type: `String` · default: `"preferred_username"`

The claim name used to retrieve the user’s login name from the token or user info response. Common values are preferred_username, email, or sub depending on your provider’s configuration. If the claim value is not an email address and usernameDomain is set, the domain will be appended automatically (e.g. john becomes [[email protected]](/cdn-cgi/l/email-protection#1a707572745a7f627b776a767f34797577)). If the claim value already contains an @, it is used as-is. If the claim value is not an email address and no usernameDomain is configured, Stalwart will fall back to the email claim. If neither yields a valid email address, authentication will be rejected.

##### usernameDomain

[Section titled “usernameDomain”](#usernamedomain)

Type: `String?`

The domain name to append to the username when the value of claimUsername does not contain an @ symbol (e.g. setting this to example.com will turn john into [[email protected]](/cdn-cgi/l/email-protection#91fbfef9ffd1f4e9f0fce1fdf4bff2fefc)). If not set, Stalwart will fall back to the email claim when the username claim does not contain a valid email address.

##### claimName

[Section titled “claimName”](#claimname)

Type: `String?` · default: `"name"`

The claim name used to retrieve the user’s display name from the token or user info response. Common values are name or display_name. If not set, the display name will not be populated.

##### claimGroups

[Section titled “claimGroups”](#claimgroups)

Type: `String?`

The claim name used to retrieve the user’s group memberships from the token or user info response. Common values are groups or roles depending on your provider’s configuration. If not set, group information will not be populated. Note that some providers omit group claims from the token to keep its size small and only return them via the user info endpoint, if group information is missing, ensure your provider is configured to include it.

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-2)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this directory belongs to

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Directory object is available via the `urn:stalwart:jmap` capability.

### x:Directory/get

[Section titled “x:Directory/get”](#xdirectoryget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysDirectoryGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Directory/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Directory/set

[Section titled “x:Directory/set”](#xdirectoryset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysDirectoryCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Directory/set",          {            "create": {              "new1": {                "@type": "Ldap",                "baseDn": "Example",                "bindSecret": {                  "@type": "None"                },                "description": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysDirectoryUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Directory/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysDirectoryDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Directory/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Directory/query

[Section titled “x:Directory/query”](#xdirectoryquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysDirectoryQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Directory/query",          {            "filter": {              "memberTenantId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Directory/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Directory id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Directory/Ldap \  --field description=Example \  --field baseDn=Example \  --field 'bindSecret={"@type":"None"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Directorystalwart-cli query Directory --where memberTenantId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Directory id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Directory --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

Password or secret value

#### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

#### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

### SqlAuthStore

[Section titled “SqlAuthStore”](#sqlauthstore)

Defines the SQL database used to store account and group information for SQL directories.

`Default`: Use data store (SQL only). No additional fields.
`PostgreSql`: PostgreSQL. Carries the fields of [`PostgreSqlStore`](#postgresqlstore).
`MySql`: mySQL. Carries the fields of [`MySqlStore`](#mysqlstore).
`Sqlite`: SQLite. Carries the fields of [`SqliteStore`](#sqlitestore).

#### PostgreSqlStore

[Section titled “PostgreSqlStore”](#postgresqlstore)

PostgreSQL data store.

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls-1)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-1)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the store

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-1)

Type: `UnsignedInt?` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolRecyclingMethod

[Section titled “poolRecyclingMethod”](#poolrecyclingmethod)

Type: [`PostgreSqlRecyclingMethod`](#postgresqlrecyclingmethod) · default: `"fast"`

Method to use when recycling connections in the pool

##### readReplicas

[Section titled “readReplicas”](#readreplicas)

Type: [`PostgreSqlSettings`](#postgresqlsettings)`[]` · [[server/enterprise|enterprise]]

List of read replicas for the store

##### host

[Section titled “host”](#host)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `5432` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### options

[Section titled “options”](#options)

Type: `String?`

Additional connection options

##### PostgreSqlSettings

[Section titled “PostgreSqlSettings”](#postgresqlsettings)

PostgreSQL connection settings.

##### host

[Section titled “host”](#host-1)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-1)

Type: `UnsignedInt` · default: `5432` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-1)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-1)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### options

[Section titled “options”](#options-1)

Type: `String?`

Additional connection options

#### MySqlStore

[Section titled “MySqlStore”](#mysqlstore)

MySQL data store.

##### timeout

[Section titled “timeout”](#timeout-2)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls-2)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-2)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the store

##### maxAllowedPacket

[Section titled “maxAllowedPacket”](#maxallowedpacket)

Type: `UnsignedInt?` · max: 1073741824 · min: 1024

Maximum size of a packet in bytes

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-2)

Type: `UnsignedInt?` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolMinConnections

[Section titled “poolMinConnections”](#poolminconnections)

Type: `UnsignedInt?` · default: `5` · max: 8192 · min: 1

Minimum number of connections to the store

##### readReplicas

[Section titled “readReplicas”](#readreplicas-1)

Type: [`MySqlSettings`](#mysqlsettings)`[]` · [[server/enterprise|enterprise]]

List of read replicas for the store

##### host

[Section titled “host”](#host-2)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-2)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-2)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-2)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-2)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### MySqlSettings

[Section titled “MySqlSettings”](#mysqlsettings)

MySQL connection settings.

##### host

[Section titled “host”](#host-3)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-3)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-3)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-3)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-3)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

#### SqliteStore

[Section titled “SqliteStore”](#sqlitestore)

SQLite embedded data store.

##### path

[Section titled “path”](#path)

Type: `String` · required

Path to the SQLite data directory

##### poolWorkers

[Section titled “poolWorkers”](#poolworkers)

Type: `UnsignedInt?` · max: 64 · min: 1

Number of worker threads to use for the store, defaults to the number of cores

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-3)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

## Enums

[Section titled “Enums”](#enums)

### PostgreSqlRecyclingMethod

[Section titled “PostgreSqlRecyclingMethod”](#postgresqlrecyclingmethod)
ValueLabel`fast`Fast recycling method`verified`Verified recycling method`clean`Clean recycling method