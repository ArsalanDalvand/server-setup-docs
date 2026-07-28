---
title: "Walkthrough Setup"
source: "https://docs.nats.io/nats-concepts/what-is-nats/walkthrough_setup.md"
source_format: official-markdown
language: "en"
doc_slug: "nats-concepts/what-is-nats/walkthrough_setup"
category: "nats-concepts"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Walkthrough Setup"
---
# Walkthrough Setup

We have provided Walkthroughs for you to try NATS (and JetStream) on your own. In order to follow along with the walkthroughs, you could choose one of these options:

* The `nats` CLI tool must be installed, and a local NATS server must be installed (or you can use a remote server you have access to).
* You can use Synadia's NGS.
* You could even use the demo server from where you installed NATS. This is accessible via `nats://demo.nats.io` (this is a NATS connection URL; not a browser URL. You pass it to a NATS client application).

## Installing the [`nats`](/using-nats/nats-tools/nats_cli.md) CLI Tool

Please refer to the [installation section in the readme](https://github.com/nats-io/natscli?tab=readme-ov-file#installation).

## Installing the NATS server locally (if needed)

If you are going to run a server locally you need to first install it and start it.\
Please refer to the [nats server installation doc](/running-a-nats-service/introduction/installation.md)

Alternatively if you already know how to use NATS on a remote server, you only need to pass the server URL to `nats` using the `-s` option or preferably create a context using `nats context add`, to specify the server URL(s) and credentials file containing your user JWT.

### Start the NATS server (if needed)

To start a simple demonstration server locally, simply run:

```bash
nats-server
```

(or `nats-server -m 8222` if you want to enable the HTTP monitoring functionality)

When the server starts successfully, you will see the following messages:

```
[14524] 2021/10/25 22:53:53.525530 [INF] Starting nats-server
[14524] 2021/10/25 22:53:53.525640 [INF]   Version:  2.6.1
[14524] 2021/10/25 22:53:53.525643 [INF]   Git:      [not set]
[14524] 2021/10/25 22:53:53.525647 [INF]   Name:     NDAUZCA4GR3FPBX4IFLBS4VLAETC5Y4PJQCF6APTYXXUZ3KAPBYXLACC
[14524] 2021/10/25 22:53:53.525650 [INF]   ID:       NDAUZCA4GR3FPBX4IFLBS4VLAETC5Y4PJQCF6APTYXXUZ3KAPBYXLACC
[14524] 2021/10/25 22:53:53.526392 [INF] Starting http monitor on 0.0.0.0:8222
[14524] 2021/10/25 22:53:53.526445 [INF] Listening for client connections on 0.0.0.0:4222
[14524] 2021/10/25 22:53:53.526684 [INF] Server is ready
```

The NATS server listens for client connections on TCP Port 4222.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/nats-concepts/what-is-nats/walkthrough_setup.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
