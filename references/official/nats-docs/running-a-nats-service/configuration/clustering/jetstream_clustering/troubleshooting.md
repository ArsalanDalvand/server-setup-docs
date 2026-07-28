---
title: "Troubleshooting"
source: "https://docs.nats.io/running-a-nats-service/configuration/clustering/jetstream_clustering/troubleshooting.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/clustering/jetstream_clustering/troubleshooting"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Troubleshooting"
---
# Troubleshooting

Diagnosing problems in NATS JetStream clusters requires:

* knowledge of [JetStream concepts](/nats-concepts/jetstream.md)
* knowledge of the [NATS Command Line Interface (CLI)](https://github.com/nats-io/natscli#the-nats-command-line-interface)

The following tips and commands (while not an exhaustive list) can be useful when diagnosing problems in NATS JetStream clusters:

## Troubleshooting tips

1. Look at [nats-server](https://github.com/nats-io/nats-server) logs. By default, only warning and error logs are produced, but debug and trace logs can be turned on from the command line using `-D` and `-DV`, respectively. Alternatively, enabling `debug` or `trace` in the [[running-a-nats-service/configuration#monitoring-and-tracing|server config]].
2. Make sure that in the [NATS JetStream configuration](/running-a-nats-service/configuration/clustering/jetstream_clustering.md#configuration), at least one system user is configured in this section: `{ $SYS { users } }`.

### `nats account` commands

| Command                                                                              | Description                                 |
| ------------------------------------------------------------------------------------ | ------------------------------------------- |
| [`nats account info`](/running-a-nats-service/nats_admin/jetstream_admin/account.md) | Verify that JetStream is enabled on account |

### Basic `nats server` commands

| Command                                                                      | Description                            |
| ---------------------------------------------------------------------------- | -------------------------------------- |
| `nats server ls`                                                             | List known servers                     |
| `nats server ping`                                                           | Ping all servers                       |
| `nats server info`                                                           | Show information about a single server |
| [`nats server check`](/running-a-nats-service/clients.md#testing-your-setup) | Health check for NATS servers          |

### `nats server report` commands

| Command                                                                                                                                             | Description                  |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `nats server report connections`                                                                                                                    | Report on connections        |
| `nats server report accounts`                                                                                                                       | Report on account activity   |
| [`nats server report jetstream`](/running-a-nats-service/configuration/clustering/jetstream_clustering/administration.md#viewing-the-cluster-state) | Report on JetStream activity |

### `nats server request` commands

| Command                                                                                                                                              | Description                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| [`nats server request jetstream`](/running-a-nats-service/configuration/clustering/jetstream_clustering/administration.md#viewing-the-cluster-state) | Show JetStream details        |
| `nats server request subscriptions`                                                                                                                  | Show subscription information |
| `nats server request variables`                                                                                                                      | Show runtime variables        |
| `nats server request connections`                                                                                                                    | Show connection details       |
| `nats server request routes`                                                                                                                         | Show route details            |
| `nats server request gateways`                                                                                                                       | Show gateway details          |
| `nats server request leafnodes`                                                                                                                      | Show leafnode details         |
| `nats server request accounts`                                                                                                                       | Show account details          |

### `nats server cluster` commands

| Command                                                                                                                                                                | Description                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| [`nats server cluster step-down`](/running-a-nats-service/configuration/clustering/jetstream_clustering/administration.md#forcing-stream-and-consumer-leader-election) | Force a new leader election by standing down the current meta leader |
| [`nats server cluster peer-remove`](/running-a-nats-service/configuration/clustering/jetstream_clustering/administration.md#evicting-a-peer)                           | Removes a server from a JetStream cluster                            |

### Experimental commands

| Command                                                                               | Description                                      |
| ------------------------------------------------------------------------------------- | ------------------------------------------------ |
| [`nats traffic`](https://github.com/nats-io/natscli/blob/main/cli/traffic_command.go) | Monitor NATS traffic. (**Experimental command**) |

## Further troubleshooting references

* [Testing your setup](/running-a-nats-service/clients.md#testing-your-setup)


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/clustering/jetstream_clustering/troubleshooting.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
