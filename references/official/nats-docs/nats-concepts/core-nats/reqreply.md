---
title: "Request-Reply"
source: "https://docs.nats.io/nats-concepts/core-nats/reqreply.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/core-nats/reqreply"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Request-Reply"
---
# Request-Reply

Request-Reply is a common pattern in modern distributed systems. A request is sent, and the application either waits on the response with a certain timeout, or receives a response asynchronously.

The increased complexity of modern systems necessitates features like [location transparency](https://en.wikipedia.org/wiki/Location_transparency), scale-up and scale-down, observability (measuring a system's state based on the data it generates) and more. In order to implement this feature-set, various other technologies needed to incorporate additional components, sidecars (processes or services that support the primary application) and proxies. NATS on the other hand, implemented Request-Reply much more easily.

### NATS makes Request-Reply simple and powerful

* NATS supports the Request-Reply pattern using its core communication mechanism — publish and subscribe. A request is published on a given subject using a reply subject. Responders listen on that subject and send responses to the reply subject. Reply subjects are called "**inbox**". These are unique subjects that are dynamically directed back to the requester, regardless of the location of either party.
* Multiple NATS responders can form dynamic queue groups. Therefore, it's not necessary to manually add or remove subscribers from the group for them to start or stop being distributed messages. It’s done automatically. This allows responders to scale up or down as per demand.
* NATS applications "drain before exiting" (processing buffered messages before closing the connection). This allows the applications to scale down without dropping requests.
* Since NATS is based on publish-subscribe, observability is as simple as running another application that can view requests and responses to measure latency, watch for anomalies, direct scalability and more.
* The power of NATS even allows multiple responses, where the first response is utilized and the system efficiently discards the additional ones. This allows for a sophisticated pattern to have multiple responders, reduce response latency and jitter.

### The pattern

![](/files/-LqMZgh0PE7kV9Q2l3BV)

Try NATS request-reply on your own, using a live server by walking through the [request-reply walkthrough.](/nats-concepts/core-nats/reqreply/reqreply_walkthrough.md)

### No responders

When a request is sent to a subject that has no subscribers, it can be convenient to know about it right away. For this use-case, a NATS client can [opt-into no\_responder messages](/reference/reference-protocols/nats-protocol.md#syntax-1). This requires a server and client that support headers. When enabled, a request sent to a subject with no subscribers will immediately receive a reply that has no body, and a `503` status.

Most clients will represent this case by raising or returning an error. For example:

```go
m, err := nc.Request("foo", nil, time.Second);
# err == nats.ErrNoResponders
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/core-nats/reqreply.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
