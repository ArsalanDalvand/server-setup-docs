---
title: "JetStream"
source: "https://docs.nats.io/running-a-nats-service/nats_docker/jetstream_docker.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_docker/jetstream_docker"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "JetStream"
---
# JetStream

This mini-tutorial shows how to run a NATS server with JetStream enabled in a local Docker container. This enables quick and consequence-free experimentation with the many features of JetStream.

Using the official `nats` image, start a server. The `-js` option is passed to the server to enable JetStream. The `-p` option forwards your local 4222 port to the server inside the container, 4222 is the default client connection port.

```shell
docker run -p 4222:4222 nats -js
```

To persist JetStream data to a volume, you can use the `-v` option in combination with `-sd`:

```shell
docker run -p 4222:4222 -v nats:/data nats -js -sd /data
```

With the server running, use `nats bench` to create a stream and publish some messages to it.

```shell
nats bench -s localhost:4222 benchsubject --js --pub 1 --msgs=100000
```

JetStream persists the messages (on disk by default). Now consume them with:

```shell
nats bench -s localhost:4222 benchsubject --js --sub 3 --msgs=100000
```

You can use `nats` to inspect various aspects of the stream, for example:

```shell
nats -s localhost:4222 stream list
╭────────────────────────────────────────────────────────────────────────────────────╮
│                                       Streams                                      │
├─────────────┬─────────────┬─────────────────────┬──────────┬────────┬──────────────┤
│ Name        │ Description │ Created             │ Messages │ Size   │ Last Message │
├─────────────┼─────────────┼─────────────────────┼──────────┼────────┼──────────────┤
│ benchstream │             │ 2024-06-07 20:26:38 │ 100,000  │ 16 MiB │ 35s          │
╰─────────────┴─────────────┴─────────────────────┴──────────┴────────┴──────────────╯
```

### Related and useful:

* Official [Docker image for the NATS server on GitHub](https://github.com/nats-io/nats-docker) and [issues](https://github.com/nats-io/nats-docker/issues)
* [`nats` images on DockerHub](https://hub.docker.com/_/nats)
* [`nats` CLI tool](/using-nats/nats-tools/nats_cli.md) and [`nats bench`](/using-nats/nats-tools/nats_cli/natsbench.md)
* [`Administer JetStream`](/running-a-nats-service/nats_admin/jetstream_admin.md)


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_docker/jetstream_docker.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
