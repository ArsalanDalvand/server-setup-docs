---
title: "Protocol"
source: "https://stalw.art/docs/http/webdav/protocol"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/webdav/protocol"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Protocol"
---
# Protocol

      

WebDAV behaviour is tuned through the [[ref/object/web-dav|WebDav]] singleton (found in the WebUI under  Settings ›  Network › WebDAV). Two fields in particular control resource usage under heavy client activity: the maximum incoming request size and the maximum number of results returned in a single response.

[[ref/object/web-dav#requestmaxsize|`requestMaxSize`]]: the maximum XML size of a WebDAV request that the server will accept. Default `"25mb"`. This limit matters most for file uploads, calendar imports, and contact synchronisation, which tend to carry large payloads. Administrators can raise it to support larger transfers or lower it to reduce resource consumption.
[[ref/object/web-dav#maxresults|`maxResults`]]: the maximum number of items (calendar events, contact entries, and similar) that the server will include in a single WebDAV response. Default `2000`. Adjust this value to balance response size against network and memory efficiency during bulk sync operations.

Example:

```
{  "requestMaxSize": "25mb",  "maxResults": 2000}
```