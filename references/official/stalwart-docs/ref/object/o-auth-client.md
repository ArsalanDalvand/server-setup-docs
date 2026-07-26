---
title: "OAuthClient"
source: "https://stalw.art/docs/ref/object/o-auth-client"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/o-auth-client"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "OAuthClient"
---
# OAuthClient

      

Defines a registered OAuth client application.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Directory › OAuth Clients

## Fields

[Section titled “Fields”](#fields)

##### clientId

[Section titled “clientId”](#clientid)

Type: `String` · required

Unique identifier of the OAuth client

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the OAuth client

##### contacts

[Section titled “contacts”](#contacts)

Type: `EmailAddress[]`

Contact email addresses for the OAuth client

##### secret

[Section titled “secret”](#secret)

Type: `String?` · secret

Secret value of the OAuth client

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · read-only · server-set

Creation date of the OAuth client

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?`

Expiration date of the OAuth client

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this OAuth client belongs to

##### redirectUris

[Section titled “redirectUris”](#redirecturis)

Type: `Uri[]`

List of redirect URIs for the OAuth client

##### logo

[Section titled “logo”](#logo)

Type: `String?`

URL or base64-encoded image representing the OAuth client

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The OAuthClient object is available via the `urn:stalwart:jmap` capability.

### x:OAuthClient/get

[Section titled “x:OAuthClient/get”](#xoauthclientget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysOAuthClientGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OAuthClient/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:OAuthClient/set

[Section titled “x:OAuthClient/set”](#xoauthclientset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysOAuthClientCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OAuthClient/set",          {            "create": {              "new1": {                "clientId": "Example",                "contacts": {},                "redirectUris": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysOAuthClientUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OAuthClient/set",          {            "update": {              "id1": {                "clientId": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysOAuthClientDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OAuthClient/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:OAuthClient/query

[Section titled “x:OAuthClient/query”](#xoauthclientquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysOAuthClientQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:OAuthClient/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:OAuthClient/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`clientId`text`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get OAuthClient id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create OAuthClient \  --field clientId=Example \  --field 'contacts={}' \  --field 'redirectUris={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query OAuthClientstalwart-cli query OAuthClient --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update OAuthClient id1 --field clientId='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete OAuthClient --ids id1
```