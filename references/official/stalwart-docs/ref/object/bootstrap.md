---
title: "Bootstrap"
source: "https://stalw.art/docs/ref/object/bootstrap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/bootstrap"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Bootstrap"
---
# Bootstrap

      

Initial setup shown the first time Stalwart starts. Configures the server’s identity, storage, user accounts, logging, and DNS management.

## Fields

[Section titled “Fields”](#fields)

##### serverHostname

[Section titled “serverHostname”](#serverhostname)

Type: `String` · required

The public hostname this server answers to, for example mail.example.com.
Used in SMTP greetings, outgoing message headers, and TLS certificate requests.

##### defaultDomain

[Section titled “defaultDomain”](#defaultdomain)

Type: `String` · required

The primary email domain this installation will serve, for example example.com.
Additional domains can be added at any time after setup.

##### requestTlsCertificate

[Section titled “requestTlsCertificate”](#requesttlscertificate)

Type: `Boolean` · default: `true`

Automatically obtain a free TLS certificate for the server hostname from Let’s Encrypt using the ACME protocol, so clients can connect securely out of the box.
Turn this off if you plan to install a certificate manually.

##### generateDkimKeys

[Section titled “generateDkimKeys”](#generatedkimkeys)

Type: `Boolean` · default: `true`

Generate DKIM signing keys for the default domain. DKIM cryptographically signs outgoing mail and significantly improves the chances that messages reach the recipient’s inbox instead of spam.
Turn this off only if you plan to manage DKIM keys yourself.

##### dataStore

[Section titled “dataStore”](#datastore)

Type: [[ref/object/data-store|`DataStore`]] · default: `{"@type":"RocksDb","path":"/var/lib/stalwart/"}`

Where structured data is kept: email metadata, calendars, contacts, mailbox state, and server settings.
RocksDB is recommended for single-node installations; PostgreSQL, MySQL, SQLite, and FoundationDB are also supported.

##### blobStore

[Section titled “blobStore”](#blobstore)

Type: [[ref/object/blob-store|`BlobStore`]] · default: `{"@type":"Default"}`

Where the raw content of email messages, attachments, and other large files is stored.
Leave as default to reuse the data store, or point to an object storage service such as S3 for larger deployments.

##### searchStore

[Section titled “searchStore”](#searchstore)

Type: [[ref/object/search-store|`SearchStore`]] · default: `{"@type":"Default"}`

Where the full-text search index is kept, so users can search across message bodies and attachments.
Leave as default to reuse the data store, or point to a dedicated search backend for larger deployments.

##### inMemoryStore

[Section titled “inMemoryStore”](#inmemorystore)

Type: [[ref/object/in-memory-store|`InMemoryStore`]] · default: `{"@type":"Default"}`

Where short-lived data lives: session caches, rate-limit counters, and temporary tokens.
Leave as default to reuse the data store, or point to Redis for faster lookups and multi-node deployments.

##### directory

[Section titled “directory”](#directory)

Type: [`DirectoryBootstrap`](#directorybootstrap) · default: `{"@type":"Internal"}`

Where user accounts and credentials come from.
The internal directory is recommended for ease of setup and management through the WebUI, but external OIDC or LDAP directories can be used for single sign-on and user provisioning in larger organizations.

##### tracer

[Section titled “tracer”](#tracer)

Type: [[ref/object/tracer|`Tracer`]] · default: `{"@type":"Log","path":"/var/log/stalwart/"}`

Where the server writes log messages, traces, and diagnostic events.
Defaults to log files on disk; remote destinations such as OpenTelemetry or webhooks can be added after setup.

##### dnsServer

[Section titled “dnsServer”](#dnsserver)

Type: [`DnsServerBootstrap`](#dnsserverbootstrap) · default: `{"@type":"Manual"}`

Optionally automate the DNS records your mail server needs (SPF, DKIM, DMARC, and more) by connecting to your DNS provider’s API.
Leave as manual unless your DNS is hosted by a supported provider; this can always be enabled later.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Bootstrap singleton is available via the `urn:stalwart:jmap` capability.

### x:Bootstrap/get

[Section titled “x:Bootstrap/get”](#xbootstrapget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysBootstrapGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Bootstrap/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Bootstrap/set

[Section titled “x:Bootstrap/set”](#xbootstrapset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysBootstrapUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Bootstrap/set",          {            "update": {              "singleton": {                "serverHostname": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Bootstrap
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Bootstrap --field serverHostname='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### DirectoryBootstrap

[Section titled “DirectoryBootstrap”](#directorybootstrap)

External directory service configuration for user authentication and lookup.

`Internal`: Use the internal directory. No additional fields.
`Ldap`: LDAP Directory. Carries the fields of [`LdapDirectory`](#ldapdirectory).
`Sql`: SQL Database. Carries the fields of [`SqlDirectory`](#sqldirectory).
`Oidc`: OpenID Connect. Carries the fields of [`OidcDirectory`](#oidcdirectory).

#### LdapDirectory

[Section titled “LdapDirectory”](#ldapdirectory)

LDAP directory connection and mapping settings.

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

##### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

Password or secret value

##### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

##### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

#### SqlDirectory

[Section titled “SqlDirectory”](#sqldirectory)

SQL database directory settings.

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

##### SqlAuthStore

[Section titled “SqlAuthStore”](#sqlauthstore)

Defines the SQL database used to store account and group information for SQL directories.

`Default`: Use data store (SQL only). No additional fields.
`PostgreSql`: PostgreSQL. Carries the fields of [`PostgreSqlStore`](#postgresqlstore).
`MySql`: mySQL. Carries the fields of [`MySqlStore`](#mysqlstore).
`Sqlite`: SQLite. Carries the fields of [`SqliteStore`](#sqlitestore).

##### PostgreSqlStore

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

##### MySqlStore

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

##### SqliteStore

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

#### OidcDirectory

[Section titled “OidcDirectory”](#oidcdirectory)

OpenID Connect directory settings.

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

The claim name used to retrieve the user’s login name from the token or user info response. Common values are preferred_username, email, or sub depending on your provider’s configuration. If the claim value is not an email address and usernameDomain is set, the domain will be appended automatically (e.g. john becomes [[email protected]](/cdn-cgi/l/email-protection#c0aaafa8ae80a5b8a1adb0aca5eea3afad)). If the claim value already contains an @, it is used as-is. If the claim value is not an email address and no usernameDomain is configured, Stalwart will fall back to the email claim. If neither yields a valid email address, authentication will be rejected.

##### usernameDomain

[Section titled “usernameDomain”](#usernamedomain)

Type: `String?`

The domain name to append to the username when the value of claimUsername does not contain an @ symbol (e.g. setting this to example.com will turn john into [[email protected]](/cdn-cgi/l/email-protection#375d585f5977524f565a475b521954585a)). If not set, Stalwart will fall back to the email claim when the username claim does not contain a valid email address.

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

### DnsServerBootstrap

[Section titled “DnsServerBootstrap”](#dnsserverbootstrap)

Automatic DNS server management.

`Manual`: Manual DNS server management. No additional fields.
`Tsig`: RFC2136 (TSIG). Carries the fields of [`DnsServerTsig`](#dnsservertsig).
`Deprecated1`: RFC2136 (SIG0 - deprecated). No additional fields.
`Cloudflare`: Cloudflare. Carries the fields of [`DnsServerCloudflare`](#dnsservercloudflare).
`DigitalOcean`: DigitalOcean. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`DeSEC`: DeSEC. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Ovh`: OVH. Carries the fields of [`DnsServerOvh`](#dnsserverovh).
`Bunny`: BunnyDNS. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Porkbun`: Porkbun. Carries the fields of [`DnsServerPorkbun`](#dnsserverporkbun).
`Dnsimple`: DNSimple. Carries the fields of [`DnsServerDnsimple`](#dnsserverdnsimple).
`Spaceship`: Spaceship. Carries the fields of [`DnsServerSpaceship`](#dnsserverspaceship).
`Route53`: AWS Route53. Carries the fields of [`DnsServerRoute53`](#dnsserverroute53).
`GoogleCloudDns`: Google Cloud DNS. Carries the fields of [`DnsServerGoogleCloudDns`](#dnsservergoogleclouddns).
`Alidns`: Alibaba Cloud DNS. Carries the fields of [`DnsServerAlidns`](#dnsserveralidns).
`ArvanCloud`: ArvanCloud. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Autodns`: InterNetX AutoDNS. Carries the fields of [`DnsServerAutodns`](#dnsserverautodns).
`AzureDns`: Microsoft Azure DNS. Carries the fields of [`DnsServerAzureDns`](#dnsserverazuredns).
`BaiduCloud`: Baidu Cloud DNS. Carries the fields of [`DnsServerBaiduCloud`](#dnsserverbaiducloud).
`BluecatV2`: BlueCat Address Manager. Carries the fields of [`DnsServerBluecatV2`](#dnsserverbluecatv2).
`ClouDns`: ClouDNS. Carries the fields of [`DnsServerClouDns`](#dnsservercloudns).
`Constellix`: Constellix. Carries the fields of [`DnsServerConstellix`](#dnsserverconstellix).
`Cpanel`: cPanel. Carries the fields of [`DnsServerCpanel`](#dnsservercpanel).
`Ddnss`: DDNSS.de. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`DnsMadeEasy`: DNS Made Easy. Carries the fields of [`DnsServerDnsMadeEasy`](#dnsserverdnsmadeeasy).
`Domeneshop`: Domeneshop. Carries the fields of [`DnsServerDomeneshop`](#dnsserverdomeneshop).
`Dreamhost`: Dreamhost. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`DuckDns`: DuckDNS. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Dynu`: Dynu. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`EasyDns`: EasyDNS. Carries the fields of [`DnsServerEasyDns`](#dnsservereasydns).
`EdgeDns`: Akamai EdgeDNS. Carries the fields of [`DnsServerEdgeDns`](#dnsserveredgedns).
`Exoscale`: Exoscale. Carries the fields of [`DnsServerExoscale`](#dnsserverexoscale).
`FreeMyIp`: freemyip.com. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`GandiV5`: Gandi LiveDNS v5. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Gcore`: Gcore. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Glesys`: GleSYS. Carries the fields of [`DnsServerGlesys`](#dnsserverglesys).
`Godaddy`: GoDaddy. Carries the fields of [`DnsServerGodaddy`](#dnsservergodaddy).
`Hetzner`: Hetzner. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`HostingDe`: hosting.de. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Hostinger`: Hostinger. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`HuaweiCloud`: Huawei Cloud DNS. Carries the fields of [`DnsServerHuaweiCloud`](#dnsserverhuaweicloud).
`Hurricane`: Hurricane Electric. Carries the fields of [`DnsServerHurricane`](#dnsserverhurricane).
`IbmCloud`: IBM Cloud. Carries the fields of [`DnsServerIbmCloud`](#dnsserveribmcloud).
`Infoblox`: Infoblox NIOS WAPI. Carries the fields of [`DnsServerInfoblox`](#dnsserverinfoblox).
`Infomaniak`: Infomaniak. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Inwx`: INWX. Carries the fields of [`DnsServerInwx`](#dnsserverinwx).
`Ionos`: IONOS. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Ipv64`: IPv64. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Joker`: Joker. Carries the fields of [`DnsServerJoker`](#dnsserverjoker).
`Lightsail`: AWS Lightsail. Carries the fields of [`DnsServerLightsail`](#dnsserverlightsail).
`Linode`: Linode. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`LuaDns`: LuaDNS. Carries the fields of [`DnsServerLuaDns`](#dnsserverluadns).
`MythicBeasts`: Mythic Beasts. Carries the fields of [`DnsServerMythicBeasts`](#dnsservermythicbeasts).
`Namecheap`: Namecheap. Carries the fields of [`DnsServerNamecheap`](#dnsservernamecheap).
`NameDotCom`: Name.com. Carries the fields of [`DnsServerNameDotCom`](#dnsservernamedotcom).
`NameSilo`: NameSilo. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Netcup`: Netcup. Carries the fields of [`DnsServerNetcup`](#dnsservernetcup).
`Netlify`: Netlify. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Nifcloud`: Nifcloud. Carries the fields of [`DnsServerNifcloud`](#dnsservernifcloud).
`Ns1`: NS1. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`OracleCloud`: Oracle Cloud. Carries the fields of [`DnsServerOracleCloud`](#dnsserveroraclecloud).
`Plesk`: Plesk. Carries the fields of [`DnsServerPlesk`](#dnsserverplesk).
`Safedns`: ANS SafeDNS. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`Scaleway`: Scaleway. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`TencentCloud`: Tencent Cloud DNSPod. Carries the fields of [`DnsServerTencentCloud`](#dnsservertencentcloud).
`Transip`: TransIP. Carries the fields of [`DnsServerTransip`](#dnsservertransip).
`UltraDns`: UltraDNS. Carries the fields of [`DnsServerUltraDns`](#dnsserverultradns).
`Vercel`: Vercel. Carries the fields of [`DnsServerVercel`](#dnsserververcel).
`Volcengine`: Volcano Engine. Carries the fields of [`DnsServerVolcengine`](#dnsservervolcengine).
`Vultr`: Vultr. Carries the fields of [`DnsServerCloud`](#dnsservercloud).
`WebSupport`: WebSupport. Carries the fields of [`DnsServerWebSupport`](#dnsserverwebsupport).
`YandexCloud`: Yandex Cloud. Carries the fields of [`DnsServerYandexCloud`](#dnsserveryandexcloud).

#### DnsServerTsig

[Section titled “DnsServerTsig”](#dnsservertsig)

RFC2136 TSIG DNS server.

##### host

[Section titled “host”](#host-4)

Type: `IpAddr` · required

The IP address of the DNS server

##### port

[Section titled “port”](#port-4)

Type: `UnsignedInt` · default: `53` · max: 65535 · min: 1

The port used to communicate with the DNS server

##### keyName

[Section titled “keyName”](#keyname)

Type: `String` · required

The key used to authenticate with the DNS server

##### key

[Section titled “key”](#key)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### protocol

[Section titled “protocol”](#protocol)

Type: [`IpProtocol`](#ipprotocol) · default: `"udp"`

The protocol used to communicate with the DNS server

##### tsigAlgorithm

[Section titled “tsigAlgorithm”](#tsigalgorithm)

Type: [`TsigAlgorithm`](#tsigalgorithm) · default: `"hmac-sha512"`

The TSIG algorithm used to authenticate with the DNS server

##### description

[Section titled “description”](#description-3)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-3)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-3)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

##### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### DnsServerCloudflare

[Section titled “DnsServerCloudflare”](#dnsservercloudflare)

Cloudflare DNS server.

##### secret

[Section titled “secret”](#secret-1)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-4)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-4)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-4)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-1)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-1)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-1)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-1)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerCloud

[Section titled “DnsServerCloud”](#dnsservercloud)

Cloud DNS server with token authentication.

##### secret

[Section titled “secret”](#secret-2)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-5)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-5)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-5)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-2)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-2)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-2)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-2)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerOvh

[Section titled “DnsServerOvh”](#dnsserverovh)

OVH DNS server.

##### applicationKey

[Section titled “applicationKey”](#applicationkey)

Type: `String` · required

The application key used to authenticate with the OVH DNS server

##### applicationSecret

[Section titled “applicationSecret”](#applicationsecret)

Type: [`SecretKey`](#secretkey) · required

The application secret used to authenticate with the OVH DNS server

##### consumerKey

[Section titled “consumerKey”](#consumerkey)

Type: [`SecretKey`](#secretkey) · required

The consumer key used to authenticate with the OVH DNS server

##### ovhEndpoint

[Section titled “ovhEndpoint”](#ovhendpoint)

Type: [`OvhEndpoint`](#ovhendpoint) · default: `"ovh-eu"`

Which OVH endpoint to use

##### description

[Section titled “description”](#description-6)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-6)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-6)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-3)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-3)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-3)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-3)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerPorkbun

[Section titled “DnsServerPorkbun”](#dnsserverporkbun)

Porkbun DNS server.

##### apiKey

[Section titled “apiKey”](#apikey)

Type: `String` · required

The API key used to authenticate with Porkbun

##### secretApiKey

[Section titled “secretApiKey”](#secretapikey)

Type: [`SecretKey`](#secretkey) · required

The secret API key used to authenticate with Porkbun

##### description

[Section titled “description”](#description-7)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-7)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-7)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-4)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-4)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-4)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-4)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerDnsimple

[Section titled “DnsServerDnsimple”](#dnsserverdnsimple)

DNSimple DNS server.

##### authToken

[Section titled “authToken”](#authtoken)

Type: [`SecretKey`](#secretkey) · required

The authentication token used to authenticate with DNSimple

##### accountIdentifier

[Section titled “accountIdentifier”](#accountidentifier)

Type: `String` · required

The account ID used to authenticate with DNSimple

##### description

[Section titled “description”](#description-8)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-8)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-8)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-5)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-5)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-5)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-5)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerSpaceship

[Section titled “DnsServerSpaceship”](#dnsserverspaceship)

Spaceship DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-1)

Type: `String` · required

The API key used to authenticate with Spaceship

##### secret

[Section titled “secret”](#secret-3)

Type: [`SecretKey`](#secretkey) · required

The secret or token used to authenticate with the DNS server

##### description

[Section titled “description”](#description-9)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-9)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-9)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-6)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-6)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-6)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-6)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerRoute53

[Section titled “DnsServerRoute53”](#dnsserverroute53)

AWS Route53 DNS server.

##### accessKeyId

[Section titled “accessKeyId”](#accesskeyid)

Type: `String` · required

The AWS access key ID

##### secretAccessKey

[Section titled “secretAccessKey”](#secretaccesskey)

Type: [`SecretKey`](#secretkey) · required

The AWS secret access key

##### sessionToken

[Section titled “sessionToken”](#sessiontoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional session token for temporary AWS credentials

##### region

[Section titled “region”](#region)

Type: `String` · default: `"us-east-1"`

The AWS region

##### hostedZoneId

[Section titled “hostedZoneId”](#hostedzoneid)

Type: `String?`

Hosted zone ID to use (resolved automatically by name if not set)

##### privateZoneOnly

[Section titled “privateZoneOnly”](#privatezoneonly)

Type: `Boolean` · default: `false`

Whether to restrict zone resolution to private zones only

##### description

[Section titled “description”](#description-10)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-10)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-10)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-7)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-7)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-7)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-7)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerGoogleCloudDns

[Section titled “DnsServerGoogleCloudDns”](#dnsservergoogleclouddns)

Google Cloud DNS server.

##### serviceAccountJson

[Section titled “serviceAccountJson”](#serviceaccountjson)

Type: [`SecretText`](#secrettext) · required

Service account JSON credentials used to authenticate with Google Cloud

##### projectId

[Section titled “projectId”](#projectid)

Type: `String` · required

The Google Cloud project ID that owns the managed zone

##### managedZone

[Section titled “managedZone”](#managedzone)

Type: `String?`

Managed zone name (resolved automatically by longest suffix match if not set)

##### privateZone

[Section titled “privateZone”](#privatezone)

Type: `Boolean` · default: `false`

Whether to restrict zone resolution to private zones only

##### impersonateServiceAccount

[Section titled “impersonateServiceAccount”](#impersonateserviceaccount)

Type: `String?`

Optional service account email to impersonate

##### description

[Section titled “description”](#description-11)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-11)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-11)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-8)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-8)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-8)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-8)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

##### SecretText

[Section titled “SecretText”](#secrettext)

A secret text value provided directly, from an environment variable, or from a file.

`Text`: Secret value. Carries the fields of [`SecretTextValue`](#secrettextvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretTextValue

[Section titled “SecretTextValue”](#secrettextvalue)

A secret text value provided directly.

##### secret

[Section titled “secret”](#secret-4)

Type: `Text` · required · secret

Password or secret value

#### DnsServerAlidns

[Section titled “DnsServerAlidns”](#dnsserveralidns)

Alibaba Cloud DNS server.

##### accessKey

[Section titled “accessKey”](#accesskey)

Type: `String` · required

The Alibaba Cloud access key ID

##### secretKey

[Section titled “secretKey”](#secretkey-1)

Type: [`SecretKey`](#secretkey) · required

The Alibaba Cloud access key secret

##### region

[Section titled “region”](#region-1)

Type: `String?`

Optional regional endpoint (defaults to the global endpoint)

##### securityToken

[Section titled “securityToken”](#securitytoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional STS security token for temporary credentials

##### line

[Section titled “line”](#line)

Type: `String?`

Optional ISP line identifier (used for split-resolution accounts)

##### description

[Section titled “description”](#description-12)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-12)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-12)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-9)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-9)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-9)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-9)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerAutodns

[Section titled “DnsServerAutodns”](#dnsserverautodns)

InterNetX AutoDNS server.

##### username

[Section titled “username”](#username)

Type: `String` · required

AutoDNS account username

##### password

[Section titled “password”](#password)

Type: [`SecretKey`](#secretkey) · required

AutoDNS account password

##### context

[Section titled “context”](#context)

Type: `UnsignedInt?`

Optional account context identifier

##### description

[Section titled “description”](#description-13)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-13)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-13)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-10)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-10)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-10)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-10)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerAzureDns

[Section titled “DnsServerAzureDns”](#dnsserverazuredns)

Microsoft Azure DNS server (OAuth2 client credentials).

##### tenantId

[Section titled “tenantId”](#tenantid)

Type: `String` · required

Azure Active Directory tenant ID

##### clientId

[Section titled “clientId”](#clientid)

Type: `String` · required

Application (client) ID

##### clientSecret

[Section titled “clientSecret”](#clientsecret)

Type: [`SecretKey`](#secretkey) · required

Application client secret

##### subscriptionId

[Section titled “subscriptionId”](#subscriptionid)

Type: `String` · required

Azure subscription ID that owns the DNS zone

##### resourceGroup

[Section titled “resourceGroup”](#resourcegroup)

Type: `String` · required

Resource group that contains the DNS zone

##### environment

[Section titled “environment”](#environment)

Type: [`AzureEnvironment`](#azureenvironment) · default: `"public"`

Azure cloud environment

##### description

[Section titled “description”](#description-14)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-14)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-14)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-11)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-11)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-11)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-11)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerBaiduCloud

[Section titled “DnsServerBaiduCloud”](#dnsserverbaiducloud)

Baidu Cloud DNS server.

##### accessKey

[Section titled “accessKey”](#accesskey-1)

Type: `String` · required

Baidu Cloud access key

##### secretKey

[Section titled “secretKey”](#secretkey-2)

Type: [`SecretKey`](#secretkey) · required

Baidu Cloud secret key

##### description

[Section titled “description”](#description-15)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-15)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-15)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-12)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-12)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-12)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-12)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerBluecatV2

[Section titled “DnsServerBluecatV2”](#dnsserverbluecatv2)

BlueCat Address Manager v2 REST API.

##### baseUrl

[Section titled “baseUrl”](#baseurl)

Type: `String` · required

Base URL of the BlueCat Address Manager

##### username

[Section titled “username”](#username-1)

Type: `String` · required

BlueCat account username

##### password

[Section titled “password”](#password-1)

Type: [`SecretKey`](#secretkey) · required

BlueCat account password

##### configName

[Section titled “configName”](#configname)

Type: `String` · required

BlueCat configuration name

##### viewName

[Section titled “viewName”](#viewname)

Type: `String` · required

BlueCat DNS view name

##### skipDeploy

[Section titled “skipDeploy”](#skipdeploy)

Type: `Boolean` · default: `false`

Skip deploying changes after applying them

##### description

[Section titled “description”](#description-16)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-16)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-16)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-13)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-13)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-13)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-13)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerClouDns

[Section titled “DnsServerClouDns”](#dnsservercloudns)

ClouDNS server.

##### authId

[Section titled “authId”](#authid)

Type: `String?`

ClouDNS auth ID (use either auth-id or sub-auth-id)

##### subAuthId

[Section titled “subAuthId”](#subauthid)

Type: `String?`

ClouDNS sub-auth ID

##### password

[Section titled “password”](#password-2)

Type: [`SecretKey`](#secretkey) · required

ClouDNS auth password

##### description

[Section titled “description”](#description-17)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-17)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-17)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-14)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-14)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-14)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-14)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerConstellix

[Section titled “DnsServerConstellix”](#dnsserverconstellix)

Constellix DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-2)

Type: `String` · required

Constellix API key

##### secretKey

[Section titled “secretKey”](#secretkey-3)

Type: [`SecretKey`](#secretkey) · required

Constellix secret key

##### description

[Section titled “description”](#description-18)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-18)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-18)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-15)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-15)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-15)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-15)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerCpanel

[Section titled “DnsServerCpanel”](#dnsservercpanel)

cPanel UAPI DNS server.

##### baseUrl

[Section titled “baseUrl”](#baseurl-1)

Type: `String` · required

Base URL of the cPanel server (e.g. [https://host:2083](https://host:2083))

##### username

[Section titled “username”](#username-2)

Type: `String` · required

cPanel account username

##### token

[Section titled “token”](#token)

Type: [`SecretKey`](#secretkey) · required

cPanel API token

##### description

[Section titled “description”](#description-19)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-19)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-19)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-16)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-16)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-16)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-16)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerDnsMadeEasy

[Section titled “DnsServerDnsMadeEasy”](#dnsserverdnsmadeeasy)

DNS Made Easy DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-3)

Type: `String` · required

DNS Made Easy API key

##### secret

[Section titled “secret”](#secret-5)

Type: [`SecretKey`](#secretkey) · required

DNS Made Easy API secret

##### description

[Section titled “description”](#description-20)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-20)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-20)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-17)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-17)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-17)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-17)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerDomeneshop

[Section titled “DnsServerDomeneshop”](#dnsserverdomeneshop)

Domeneshop DNS server.

##### authToken

[Section titled “authToken”](#authtoken-1)

Type: `String` · required

Domeneshop API token

##### secret

[Section titled “secret”](#secret-6)

Type: [`SecretKey`](#secretkey) · required

Domeneshop API secret

##### description

[Section titled “description”](#description-21)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-21)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-21)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-18)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-18)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-18)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-18)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerEasyDns

[Section titled “DnsServerEasyDns”](#dnsservereasydns)

EasyDNS REST API server.

##### token

[Section titled “token”](#token-1)

Type: `String` · required

EasyDNS token

##### key

[Section titled “key”](#key-1)

Type: [`SecretKey`](#secretkey) · required

EasyDNS key

##### description

[Section titled “description”](#description-22)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-22)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-22)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-19)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-19)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-19)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-19)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerEdgeDns

[Section titled “DnsServerEdgeDns”](#dnsserveredgedns)

Akamai EdgeDNS server.

##### host

[Section titled “host”](#host-5)

Type: `String` · required

Akamai API host

##### clientToken

[Section titled “clientToken”](#clienttoken)

Type: `String` · required

Akamai client token

##### clientSecret

[Section titled “clientSecret”](#clientsecret-1)

Type: [`SecretKey`](#secretkey) · required

Akamai client secret

##### accessToken

[Section titled “accessToken”](#accesstoken)

Type: [`SecretKey`](#secretkey) · required

Akamai access token

##### accountSwitchKey

[Section titled “accountSwitchKey”](#accountswitchkey)

Type: `String?`

Optional account switch key for managing multiple accounts

##### description

[Section titled “description”](#description-23)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-23)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-23)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-20)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-20)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-20)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-20)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerExoscale

[Section titled “DnsServerExoscale”](#dnsserverexoscale)

Exoscale DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-4)

Type: `String` · required

Exoscale API key

##### secret

[Section titled “secret”](#secret-7)

Type: [`SecretKey`](#secretkey) · required

Exoscale API secret

##### description

[Section titled “description”](#description-24)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-24)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-24)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-21)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-21)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-21)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-21)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerGlesys

[Section titled “DnsServerGlesys”](#dnsserverglesys)

GleSYS DNS server.

##### apiUser

[Section titled “apiUser”](#apiuser)

Type: `String` · required

GleSYS API user

##### apiKey

[Section titled “apiKey”](#apikey-5)

Type: [`SecretKey`](#secretkey) · required

GleSYS API key

##### description

[Section titled “description”](#description-25)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-25)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-25)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-22)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-22)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-22)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-22)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerGodaddy

[Section titled “DnsServerGodaddy”](#dnsservergodaddy)

GoDaddy DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-6)

Type: `String` · required

GoDaddy API key

##### secret

[Section titled “secret”](#secret-8)

Type: [`SecretKey`](#secretkey) · required

GoDaddy API secret

##### description

[Section titled “description”](#description-26)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-26)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-26)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-23)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-23)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-23)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-23)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerHuaweiCloud

[Section titled “DnsServerHuaweiCloud”](#dnsserverhuaweicloud)

Huawei Cloud DNS server.

##### accessKey

[Section titled “accessKey”](#accesskey-2)

Type: `String` · required

Huawei Cloud access key

##### secretKey

[Section titled “secretKey”](#secretkey-4)

Type: [`SecretKey`](#secretkey) · required

Huawei Cloud secret key

##### region

[Section titled “region”](#region-2)

Type: `String` · default: `"ap-southeast-1"`

Huawei Cloud region

##### description

[Section titled “description”](#description-27)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-27)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-27)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-24)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-24)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-24)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-24)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerHurricane

[Section titled “DnsServerHurricane”](#dnsserverhurricane)

Hurricane Electric free DNS service.

##### credentials

[Section titled “credentials”](#credentials)

Type: [`HurricaneCredential`](#hurricanecredential)`[]` · min items: 1

Per-zone Hurricane Electric DDNS keys

##### description

[Section titled “description”](#description-28)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-28)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-28)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-25)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-25)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-25)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-25)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

##### HurricaneCredential

[Section titled “HurricaneCredential”](#hurricanecredential)

Hurricane Electric per-zone DDNS credential.

##### zone

[Section titled “zone”](#zone)

Type: `DomainName` · required

DNS zone (origin) the credential applies to

##### secret

[Section titled “secret”](#secret-9)

Type: [`SecretKey`](#secretkey) · required

DDNS key for the zone

#### DnsServerIbmCloud

[Section titled “DnsServerIbmCloud”](#dnsserveribmcloud)

IBM Cloud (SoftLayer classic) DNS server.

##### username

[Section titled “username”](#username-3)

Type: `String` · required

IBM Cloud account username

##### apiKey

[Section titled “apiKey”](#apikey-7)

Type: [`SecretKey`](#secretkey) · required

IBM Cloud API key

##### description

[Section titled “description”](#description-29)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-29)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-29)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-26)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-26)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-26)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-26)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerInfoblox

[Section titled “DnsServerInfoblox”](#dnsserverinfoblox)

Infoblox NIOS WAPI server.

##### host

[Section titled “host”](#host-6)

Type: `String` · required

Infoblox grid master host

##### port

[Section titled “port”](#port-5)

Type: `String?`

Optional port (defaults to 443)

##### username

[Section titled “username”](#username-4)

Type: `String` · required

Infoblox account username

##### password

[Section titled “password”](#password-3)

Type: [`SecretKey`](#secretkey) · required

Infoblox account password

##### wapiVersion

[Section titled “wapiVersion”](#wapiversion)

Type: `String?`

WAPI version to use (defaults to 2.11)

##### dnsView

[Section titled “dnsView”](#dnsview)

Type: `String?`

DNS view name (defaults to External)

##### description

[Section titled “description”](#description-30)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-30)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-30)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-27)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-27)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-27)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-27)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerInwx

[Section titled “DnsServerInwx”](#dnsserverinwx)

INWX DNS server.

##### username

[Section titled “username”](#username-5)

Type: `String` · required

INWX account username

##### password

[Section titled “password”](#password-4)

Type: [`SecretKey`](#secretkey) · required

INWX account password

##### sharedSecret

[Section titled “sharedSecret”](#sharedsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional shared secret for TOTP-based two-factor authentication

##### sandbox

[Section titled “sandbox”](#sandbox)

Type: `Boolean` · default: `false`

Use the INWX sandbox API instead of production

##### description

[Section titled “description”](#description-31)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-31)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-31)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-28)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-28)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-28)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-28)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerJoker

[Section titled “DnsServerJoker”](#dnsserverjoker)

Joker DMAPI DNS server.

##### auth

[Section titled “auth”](#auth)

Type: [`JokerAuth`](#jokerauth) · required

Joker authentication method

##### description

[Section titled “description”](#description-32)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-32)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-32)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-29)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-29)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-29)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-29)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

##### JokerAuth

[Section titled “JokerAuth”](#jokerauth)

Joker DMAPI authentication credentials.

`ApiKey`: API Key. Carries the fields of [`JokerAuthApiKey`](#jokerauthapikey).
`UsernamePassword`: Username and Password. Carries the fields of [`JokerAuthUsernamePassword`](#jokerauthusernamepassword).

##### JokerAuthApiKey

[Section titled “JokerAuthApiKey”](#jokerauthapikey)

Joker API key authentication.

##### apiKey

[Section titled “apiKey”](#apikey-8)

Type: [`SecretKey`](#secretkey) · required

Joker DMAPI API key

##### JokerAuthUsernamePassword

[Section titled “JokerAuthUsernamePassword”](#jokerauthusernamepassword)

Joker username/password authentication.

##### username

[Section titled “username”](#username-6)

Type: `String` · required

Joker DMAPI account username

##### password

[Section titled “password”](#password-5)

Type: [`SecretKey`](#secretkey) · required

Joker DMAPI account password

#### DnsServerLightsail

[Section titled “DnsServerLightsail”](#dnsserverlightsail)

AWS Lightsail DNS server.

##### accessKeyId

[Section titled “accessKeyId”](#accesskeyid-1)

Type: `String` · required

AWS access key ID

##### secretAccessKey

[Section titled “secretAccessKey”](#secretaccesskey-1)

Type: [`SecretKey`](#secretkey) · required

AWS secret access key

##### sessionToken

[Section titled “sessionToken”](#sessiontoken-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional session token for temporary AWS credentials

##### region

[Section titled “region”](#region-3)

Type: `String?`

AWS region (defaults to us-east-1)

##### domain

[Section titled “domain”](#domain)

Type: `String?`

Optional Lightsail domain name to scope record operations to

##### description

[Section titled “description”](#description-33)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-33)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-33)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-30)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-30)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-30)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-30)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerLuaDns

[Section titled “DnsServerLuaDns”](#dnsserverluadns)

LuaDNS server.

##### username

[Section titled “username”](#username-7)

Type: `String` · required

LuaDNS account email or username

##### authToken

[Section titled “authToken”](#authtoken-2)

Type: [`SecretKey`](#secretkey) · required

LuaDNS API token

##### description

[Section titled “description”](#description-34)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-34)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-34)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-31)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-31)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-31)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-31)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerMythicBeasts

[Section titled “DnsServerMythicBeasts”](#dnsservermythicbeasts)

Mythic Beasts DNSv2 server.

##### username

[Section titled “username”](#username-8)

Type: `String` · required

Mythic Beasts API key ID

##### password

[Section titled “password”](#password-6)

Type: [`SecretKey`](#secretkey) · required

Mythic Beasts API key secret

##### description

[Section titled “description”](#description-35)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-35)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-35)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-32)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-32)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-32)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-32)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerNamecheap

[Section titled “DnsServerNamecheap”](#dnsservernamecheap)

Namecheap DNS server.

##### apiUser

[Section titled “apiUser”](#apiuser-1)

Type: `String` · required

Namecheap API user

##### apiKey

[Section titled “apiKey”](#apikey-9)

Type: [`SecretKey`](#secretkey) · required

Namecheap API key

##### clientIp

[Section titled “clientIp”](#clientip)

Type: `String` · required

Whitelisted client IP address registered with Namecheap

##### username

[Section titled “username”](#username-9)

Type: `String?`

Optional account username (defaults to the API user)

##### description

[Section titled “description”](#description-36)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-36)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-36)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-33)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-33)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-33)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-33)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerNameDotCom

[Section titled “DnsServerNameDotCom”](#dnsservernamedotcom)

Name.com v4 DNS server.

##### username

[Section titled “username”](#username-10)

Type: `String` · required

Name.com account username

##### authToken

[Section titled “authToken”](#authtoken-3)

Type: [`SecretKey`](#secretkey) · required

Name.com API token

##### description

[Section titled “description”](#description-37)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-37)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-37)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-34)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-34)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-34)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-34)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerNetcup

[Section titled “DnsServerNetcup”](#dnsservernetcup)

Netcup CCP DNS server.

##### customerNumber

[Section titled “customerNumber”](#customernumber)

Type: `String` · required

Netcup customer number

##### apiKey

[Section titled “apiKey”](#apikey-10)

Type: `String` · required

Netcup API key

##### password

[Section titled “password”](#password-7)

Type: [`SecretKey`](#secretkey) · required

Netcup API password

##### description

[Section titled “description”](#description-38)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-38)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-38)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-35)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-35)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-35)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-35)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerNifcloud

[Section titled “DnsServerNifcloud”](#dnsservernifcloud)

Nifcloud DNS server.

##### accessKey

[Section titled “accessKey”](#accesskey-3)

Type: `String` · required

Nifcloud access key

##### secretKey

[Section titled “secretKey”](#secretkey-5)

Type: [`SecretKey`](#secretkey) · required

Nifcloud secret key

##### description

[Section titled “description”](#description-39)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-39)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-39)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-36)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-36)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-36)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-36)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerOracleCloud

[Section titled “DnsServerOracleCloud”](#dnsserveroraclecloud)

Oracle Cloud Infrastructure DNS server.

##### tenancyOcid

[Section titled “tenancyOcid”](#tenancyocid)

Type: `String` · required

Tenancy OCID

##### userOcid

[Section titled “userOcid”](#userocid)

Type: `String` · required

User OCID

##### fingerprint

[Section titled “fingerprint”](#fingerprint)

Type: `String` · required

API signing key fingerprint

##### privateKeyPem

[Section titled “privateKeyPem”](#privatekeypem)

Type: [`SecretText`](#secrettext) · required

API signing private key in PEM format

##### privateKeyPassword

[Section titled “privateKeyPassword”](#privatekeypassword)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional passphrase for the private key

##### region

[Section titled “region”](#region-4)

Type: `String` · required

OCI region (e.g. us-ashburn-1)

##### compartmentOcid

[Section titled “compartmentOcid”](#compartmentocid)

Type: `String` · required

Compartment OCID that owns the DNS zone

##### description

[Section titled “description”](#description-40)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-40)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-40)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-37)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-37)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-37)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-37)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerPlesk

[Section titled “DnsServerPlesk”](#dnsserverplesk)

Plesk REST API DNS server.

##### baseUrl

[Section titled “baseUrl”](#baseurl-2)

Type: `String` · required

Base URL of the Plesk server (e.g. [https://host:8443](https://host:8443))

##### apiKey

[Section titled “apiKey”](#apikey-11)

Type: [`SecretKey`](#secretkey) · required

Plesk API key

##### description

[Section titled “description”](#description-41)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-41)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-41)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-38)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-38)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-38)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-38)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerTencentCloud

[Section titled “DnsServerTencentCloud”](#dnsservertencentcloud)

Tencent Cloud DNSPod server.

##### secretId

[Section titled “secretId”](#secretid)

Type: `String` · required

Tencent Cloud secret ID

##### secretKey

[Section titled “secretKey”](#secretkey-6)

Type: [`SecretKey`](#secretkey) · required

Tencent Cloud secret key

##### region

[Section titled “region”](#region-5)

Type: `String?`

Optional regional endpoint

##### sessionToken

[Section titled “sessionToken”](#sessiontoken-2)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Optional STS session token for temporary credentials

##### description

[Section titled “description”](#description-42)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-42)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-42)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-39)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-39)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-39)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-39)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerTransip

[Section titled “DnsServerTransip”](#dnsservertransip)

TransIP v6 DNS server.

##### username

[Section titled “username”](#username-11)

Type: `String` · required

TransIP account login

##### privateKeyPem

[Section titled “privateKeyPem”](#privatekeypem-1)

Type: [`SecretText`](#secrettext) · required

TransIP private key in PEM format

##### description

[Section titled “description”](#description-43)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-43)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-43)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-40)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-40)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-40)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-40)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerUltraDns

[Section titled “DnsServerUltraDns”](#dnsserverultradns)

UltraDNS REST API server.

##### username

[Section titled “username”](#username-12)

Type: `String` · required

UltraDNS account username

##### password

[Section titled “password”](#password-8)

Type: [`SecretKey`](#secretkey) · required

UltraDNS account password

##### endpoint

[Section titled “endpoint”](#endpoint)

Type: `String?`

Optional REST API endpoint override

##### description

[Section titled “description”](#description-44)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-44)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-44)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-41)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-41)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-41)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-41)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerVercel

[Section titled “DnsServerVercel”](#dnsserververcel)

Vercel DNS server.

##### authToken

[Section titled “authToken”](#authtoken-4)

Type: [`SecretKey`](#secretkey) · required

Vercel auth token

##### teamId

[Section titled “teamId”](#teamid)

Type: `String?`

Optional team ID to scope API requests to

##### description

[Section titled “description”](#description-45)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-45)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-45)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-42)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-42)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-42)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-42)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerVolcengine

[Section titled “DnsServerVolcengine”](#dnsservervolcengine)

Volcano Engine DNS server.

##### accessKey

[Section titled “accessKey”](#accesskey-4)

Type: `String` · required

Volcengine access key

##### secretKey

[Section titled “secretKey”](#secretkey-7)

Type: [`SecretKey`](#secretkey) · required

Volcengine secret key

##### region

[Section titled “region”](#region-6)

Type: `String?`

Optional regional endpoint

##### host

[Section titled “host”](#host-7)

Type: `String?`

Optional API host override

##### scheme

[Section titled “scheme”](#scheme)

Type: `String?`

Optional URL scheme (http or https)

##### description

[Section titled “description”](#description-46)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-46)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-46)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-43)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-43)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-43)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-43)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerWebSupport

[Section titled “DnsServerWebSupport”](#dnsserverwebsupport)

WebSupport DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-12)

Type: `String` · required

WebSupport API key

##### secret

[Section titled “secret”](#secret-10)

Type: [`SecretKey`](#secretkey) · required

WebSupport API secret

##### description

[Section titled “description”](#description-47)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-47)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-47)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-44)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-44)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-44)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-44)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

#### DnsServerYandexCloud

[Section titled “DnsServerYandexCloud”](#dnsserveryandexcloud)

Yandex Cloud DNS server.

##### apiKey

[Section titled “apiKey”](#apikey-13)

Type: [`SecretText`](#secrettext) · required

Base64-encoded IAM service account key JSON

##### folderId

[Section titled “folderId”](#folderid)

Type: `String` · required

Yandex Cloud folder ID that owns the DNS zone

##### description

[Section titled “description”](#description-48)

Type: `String` · required

Short description of this DNS server

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-48)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this DNS server belongs to

##### timeout

[Section titled “timeout”](#timeout-48)

Type: `Duration` · default: `"30s"`

Request timeout for the DNS server

##### ttl

[Section titled “ttl”](#ttl-45)

Type: `Duration` · default: `"5m"`

The TTL for new DNS record

##### pollingInterval

[Section titled “pollingInterval”](#pollinginterval-45)

Type: `Duration` · default: `"15s"`

How often to check for DNS records to propagate

##### propagationTimeout

[Section titled “propagationTimeout”](#propagationtimeout-45)

Type: `Duration` · default: `"1m"`

How long to wait for DNS records to propagate

##### propagationDelay

[Section titled “propagationDelay”](#propagationdelay-45)

Type: `Duration?`

Initial delay before first propagation check (useful for slow providers)

## Enums

[Section titled “Enums”](#enums)

### PostgreSqlRecyclingMethod

[Section titled “PostgreSqlRecyclingMethod”](#postgresqlrecyclingmethod)
ValueLabel`fast`Fast recycling method`verified`Verified recycling method`clean`Clean recycling method

### IpProtocol

[Section titled “IpProtocol”](#ipprotocol)
ValueLabel`udp`UDP`tcp`TCP

### TsigAlgorithm

[Section titled “TsigAlgorithm”](#tsigalgorithm-1)
ValueLabel`hmac-md5`HMAC-MD5`gss`GSS`hmac-sha1`HMAC-SHA1`hmac-sha224`HMAC-SHA224`hmac-sha256`HMAC-SHA256`hmac-sha256-128`HMAC-SHA256-128`hmac-sha384`HMAC-SHA384`hmac-sha384-192`HMAC-SHA384-192`hmac-sha512`HMAC-SHA512`hmac-sha512-256`HMAC-SHA512-256

### OvhEndpoint

[Section titled “OvhEndpoint”](#ovhendpoint-1)
ValueLabel`ovh-eu`OVH EU`ovh-ca`OVH CA`kimsufi-eu`Kimsufi EU`kimsufi-ca`Kimsufi CA`soyoustart-eu`Soyoustart EU`soyoustart-ca`Soyoustart CA

### AzureEnvironment

[Section titled “AzureEnvironment”](#azureenvironment)
ValueLabel`public`Public`china`China`us-government`US Government