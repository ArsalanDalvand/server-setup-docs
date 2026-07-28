---
title: "Tokens"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/tokens.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro/tokens"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Tokens"
---
# Tokens

Token authentication is a string that if provided by a client, allows it to connect. It is the most straightforward authentication provided by the NATS server.

To use token authentication, you can specify an `authorization` section with the `token` property set:

```
authorization {
    token: "s3cr3t"
}
```

Token authentication can be used in the authorization section for clients and clusters.

Or start the server with the `--auth` flag:

```shell
nats-server --auth s3cr3t
```

A client can easily connect by specifying the server URL:

```shell
nats sub -s nats://s3cr3t@localhost:4222 ">"
```

## Bcrypted Tokens

Tokens can be bcrypted enabling an additional layer of security, as the clear-text version of the token would not be persisted on the server configuration file.

You can generate bcrypted tokens and passwords using the [`nats`](/using-nats/nats-tools/nats_cli.md) tool:

```shell
nats server passwd
```

```
? Enter password [? for help] **********************
? Reenter password [? for help] **********************

$2a$11$PWIFAL8RsWyGI3jVZtO9Nu8.6jOxzxfZo7c/W0eLk017hjgUKWrhy
```

Here's a simple configuration file:

```
authorization {
    token: "$2a$11$PWIFAL8RsWyGI3jVZtO9Nu8.6jOxzxfZo7c/W0eLk017hjgUKWrhy"
}
```

The client will still require the clear-text token to connect:

```shell
nats sub -s nats://dag0HTXl4RGg7dXdaJwbC8@localhost:4222 ">"
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/tokens.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
