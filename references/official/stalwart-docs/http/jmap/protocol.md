---
title: "Protocol"
source: "https://stalw.art/docs/http/jmap/protocol"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/jmap/protocol"
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

      

JMAP protocol behaviour is configured on the [[ref/object/jmap|Jmap]] singleton (found in the WebUI under  Settings ›  Network › JMAP › Limits,  Settings ›  Network › JMAP › Push,  Settings ›  Network › JMAP › WebSocket). The fields below influence request handling, upload quotas, and the size of responses returned by get, set, query, and changes methods.

## Request limits

[Section titled “Request limits”](#request-limits)

Request limits guard the JMAP server against resource exhaustion:

[[ref/object/jmap#maxconcurrentrequests|`maxConcurrentRequests`]]: the number of concurrent requests a single user may have in flight. Default `4`.
[[ref/object/jmap#maxrequestsize|`maxRequestSize`]]: the maximum size of a single request, in bytes. Default `10000000`.
[[ref/object/jmap#maxmethodcalls|`maxMethodCalls`]]: the maximum number of method calls that can be included in a single request. Default `16`.

## Upload limits

[Section titled “Upload limits”](#upload-limits)

Upload limits restrict how often and how much data users can upload:

[[ref/object/jmap#maxuploadsize|`maxUploadSize`]]: the maximum size of a single uploaded file, in bytes. Default `50000000`.
[[ref/object/jmap#maxconcurrentuploads|`maxConcurrentUploads`]]: the number of concurrent uploads a single user may have in flight. Default `4`.
[[ref/object/jmap#uploadttl|`uploadTtl`]]: how long each uploaded file is kept in temporary storage before it is [[storage/blob#maintenance|deleted]]. Default `"1h"`.
[[ref/object/jmap#maxuploadcount|`maxUploadCount`]]: the maximum number of files a user may upload within the quota window. Default `1000`.
[[ref/object/jmap#uploadquota|`uploadQuota`]]: the total aggregate size of uploaded files allowed per user within the quota window, in bytes. Default `50000000`.

## Object limits

[Section titled “Object limits”](#object-limits)

Object limits restrict the number of objects returned or modified by a single method call:

[[ref/object/jmap#getmaxresults|`getMaxResults`]]: the maximum number of objects that can be fetched in a single method call. Default `500`.
[[ref/object/jmap#setmaxobjects|`setMaxObjects`]]: the maximum number of objects that can be modified in a single method call. Default `500`.
[[ref/object/jmap#querymaxresults|`queryMaxResults`]]: the maximum number of results returned by a Query method. Default `5000`.
[[ref/object/jmap#changesmaxresults|`changesMaxResults`]]: the maximum number of change objects returned by a Changes method. Default `5000`.