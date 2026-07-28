---
title: "WebSocket"
source: "https://docs.nats.io/running-a-nats-service/configuration/websocket.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/websocket"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "WebSocket"
---
# WebSocket

*Supported since NATS Server version 2.2*

WebSocket support can be enabled in the server and may be used alongside the traditional TCP socket connections. TLS, compression and Origin Header checking are supported.

**Important**

* NATS Supports only WebSocket data frames in Binary, not Text format (<https://tools.ietf.org/html/rfc6455#section-5.6>). The server will always send in Binary and your clients MUST send in Binary too.
* For writers of client libraries: a WebSocket frame is not guaranteed to contain a full NATS protocol (actually will generally not). Any data from a frame must be going through a parser that can handle partial protocols. See the protocol description [here](/reference/reference-protocols/nats-protocol.md).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/websocket.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
