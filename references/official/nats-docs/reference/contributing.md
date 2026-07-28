---
title: "Contributing"
source: "https://docs.nats.io/reference/contributing.md"
source_format: official-markdown
language: "en"
doc_slug: "reference/contributing"
category: "reference"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Contributing"
---
# Contributing

Do you want to contribute to NATS? Awesome! We’d love your help.

The best way to get started contributing to NATS is to read our [Contributor’s Guide](https://nats.io/contributing/). Here are some examples of ways you can contribute:

* Report or fix bugs
* Add or propose new features
* Improve our documentation
* Add or update client libraries
* Host a Meetup
* Improve this website

Please [contact us](mailto:info@nats.io) with any other suggestions.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/reference/contributing.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
