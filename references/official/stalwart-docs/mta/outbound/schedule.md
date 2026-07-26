---
title: "Scheduling"
source: "https://stalw.art/docs/mta/outbound/schedule"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/outbound/schedule"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Scheduling"
---
# Scheduling

      

A scheduling strategy defines the policies and behaviour that govern how outbound message deliveries are managed over time. Scheduling strategies control which virtual queue a message uses, how often delivery is retried after a failure, when delayed delivery-status notifications are generated, and when undeliverable messages are expired and bounced to the sender.

Scheduling strategies are defined as [[ref/object/mta-delivery-schedule|MtaDeliverySchedule]] objects (found in the WebUI under  Settings ›  MTA › Outbound › Delivery Schedules) and selected dynamically at runtime via the schedule expression on [[ref/object/mta-outbound-strategy|MtaOutboundStrategy]]; see [[mta/outbound/strategy|Strategies]] for details.

Each scheduling strategy controls the following aspects:

Virtual queue assignment: determines which [[mta/outbound/queue|virtual queue]] a message recipient is placed into for delivery.
Retry intervals: specify how frequently the MTA retries delivery after a failed attempt.
Delay notifications: control when delayed [[mta/reports/dsn|delivery-status notifications]] (DSNs) are generated and sent to the sender.
Expiration: defines how long the system should continue retrying before considering the message undeliverable.

## Queue

