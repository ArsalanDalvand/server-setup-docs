---
title: "AccountPassword"
source: "https://stalw.art/docs/ref/object/account-password"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/account-password"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AccountPassword"
---
# AccountPassword

      

Password-based authentication credential.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Credentials › Password

## Fields

[Section titled “Fields”](#fields)

##### secret

[Section titled “secret”](#secret)

Type: `String?` · secret

Secret value of the account

##### currentSecret

[Section titled “currentSecret”](#currentsecret)

Type: `String?` · secret

The current secret value of the account, used for password verification.

##### otpAuth

[Section titled “otpAuth”](#otpauth)

Type: [`OtpAuth`](#otpauth) · required

OTP authentication settings for the account

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The AccountPassword singleton is available via the `urn:stalwart:jmap` capability.

### x:AccountPassword/get

[Section titled “x:AccountPassword/get”](#xaccountpasswordget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysAccountPasswordGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AccountPassword/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AccountPassword/set

[Section titled “x:AccountPassword/set”](#xaccountpasswordset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysAccountPasswordUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AccountPassword/set",          {            "update": {              "singleton": {                "secret": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get AccountPassword
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update AccountPassword --field secret='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### OtpAuth

[Section titled “OtpAuth”](#otpauth-1)

OTP-based authentication credential.

##### otpCode

[Section titled “otpCode”](#otpcode)

Type: `String?` · secret

OTP code for the account, required for credential changes.

##### otpUrl

[Section titled “otpUrl”](#otpurl)

Type: `Uri?` · secret

OTP authentication URI for the account