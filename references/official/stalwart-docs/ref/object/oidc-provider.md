---
title: "OidcProvider"
source: "https://stalw.art/docs/ref/object/oidc-provider"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/oidc-provider"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "OidcProvider"
---
# OidcProvider

      

Configures the OAuth and OpenID Connect provider settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Authentication › OIDC Provider

## Fields

[Section titled “Fields”](#fields)

##### authCodeMaxAttempts

[Section titled “authCodeMaxAttempts”](#authcodemaxattempts)

Type: `UnsignedInt` · default: `3` · min: 1 · max: 1000

Number of failed login attempts before an authorization code is invalidated

##### anonymousClientRegistration

[Section titled “anonymousClientRegistration”](#anonymousclientregistration)

Type: `Boolean` · default: `true`

Whether to allow OAuth clients to register without authentication

##### requireClientRegistration

[Section titled “requireClientRegistration”](#requireclientregistration)

Type: `Boolean` · default: `false`

Whether to require OAuth client_ids to be registered before they can be used

##### authCodeExpiry

[Section titled “authCodeExpiry”](#authcodeexpiry)

Type: `Duration` · default: `"10m"`

Expiration time of an authorization code issued by the authorization code flow

##### refreshTokenExpiry

[Section titled “refreshTokenExpiry”](#refreshtokenexpiry)

Type: `Duration` · default: `"30d"`

Expiration time of an OAuth refresh token

##### refreshTokenRenewal

[Section titled “refreshTokenRenewal”](#refreshtokenrenewal)

Type: `Duration` · default: `"4d"`

Remaining time in a refresh token before a new one is issued to the client

##### accessTokenExpiry

[Section titled “accessTokenExpiry”](#accesstokenexpiry)

Type: `Duration` · default: `"1h"`

Expiration time of an OAuth access token

##### userCodeExpiry

[Section titled “userCodeExpiry”](#usercodeexpiry)

Type: `Duration` · default: `"30m"`

Expiration time of a user code issued by the device authentication flow

##### idTokenExpiry

[Section titled “idTokenExpiry”](#idtokenexpiry)

Type: `Duration` · default: `"15m"`

Expiration time of an OpenID Connect ID token

##### encryptionKey

[Section titled “encryptionKey”](#encryptionkey)

Type: [`SecretKey`](#secretkey) · required

Encryption key to use for OAuth

##### signatureAlgorithm

[Section titled “signatureAlgorithm”](#signaturealgorithm)

Type: [`JwtSignatureAlgorithm`](#jwtsignaturealgorithm) · default: `"hs256"`

JWT signature algorithm to use for OpenID Connect.

##### signatureKey

[Section titled “signatureKey”](#signaturekey)

Type: [`SecretText`](#secrettext) · required

Contents of the private key PEM used to sign JWTs for OpenID Connect.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The OidcProvider singleton is available via the `urn:stalwart:jmap` capability.

### x:OidcProvider/get

[Section titled “x:OidcProvider/get”](#xoidcproviderget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysOidcProviderGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OidcProvider/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:OidcProvider/set

[Section titled “x:OidcProvider/set”](#xoidcproviderset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysOidcProviderUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OidcProvider/set",          {            "update": {              "singleton": {                "authCodeMaxAttempts": 3              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get OidcProvider
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update OidcProvider --field authCodeMaxAttempts=3
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

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

### SecretText

[Section titled “SecretText”](#secrettext)

A secret text value provided directly, from an environment variable, or from a file.

`Text`: Secret value. Carries the fields of [`SecretTextValue`](#secrettextvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretTextValue

[Section titled “SecretTextValue”](#secrettextvalue)

A secret text value provided directly.

##### secret

[Section titled “secret”](#secret-1)

Type: `Text` · required · secret

Password or secret value

## Enums

[Section titled “Enums”](#enums)

### JwtSignatureAlgorithm

[Section titled “JwtSignatureAlgorithm”](#jwtsignaturealgorithm)
ValueLabel`es256`ECDSA using P-256 and SHA-256`es384`ECDSA using P-384 and SHA-384`ps256`RSASSA-PSS using SHA-256 and MGF1 with SHA-256`ps384`RSASSA-PSS using SHA-384 and MGF1 with SHA-384`ps512`RSASSA-PSS using SHA-512 and MGF1 with SHA-512`rs256`RSASSA-PKCS1-v1_5 using SHA-256`rs384`RSASSA-PKCS1-v1_5 using SHA-384`rs512`RSASSA-PKCS1-v1_5 using SHA-512`hs256`HMAC using SHA-256`hs384`HMAC using SHA-384`hs512`HMAC using SHA-512