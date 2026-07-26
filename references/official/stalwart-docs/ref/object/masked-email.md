---
title: "MaskedEmail"
source: "https://stalw.art/docs/ref/object/masked-email"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/masked-email"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MaskedEmail"
---
# MaskedEmail

      

Defines a masked email address for privacy protection.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Masked Addresses

## Fields

[Section titled “Fields”](#fields)

##### enabled

[Section titled “enabled”](#enabled)

Type: `Boolean` · default: `true`

Whether this masked email address is enabled

##### accountId

[Section titled “accountId”](#accountid)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier for the account this masked email address belongs to

##### email

[Section titled “email”](#email)

Type: `String` · server-set

The masked email address

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the masked email address

##### forDomain

[Section titled “forDomain”](#fordomain)

Type: `String?`

The domain name of the site this address was created for, e.g. “[https://example.com](https://example.com)”. This is intended to be added automatically by password managers.

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

The date-time the email address was created.

##### createdBy

[Section titled “createdBy”](#createdby)

Type: `String?`

The name of the client that created this email address. This will be set by the server automatically based on the credentials used to authenticate the request, e.g. “ACME Password Manager”.

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?` · read-only

Expiration date of the email address

##### url

[Section titled “url”](#url)

Type: `String?`

A URL pointing back to the integrator’s use of this email address, e.g. a custom-uri to open “ACME Password Manager” at the appropriate entry.

##### emailPrefix

[Section titled “emailPrefix”](#emailprefix)

Type: `EmailLocalPart?` · read-only

This is only used on create and otherwise ignored; if supplied, the server-assigned email will start with the given prefix. The string MUST be <= 64 characters in length and MUST only contain characters a-z, 0-9 and _ (underscore).

##### emailDomain

[Section titled “emailDomain”](#emaildomain)

Type: `DomainName?` · read-only

This is only used on create and otherwise ignored; if supplied, the server-assigned email will end with the given domain.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MaskedEmail object is available via the `urn:stalwart:jmap` capability.

### x:MaskedEmail/get

[Section titled “x:MaskedEmail/get”](#xmaskedemailget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMaskedEmailGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MaskedEmail/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MaskedEmail/set

[Section titled “x:MaskedEmail/set”](#xmaskedemailset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMaskedEmailCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MaskedEmail/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMaskedEmailUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MaskedEmail/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMaskedEmailDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MaskedEmail/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MaskedEmail/query

[Section titled “x:MaskedEmail/query”](#xmaskedemailquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMaskedEmailQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MaskedEmail/query",          {            "filter": {              "accountId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MaskedEmail/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`accountId`id of Account

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MaskedEmail id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MaskedEmail
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MaskedEmailstalwart-cli query MaskedEmail --where accountId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MaskedEmail id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MaskedEmail --ids id1
```