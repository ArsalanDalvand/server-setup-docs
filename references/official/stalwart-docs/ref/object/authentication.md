---
title: "Authentication"
source: "https://stalw.art/docs/ref/object/authentication"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/authentication"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Authentication"
---
# Authentication

      

Configures authentication settings including password policies and default roles.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Authentication › General

## Fields

[Section titled “Fields”](#fields)

##### directoryId

[Section titled “directoryId”](#directoryid)

Type: `Id<`[[ref/object/directory|`Directory`]]`>?`

External directory used for authentication, or null to use the internal directory

##### defaultUserRoleIds

[Section titled “defaultUserRoleIds”](#defaultuserroleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]`

Default roles to assign for accounts.

##### defaultGroupRoleIds

[Section titled “defaultGroupRoleIds”](#defaultgrouproleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]`

Default roles to assign for groups.

##### defaultTenantRoleIds

[Section titled “defaultTenantRoleIds”](#defaulttenantroleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]` · [[server/enterprise|enterprise]]

Default roles to assign for tenants in multi-tenant environments.

##### defaultAdminRoleIds

[Section titled “defaultAdminRoleIds”](#defaultadminroleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]`

Default roles to assign for administrators.

##### passwordHashAlgorithm

[Section titled “passwordHashAlgorithm”](#passwordhashalgorithm)

Type: [`PasswordHashAlgorithm`](#passwordhashalgorithm) · default: `"argon2id"`

Password hashing algorithm to use for storing user passwords in the internal directory.

##### passwordMinLength

[Section titled “passwordMinLength”](#passwordminlength)

Type: `UnsignedInt` · default: `8` · min: 1 · max: 100

Minimum length for user passwords in the internal directory.

##### passwordMaxLength

[Section titled “passwordMaxLength”](#passwordmaxlength)

Type: `UnsignedInt` · default: `128` · min: 1 · max: 1000

Maximum length for user passwords in the internal directory.

##### passwordMinStrength

[Section titled “passwordMinStrength”](#passwordminstrength)

Type: [`PasswordStrength`](#passwordstrength) · default: `"three"`

Minimum strength for user passwords in the internal directory, calculated using the zxcvbn algorithm.

##### passwordDefaultExpiry

[Section titled “passwordDefaultExpiry”](#passworddefaultexpiry)

Type: `Duration?`

Default expiration time for user passwords in the internal directory, after which the user will be required to change their password.

##### maxAppPasswords

[Section titled “maxAppPasswords”](#maxapppasswords)

Type: `UnsignedInt?` · default: `5` · min: 1

The default maximum number of app passwords a user can create

##### maxApiKeys

[Section titled “maxApiKeys”](#maxapikeys)

Type: `UnsignedInt?` · default: `5` · min: 1

The default maximum number of API keys a user can create

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Authentication singleton is available via the `urn:stalwart:jmap` capability.

### x:Authentication/get

[Section titled “x:Authentication/get”](#xauthenticationget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysAuthenticationGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Authentication/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Authentication/set

[Section titled “x:Authentication/set”](#xauthenticationset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysAuthenticationUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Authentication/set",          {            "update": {              "singleton": {                "directoryId": "<Directory id>"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Authentication
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Authentication --field directoryId='<Directory id>'
```

## Enums

[Section titled “Enums”](#enums)

### PasswordHashAlgorithm

[Section titled “PasswordHashAlgorithm”](#passwordhashalgorithm-1)
ValueLabel`argon2id`Argon2id`bcrypt`Bcrypt`scrypt`Scrypt`pbkdf2`Pbkdf2

### PasswordStrength

[Section titled “PasswordStrength”](#passwordstrength)
ValueLabel`zero`Too guessable: risky password. (guesses < 10^3)`one`Very guessable: protection from throttled online attacks. (guesses < 10^6)`two`Somewhat guessable: protection from unthrottled online attacks. (guesses < 10^8)`three`Safely unguessable: moderate protection from offline slow-hash scenario. (guesses < 10^10)`four`Very unguessable: strong protection from offline slow-hash scenario. (guesses >= 10^10)