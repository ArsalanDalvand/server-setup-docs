---
title: "NATS Tools"
source: "https://docs.nats.io/using-nats/nats-tools.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/nats-tools"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "NATS Tools"
---
# NATS Tools

## Using NATS from client application

The most common form of connecting to the NATS messaging system will be through an application built with any of the [40+ client libraries](/using-nats/developer.md) available for NATS.

The client application will connect to an instance of the NATS server, be it a single server, a cluster of servers or even a global super-cluster such as [Synadia Cloud](https://www.synadia.com/cloud?utm_source=nats_docs\&utm_medium=nats), sending and receiving messages via a range of subscribers contracts. If the application is written in GoLang the NATS server can even be [embedded into a Go](https://dev.to/karanpratapsingh/embedding-nats-in-go-19o) application.

Client APIs will also allow access to almost all server configuration tasks when using an account with sufficient permissions.

## Command Line Tooling

Besides using the client API to manage NATS servers, the NATS ecosystem also has many tools to interact with other applications and services over NATS and streams, support server configuration, enhance monitoring or tune performance such as:

* General interaction and management
  * [nats](/using-nats/nats-tools/nats_cli.md) - The `nats` Command Line Tool is the easiest way to interact with, test and manage NATS and JetStream from a terminal or from scripts. It's list of features are ever growing, so please download the [latest version](https://github.com/nats-io/natscli/releases).
* Security
  * [nk](/using-nats/nats-tools/nk.md) - Generate NKeys for use with JSon Web Tokens (JWT) used with nsc
  * [nsc](/using-nats/nats-tools/nsc.md) - Configure Operators, Accounts, Users and permission offline to later push them to a production server. This is the preferred tools to create security configuration unless you are using [Synadia Control Plane](https://www.docs.synadia.com/platform/control-plane?utm_source=nats_docs\&utm_medium=nats)
  * [nats account server](https://nats-io.gitbook.io/legacy-nats-docs/nats-account-server) - (**legacy, replaced by the built-in NATS resolver**) a custom security server. NAS can still be used as a reference implementation for you tailor-made security integration.
* Monitoring
  * [nats top](/using-nats/nats-tools/nats_top.md) - Monitor NATS Servers
  * [prometheus-nats-exporter](https://github.com/nats-io/prometheus-nats-exporter) - Export NATS server metrics to [Prometheus](https://prometheus.io/) and a [Grafana](https://grafana.com) dashboard.
* Benchmarking
  * see [nats bench](/using-nats/nats-tools/nats_cli/natsbench.md) subcommand of the [nats](/using-nats/nats-tools/nats_cli.md) tool


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/nats-tools.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
