---
title: "Enabling Monitoring"
source: "https://docs.nats.io/running-a-nats-service/configuration/monitoring.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/monitoring"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Enabling Monitoring"
---
# Enabling Monitoring

## NATS Server Monitoring

To monitor the NATS messaging system, `nats-server` provides a lightweight HTTP server on a dedicated monitoring port. The monitoring server provides several endpoints, providing statistics and other information.

The [NATS monitoring endpoints](/running-a-nats-service/nats_admin/monitoring.md) support [JSONP](https://en.wikipedia.org/wiki/JSONP) and [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing#How_CORS_works), making it easy to create single page monitoring web applications.

> Warning: `nats-server` does not have authentication/authorization for the monitoring endpoint. When you plan to open your `nats-server` to the internet make sure to not expose the monitoring port as well. By default monitoring binds to every interface `0.0.0.0` so consider setting monitoring to `localhost` or have appropriate firewall rules.

### Enabling monitoring from the command line

To enable the monitoring server, start the NATS server with the monitoring flag `-m` and the monitoring port, or turn it on in the [configuration file](#enable-monitoring-from-the-configuration-file).

```
-m, --http_port PORT             HTTP PORT for monitoring
-ms,--https_port PORT            Use HTTPS PORT for monitoring
```

Example:

```bash
nats-server -m 8222
```

```
[4528] 2019/06/01 20:09:58.572939 [INF] Starting nats-server version 2.0.0
[4528] 2019/06/01 20:09:58.573007 [INF] Starting http monitor on port 8222
[4528] 2019/06/01 20:09:58.573071 [INF] Listening for client connections on 0.0.0.0:4222
[4528] 2019/06/01 20:09:58.573090 [INF] nats-server is ready
```

To test, run `nats-server -m 8222`, then go to <http://localhost:8222/>

### Enable monitoring from the configuration file

You can also enable monitoring using the configuration file as follows:

```yaml
http_port: 8222
```

Binding to `localhost` as well:

```yaml
http: localhost:8222
```

For example, to monitor this server locally, the endpoint would be <http://localhost:8222/varz>. It reports various general statistics.

## Monitoring Tools

In addition to writing custom monitoring tools, you can monitor nats-server in Prometheus. The [Prometheus NATS Exporter](https://github.com/nats-io/prometheus-nats-exporter) allows you to configure the metrics you want to observe and store in Prometheus. There's a sample [Grafana](https://grafana.com) dashboard that you can use to visualize the server metrics.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/monitoring.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
