---
title: "Application"
source: "https://stalw.art/docs/ref/object/application"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/application"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Application"
---
# Application

      

Defines a web application served by the server.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Web Applications

## Fields

[Section titled “Fields”](#fields)

##### enabled

[Section titled “enabled”](#enabled)

Type: `Boolean` · default: `true`

Whether the application is enabled and should be served by the server

##### description

[Section titled “description”](#description)

Type: `String` · required

A short description of the web application

##### resourceUrl

[Section titled “resourceUrl”](#resourceurl)

Type: `String` · required

Override the URL to download application updates from.

##### urlPrefix

[Section titled “urlPrefix”](#urlprefix)

Type: `String[]` · min items: 1

The URL prefixes to serve the application on. For example, if set to “/admin”, the application will be accessible at [http://server/admin](http://server/admin).

##### autoUpdateFrequency

[Section titled “autoUpdateFrequency”](#autoupdatefrequency)

Type: `Duration` · default: `"90d"`

Frequency to check for application updates

##### unpackDirectory

[Section titled “unpackDirectory”](#unpackdirectory)

Type: `String?`

The local path to unpack the application bundle to. If left empty, the application will be unpacked to /tmp.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Application object is available via the `urn:stalwart:jmap` capability.

### x:Application/get

[Section titled “x:Application/get”](#xapplicationget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysApplicationGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Application/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Application/set

[Section titled “x:Application/set”](#xapplicationset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysApplicationCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Application/set",          {            "create": {              "new1": {                "description": "Example",                "resourceUrl": "Example",                "urlPrefix": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysApplicationUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Application/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysApplicationDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Application/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Application/query

[Section titled “x:Application/query”](#xapplicationquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysApplicationQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Application/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Application id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Application \  --field description=Example \  --field resourceUrl=Example \  --field 'urlPrefix={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Application
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Application id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Application --ids id1
```