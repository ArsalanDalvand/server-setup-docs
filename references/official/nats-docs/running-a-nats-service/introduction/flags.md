---
title: "Flags"
source: "https://docs.nats.io/running-a-nats-service/introduction/flags.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/introduction/flags"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Flags"
---
# Flags

The NATS server has many flags to customize its behavior without having to write a configuration file.

The configuration flags revolve around:

* Server Options
* Logging
* Authorization
* TLS Security
* Clustering
* Information

## Server Options

| Flag                            | Description                                                                                                   |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `-a`, `--addr`, `--net`         | Host address to bind to (default: `0.0.0.0` - all interfaces).                                                |
| `-p`, `--port`                  | NATS client port (default: 4222).                                                                             |
| `-n`, `--name`, `--server_name` | Server name (default auto).                                                                                   |
| `-P`, `--pid`                   | File to store the process ID (PID).                                                                           |
| `-m`, `--http_port`             | HTTP port for monitoring dashboard (exclusive of `--https_port`).                                             |
| `-ms`, `--https_port`           | HTTPS port monitoring for monitoring dashboard (exclusive of `--http_port`).                                  |
| `-c`, `--config`                | Path to NATS server configuration file.                                                                       |
| `-sl`, `--signal`               | Send a signal to nats-server process. See [process signaling](/running-a-nats-service/nats_admin/signals.md). |
| `--client_advertise`            | Client HostPort to advertise to other servers.                                                                |
| `-t`                            | Test configuration and exit                                                                                   |
| \`--ports\_file\_dir            | Creates a ports file in the specified directory (\<executable\_name>\_.ports).                                |

## JetStream Options

| Flag                 | Description                     |
| -------------------- | ------------------------------- |
| `-js`, `--jetstream` | Enable JetStream functionality. |
| `-sd`, `--store_dir` | Set the storage directory.      |

## Authentication Options

The following options control straightforward authentication:

| Flag     | Description                                                                              |
| -------- | ---------------------------------------------------------------------------------------- |
| `--user` | Required *username* for connections (exclusive of `--auth`).                             |
| `--pass` | Required *password* for connections (exclusive of `--auth`).                             |
| `--auth` | Required *authorization token* for connections (exclusive of `--user` and `--password`). |

See [token authentication](/running-a-nats-service/configuration/securing_nats/auth_intro/tokens.md), and [username/password](/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md) for more information.

## Logging Options

The following flags are available on the server to configure logging:

| Flag                    | Description                                                       |
| ----------------------- | ----------------------------------------------------------------- |
| `-l`, `--log`           | File to redirect log output                                       |
| `-T`, `--logtime`       | Specify `-T=false` to disable timestamping log entries            |
| `-s`, `--syslog`        | Log to syslog or windows event log                                |
| `-r`, `--remote_syslog` | The syslog server address, like `udp://localhost:514`             |
| `-D`, `--debug`         | Enable debugging output                                           |
| `-V`, `--trace`         | Enable protocol trace log messages                                |
| `-VV`                   | Verbose trace (traces system account as well)                     |
| `-DV`                   | Enable both debug and protocol trace messages                     |
| `-DVV`                  | Debug and verbose trace (traces system account as well)           |
| `--max_traced_msg_len`  | Maximum printable length for traced messages. 0 for unlimited     |
| `--max_traced_msg_len`  | Maximum printable length for traced messages (default: unlimited) |

You can read more about [logging configuration here](/running-a-nats-service/configuration/logging.md).

## TLS Options

| Flag          | Description                                |
| ------------- | ------------------------------------------ |
| `--tls`       | Enable TLS, do not verify clients          |
| `--tlscert`   | Server certificate file                    |
| `--tlskey`    | Private key for server certificate         |
| `--tlsverify` | Enable client TLS certificate verification |
| `--tlscacert` | Client certificate CA for verification     |

You can read more about [tls configuration here](/running-a-nats-service/configuration/securing_nats/tls.md).

## Cluster Options

The following flags are available on the server to configure clustering:

| Flag                  | Description                                                 |
| --------------------- | ----------------------------------------------------------- |
| `--routes`            | Comma-separated list of cluster URLs to solicit and connect |
| `--cluster`           | Cluster URL for clustering requests                         |
| `--no_advertise`      | Do not advertise known cluster information to clients       |
| `--cluster_advertise` | Cluster URL to advertise to other servers                   |
| `--connect_retries`   | For implicit routes, number of connect retries              |
| `--cluster_listen`    | Cluster url from which members can solicit routes           |

You can read more about [clustering configuration here](/running-a-nats-service/configuration/clustering.md).

## Common Options

| Flag              | Description       |
| ----------------- | ----------------- |
| `-h`, `--help`    | Show this message |
| `-v`, `--version` | Show version      |
| `--help_tls`      | TLS help          |


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/introduction/flags.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
