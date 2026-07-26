---
title: "Enterprise"
source: "https://stalw.art/docs/ref/object/enterprise"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/enterprise"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Enterprise"
---
# Enterprise

      

Configures enterprise licensing and branding settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Enterprise

## Fields

[Section titled “Fields”](#fields)

##### apiKey

[Section titled “apiKey”](#apikey)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

API key for license retrieval and automatic renewals. Obtain your API key at [https://license.stalw.art](https://license.stalw.art).

##### licenseKey

[Section titled “licenseKey”](#licensekey)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Upgrade to the enterprise version of Stalwart by entering your license key here. Obtain your license at [https://license.stalw.art/buy](https://license.stalw.art/buy)

##### logoUrl

[Section titled “logoUrl”](#logourl)

Type: `Uri?` · [[server/enterprise|enterprise]]

URL to the default logo to use in the Webadmin interface.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Enterprise singleton is available via the `urn:stalwart:jmap` capability.

### x:Enterprise/get

[Section titled “x:Enterprise/get”](#xenterpriseget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysEnterpriseGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Enterprise/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Enterprise/set

[Section titled “x:Enterprise/set”](#xenterpriseset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysEnterpriseUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Enterprise/set",          {            "update": {              "singleton": {                "apiKey": {                  "@type": "None"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Enterprise
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Enterprise --field apiKey='{"@type":"None"}'
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