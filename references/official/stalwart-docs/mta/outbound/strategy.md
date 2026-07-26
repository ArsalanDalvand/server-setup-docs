---
title: "Strategies"
source: "https://stalw.art/docs/mta/outbound/strategy"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/outbound/strategy"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Strategies"
---
# Strategies

      

Delivery strategies define the behaviour and policies used when Stalwart MTA delivers email messages, whether to local recipients or remote systems. Strategies allow dynamic delivery logic driven by runtime conditions: [[configuration/expressions|expressions]] are evaluated for each recipient and message, so routing, scheduling, connection parameters, and transport security can all adapt to the context of each delivery.

Each message processed by the MTA is evaluated against four strategy expressions to determine how it should be handled. The expressions inspect attributes such as sender, recipient, message headers, IP address, or other metadata, and return the name of a named strategy (defined by the appropriate reference object) that should be applied.

There are four types of strategies:

[[mta/outbound/routing|Routing strategy]]: determines the delivery target. A recipient may be routed for local delivery, to a remote host via MX resolution, or through a predefined relay host.
[[mta/outbound/schedule|Scheduling strategy]]: governs how messages are queued and retried. It determines which virtual queue should be used, how frequently delivery attempts are made, how messages are prioritised, and when undeliverable messages expire.
[[mta/outbound/connection|Connection strategy]]: defines how outbound connections to remote mail servers are established, including source IP address, connection timeouts, and custom EHLO domains.
[[mta/outbound/tls|TLS strategy]]: determines the transport-layer security settings applied to the connection, including DANE, MTA-STS, STARTTLS, and certificate-validation policy.

Each strategy type is evaluated independently, but together they define a complete delivery policy.

## Configuration

[Section titled “Configuration”](#configuration)

Delivery behaviour is governed by four fields on the [[ref/object/mta-outbound-strategy|MtaOutboundStrategy]] singleton (found in the WebUI under  Settings ›  MTA › Outbound › Strategy):

[[ref/object/mta-outbound-strategy#route|`route`]]: expression selecting the route name, resolved to an [[ref/object/mta-route|MtaRoute]] object.
[[ref/object/mta-outbound-strategy#schedule|`schedule`]]: expression selecting the scheduling strategy name, resolved to an [[ref/object/mta-delivery-schedule|MtaDeliverySchedule]] object.
[[ref/object/mta-outbound-strategy#connection|`connection`]]: expression selecting the connection strategy name, resolved to an [[ref/object/mta-connection-strategy|MtaConnectionStrategy]] object.
[[ref/object/mta-outbound-strategy#tls|`tls`]]: expression selecting the TLS strategy name, resolved to an [[ref/object/mta-tls-strategy|MtaTlsStrategy]] object.

Each expression is evaluated for every recipient in a message. They have access to a wide set of [[configuration/variables#mta-variables|variables]] describing the message, its origin, recipients, delivery status, and more, so expressions can match against domain names, message metadata, delivery conditions, or error states. A named strategy object with the returned name must exist; if it does not, delivery fails.

## Example

[Section titled “Example”](#example)

A typical configuration sets each expression to pick a different strategy name based on context: `route` returns `'local'` when the recipient domain is local (`is_local_domain('', rcpt_domain)`) and `'mx'` otherwise; `schedule` returns `'local'` for local recipients, `'dsn'` for delivery-status notifications, `'report'` for aggregate reports, and `'remote'` otherwise; `connection` returns `'long-timeout'` when a previous attempt failed with a connection error and `'default'` otherwise; and `tls` returns `'invalid-tls'` when a previous attempt failed due to a TLS error and `'default'` otherwise. Corresponding [[ref/object/mta-route|MtaRoute]], [[ref/object/mta-delivery-schedule|MtaDeliverySchedule]], [[ref/object/mta-connection-strategy|MtaConnectionStrategy]], and [[ref/object/mta-tls-strategy|MtaTlsStrategy]] objects must be defined with the names referenced by the expressions.

```
{  "route": {    "match": [{"if": "is_local_domain('', rcpt_domain)", "then": "'local'"}],    "else": "'mx'"  },  "schedule": {    "match": [      {"if": "is_local_domain('*', rcpt_domain)", "then": "'local'"},      {"if": "source == 'dsn'", "then": "'dsn'"},      {"if": "source == 'report'", "then": "'report'"}    ],    "else": "'remote'"  },  "connection": {    "match": [{"if": "retry_num > 0 && last_error == 'connection'", "then": "'long-timeout'"}],    "else": "'default'"  },  "tls": {    "match": [{"if": "retry_num > 0 && last_error == 'tls'", "then": "'invalid-tls'"}],    "else": "'default'"  }}
```