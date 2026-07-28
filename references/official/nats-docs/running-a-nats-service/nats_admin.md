---
title: "Managing and Monitoring your NATS Server Infrastructure"
source: "https://docs.nats.io/running-a-nats-service/nats_admin.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Managing and Monitoring your NATS Server Infrastructure"
---
# Managing and Monitoring your NATS Server Infrastructure

Managing a NATS Server is simple, typical lifecycle operations include:

* Using the [`nats`](/using-nats/nats-tools/nats_cli.md) CLI tool to check server cluster connectivity and latencies, as well as get account information, and manage and interact with streams (and other NATS applications). Try the following examples to learn about the most common ways to use `nats`.
  * `nats cheat`
  * `nats cheat server`
  * `nats stream --help` to monitor, manage and interact with streams
  * `nats consumer --help` to monitor, manage stream consumers
  * `nats context --help` if you need to switch between servers, clusters or user credentials
* Using the [`nsc`](/using-nats/nats-tools/nsc.md) CLI tool when using JWT based authentication and authorization, to create, revoke operators, accounts, and user (i.e. client applications) JWTs and keys.
* [Sending signals](/running-a-nats-service/nats_admin/signals.md) to a server to reload a configuration or rotate log files
* [Upgrading](/running-a-nats-service/nats_admin/upgrading_cluster.md) a server (or cluster)
* Understanding [slow consumers](/running-a-nats-service/nats_admin/slow_consumers.md)
* Monitoring the server via:
  * The monitoring [endpoint](/running-a-nats-service/nats_admin/monitoring.md) and tools like [nats-top](/using-nats/nats-tools/nats_top.md)
  * By subscribing to [system events](/running-a-nats-service/configuration/sys_accounts.md)
    * Commerical option for high-cardinality monitoring via the system account: [Synadia Insights](https://www.synadia.com/insights)
* Gracefully shut down a server with [Lame Duck Mode](/running-a-nats-service/nats_admin/lame_duck_mode.md)


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
