---
title: "PublicKey"
source: "https://stalw.art/docs/ref/object/public-key"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/public-key"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "PublicKey"
---
# PublicKey

      

Defines a public key for email encryption (OpenPGP or S/MIME).

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Public Keys

## Fields

[Section titled “Fields”](#fields)

##### accountId

[Section titled “accountId”](#accountid)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier for the account this public key belongs to

##### key

[Section titled “key”](#key)

Type: `Text` · required

OpenPGP or S/MIME public key data

##### description

[Section titled “description”](#description)

Type: `String` · required

Description of the public key

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

Creation date of the public key

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?`

Expiration date of the public key

##### emailAddresses

[Section titled “emailAddresses”](#emailaddresses)

Type: `EmailAddress[]`

Email addresses associated with the public key

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The PublicKey object is available via the `urn:stalwart:jmap` capability.

### x:PublicKey/get

[Section titled “x:PublicKey/get”](#xpublickeyget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysPublicKeyGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:PublicKey/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:PublicKey/set

[Section titled “x:PublicKey/set”](#xpublickeyset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysPublicKeyCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:PublicKey/set",          {            "create": {              "new1": {                "description": "Example",                "emailAddresses": {},                "key": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysPublicKeyUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:PublicKey/set",          {            "update": {              "id1": {                "key": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysPublicKeyDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:PublicKey/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:PublicKey/query

[Section titled “x:PublicKey/query”](#xpublickeyquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysPublicKeyQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:PublicKey/query",          {            "filter": {              "accountId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:PublicKey/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`accountId`id of Account

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get PublicKey id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create PublicKey \  --field key=Example \  --field description=Example \  --field 'emailAddresses={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query PublicKeystalwart-cli query PublicKey --where accountId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update PublicKey id1 --field key='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete PublicKey --ids id1
```