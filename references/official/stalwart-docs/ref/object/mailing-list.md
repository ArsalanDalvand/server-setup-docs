---
title: "MailingList"
source: "https://stalw.art/docs/ref/object/mailing-list"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mailing-list"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MailingList"
---
# MailingList

      

Defines a mailing list that distributes messages to a group of recipients.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Directory › Mailing Lists

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `EmailLocalPart` · required

Name of the mailing list, typically an email address local part.

##### domainId

[Section titled “domainId”](#domainid)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this mailing list belongs to. This is used to determine the email address of the mailing list, which is formed as name@domain.

##### emailAddress

[Section titled “emailAddress”](#emailaddress)

Type: `EmailAddress` · server-set

The email address of the mailing list, formed as name@domain.

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the mailing list

##### aliases

[Section titled “aliases”](#aliases)

Type: [`EmailAlias`](#emailalias)`[]`

List of email aliases for the mailing list

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?`

Identifier for the tenant this mailing list belongs to

##### recipients

[Section titled “recipients”](#recipients)

Type: `EmailAddress[]`

List of email addresses that are members of the mailing list

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MailingList object is available via the `urn:stalwart:jmap` capability.

### x:MailingList/get

[Section titled “x:MailingList/get”](#xmailinglistget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMailingListGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MailingList/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MailingList/set

[Section titled “x:MailingList/set”](#xmailinglistset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMailingListCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MailingList/set",          {            "create": {              "new1": {                "aliases": {},                "domainId": "<Domain id>",                "name": "alice",                "recipients": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMailingListUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MailingList/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMailingListDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MailingList/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MailingList/query

[Section titled “x:MailingList/query”](#xmailinglistquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMailingListQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MailingList/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MailingList/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MailingList id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MailingList \  --field name=alice \  --field 'domainId=<Domain id>' \  --field 'aliases={}' \  --field 'recipients={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MailingListstalwart-cli query MailingList --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MailingList id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MailingList --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### EmailAlias

[Section titled “EmailAlias”](#emailalias)

Defines an email alias for an account or mailing list.

##### enabled

[Section titled “enabled”](#enabled)

Type: `Boolean` · default: `true`

Whether this email alias is enabled

##### name

[Section titled “name”](#name-1)

Type: `EmailLocalPart` · required

The local part of the email alias (the part before the @ symbol)

##### domainId

[Section titled “domainId”](#domainid-1)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain of the email alias (the part after the @ symbol).

##### description

[Section titled “description”](#description-1)

Type: `String?`

Description of the email alias