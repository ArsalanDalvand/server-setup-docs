---
title: "Monitoring JetStream"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/monitoring/monitoring_jetstream.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/monitoring/monitoring_jetstream"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Monitoring JetStream"
---
# Monitoring JetStream

## Commercial Options

If you want a "batteries-included" approach to high-cardinality NATS monitoring and observability, including JetStream streams and consumers, try the standalone [Synadia Insights](https://www.synadia.com/insights).

## Server Metrics

JetStream has a /[jsz ](/running-a-nats-service/configuration/monitoring.md#jetstream-information)HTTP endpoint and advisories available.

## Advisories

JetStream publishes a number of advisories that can inform operations about the health and the state of the Streams. These advisories are published to normal NATS subjects below `$JS.EVENT.ADVISORY.>` and one can store these advisories in JetStream Streams if desired.

The command `nats event --js-advisory` can view all these events on your console. The Golang package [jsm.go](https://github.com/nats-io/jsm.go) can consume and render these events and have data types for each of these events.

All these events have JSON Schemas that describe them, schemas can be viewed on the CLI using the `nats schema show <schema kind>` command.

| Description                                 | Subject                                                          | Kind                                                    |
| ------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------- |
| API interactions                            | `$JS.EVENT.ADVISORY.API`                                         | `io.nats.jetstream.advisory.v1.api_audit`               |
| Stream CRUD operations                      | `$JS.EVENT.ADVISORY.STREAM.CREATED.<STREAM>`                     | `io.nats.jetstream.advisory.v1.stream_action`           |
| Consumer CRUD operations                    | `$JS.EVENT.ADVISORY.CONSUMER.CREATED.<STREAM>.<CONSUMER>`        | `io.nats.jetstream.advisory.v1.consumer_action`         |
| Snapshot started using `nats stream backup` | `$JS.EVENT.ADVISORY.STREAM.SNAPSHOT_CREATE.<STREAM>`             | `io.nats.jetstream.advisory.v1.snapshot_create`         |
| Snapshot completed                          | `$JS.EVENT.ADVISORY.STREAM.SNAPSHOT_COMPLETE.<STREAM>`           | `io.nats.jetstream.advisory.v1.snapshot_complete`       |
| Restore started using `nats stream restore` | `$JS.EVENT.ADVISORY.STREAM.RESTORE_CREATE.<STREAM>`              | `io.nats.jetstream.advisory.v1.restore_create`          |
| Restore completed                           | `$JS.EVENT.ADVISORY.STREAM.RESTORE_COMPLETE.<STREAM>`            | `io.nats.jetstream.advisory.v1.restore_complete`        |
| Consumer maximum delivery reached           | `$JS.EVENT.ADVISORY.CONSUMER.MAX_DELIVERIES.<STREAM>.<CONSUMER>` | `io.nats.jetstream.advisory.v1.max_deliver`             |
| Message delivery naked using AckNak         | `$JS.EVENT.ADVISORY.CONSUMER.MSG_NAKED.<STREAM>.<CONSUMER>`      | `io.nats.jetstream.advisory.v1.nak`                     |
| Message delivery terminated using AckTerm   | `$JS.EVENT.ADVISORY.CONSUMER.MSG_TERMINATED.<STREAM>.<CONSUMER>` | `io.nats.jetstream.advisory.v1.terminated`              |
| Message acknowledged in a sampled Consumer  | `$JS.EVENT.METRIC.CONSUMER.ACK.<STREAM>.<CONSUMER>`              | `io.nats.jetstream.metric.v1.consumer_ack`              |
| Clustered Stream elected a new leader       | `$JS.EVENT.ADVISORY.STREAM.LEADER_ELECTED.<STREAM>`              | `io.nats.jetstream.advisory.v1.stream_leader_elected`   |
| Clustered Stream lost quorum                | `$JS.EVENT.ADVISORY.STREAM.QUORUM_LOST.<STREAM>`                 | `io.nats.jetstream.advisory.v1.stream_quorum_lost`      |
| Clustered Consumer elected a new leader     | `$JS.EVENT.ADVISORY.CONSUMER.LEADER_ELECTED.<STREAM>.<CONSUMER>` | `io.nats.jetstream.advisory.v1.consumer_leader_elected` |
| Clustered Consumer lost quorum              | `$JS.EVENT.ADVISORY.CONSUMER.QUORUM_LOST.<STREAM>.<CONSUMER>`    | `io.nats.jetstream.advisory.v1.consumer_quorum_lost`    |

## Dashboards

Check out [NATS Surveyor Dashboards](https://github.com/nats-io/nats-surveyor/tree/main/docker-compose/grafana/provisioning/dashboards).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/monitoring/monitoring_jetstream.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
