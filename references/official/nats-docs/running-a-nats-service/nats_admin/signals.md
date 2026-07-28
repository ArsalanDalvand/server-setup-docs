---
title: "Signals"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/signals.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/signals"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Signals"
---
# Signals

## Command Line

On Unix systems, the NATS server responds to the following signals.\
You can send these using the standard Unix `kill` command, or use the `nats-server --signal` command for convenience.

| nats-server command | Unix Signal | Description                                                                                                                            |
| ------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `--signal ldm`      | `SIGUSR2`   | Graceful shutdown (evicts clients gradually) ([lame duck mode](/running-a-nats-service/nats_admin/lame_duck_mode.md))                  |
| `--signal quit`     | `SIGINT`    | Stops the server gracefully                                                                                                            |
| `--signal term`     | `SIGTERM`   | Stops the server gracefully                                                                                                            |
| `--signal stop`     | `SIGKILL`   | Kills the process immediately                                                                                                          |
| `--signal reload`   | `SIGHUP`    | Reloads server configuration file                                                                                                      |
| `--signal reopen`   | `SIGUSR1`   | Reopens the log file for log rotation                                                                                                  |
| *(kill only)*       | `SIGQUIT`   | Kills the process immediately and performs a [stack dump](https://pkg.go.dev/os/signal#hdr-Default_behavior_of_signals_in_Go_programs) |

### Usage

To send a signal to a running nats-server:

```shell
nats-server --signal <command>
```

For example, to gracefully stop the server with lame duck mode:

```shell
nats-server --signal ldm
```

### Multiple processes

If there are multiple `nats-server` processes running, or if `pgrep` isn't available, you must either specify a PID or the absolute path to a PID file:

```shell
nats-server --signal stop=<pid>
```

```shell
nats-server --signal stop=/path/to/pidfile
```

As of NATS v2.10.0, a glob expression can be used to match one or more process IDs, such as:

```shell
nats-server --signal ldm=12*
```

## Windows

See the [Windows Service](/running-a-nats-service/introduction/windows_srv.md) section for information on signaling the NATS server on Windows.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/signals.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