[Section titled “Queue”](#queue)

Each scheduling strategy must be associated with a [[mta/outbound/queue|virtual queue]], which determines where recipients are placed for delivery processing. The [[ref/object/mta-delivery-schedule#queueid|`queueId`]] field references the [[ref/object/mta-virtual-queue|MtaVirtualQueue]] object to use. Different scheduling strategies can reference different queues to isolate message classes.

Each recipient is evaluated independently and placed into the appropriate queue based on the selected strategy, so local deliveries can be processed separately from remote ones, or high-importance messages can bypass a congested general-purpose queue. The referenced virtual queue must exist before the scheduling strategy can be applied, as queues are not created automatically.

## Retries

[Section titled “Retries”](#retries)

When a delivery attempt fails temporarily (for example because the remote server is unavailable), the MTA retries delivery later. Retry timing is controlled by the [[ref/object/mta-delivery-schedule#retry|`retry`]] field, which is a multi-variant value: the `Default` variant uses the built-in retry schedule, and the `Custom` variant carries an `intervals` array of `MtaDeliveryScheduleInterval` entries, each with a `duration` field.

With intervals such as `2m`, `5m`, `10m`, `15m`, `30m`, `1h`, `2h`, delivery is retried 2 minutes after the first failure, 5 minutes after that, and so on. When the list is exhausted, the last duration is reused for all subsequent attempts, until the message is delivered, expires, or exceeds the maximum number of delivery attempts. Retries are scheduled independently for each recipient.

## Delay notifications

[Section titled “Delay notifications”](#delay-notifications)

Delayed Delivery Status Notifications (DSNs) inform the sender that a message has not yet been delivered but is still being retried. These notifications are useful for alerting users to delivery delays before the final expiration.

Delay notifications are configured via [[ref/object/mta-delivery-schedule#notify|`notify`]], which accepts the same multi-variant shape as `retry`: the `Default` variant uses the built-in schedule and the `Custom` variant carries an `intervals` array. Durations are measured from the moment the message enters the queue; for example an interval list of `1d`, `3d` sends a delay DSN one day and three days after queue entry, provided the message is still undelivered.

Each interval triggers at most one notification. These delay notifications are separate from the final bounce message, which is sent when the message is permanently undeliverable.

## Expiration

[Section titled “Expiration”](#expiration)

Message expiration determines how long the MTA should continue attempting delivery before giving up and returning a bounce [[mta/reports/dsn|DSN]] to the sender. Expiration is configured via [[ref/object/mta-delivery-schedule#expiry|`expiry`]], a multi-variant field with two variants:

`Ttl`: Time-To-Live. The message expires after a fixed duration, regardless of the number of delivery attempts. Carries an `expire` duration (default `3d`).
`Attempts`: Attempt-based. The message expires after a specified number of failed delivery attempts, regardless of how much time has passed. Carries a `maxAttempts` count (default 5).

TTL-based expiration suits deployments where guaranteed delivery within a fixed time window is important; attempt-based expiration suits environments where retry frequency may vary, but a maximum effort should be enforced.

A schedule that expires messages after 4 days with custom retry intervals:

```
{  "name": "local",  "queueId": "<MtaVirtualQueue id>",  "retry": {    "@type": "Custom",    "intervals": [      {"duration": "2m"}, {"duration": "5m"}, {"duration": "10m"},      {"duration": "15m"}, {"duration": "30m"}, {"duration": "1h"}, {"duration": "2h"}    ]  },  "notify": {    "@type": "Custom",    "intervals": [{"duration": "1d"}, {"duration": "3d"}]  },  "expiry": {"@type": "Ttl", "expire": "4d"}}
```

A schedule that gives up after 15 delivery attempts uses the `Attempts` variant instead:

```
{  "name": "relay",  "queueId": "<MtaVirtualQueue id>",  "retry": {"@type": "Default"},  "notify": {"@type": "Default"},  "expiry": {"@type": "Attempts", "maxAttempts": 15}}
```

## Examples

[Section titled “Examples”](#examples)

### Queues by message type

[Section titled “Queues by message type”](#queues-by-message-type)

Different message types (DSNs, reports, auto-generated notifications, local deliveries, general outbound mail) can be placed into separate virtual queues. The schedule expression on [[ref/object/mta-outbound-strategy|MtaOutboundStrategy]] branches on context variables (`is_local_domain('*', rcpt_domain)`, `source == 'dsn'`, `source == 'report'`, `source == 'autogenerated'`) and selects the appropriate scheduling-strategy name. Each referenced [[ref/object/mta-delivery-schedule|MtaDeliverySchedule]] sets [[ref/object/mta-delivery-schedule#queueid|`queueId`]] to a different [[ref/object/mta-virtual-queue|MtaVirtualQueue]] (`local`, `dsn`, `report`, `autogen`, `remote`), and each virtual queue has an appropriate [[ref/object/mta-virtual-queue#threadspernode|`threadsPerNode`]] value.

The schedule expression:

```
{  "schedule": {    "match": [      {"if": "is_local_domain('*', rcpt_domain)", "then": "'local'"},      {"if": "source == 'dsn'", "then": "'dsn'"},      {"if": "source == 'report'", "then": "'report'"},      {"if": "source == 'autogenerated'", "then": "'autogen'"}    ],    "else": "'remote'"  }}
```

Paired with five MtaVirtualQueue objects (`local`/1000, `dsn`/50, `report`/10, `autogen`/20, `remote`/1000 threads per node) and five MtaDeliverySchedule objects whose [[ref/object/mta-delivery-schedule#queueid|`queueId`]] points at the matching queue.

### Priority-based delivery queues

[Section titled “Priority-based delivery queues”](#priority-based-delivery-queues)

Delivery priority can be driven by the `MT-PRIORITY` SMTP extension. A schedule expression that branches on `priority == 1` and `priority == 3` selects `high-priority`, `low-priority`, or `normal-priority` scheduling strategies. The high-priority strategy uses a short retry interval (`Custom` variant with intervals `["1m"]`) and a high-concurrency virtual queue; the low-priority strategy throttles retries (`["30m"]`) and uses a queue with very few threads.

```
{  "schedule": {    "match": [      {"if": "priority == 1", "then": "'high-priority'"},      {"if": "priority == 3", "then": "'low-priority'"}    ],    "else": "'normal-priority'"  }}
```

The `high-priority` MtaDeliverySchedule uses `retry = {"@type": "Custom", "intervals": [{"duration": "1m"}]}` against a 2000-thread `high-priority` virtual queue; `low-priority` uses `retry = {"@type": "Custom", "intervals": [{"duration": "30m"}]}` against a 2-thread `low-priority` virtual queue; `normal-priority` uses the default schedule against a 100-thread `normal-priority` virtual queue.

### VIP client queue

[Section titled “VIP client queue”](#vip-client-queue)

Messages involving VIP clients can be routed to a dedicated queue. The schedule expression invokes `sql_query` against a lookup store to check whether sender or recipient is listed in a VIP table, and selects a `vip-client` scheduling strategy when it matches. The VIP scheduling strategy targets a high-concurrency queue and uses aggressive retry intervals; the default scheduling strategy targets a smaller queue with the standard retry schedule.

The `?` placeholders below use SQLite or MySQL syntax; on PostgreSQL use `$1`, `$2`, … instead, since the query is passed to the database driver verbatim.

```
{  "schedule": {    "match": [      {"if": "sql_query('my-db', 'SELECT 1 FROM vip_clients WHERE email = ? OR email = ?', [rcpt, sender])", "then": "'vip-client'"}    ],    "else": "'default'"  }}
```

The `vip-client` MtaDeliverySchedule targets a 1000-thread `vip` virtual queue with intervals `["1m", "5m", "10m"]`; `default` targets a 100-thread `default` virtual queue with the standard retry schedule.