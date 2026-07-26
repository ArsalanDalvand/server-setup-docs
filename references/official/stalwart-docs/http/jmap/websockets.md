---
title: "WebSockets"
source: "https://stalw.art/docs/http/jmap/websockets"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/jmap/websockets"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "WebSockets"
---
# WebSockets

      

WebSockets, described in [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455), allow clients to open a two-way communication channel with a web server. Unlike HTTP, which is unidirectional and requires a new connection for each request, a WebSocket maintains a full-duplex persistent connection in which data can flow in both directions.

## JMAP over WebSocket

[Section titled “JMAP over WebSocket”](#jmap-over-websocket)

JMAP over WebSocket is a subprotocol described in [RFC 8887](https://www.rfc-editor.org/rfc/rfc8887.html) that runs the JMAP protocol over a WebSocket transport layer. It delivers higher throughput than JMAP over HTTP by keeping a single persistent connection open between the client and the server, removing the per-request overhead of setting up and authenticating a new HTTP request. Clients authenticate once when the connection is established; credentials remain in effect for the lifetime of the connection.

JMAP over WebSocket is enabled by default in Stalwart and is available to JMAP clients at `wss://mail.example.com`.

## Configuration

[Section titled “Configuration”](#configuration)

WebSocket behaviour is controlled by the following fields on the [[ref/object/jmap|Jmap]] singleton (found in the WebUI under  Settings ›  Network › JMAP › Limits,  Settings ›  Network › JMAP › Push,  Settings ›  Network › JMAP › WebSocket):

[[ref/object/jmap#websockettimeout|`websocketTimeout`]]: time after which an inactive WebSocket connection is closed. Default `"10m"`.
[[ref/object/jmap#websocketheartbeat|`websocketHeartbeat`]]: interval between heartbeat packets sent to WebSocket clients. Default `"1m"`.
[[ref/object/jmap#websocketthrottle|`websocketThrottle`]]: time to wait before sending a batch of notifications to a WebSocket client. Default `"1s"`.

## Conformed RFCs

[Section titled “Conformed RFCs”](#conformed-rfcs)

[RFC 6455 - The WebSocket Protocol](https://www.rfc-editor.org/rfc/rfc6455)
[RFC 8307 - Well-Known URIs for the WebSocket Protocol](https://www.rfc-editor.org/rfc/rfc8307)
[RFC 8441 - Bootstrapping WebSockets with HTTP/2](https://www.rfc-editor.org/rfc/rfc8441)
[RFC 8887 - A JSON Meta Application Protocol (JMAP) Subprotocol for WebSocket](https://www.rfc-editor.org/rfc/rfc8887)