---
title: "ArchivedItem"
source: "https://stalw.art/docs/ref/object/archived-item"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/archived-item"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ArchivedItem"
---
# ArchivedItem

      

Represents an archived item that can be restored.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Archived Items

## Fields

[Section titled “Fields”](#fields)

ArchivedItem is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Email"

[Section titled “@type: "Email"”](#type-email)

Archived Email message

##### from

[Section titled “from”](#from)

Type: `String` · required

Sender of the archived email message

##### subject

[Section titled “subject”](#subject)

Type: `String` · required

Subject of the archived email message

##### receivedAt

[Section titled “receivedAt”](#receivedat)

Type: `UTCDateTime` · required

Received date of the archived email message

##### size

[Section titled “size”](#size)

Type: `Size` · read-only · default: `0`

Size of the archived email message

##### accountId

[Section titled “accountId”](#accountid)

Type: `Id<`[[ref/object/account|`Account`]]`>` · required

The account to which the archived item belongs

##### archivedAt

[Section titled “archivedAt”](#archivedat)

Type: `UTCDateTime` · required

Timestamp when the item was archived

##### archivedUntil

[Section titled “archivedUntil”](#archiveduntil)

Type: `UTCDateTime` · required

Timestamp until which the archived item will be deleted permanently if not restored

##### blobId

[Section titled “blobId”](#blobid)

Type: `BlobId` · required

Identifier of the archived blob

##### status

[Section titled “status”](#status)

Type: [`ArchivedItemStatus`](#archiveditemstatus) · required

Current status of the archived item

### @type: "FileNode"

[Section titled “@type: "FileNode"”](#type-filenode)

Archived File

##### name

[Section titled “name”](#name)

Type: `String` · required

Name of the archived file

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

Creation date of the archived file

##### accountId

[Section titled “accountId”](#accountid-1)

Type: `Id<`[[ref/object/account|`Account`]]`>` · required

The account to which the archived item belongs

##### archivedAt

[Section titled “archivedAt”](#archivedat-1)

Type: `UTCDateTime` · required

Timestamp when the item was archived

##### archivedUntil

[Section titled “archivedUntil”](#archiveduntil-1)

Type: `UTCDateTime` · required

Timestamp until which the archived item will be deleted permanently if not restored

##### blobId

[Section titled “blobId”](#blobid-1)

Type: `BlobId` · required

Identifier of the archived blob

##### status

[Section titled “status”](#status-1)

Type: [`ArchivedItemStatus`](#archiveditemstatus) · required

Current status of the archived item

### @type: "CalendarEvent"

[Section titled “@type: "CalendarEvent"”](#type-calendarevent)

Archived Calendar Event

##### title

[Section titled “title”](#title)

Type: `String` · required

Title of the archived calendar event

##### startTime

[Section titled “startTime”](#starttime)

Type: `UTCDateTime?`

Start time of the archived calendar event

##### createdAt

[Section titled “createdAt”](#createdat-1)

Type: `UTCDateTime` · server-set

Creation date of the archived calendar event

##### accountId

[Section titled “accountId”](#accountid-2)

Type: `Id<`[[ref/object/account|`Account`]]`>` · required

The account to which the archived item belongs

##### archivedAt

[Section titled “archivedAt”](#archivedat-2)

Type: `UTCDateTime` · required

Timestamp when the item was archived

##### archivedUntil

[Section titled “archivedUntil”](#archiveduntil-2)

Type: `UTCDateTime` · required

Timestamp until which the archived item will be deleted permanently if not restored

##### blobId

[Section titled “blobId”](#blobid-2)

Type: `BlobId` · required

Identifier of the archived blob

##### status

[Section titled “status”](#status-2)

Type: [`ArchivedItemStatus`](#archiveditemstatus) · required

Current status of the archived item

### @type: "ContactCard"

[Section titled “@type: "ContactCard"”](#type-contactcard)

Archived Contact Card

##### name

[Section titled “name”](#name-1)

Type: `String?`

Full name of the archived contact card

##### createdAt

[Section titled “createdAt”](#createdat-2)

Type: `UTCDateTime` · server-set

Creation date of the archived contact card

##### accountId

[Section titled “accountId”](#accountid-3)

Type: `Id<`[[ref/object/account|`Account`]]`>` · required

The account to which the archived item belongs

##### archivedAt

[Section titled “archivedAt”](#archivedat-3)

Type: `UTCDateTime` · required

Timestamp when the item was archived

##### archivedUntil

[Section titled “archivedUntil”](#archiveduntil-3)

Type: `UTCDateTime` · required

Timestamp until which the archived item will be deleted permanently if not restored

##### blobId

[Section titled “blobId”](#blobid-3)

Type: `BlobId` · required

Identifier of the archived blob

##### status

[Section titled “status”](#status-3)

Type: [`ArchivedItemStatus`](#archiveditemstatus) · required

Current status of the archived item

### @type: "SieveScript"

[Section titled “@type: "SieveScript"”](#type-sievescript)

Archived Sieve Script

##### name

[Section titled “name”](#name-2)

Type: `String` · required

Name of the archived sieve script

##### createdAt

[Section titled “createdAt”](#createdat-3)

Type: `UTCDateTime` · server-set

Creation date of the archived sieve script

##### content

[Section titled “content”](#content)

Type: `String` · required

Content of the archived sieve script

##### accountId

[Section titled “accountId”](#accountid-4)

Type: `Id<`[[ref/object/account|`Account`]]`>` · required

The account to which the archived item belongs

##### archivedAt

[Section titled “archivedAt”](#archivedat-4)

Type: `UTCDateTime` · required

Timestamp when the item was archived

##### archivedUntil

[Section titled “archivedUntil”](#archiveduntil-4)

Type: `UTCDateTime` · required

Timestamp until which the archived item will be deleted permanently if not restored

##### blobId

[Section titled “blobId”](#blobid-4)

Type: `BlobId` · required

Identifier of the archived blob

##### status

[Section titled “status”](#status-4)

Type: [`ArchivedItemStatus`](#archiveditemstatus) · required

Current status of the archived item

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The ArchivedItem object is available via the `urn:stalwart:jmap` capability.

### x:ArchivedItem/get

[Section titled “x:ArchivedItem/get”](#xarchiveditemget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysArchivedItemGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArchivedItem/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ArchivedItem/set

[Section titled “x:ArchivedItem/set”](#xarchiveditemset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysArchivedItemCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArchivedItem/set",          {            "create": {              "new1": {                "@type": "Email",                "accountId": "<Account id>",                "archivedAt": "2026-01-01T00:00:00Z",                "archivedUntil": "2026-01-01T00:00:00Z",                "blobId": "abc123",                "from": "Example",                "receivedAt": "2026-01-01T00:00:00Z",                "subject": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysArchivedItemUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArchivedItem/set",          {            "update": {              "id1": {                "from": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysArchivedItemDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArchivedItem/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ArchivedItem/query

[Section titled “x:ArchivedItem/query”](#xarchiveditemquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysArchivedItemQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArchivedItem/query",          {            "filter": {              "accountId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:ArchivedItem/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`accountId`id of Account

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get ArchivedItem id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create ArchivedItem/Email \  --field from=Example \  --field subject=Example \  --field receivedAt=2026-01-01T00:00:00Z \  --field 'accountId=<Account id>' \  --field archivedAt=2026-01-01T00:00:00Z \  --field archivedUntil=2026-01-01T00:00:00Z \  --field blobId=abc123
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query ArchivedItemstalwart-cli query ArchivedItem --where accountId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update ArchivedItem id1 --field from='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete ArchivedItem --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### ArchivedItemStatus

[Section titled “ArchivedItemStatus”](#archiveditemstatus)
ValueLabel`archived`Item is archived and available for restoration`requestRestore`Request item restoration