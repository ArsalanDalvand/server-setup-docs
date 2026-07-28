---
title: "Request-Reply Walkthrough"
source: "https://docs.nats.io/nats-concepts/core-nats/reqreply/reqreply_walkthrough.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/core-nats/reqreply/reqreply_walkthrough"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Request-Reply Walkthrough"
---
# Request-Reply Walkthrough

NATS supports [request-reply](/nats-concepts/core-nats/reqreply.md) messaging. In this tutorial you explore how to exchange point-to-point messages using NATS.

## Prerequisites

If you have not already done so, you need to [install](/nats-concepts/what-is-nats/walkthrough_setup.md) the `nats` CLI Tool and optionally, the nats-server on your machine.

## Walkthrough

Start two terminal sessions. These will be used to run the NATS request and reply clients.

### In one terminal, run the reply client listener

```bash
nats reply help.please 'OK, I CAN HELP!!!'
```

You should see the message: *Listening on \[help.please]*

This means that the NATS receiver client is listening for request messages on the "help.please" subject. In NATS, the receiver is a subscriber.

### In the other terminal, run the request client

```bash
nats request help.please 'I need help!'
```

The NATS requestor client makes a request by sending the message "I need help!" on the “help.please” subject.

The NATS receiver client receives the message, formulates the reply ("OK, I CAN HELP!!!"), and sends it to the inbox of the requester.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/core-nats/reqreply/reqreply_walkthrough.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
