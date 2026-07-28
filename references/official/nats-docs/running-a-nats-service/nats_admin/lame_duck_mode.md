---
title: "Lame Duck Mode"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/lame_duck_mode.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/lame_duck_mode"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Lame Duck Mode"
---
# Lame Duck Mode

In production we recommend that a server is shut down with ​lame duck mode​ as a graceful way to slowly evict clients. With large deployments this mitigates the "thundering herd" situation that will place CPU pressure on servers as TLS enabled clients reconnect.

## Server

Lame duck mode is initiated by [signaling](/running-a-nats-service/nats_admin/signals.md) the server:

```shell
nats-server --signal ldm
```

After entering lame duck mode, the server will stop accepting new connections, wait for a 10 second grace period, then begin to evict clients over a period of time configurable by the [lame\_duck\_duration](https://docs.nats.io/nats-server/configuration#runtime-configuration) configuration option. This period defaults to 2 minutes.

## Clients

When entering lame duck mode, the server will send a message to clients. Some maintainer supported clients will invoke an optional callback indicating that a server is entering lame duck mode. This is used for cases where an application can benefit from preparing for the short outage between the time it is evicted and automatically reconnected to another server.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/lame_duck_mode.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
