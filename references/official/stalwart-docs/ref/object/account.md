---
title: "Account"
source: "https://stalw.art/docs/ref/object/account"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/account"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Account"
---
# Account

      

Defines a user or group account for authentication and email access.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Directory › Groups Management ›  Directory › Accounts

## Fields

[Section titled “Fields”](#fields)

Account is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "User"

[Section titled “@type: "User"”](#type-user)

User account

##### name

[Section titled “name”](#name)

Type: `EmailLocalPart` · required

Name of the account, typically an email address local part.

##### domainId

[Section titled “domainId”](#domainid)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this account belongs to. This is used to determine the email address of the account, which is formed as name@domain.

##### emailAddress

[Section titled “emailAddress”](#emailaddress)

Type: `EmailAddress` · server-set

Email address for the user account, formed as name@domain.

##### credentials

[Section titled “credentials”](#credentials)

Type: [`Credential`](#credential)`[]`

List of credential objects representing authentication methods for the account

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

Creation date of the account

##### memberGroupIds

[Section titled “memberGroupIds”](#membergroupids)

Type: `Id<`[[ref/object/account|`Account`]]`>[]`

List of groups that this account is a member of

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this account belongs to

##### roles

[Section titled “roles”](#roles)

Type: [`UserRoles`](#userroles) · required

Roles assigned to this user account

##### permissions

[Section titled “permissions”](#permissions)

Type: [`Permissions`](#permissions) · required

Permissions assigned to this account

##### quotas

[Section titled “quotas”](#quotas)

Type: `Map<`[`StorageQuota`](#storagequota)`, UnsignedInt>`

Quotas for different object types within this account

##### usedDiskQuota

[Section titled “usedDiskQuota”](#useddiskquota)

Type: `Size` · server-set

Amount of disk space currently used by this account (bytes)

##### aliases

[Section titled “aliases”](#aliases)

Type: [`EmailAlias`](#emailalias)`[]`

List of email aliases for the account

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the account

##### locale

[Section titled “locale”](#locale)

Type: [[ref/enum/locale|`Locale`]] · default: `"en_US"`

Preferred locale for the account

##### timeZone

[Section titled “timeZone”](#timezone)

Type: [[ref/enum/time-zone|`TimeZone`]]`?`

Preferred time zone for the account

##### encryptionAtRest

[Section titled “encryptionAtRest”](#encryptionatrest)

Type: [`EncryptionAtRest`](#encryptionatrest) · required

Encryption-at-rest settings for the account

### @type: "Group"

[Section titled “@type: "Group"”](#type-group)

Group account

##### name

[Section titled “name”](#name-1)

Type: `EmailLocalPart` · required

Name of the group, typically an email address local part.

##### domainId

[Section titled “domainId”](#domainid-1)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this group belongs to. This is used to determine the email address of the group, which is formed as name@domain.

##### emailAddress

[Section titled “emailAddress”](#emailaddress-1)

Type: `EmailAddress` · server-set

Email address of the group, formed as name@domain.

##### description

[Section titled “description”](#description-1)

Type: `String?`

Description of the group

##### createdAt

[Section titled “createdAt”](#createdat-1)

Type: `UTCDateTime` · server-set

Creation date of the account

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-1)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this group belongs to

##### roles

[Section titled “roles”](#roles-1)

Type: [`Roles`](#roles) · required

Roles assigned to this group

##### quotas

[Section titled “quotas”](#quotas-1)

Type: `Map<`[`StorageQuota`](#storagequota)`, UnsignedInt>`

Quotas for different object types within this group

##### usedDiskQuota

[Section titled “usedDiskQuota”](#useddiskquota-1)

Type: `Size` · server-set

Amount of disk space currently used by this account (bytes)

##### permissions

[Section titled “permissions”](#permissions-1)

Type: [`Permissions`](#permissions) · required

Permissions assigned to this group

##### aliases

[Section titled “aliases”](#aliases-1)

Type: [`EmailAlias`](#emailalias)`[]`

List of email aliases for the group

##### locale

[Section titled “locale”](#locale-1)

Type: [[ref/enum/locale|`Locale`]] · default: `"en_US"`

Preferred locale for the group

##### timeZone

[Section titled “timeZone”](#timezone-1)

Type: [[ref/enum/time-zone|`TimeZone`]]`?`

Preferred time zone for the account

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Account object is available via the `urn:stalwart:jmap` capability.

### x:Account/get

[Section titled “x:Account/get”](#xaccountget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysAccountGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Account/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Account/set

[Section titled “x:Account/set”](#xaccountset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysAccountCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Account/set",          {            "create": {              "new1": {                "@type": "User",                "aliases": {},                "credentials": {},                "domainId": "<Domain id>",                "encryptionAtRest": {                  "@type": "Disabled"                },                "memberGroupIds": {},                "name": "alice",                "permissions": {                  "@type": "Inherit"                },                "quotas": {},                "roles": {                  "@type": "User"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysAccountUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Account/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysAccountDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Account/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Account/query

[Section titled “x:Account/query”](#xaccountquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysAccountQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Account/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Account/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`name`text`domainId`id of Domain`memberTenantId`id of Tenant`memberGroupIds`id of Account/Group

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Account id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Account/User \  --field name=alice \  --field 'domainId=<Domain id>' \  --field 'credentials={}' \  --field 'memberGroupIds={}' \  --field 'roles={"@type":"User"}' \  --field 'permissions={"@type":"Inherit"}' \  --field 'quotas={}' \  --field 'aliases={}' \  --field 'encryptionAtRest={"@type":"Disabled"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Account
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Account id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Account --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Credential

[Section titled “Credential”](#credential)

Defines an authentication credential for an account.

`Password`: Password for authenticating to the account. Carries the fields of [`PasswordCredential`](#passwordcredential).
`AppPassword`: App password for third-party applications. Carries the fields of [`SecondaryCredential`](#secondarycredential).
`ApiKey`: API key for programmatic access. Carries the fields of [`SecondaryCredential`](#secondarycredential).

#### PasswordCredential

[Section titled “PasswordCredential”](#passwordcredential)

Password-based authentication credential.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

Secret value of the account

##### otpAuth

[Section titled “otpAuth”](#otpauth)

Type: `Uri?` · secret

OTP authentication URI for the account

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?`

Expiration date of the credential

##### allowedIps

[Section titled “allowedIps”](#allowedips)

Type: `IpMask[]`

List of allowed IP addresses or CIDR ranges for this credential

#### SecondaryCredential

[Section titled “SecondaryCredential”](#secondarycredential)

App password or API key credential for programmatic access.

##### description

[Section titled “description”](#description-2)

Type: `String` · required

Description of the credential

##### secret

[Section titled “secret”](#secret-1)

Type: `String` · read-only · server-set · secret

Secret value of the credential

##### createdAt

[Section titled “createdAt”](#createdat-2)

Type: `UTCDateTime` · read-only · server-set

Creation date of the credential

##### expiresAt

[Section titled “expiresAt”](#expiresat-1)

Type: `UTCDateTime?`

Expiration date of the credential

##### permissions

[Section titled “permissions”](#permissions-2)

Type: [`CredentialPermissions`](#credentialpermissions) · required

List of permissions assigned to this credential

##### allowedIps

[Section titled “allowedIps”](#allowedips-1)

Type: `IpMask[]`

List of allowed IP addresses or CIDR ranges for this credential

##### CredentialPermissions

[Section titled “CredentialPermissions”](#credentialpermissions)

Permission assignment mode for a credential.

`Inherit`: Same permissions as account. No additional fields.
`Disable`: Disable some permissions. Carries the fields of [`CredentialPermissionsList`](#credentialpermissionslist).
`Replace`: Replace all permissions. Carries the fields of [`CredentialPermissionsList`](#credentialpermissionslist).

##### CredentialPermissionsList

[Section titled “CredentialPermissionsList”](#credentialpermissionslist)

List of permissions to assign to a credential.

##### permissions

[Section titled “permissions”](#permissions-3)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions to assign.

### UserRoles

[Section titled “UserRoles”](#userroles)

Role assignment for user accounts.

`User`: User role. No additional fields.
`Admin`: Administrator role. No additional fields.
`Custom`: Custom role. Carries the fields of [`CustomRoles`](#customroles).

#### CustomRoles

[Section titled “CustomRoles”](#customroles)

Custom role assignment with specific role references.

##### roleIds

[Section titled “roleIds”](#roleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]`

List of roles assigned to this principal.

### Permissions

[Section titled “Permissions”](#permissions-4)

Permission assignment mode for accounts, groups, and tenants.

`Inherit`: Inherited permissions. No additional fields.
`Merge`: Permissions are combined with inherited permissions. Carries the fields of [`PermissionsList`](#permissionslist).
`Replace`: Permissions replace all inherited permissions. Carries the fields of [`PermissionsList`](#permissionslist).

#### PermissionsList

[Section titled “PermissionsList”](#permissionslist)

Explicit permission grants and denials.

##### enabledPermissions

[Section titled “enabledPermissions”](#enabledpermissions)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions that are explicitly enabled.

##### disabledPermissions

[Section titled “disabledPermissions”](#disabledpermissions)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions that are explicitly disabled, even if they would be inherited through other roles or groups. This takes precedence over enabled permissions.

### EmailAlias

[Section titled “EmailAlias”](#emailalias)

Defines an email alias for an account or mailing list.

##### enabled

[Section titled “enabled”](#enabled)

Type: `Boolean` · default: `true`

Whether this email alias is enabled

##### name

[Section titled “name”](#name-2)

Type: `EmailLocalPart` · required

The local part of the email alias (the part before the @ symbol)

##### domainId

[Section titled “domainId”](#domainid-2)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain of the email alias (the part after the @ symbol).

##### description

[Section titled “description”](#description-3)

Type: `String?`

Description of the email alias

### EncryptionAtRest

[Section titled “EncryptionAtRest”](#encryptionatrest-1)

Encryption-at-rest algorithm selection.

`Disabled`: Disabled. No additional fields.
`Aes128`: AES-128. Carries the fields of [`EncryptionSettings`](#encryptionsettings).
`Aes256`: AES-256. Carries the fields of [`EncryptionSettings`](#encryptionsettings).
`Aes256Gcm`: AES-256-GCM (S/MIME only). Carries the fields of [`EncryptionSettings`](#encryptionsettings).
`ChaCha20Poly1305`: ChaCha20-Poly1305 (S/MIME only). Carries the fields of [`EncryptionSettings`](#encryptionsettings).

#### EncryptionSettings

[Section titled “EncryptionSettings”](#encryptionsettings)

Encryption-at-rest settings for an account.

##### publicKey

[Section titled “publicKey”](#publickey)

Type: `Id<`[[ref/object/public-key|`PublicKey`]]`>` · required

Public key used for encrypting emails

##### encryptOnAppend

[Section titled “encryptOnAppend”](#encryptonappend)

Type: `Boolean` · default: `false`

Whether to encrypt emails when they are appended to mailboxes

##### allowSpamTraining

[Section titled “allowSpamTraining”](#allowspamtraining)

Type: `Boolean` · default: `false`

Whether to allow training the spam classifier with plaintext emails before encryption

### Roles

[Section titled “Roles”](#roles-2)

Role assignment for groups and tenants.

`Default`: Default role. No additional fields.
`Custom`: Custom role. Carries the fields of [`CustomRoles`](#customroles).

## Enums

[Section titled “Enums”](#enums)

### StorageQuota

[Section titled “StorageQuota”](#storagequota)
ValueLabel`maxEmails`Maximum number of emails`maxMailboxes`Maximum number of mailboxes`maxEmailSubmissions`Maximum number of email submissions`maxEmailIdentities`Maximum number of email identities`maxParticipantIdentities`Maximum number of participant identities`maxSieveScripts`Maximum number of Sieve scripts`maxPushSubscriptions`Maximum number of push subscriptions`maxCalendars`Maximum number of calendars`maxCalendarEvents`Maximum number of calendar events`maxCalendarEventNotifications`Maximum number of calendar event notifications`maxAddressBooks`Maximum number of address books`maxContactCards`Maximum number of contact cards`maxFiles`Maximum number of files`maxFolders`Maximum number of folders`maxMaskedAddresses`Maximum number of masked email addresses`maxAppPasswords`Maximum number of app passwords`maxApiKeys`Maximum number of API keys`maxPublicKeys`Maximum number of public keys`maxDiskQuota`Maximum disk space allocated (bytes)