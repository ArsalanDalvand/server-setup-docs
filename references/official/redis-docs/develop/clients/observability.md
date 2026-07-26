---
title: "Observability"
source: "https://redis.io/docs/latest/develop/clients/observability/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/observability"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Observability"
---
# Observability

Monitor your client's activity for optimization and debugging.

Some Redis client libraries implement the [OpenTelemetry](https://opentelemetry.io/) (OTel)
observability framework to let you gather performance metrics
for your application. This can help you optimize performance and pinpoint problems
quickly. Currently, the following clients support OTel:

[[develop/clients/redis-py/observability|redis-py]]
[go-redis](/docs/latest/develop/clients/go/observability/)
[node-redis](/docs/latest/develop/clients/nodejs/observability/)

## Metrics overview

Metrics are quantitative measurements of the behavior of your application. They
provide information such as how often a certain operation occurs, how long it
takes to complete, or how many errors have occurred. By analyzing these metrics,
you can identify performance bottlenecks, errors, and other issues that need to
be addressed.

Redis clients follow the OTel push model. This uses the
[OLTP](https://opentelemetry.io/docs/specs/otlp/)
protocol to send metrics to a [collector](https://opentelemetry.io/docs/collector/)
(both [OLTP/gRPC](https://opentelemetry.io/docs/specs/otlp/#otlpgrpc) and
[OTLP/HTTP](https://opentelemetry.io/docs/specs/otlp/#otlphttp) are supported).
A storage layer (such as [Prometheus](https://prometheus.io/)) can then pull the
metrics from a collector endpoint, making the data available for monitoring tools
(such as [Grafana](https://grafana.com/)) to query and visualize. The basic
flow of data is shown below.

```
sequenceDiagram
    participant App as Redis Client
    participant Collector as OTel Collector
    participant Storage as Storage Layer<br/>(Prometheus)
    participant Monitor as Monitoring Tool<br/>(Grafana)

    App->>Collector: Push metrics
    Note over App,Collector: OTel push model

    Storage->>Collector: Pull metrics from endpoint
    Collector-->>Storage: Return metrics

    Monitor->>Storage: Query metrics data
    Storage-->>Monitor: Return metrics
    Note over Monitor: Visualize
```

The [Redis client observability demonstration](https://github.com/redis-developer/redis-client-observability)
on GitHub contains examples showing how to set up a local Grafana instance, then
connect it to a Redis client and visualize the metric data as it arrives.

## Redis metric groups

In Redis clients, the metrics collected by OTel are organized into the following
metric groups:

[`resiliency`](#group-resiliency): data related to the availability and health of the Redis connection.
[`connection-basic`](#group-connection-basic): minimal metrics about Redis connections made by the client.
[`connection-advanced`](#group-connection-advanced): more detailed metrics about Redis connections.
[`command`](#group-command): metrics about Redis commands executed by the client.
[`client-side-caching`](#group-client-side-caching): metrics about
[[develop/clients/client-side-caching|client-side caching]] operations.
[`streaming`](#group-streaming): metrics about
[stream](/docs/latest/develop/data-types/streams/) operations.
[`pubsub`](#group-pubsub): metrics about
[[develop/pubsub|pub/sub]] operations.

When you configure the client to activate OTel, you can select which metric groups
you are interested in. By default, only the `resiliency` and `connection-basic` groups are enabled.
The metrics in each group are described in the
[Metrics reference](#metrics-reference) below.

## Metrics reference

The metric groups and the metrics they contain are described below. The
name in parentheses after each group name is the group's identifier, which you
use when you configure the client to select which metrics to collect.

The metrics contain attributes that provide extra information (such as
the client library and server details) that you can use to filter and
aggregate the data. The attributes are described in the Attributes
section following the metric groups. The badge shown after the attribute
name can be any of the following:

`required`: This attribute will always be present in the metrics.
`optional`: This attribute may be present in the metrics.
`conditionally required`: This attribute will be present in the metrics only if a certain condition is met,
such as when a specific error occurs. The condition is described in the attribute description.
`recommended`: Specific client libraries may not support this attribute in some situations.

```
{
  "$schema": "https://redis.io/schemas/client-observability-metrics-v0.2.json",
  "attributes_definitions": {
    "attribute_index": {
      "db.client.connection.pool.name": {
        "attribute": "db.client.connection.pool.name",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The name of the connection pool.",
        "examples": "10.1.2.8/0; mypool",
        "requirement_level": "required",
        "type": "string"
      },
      "db.client.connection.state": {
        "attribute": "db.client.connection.state",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The state of a connection in the pool. Possible values: idle or used",
        "examples": "idle; used",
        "requirement_level": "required",
        "type": "string"
      },
      "db.client.geofailover.fail_from": {
        "attribute": "db.client.geofailover.fail_from",
        "definition_source_table": "semconv_defined_attributes",
        "description": "FQDN of database from which failover happened.",
        "examples": "{ip}:{port}/{weight}",
        "requirement_level": "required",
        "type": "string"
      },
      "db.client.geofailover.fail_to": {
        "attribute": "db.client.geofailover.fail_to",
        "definition_source_table": "semconv_defined_attributes",
        "description": "FQDN of database to which failover happened.",
        "examples": "{ip}:{port}/{weight}",
        "requirement_level": "required",
        "type": "string"
      },
      "db.client.geofailover.reason": {
        "attribute": "db.client.geofailover.reason",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Reason for failover to happen",
        "examples": "automatic; manual",
        "requirement_condition": "for transactions and pipelined calls",
        "requirement_level": "required",
        "type": "string"
      },
      "db.namespace": {
        "attribute": "db.namespace",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The name of the database namespace. For Redis, this is the database number.",
        "examples": "0; 1; 2",
        "requirement_level": "recommended",
        "type": "string"
      },
      "db.operation.batch.size": {
        "attribute": "db.operation.batch.size",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The number of queries included in a batch operation.",
        "examples": "2; 3; 4",
        "requirement_condition": "the operation is a pipeline or a transaction",
        "requirement_level": "conditionally_required",
        "type": "int"
      },
      "db.operation.name": {
        "attribute": "db.operation.name",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The name of the operation being executed.",
        "examples": "GET; SET; HGETALL",
        "requirement_level": "required",
        "type": "string"
      },
      "db.response.status_code": {
        "attribute": "db.response.status_code",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The Redis error prefix.",
        "examples": "OK; WRONGTYPE",
        "requirement_condition": "if the operation has failed and the status code is available",
        "requirement_level": "conditionally_required",
        "type": "string"
      },
      "db.stored_procedure.name": {
        "attribute": "db.stored_procedure.name",
        "definition_source_table": "semconv_defined_attributes",
        "description": "The name or sha1 digest of a Lua script in the database.",
        "examples": "GetCustomer",
        "requirement_condition": "if operation applies to a specific Lua script",
        "requirement_level": "recommended",
        "type": "string"
      },
      "db.system.name": {
        "attribute": "db.system.name",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Always set to `redis`",
        "examples": "redis",
        "requirement_level": "required",
        "type": "string"
      },
      "error.type": {
        "attribute": "error.type",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Class of error the operation ended with",
        "examples": "java.net.UnknownHostException, server_certificate_invalid, ClusterCrossSlotError",
        "requirement_condition": "if and only if the operation has failed",
        "requirement_level": "conditionally_required",
        "type": "string"
      },
      "network.peer.address": {
        "attribute": "network.peer.address",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Peer address of the database node where the operation was performed.",
        "examples": "10.1.2.80; /tmp/my.sock",
        "requirement_level": "recommended",
        "type": "string"
      },
      "network.peer.port": {
        "attribute": "network.peer.port",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Peer port number of the network connection.",
        "examples": "65123",
        "requirement_condition": "if and only if network.peer.address is set",
        "requirement_level": "recommended",
        "type": "int"
      },
      "redis.client.connection.close.reason": {
        "attribute": "redis.client.connection.close.reason",
        "definition_source_table": "redis_defined_attributes",
        "description": "Reason why the connection was closed. Must be one of: application_close (app/client explicitly closed); pool_eviction_idle (idle timeout / min-idle policy); server_close (server initiated FIN/RST/hangup); error (the error itself is shown in error.type); healthcheck_failed (failed ping/heartbeat)",
        "examples": "application_close; pool_eviction_idle; server_close; error; healthcheck_failed",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.connection.notification": {
        "attribute": "redis.client.connection.notification",
        "definition_source_table": "redis_defined_attributes",
        "description": "Server-side maintenance notifications containing the message type.",
        "examples": "MOVING; MIGRATING",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.connection.pubsub": {
        "attribute": "redis.client.connection.pubsub",
        "definition_source_table": "redis_defined_attributes",
        "description": "Specifies if the connection is dedicated to a pubsub subscription. Default is False",
        "examples": "true; false",
        "requirement_level": "required",
        "type": "bool"
      },
      "redis.client.consumer_name": {
        "attribute": "redis.client.consumer_name",
        "definition_source_table": "redis_defined_attributes",
        "description": "Name of the stream consumer",
        "examples": "consumer1",
        "requirement_level": "optional",
        "type": "string"
      },
      "redis.client.csc.reason": {
        "attribute": "redis.client.csc.reason",
        "definition_source_table": "redis_defined_attributes",
        "description": "The reason why an item was evicted from the client-side cache. Must be one of: full (cache size limit reached); invalidation (received server invalidation message); ttl (local expiration)",
        "examples": "full; invalidation",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.csc.result": {
        "attribute": "redis.client.csc.result",
        "definition_source_table": "redis_defined_attributes",
        "description": "Result of client-side cache lookup. Must be one of: hit (served from local cache); miss (fetched from server)",
        "examples": "hit; miss",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.errors.category": {
        "attribute": "redis.client.errors.category",
        "definition_source_table": "redis_defined_attributes",
        "description": "Error category, to facilitate grouping and filtering of errors.",
        "examples": "network (transport/DNS/socket issues); tls (security/TLS errors); auth (authentication/authorization); server (Redis server errors including cluster errors); other (uncategorized errors)",
        "requirement_condition": "for errors",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.errors.internal": {
        "attribute": "redis.client.errors.internal",
        "definition_source_table": "redis_defined_attributes",
        "description": "Specifies whether the error has been handled internally or surfaced to the user.",
        "examples": "true; false",
        "requirement_condition": "for errors",
        "requirement_level": "required",
        "type": "bool"
      },
      "redis.client.library": {
        "attribute": "redis.client.library",
        "definition_source_table": "redis_defined_attributes",
        "description": "The redis client library reporting the metrics in format: \u003clibrary_name\u003e:\u003cversion\u003e",
        "examples": "redis-py:v8.0.0b1; go-redis:v9.0.0",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.operation.retry_attempts": {
        "attribute": "redis.client.operation.retry_attempts",
        "definition_source_table": "redis_defined_attributes",
        "description": "Shows how many times the command has been retried.",
        "examples": "0; 1; 2",
        "requirement_level": "required",
        "type": "int"
      },
      "redis.client.pubsub.channel": {
        "attribute": "redis.client.pubsub.channel",
        "definition_source_table": "redis_defined_attributes",
        "description": "Name of the pub/sub channel",
        "examples": "channel1",
        "requirement_condition": "if the hide_pubsub_channel_names SDK config is disabled",
        "requirement_level": "conditionally_required",
        "type": "string"
      },
      "redis.client.pubsub.message.direction": {
        "attribute": "redis.client.pubsub.message.direction",
        "definition_source_table": "redis_defined_attributes",
        "description": "Determines whether a processed message is sent or received. Must be one of: out (published messages);in(received messages)",
        "examples": "out; in",
        "requirement_level": "required",
        "type": "string"
      },
      "redis.client.pubsub.sharded": {
        "attribute": "redis.client.pubsub.sharded",
        "definition_source_table": "redis_defined_attributes",
        "description": "Specifies it the message was sent/received on a sharded pubsub connection",
        "examples": "true; false",
        "requirement_level": "optional",
        "type": "bool"
      },
      "redis.client.stream.consumer_group": {
        "attribute": "redis.client.stream.consumer_group",
        "definition_source_table": "redis_defined_attributes",
        "description": "Name of the stream consumer group",
        "examples": "consumer_group1",
        "requirement_level": "optional",
        "type": "string"
      },
      "redis.client.stream.name": {
        "attribute": "redis.client.stream.name",
        "definition_source_table": "redis_defined_attributes",
        "description": "Name of the Redis stream",
        "examples": "stream1",
        "requirement_condition": "if the hide_stream_names SDK config is disabled",
        "requirement_level": "conditionally_required",
        "type": "string"
      },
      "server.address": {
        "attribute": "server.address",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Name of the database host.",
        "examples": "example.com; 10.1.2.80; /tmp/my.sock; redis-13206.aws-cluster-71165.cto.redislabs.com",
        "requirement_level": "recommended",
        "type": "string"
      },
      "server.port": {
        "attribute": "server.port",
        "definition_source_table": "semconv_defined_attributes",
        "description": "Server port number.",
        "examples": "80; 8080; 443",
        "requirement_condition": "using a port other than the default port for this DBMS and if server.address is set",
        "requirement_level": "conditionally_required",
        "type": "int"
      }
    },
    "tables": [
      {
        "attributes": [
          {
            "attribute": "db.system.name",
            "description": "Always set to `redis`",
            "examples": "redis",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "db.operation.name",
            "description": "The name of the operation being executed.",
            "examples": "GET; SET; HGETALL",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "db.namespace",
            "description": "The name of the database namespace. For Redis, this is the database number.",
            "examples": "0; 1; 2",
            "requirement_level": "recommended",
            "type": "string"
          },
          {
            "attribute": "db.response.status_code",
            "description": "The Redis error prefix.",
            "examples": "OK; WRONGTYPE",
            "requirement_condition": "if the operation has failed and the status code is available",
            "requirement_level": "conditionally_required",
            "type": "string"
          },
          {
            "attribute": "error.type",
            "description": "Class of error the operation ended with",
            "examples": "java.net.UnknownHostException, server_certificate_invalid, ClusterCrossSlotError",
            "requirement_condition": "if and only if the operation has failed",
            "requirement_level": "conditionally_required",
            "type": "string"
          },
          {
            "attribute": "db.operation.batch.size",
            "description": "The number of queries included in a batch operation.",
            "examples": "2; 3; 4",
            "requirement_condition": "the operation is a pipeline or a transaction",
            "requirement_level": "conditionally_required",
            "type": "int"
          },
          {
            "attribute": "db.stored_procedure.name",
            "description": "The name or sha1 digest of a Lua script in the database.",
            "examples": "GetCustomer",
            "requirement_condition": "if operation applies to a specific Lua script",
            "requirement_level": "recommended",
            "type": "string"
          },
          {
            "attribute": "network.peer.address",
            "description": "Peer address of the database node where the operation was performed.",
            "examples": "10.1.2.80; /tmp/my.sock",
            "requirement_level": "recommended",
            "type": "string"
          },
          {
            "attribute": "network.peer.port",
            "description": "Peer port number of the network connection.",
            "examples": "65123",
            "requirement_condition": "if and only if network.peer.address is set",
            "requirement_level": "recommended",
            "type": "int"
          },
          {
            "attribute": "server.address",
            "description": "Name of the database host.",
            "examples": "example.com; 10.1.2.80; /tmp/my.sock; redis-13206.aws-cluster-71165.cto.redislabs.com",
            "requirement_level": "recommended",
            "type": "string"
          },
          {
            "attribute": "server.port",
            "description": "Server port number.",
            "examples": "80; 8080; 443",
            "requirement_condition": "using a port other than the default port for this DBMS and if server.address is set",
            "requirement_level": "conditionally_required",
            "type": "int"
          },
          {
            "attribute": "db.client.connection.pool.name",
            "description": "The name of the connection pool.",
            "examples": "10.1.2.8/0; mypool",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "db.client.connection.state",
            "description": "The state of a connection in the pool. Possible values: idle or used",
            "examples": "idle; used",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "db.client.geofailover.fail_from",
            "description": "FQDN of database from which failover happened.",
            "examples": "{ip}:{port}/{weight}",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "db.client.geofailover.fail_to",
            "description": "FQDN of database to which failover happened.",
            "examples": "{ip}:{port}/{weight}",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "db.client.geofailover.reason",
            "description": "Reason for failover to happen.",
            "examples": "automatic; manual",
            "requirement_condition": "for transactions and pipelined calls",
            "requirement_level": "required",
            "type": "string"
          }
        ],
        "id": "semconv_defined_attributes",
        "title": "SemConv defined attributes"
      },
      {
        "attributes": [
          {
            "attribute": "redis.client.library",
            "description": "The redis client library reporting the metrics in format: \u003clibrary_name\u003e:\u003cversion\u003e",
            "examples": "redis-py:v8.0.0b1; go-redis:v9.0.0",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.operation.retry_attempts",
            "description": "Shows how many times the command has been retried.",
            "examples": "0; 1; 2",
            "requirement_level": "required",
            "type": "int"
          },
          {
            "attribute": "redis.client.errors.internal",
            "description": "Specifies whether the error has been handled internally or surfaced to the user.",
            "examples": "true; false",
            "requirement_condition": "for errors",
            "requirement_level": "required",
            "type": "bool"
          },
          {
            "attribute": "redis.client.errors.category",
            "description": "Error category, to facilitate grouping and filtering of errors.",
            "examples": "network (transport/DNS/socket issues); tls (security/TLS errors); auth (authentication/authorization); server (Redis server errors including cluster errors); other (uncategorized errors)",
            "requirement_condition": "for errors",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.connection.close.reason",
            "description": "Reason why the connection was closed. Must be one of: application_close (app/client explicitly closed); pool_eviction_idle (idle timeout / min-idle policy); server_close (server initiated FIN/RST/hangup); error (the error itself is shown in error.type); healthcheck_failed (failed ping/heartbeat)",
            "examples": "application_close; pool_eviction_idle; server_close; error; healthcheck_failed",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.connection.notification",
            "description": "Server-side maintenance notifications containing the message type.",
            "examples": "MOVING; MIGRATING",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.connection.pubsub",
            "description": "Specifies if the connection is dedicated to a pubsub subscription. Default is False",
            "examples": "true; false",
            "requirement_level": "required",
            "type": "bool"
          },
          {
            "attribute": "redis.client.consumer_name",
            "description": "Name of the stream consumer",
            "examples": "consumer1",
            "requirement_level": "optional",
            "type": "string"
          },
          {
            "attribute": "redis.client.csc.reason",
            "description": "The reason why an item was evicted from the client-side cache. Must be one of: full (cache size limit reached); invalidation (received server invalidation message); ttl (local expiration)",
            "examples": "full; invalidation",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.csc.result",
            "description": "Result of client-side cache lookup. Must be one of: hit (served from local cache); miss (fetched from server)",
            "examples": "hit; miss",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.pubsub.channel",
            "description": "Name of the pub/sub channel",
            "examples": "channel1",
            "requirement_condition": "if the hide_pubsub_channel_names SDK config is disabled",
            "requirement_level": "conditionally_required",
            "type": "string"
          },
          {
            "attribute": "redis.client.pubsub.message.direction",
            "description": "Determines whether a processed message is sent or received. Must be one of: out (published messages);in(received messages)",
            "examples": "out; in",
            "requirement_level": "required",
            "type": "string"
          },
          {
            "attribute": "redis.client.pubsub.sharded",
            "description": "Specifies it the message was sent/received on a sharded pubsub connection",
            "examples": "true; false",
            "requirement_level": "optional",
            "type": "bool"
          },
          {
            "attribute": "redis.client.stream.consumer_group",
            "description": "Name of the stream consumer group",
            "examples": "consumer_group1",
            "requirement_level": "optional",
            "type": "string"
          },
          {
            "attribute": "redis.client.stream.name",
            "description": "Name of the Redis stream",
            "examples": "stream1",
            "requirement_condition": "if the hide_stream_names SDK config is disabled",
            "requirement_level": "conditionally_required",
            "type": "string"
          }
        ],
        "id": "redis_defined_attributes",
        "title": "Redis defined attributes"
      }
    ]
  },
  "metric_groups": [
    {
      "id": "resiliency",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "recommended",
              "ref": "server.address"
            },
            {
              "cardinality": "recommended",
              "ref": "server.port"
            },
            {
              "cardinality": "optional",
              "ref": "network.peer.address"
            },
            {
              "cardinality": "optional",
              "ref": "network.peer.port"
            },
            {
              "cardinality": "required",
              "ref": "error.type"
            },
            {
              "cardinality": "required",
              "ref": "redis.client.errors.category"
            },
            {
              "cardinality": "optional",
              "ref": "db.response.status_code"
            },
            {
              "cardinality": "required",
              "ref": "redis.client.errors.internal"
            },
            {
              "cardinality": "required",
              "ref": "redis.client.operation.retry_attempts"
            }
          ],
          "description": "A counter of all errors (both returned to the user and handled internally in the client library)",
          "name": "redis.client.errors",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "errors",
            "symbol": "{error}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "recommended",
              "ref": "server.address"
            },
            {
              "cardinality": "recommended",
              "ref": "server.port"
            },
            {
              "cardinality": "optional",
              "ref": "network.peer.address"
            },
            {
              "cardinality": "optional",
              "ref": "network.peer.port"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.connection.notification"
            }
          ],
          "description": "Tracks server-side maintenance notifications",
          "name": "redis.client.maintenance.notifications",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "notifications",
            "symbol": "{notification}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.geofailover.reason"
            },
            {
              "cardinality": "required",
              "ref": "db.client.geofailover.fail_from"
            },
            {
              "cardinality": "required",
              "ref": "db.client.geofailover.fail_to"
            }
          ],
          "description": "Tracks client-side geographic failovers",
          "name": "redis.client.geofailover.failovers",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "failovers",
            "symbol": "{failover}"
          }
        }
      ],
      "title": "Resiliency \u0026 cluster specific metrics"
    },
    {
      "id": "command",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "recommended",
              "ref": "db.namespace"
            },
            {
              "cardinality": "recommended",
              "ref": "db.operation.name"
            },
            {
              "cardinality": "optional",
              "ref": "db.response.status_code"
            },
            {
              "cardinality": "optional",
              "ref": "error.type"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.errors.category"
            },
            {
              "cardinality": "recommended",
              "ref": "server.address"
            },
            {
              "cardinality": "recommended",
              "ref": "server.port"
            },
            {
              "cardinality": "optional",
              "ref": "network.peer.address"
            },
            {
              "cardinality": "optional",
              "ref": "network.peer.port"
            },
            {
              "cardinality": "required",
              "ref": "redis.client.operation.retry_attempts"
            }
          ],
          "description": "Command duration from the application's point of view (includes retries)",
          "name": "db.client.operation.duration",
          "otel": {
            "aggregation": "explicit_bucket",
            "async": false,
            "instrument_kind": "histogram",
            "monotonic": false
          },
          "type": "Histogram",
          "unit": {
            "semantic": "seconds",
            "symbol": "s"
          }
        }
      ],
      "title": "Command metrics"
    },
    {
      "id": "connection-basic",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            },
            {
              "cardinality": "optional",
              "ref": "db.client.connection.state"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.connection.pubsub"
            }
          ],
          "description": "Current connections by state (idle/used). Sum by state for âlive connectionsâ.",
          "name": "db.client.connection.count",
          "observable": false,
          "otel": {
            "async": false,
            "instrument_kind": "updowncounter",
            "monotonic": false
          },
          "type": "UpDownCounter",
          "unit": {
            "semantic": "connections",
            "symbol": "{connection}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            }
          ],
          "description": "The time it took to create a new connection, including handshake",
          "name": "db.client.connection.create_time",
          "otel": {
            "aggregation": "explicit_bucket",
            "async": false,
            "instrument_kind": "histogram",
            "monotonic": false
          },
          "type": "Histogram",
          "unit": {
            "semantic": "seconds",
            "symbol": "s"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.connection.notification"
            }
          ],
          "description": "How many times the connection timeout has been increased/decreased (after a server maintenance notification). Counts up for relaxed timeout, counts down for unrelaxed timeout",
          "name": "redis.client.connection.relaxed_timeout",
          "otel": {
            "async": false,
            "instrument_kind": "updowncounter",
            "monotonic": false
          },
          "type": "UpDownCounter",
          "unit": {
            "semantic": "relaxations",
            "symbol": "{relaxation}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            }
          ],
          "description": "Connections that have been handed off to another node (e.g after a MOVING notification)",
          "name": "redis.client.connection.handoff",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "handoffs",
            "symbol": "1"
          }
        }
      ],
      "title": "Connection metrics (basic)"
    },
    {
      "id": "connection-advanced",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            }
          ],
          "description": "The number of current pending requests for an open connection.",
          "name": "db.client.connection.pending_requests",
          "observable": false,
          "otel": {
            "async": false,
            "instrument_kind": "updowncounter",
            "monotonic": false
          },
          "type": "UpDownCounter",
          "unit": {
            "semantic": "requests",
            "symbol": "{request}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            }
          ],
          "description": "The time it took to obtain an open connection from the pool. Only reported for clients that wait until a new connection becomes available.",
          "name": "db.client.connection.wait_time",
          "otel": {
            "aggregation": "explicit_bucket",
            "async": false,
            "instrument_kind": "histogram",
            "monotonic": false
          },
          "type": "Histogram",
          "unit": {
            "semantic": "seconds",
            "symbol": "s"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.connection.close.reason"
            },
            {
              "cardinality": "optional",
              "ref": "error.type"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.errors.category"
            }
          ],
          "description": "Total number of closed connections (tcp sockets)",
          "name": "redis.client.connection.closed",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "connections",
            "symbol": "{connection}"
          }
        }
      ],
      "title": "Connection metrics (advanced)"
    },
    {
      "id": "client-side-caching",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "recommended",
              "ref": "db.namespace"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.csc.result"
            }
          ],
          "description": "The total number of requests to the cache. Used to calculate the hit ratio (hits / total).",
          "name": "redis.client.csc.requests",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "requests",
            "symbol": "{request}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "required",
              "ref": "db.client.connection.pool.name"
            }
          ],
          "description": "The total number of cached responses currently stored. This is an async (observable) metric.",
          "name": "redis.client.csc.items",
          "observable": true,
          "otel": {
            "async": true,
            "instrument_kind": "observable_gauge",
            "monotonic": false
          },
          "type": "Gauge",
          "unit": {
            "semantic": "items",
            "symbol": "{item}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.csc.reason"
            }
          ],
          "description": "The total number of items evicted from the cache due to size limits or server invalidation signals.",
          "name": "redis.client.csc.evictions",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "evictions",
            "symbol": "{eviction}"
          }
        },
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            }
          ],
          "description": "The total number of bytes saved by serving responses from the cache instead of the network.",
          "name": "redis.client.csc.network_saved",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "bytes",
            "symbol": "By"
          }
        }
      ],
      "title": "Client-Side caching metrics"
    },
    {
      "id": "pubsub",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.pubsub.message.direction"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.pubsub.channel"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.pubsub.sharded"
            }
          ],
          "description": "Tracks published and received messages.",
          "name": "redis.client.pubsub.messages",
          "otel": {
            "async": false,
            "instrument_kind": "counter",
            "monotonic": true
          },
          "type": "Counter",
          "unit": {
            "semantic": "messages",
            "symbol": "{message}"
          }
        }
      ],
      "title": "PubSub metrics"
    },
    {
      "id": "streaming",
      "metrics": [
        {
          "attributes": [
            {
              "cardinality": "required",
              "ref": "redis.client.library"
            },
            {
              "cardinality": "required",
              "ref": "db.system.name"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.stream.name"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.stream.consumer_group"
            },
            {
              "cardinality": "optional",
              "ref": "redis.client.consumer_name"
            }
          ],
          "description": "End-to-end lag per message, showing how stale messages are when the application starts processing them. Can be computed from stream message_id timestamp vs current time.",
          "name": "redis.client.stream.lag",
          "otel": {
            "aggregation": "explicit_bucket",
            "async": false,
            "instrument_kind": "histogram",
            "monotonic": false
          },
          "type": "Histogram",
          "unit": {
            "semantic": "seconds",
            "symbol": "s"
          }
        }
      ],
      "title": "Stream metrics"
    }
  ],
  "namespace_default": "redis.client",
  "spec_version": "0.2"
}
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page