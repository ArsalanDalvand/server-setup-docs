---
title: "AccountSettings"
source: "https://stalw.art/docs/ref/object/account-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/account-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AccountSettings"
---
# AccountSettings

      

Configures default account settings for locale and encryption.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Settings

## Fields

[Section titled “Fields”](#fields)

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

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The AccountSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:AccountSettings/get

[Section titled “x:AccountSettings/get”](#xaccountsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysAccountSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AccountSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AccountSettings/set

[Section titled “x:AccountSettings/set”](#xaccountsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysAccountSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AccountSettings/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get AccountSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update AccountSettings --field description='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

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