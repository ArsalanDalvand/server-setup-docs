---
title: "NATS Admin CLI"
source: "https://docs.nats.io/running-a-nats-service/configuration/resource_management/configuration_mgmt/nats-admin-cli.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/resource_management/configuration_mgmt/nats-admin-cli"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "NATS Admin CLI"
---
# NATS Admin CLI

## nats Admin CLI

The [`nats` CLI](https://github.com/nats-io/natscli?tab=readme-ov-file#installation) can be used to manage Streams and Consumers easily using it's `--config` flag, for example:

## Add a new Stream

This creates a new Stream based on `orders.json`. The `orders.json` file can be extracted from an existing stream using `nats stream info ORDERS -j | jq .config`

```shell
nats str add ORDERS --config orders.json
```

## Edit an existing Stream

This edits an existing stream ensuring it complies with the configuration in `orders.json`

```shell
nats str edit ORDERS --config orders.json
```

## Add a New Consumer

This creates a new Consumer based on `orders_new.json`. The `orders_new.json` file can be extracted from an existing stream using `nats con info ORDERS NEW -j | jq .config`

```shell
nats con add ORDERS NEW --config orders_new.json
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/resource_management/configuration_mgmt/nats-admin-cli.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
