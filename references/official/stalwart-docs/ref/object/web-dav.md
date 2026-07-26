---
title: "WebDav"
source: "https://stalw.art/docs/ref/object/web-dav"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/web-dav"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "WebDav"
---
# WebDav

      

Configures WebDAV protocol settings including property limits and locking.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › WebDAV

## Fields

[Section titled “Fields”](#fields)

##### enableAssistedDiscovery

[Section titled “enableAssistedDiscovery”](#enableassisteddiscovery)

Type: `Boolean` · default: `true`

Enables assisted discovery of WebDAV shared collections by modifying PROPFIND requests to the root collection. Requests with depth 1 are automatically changed to depth 2, which may cause compatibility issues with some clients that expect the original behavior.

##### maxLockTimeout

[Section titled “maxLockTimeout”](#maxlocktimeout)

Type: `Duration` · default: `"1h"`

Specifies the maximum duration for which a lock can be held on a resource

##### maxLocks

[Section titled “maxLocks”](#maxlocks)

Type: `UnsignedInt` · default: `10`

Specifies the maximum number of locks that a user can create on a resource

##### deadPropertyMaxSize

[Section titled “deadPropertyMaxSize”](#deadpropertymaxsize)

Type: `Size?` · default: `1024`

Specifies the maximum size of a WebDAV dead property value that the server will accept

##### livePropertyMaxSize

[Section titled “livePropertyMaxSize”](#livepropertymaxsize)

Type: `Size` · default: `250`

Specifies the maximum size of a WebDAV live property value that the server will accept

##### requestMaxSize

[Section titled “requestMaxSize”](#requestmaxsize)

Type: `Size` · default: `"25mb"`

Determines the maximum XML size of a WebDAV request that the server will accept

##### maxResults

[Section titled “maxResults”](#maxresults)

Type: `UnsignedInt` · default: `2000`

Specifies the maximum number of results that a WebDAV query can return

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The WebDav singleton is available via the `urn:stalwart:jmap` capability.

### x:WebDav/get

[Section titled “x:WebDav/get”](#xwebdavget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysWebDavGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebDav/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:WebDav/set

[Section titled “x:WebDav/set”](#xwebdavset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysWebDavUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebDav/set",          {            "update": {              "singleton": {                "enableAssistedDiscovery": true              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get WebDav
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update WebDav --field enableAssistedDiscovery=true
```