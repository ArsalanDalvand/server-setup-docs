---
title: "Receiving Messages"
source: "https://docs.nats.io/using-nats/developer/receiving.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/receiving"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Receiving Messages"
---
# Receiving Messages

In general, applications can receive messages asynchronously or synchronously. Receiving messages with NATS can be library dependent.

Some languages, like Go or Java, provide synchronous and asynchronous APIs, while others may only support one type of subscription.

In all cases, the process of subscribing involves having the client library tell the NATS system that an application is interested in a particular subject. When an application is done with a subscription it unsubscribes telling the server to stop sending messages.

A client will receive a message for each matching subscription, so if a connection has multiple subscriptions using identical or overlapping subjects (say `foo` and `>`) the same message will be sent to the client multiple times.

Note: The client API (asynchronous) subscribe call can return before the subscription is actually fully established at the nats-server. Call `Flush()` on the connection right after you call subscribe if you need to synchronize with the subscription being ready at the server level.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer/receiving.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
