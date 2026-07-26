---
title: "HttpLookup"
source: "https://stalw.art/docs/ref/object/http-lookup"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/http-lookup"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "HttpLookup"
---
# HttpLookup

      

Defines an HTTP-based lookup list.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Lookups › HTTP Lists

## Fields

[Section titled “Fields”](#fields)

##### namespace

[Section titled “namespace”](#namespace)

Type: `String` · read-only

Unique identifier for this store when used in lookups

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable this HTTP list

##### format

[Section titled “format”](#format)

Type: [`HttpLookupFormat`](#httplookupformat) · required

Format of the list

##### isGzipped

[Section titled “isGzipped”](#isgzipped)

Type: `Boolean` · default: `false`

Whether to use gzip compression when downloading the list

##### maxEntries

[Section titled “maxEntries”](#maxentries)

Type: `UnsignedInt` · default: `100000` · max: 1048576 · min: 1

Maximum number of entries allowed in the list. The list is truncated if it exceeds this limit.

##### maxEntrySize

[Section titled “maxEntrySize”](#maxentrysize)

Type: `Size` · default: `512` · max: 1048576 · min: 1

Maximum length of an entry in the list.

##### maxSize

[Section titled “maxSize”](#maxsize)

Type: `Size` · default: `"100mb"` · max: 1073741824 · min: 10

Maximum size of the list. The list is truncated if it exceeds this size.

##### refresh

[Section titled “refresh”](#refresh)

Type: `Duration` · default: `"12h"`

How often to refresh the list

##### retry

[Section titled “retry”](#retry)

Type: `Duration` · default: `"1h"`

How long to wait before retrying to download the list in case of failure.

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

How long to wait for the list to download before timing out

##### url

[Section titled “url”](#url)

Type: `Uri` · required

URL of the list

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The HttpLookup object is available via the `urn:stalwart:jmap` capability.

### x:HttpLookup/get

[Section titled “x:HttpLookup/get”](#xhttplookupget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysHttpLookupGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpLookup/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:HttpLookup/set

[Section titled “x:HttpLookup/set”](#xhttplookupset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysHttpLookupCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpLookup/set",          {            "create": {              "new1": {                "format": {                  "@type": "Csv"                },                "url": "https://example.com"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysHttpLookupUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpLookup/set",          {            "update": {              "id1": {                "enable": true              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysHttpLookupDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpLookup/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:HttpLookup/query

[Section titled “x:HttpLookup/query”](#xhttplookupquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysHttpLookupQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpLookup/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get HttpLookup id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create HttpLookup \  --field 'format={"@type":"Csv"}' \  --field url=https://example.com
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query HttpLookup
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update HttpLookup id1 --field enable=true
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete HttpLookup --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### HttpLookupFormat

[Section titled “HttpLookupFormat”](#httplookupformat)

Format of HTTP lookup lists.

`Csv`: CSV. Carries the fields of [`HttpLookupCsv`](#httplookupcsv).
`List`: List. No additional fields.

#### HttpLookupCsv

[Section titled “HttpLookupCsv”](#httplookupcsv)

CSV parsing settings for HTTP lookup lists.

##### indexKey

[Section titled “indexKey”](#indexkey)

Type: `UnsignedInt` · default: `0`

The position of the key field in the HTTP List.

##### indexValue

[Section titled “indexValue”](#indexvalue)

Type: `UnsignedInt?`

The position of the value field in the HTTP List.

##### separator

[Section titled “separator”](#separator)

Type: `String` · default: `","`

The separator character used to parse the HTTP list.

##### skipFirst

[Section titled “skipFirst”](#skipfirst)

Type: `Boolean` · default: `false`

Whether to skip the first line of the list