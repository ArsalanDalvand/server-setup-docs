---
title: "Certificate"
source: "https://stalw.art/docs/ref/object/certificate"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/certificate"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Certificate"
---
# Certificate

      

Defines a TLS certificate and its associated private key.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  TLS › Certificates

## Fields

[Section titled “Fields”](#fields)

##### certificate

[Section titled “certificate”](#certificate)

Type: [`PublicText`](#publictext) · required

TLS certificate in PEM format

##### privateKey

[Section titled “privateKey”](#privatekey)

Type: [`SecretText`](#secrettext) · required

Private key in PEM format

##### subjectAlternativeNames

[Section titled “subjectAlternativeNames”](#subjectalternativenames)

Type: `String[]` · server-set

Subject Alternative Names (SAN) for the certificate

##### notValidAfter

[Section titled “notValidAfter”](#notvalidafter)

Type: `UTCDateTime` · server-set

Expiration date of the certificate

##### notValidBefore

[Section titled “notValidBefore”](#notvalidbefore)

Type: `UTCDateTime` · server-set

Issuance date of the certificate

##### issuer

[Section titled “issuer”](#issuer)

Type: `String` · server-set

Certificate issuer

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Certificate object is available via the `urn:stalwart:jmap` capability.

### x:Certificate/get

[Section titled “x:Certificate/get”](#xcertificateget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysCertificateGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Certificate/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Certificate/set

[Section titled “x:Certificate/set”](#xcertificateset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysCertificateCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Certificate/set",          {            "create": {              "new1": {                "certificate": {                  "@type": "Text",                  "value": "Example"                },                "privateKey": {                  "@type": "Text",                  "secret": "Example"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysCertificateUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Certificate/set",          {            "update": {              "id1": {                "certificate": {                  "@type": "Text",                  "value": "Example"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysCertificateDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Certificate/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Certificate/query

[Section titled “x:Certificate/query”](#xcertificatequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysCertificateQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Certificate/query",          {            "filter": {              "subjectAlternativeNames": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Certificate/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`subjectAlternativeNames`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Certificate id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Certificate \  --field 'certificate={"@type":"Text","value":"Example"}' \  --field 'privateKey={"@type":"Text","secret":"Example"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Certificatestalwart-cli query Certificate --where subjectAlternativeNames=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Certificate id1 --field certificate='{"@type":"Text","value":"Example"}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Certificate --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### PublicText

[Section titled “PublicText”](#publictext)

A text value provided directly, from an environment variable, or from a file.

`Text`: Text value. Carries the fields of [`PublicTextValue`](#publictextvalue).
`EnvironmentVariable`: Text value read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Text value read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### PublicTextValue

[Section titled “PublicTextValue”](#publictextvalue)

A text value provided directly.

##### value

[Section titled “value”](#value)

Type: `Text` · required

Text value

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

[Section titled “secret”](#secret)

Type: `Text` · required · secret

Password or secret value