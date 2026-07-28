---
title: "Naming Streams, Consumers, and Accounts"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/jetstream_admin/naming.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/jetstream_admin/naming"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Naming Streams, Consumers, and Accounts"
---
# Naming Streams, Consumers, and Accounts

Stream, Consumer (durable name), and Account names are used in both the subject namespace used by JetStream and the filesystem backing JetStream persistence. This means that when naming streams, consumers, and accounts, names must adhere to subject naming rules as well as being friendly to the file system.

We recommend the following guideline for stream, consumer, and account names:

* Alphanumeric values are recommended.
* Spaces, tabs, period (`.`), greater than (`>`) or asterisk (`*`) are prohibited.
* Path separators (i.e. forward slash and backward slash) are prohibited.
* Limit name length: The JetStream storage directories will include the account, stream name, and consumer name, so a generally safe approach would be to keep names **under 32 characters.**
* Do not use reserved file names like NUL, LPT1, etc.
* Be aware that some file systems are case insensitive so do not use stream or account names that would collide in a file system. For example, `Foo` and `foo` would collide on a Windows or Mac OSx System.

We plan to address these limitations in a future release.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/jetstream_admin/naming.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
