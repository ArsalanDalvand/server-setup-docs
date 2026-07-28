---
title: "Tutorial"
source: "https://docs.nats.io/using-nats/nats-tools/nats_top/nats-top-tutorial.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/nats-tools/nats_top/nats-top-tutorial"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Tutorial"
---
# Tutorial

You can use [nats-top](https://github.com/nats-io/nats-top) to monitor in realtime NATS server connections and message statistics.

## Prerequisites

* [Set up your Go environment](https://golang.org/doc/install)
* [Install the NATS server](/running-a-nats-service/introduction/installation.md)

## 1. Install nats-top

```bash
go install github.com/nats-io/nats-top@latest
```

You may need to run the following instead:

```bash
sudo -E go install github.com/nats-io/nats-top
```

## 2. Start the NATS server with monitoring enabled

```bash
nats-server -m 8222
```

## 3. Start nats-top

```bash
nats-top
```

Result:

```
nats-server version 0.6.6 (uptime: 2m2s)
Server:
  Load: CPU:  0.0%  Memory: 6.3M  Slow Consumers: 0
  In:   Msgs: 0  Bytes: 0  Msgs/Sec: 0.0  Bytes/Sec: 0
  Out:  Msgs: 0  Bytes: 0  Msgs/Sec: 0.0  Bytes/Sec: 0

Connections: 0
  HOST                 CID      SUBS    PENDING     MSGS_TO     MSGS_FROM   BYTES_TO    BYTES_FROM  LANG     VERSION
```

## 4. Run NATS client programs

Run some NATS client programs and exchange messages.

For the best experience, you will want to run multiple subscribers, at least 2 or 3. Refer to the [example pub-sub clients](/running-a-nats-service/clients.md).

## 5. Check nats-top for statistics

```
nats-server version 0.6.6 (uptime: 30m51s)
Server:
  Load: CPU:  0.0%  Memory: 10.3M  Slow Consumers: 0
  In:   Msgs: 56  Bytes: 302  Msgs/Sec: 0.0  Bytes/Sec: 0
  Out:  Msgs: 98  Bytes: 512  Msgs/Sec: 0.0  Bytes/Sec: 0

Connections: 3
  HOST                 CID      SUBS    PENDING     MSGS_TO     MSGS_FROM   BYTES_TO    BYTES_FROM  LANG     VERSION
  ::1:58651            6        1       0           52          0           260         0           go       1.1.0
  ::1:58922            38       1       0           21          0           105         0           go       1.1.0
  ::1:58953            39       1       0           21          0           105         0           go       1.1.0
```

## 6. Sort nats-top statistics

In nats-top, enter the command `o` followed by the option, such as `bytes_to`. You see that nats-top sorts the BYTES\_TO column in ascending order.

```
nats-server version 0.6.6 (uptime: 45m40s)
Server:
  Load: CPU:  0.0%  Memory: 10.4M  Slow Consumers: 0
  In:   Msgs: 81  Bytes: 427  Msgs/Sec: 0.0  Bytes/Sec: 0
  Out:  Msgs: 154  Bytes: 792  Msgs/Sec: 0.0  Bytes/Sec: 0
sort by [bytes_to]:
Connections: 3
  HOST                 CID      SUBS    PENDING     MSGS_TO     MSGS_FROM   BYTES_TO    BYTES_FROM  LANG     VERSION
  ::1:59259            83       1       0           4           0           20          0           go       1.1.0
  ::1:59349            91       1       0           2           0           10          0           go       1.1.0
  ::1:59342            90       1       0           0           0           0           0           go       1.1.0
```

## 7. Use different sort options

Use some different sort options to explore nats-top, such as:

`cid`, `subs`, `pending`, `msgs_to`, `msgs_from`, `bytes_to`, `bytes_from`, `lang`, `version`

You can also set the sort option on the command line using the `-sort` flag. For example: `nats-top -sort bytes_to`.

## 8. Display the registered subscriptions.

In nats-top, enter the command `s` to toggle displaying connection subscriptions. When enabled, you see the subscription subject in nats-top table:

```
nats-server version 0.6.6 (uptime: 1h2m23s)
Server:
  Load: CPU:  0.0%  Memory: 10.4M  Slow Consumers: 0
  In:   Msgs: 108  Bytes: 643  Msgs/Sec: 0.0  Bytes/Sec: 0
  Out:  Msgs: 185  Bytes: 1.0K  Msgs/Sec: 0.0  Bytes/Sec: 0

Connections: 3
  HOST                 CID      SUBS    PENDING     MSGS_TO     MSGS_FROM   BYTES_TO    BYTES_FROM  LANG     VERSION SUBSCRIPTIONS
  ::1:59708            115      1       0           6           0           48          0           go       1.1.0   foo.bar
  ::1:59758            122      1       0           1           0           8           0           go       1.1.0   foo
  ::1:59817            124      1       0           0           0           0           0           go       1.1.0   foo
```

## 9. Quit nats-top

Use the `q` command to quit nats-top.

## 10. Restart nats-top with a specified query

For example, to query for the connection with largest number of subscriptions:

```bash
nats-top -n 1 -sort subs
```

Result: nats-top displays only the client connection with the largest number of subscriptions:

```
nats-server version 0.6.6 (uptime: 1h7m0s)
Server:
  Load: CPU:  0.0%  Memory: 10.4M  Slow Consumers: 0
  In:   Msgs: 109  Bytes: 651  Msgs/Sec: 0.0  Bytes/Sec: 0
  Out:  Msgs: 187  Bytes: 1.0K  Msgs/Sec: 0.0  Bytes/Sec: 0

Connections: 3
  HOST                 CID      SUBS    PENDING     MSGS_TO     MSGS_FROM   BYTES_TO    BYTES_FROM  LANG     VERSION
  ::1:59708            115      1       0           6           0           48          0           go       1.1.0
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/nats-tools/nats_top/nats-top-tutorial.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
