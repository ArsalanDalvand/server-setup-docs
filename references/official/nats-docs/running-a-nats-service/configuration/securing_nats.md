---
title: "Securing NATS"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Securing NATS"
---
# Securing NATS

The NATS server provides several forms of security:

* Connections can be [*encrypted* with TLS](/running-a-nats-service/configuration/securing_nats/tls.md)
* Client connections can require [*authentication*](/running-a-nats-service/configuration/securing_nats/auth_intro.md)
* Clients can require [*authorization*](/running-a-nats-service/configuration/securing_nats/authorization.md) for subjects they publish or subscribe to


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
