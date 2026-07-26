---
title: "MtaDeliverySchedule"
source: "https://stalw.art/docs/ref/object/mta-delivery-schedule"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-delivery-schedule"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaDeliverySchedule"
---
# MtaDeliverySchedule

      

Defines retry and notification intervals for message delivery.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Outbound › Delivery Schedules

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Short identifier for the schedule

##### description

[Section titled “description”](#description)

Type: `String?`

A short description of the schedule, which can be used to identify it in the list of schedules

##### expiry

[Section titled “expiry”](#expiry)

Type: [`MtaDeliveryExpiration`](#mtadeliveryexpiration) · required

Whether to expire messages after a number of delivery attempts or after certain time (TTL)

##### notify

[Section titled “notify”](#notify)

Type: [`MtaDeliveryScheduleIntervalsOrDefault`](#mtadeliveryscheduleintervalsordefault) · required

List of delayed delivery DSN notification intervals

##### queueId

[Section titled “queueId”](#queueid)

Type: `Id<`[[ref/object/mta-virtual-queue|`MtaVirtualQueue`]]`>` · required

The name of the virtual queue to use for this schedule

##### retry

[Section titled “retry”](#retry)

Type: [`MtaDeliveryScheduleIntervalsOrDefault`](#mtadeliveryscheduleintervalsordefault) · required

List of retry intervals for message delivery

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaDeliverySchedule object is available via the `urn:stalwart:jmap` capability.

### x:MtaDeliverySchedule/get

[Section titled “x:MtaDeliverySchedule/get”](#xmtadeliveryscheduleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaDeliveryScheduleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaDeliverySchedule/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaDeliverySchedule/set

[Section titled “x:MtaDeliverySchedule/set”](#xmtadeliveryscheduleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaDeliveryScheduleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaDeliverySchedule/set",          {            "create": {              "new1": {                "expiry": {                  "@type": "Ttl"                },                "notify": {                  "@type": "Default"                },                "queueId": "<MtaVirtualQueue id>",                "retry": {                  "@type": "Default"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaDeliveryScheduleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaDeliverySchedule/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaDeliveryScheduleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaDeliverySchedule/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaDeliverySchedule/query

[Section titled “x:MtaDeliverySchedule/query”](#xmtadeliveryschedulequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaDeliveryScheduleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaDeliverySchedule/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MtaDeliverySchedule/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaDeliverySchedule id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaDeliverySchedule \  --field 'expiry={"@type":"Ttl"}' \  --field 'notify={"@type":"Default"}' \  --field 'queueId=<MtaVirtualQueue id>' \  --field 'retry={"@type":"Default"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaDeliverySchedulestalwart-cli query MtaDeliverySchedule --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaDeliverySchedule id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaDeliverySchedule --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### MtaDeliveryExpiration

[Section titled “MtaDeliveryExpiration”](#mtadeliveryexpiration)

Defines the message expiration policy for undelivered messages.

`Ttl`: Time To Live. Carries the fields of [`MtaDeliveryExpirationTtl`](#mtadeliveryexpirationttl).
`Attempts`: Delivery Attempts. Carries the fields of [`MtaDeliveryExpirationAttempts`](#mtadeliveryexpirationattempts).

#### MtaDeliveryExpirationTtl

[Section titled “MtaDeliveryExpirationTtl”](#mtadeliveryexpirationttl)

Defines a time-to-live based message expiration policy.

##### expire

[Section titled “expire”](#expire)

Type: `Duration` · default: `"3d"`

Time after which the message will be expired if it is not delivered

#### MtaDeliveryExpirationAttempts

[Section titled “MtaDeliveryExpirationAttempts”](#mtadeliveryexpirationattempts)

Defines a delivery-attempts based message expiration policy.

##### maxAttempts

[Section titled “maxAttempts”](#maxattempts)

Type: `UnsignedInt` · default: `5` · min: 1

Maximum number of delivery attempts before the message is considered failed

### MtaDeliveryScheduleIntervalsOrDefault

[Section titled “MtaDeliveryScheduleIntervalsOrDefault”](#mtadeliveryscheduleintervalsordefault)

Defines whether to use the default delivery schedule intervals or specify custom intervals.

`Default`: Use default intervals. No additional fields.
`Custom`: Specify custom intervals. Carries the fields of [`MtaDeliveryScheduleIntervals`](#mtadeliveryscheduleintervals).

#### MtaDeliveryScheduleIntervals

[Section titled “MtaDeliveryScheduleIntervals”](#mtadeliveryscheduleintervals)

Defines a custom list of delivery retry or notification intervals.

##### intervals

[Section titled “intervals”](#intervals)

Type: [`MtaDeliveryScheduleInterval`](#mtadeliveryscheduleinterval)`[]` · min items: 1

List of intervals

##### MtaDeliveryScheduleInterval

[Section titled “MtaDeliveryScheduleInterval”](#mtadeliveryscheduleinterval)

Defines a single time interval entry used in a delivery schedule.

##### duration

[Section titled “duration”](#duration)

Type: `Duration` · default: `"1h"`

Time interval for retries or notifications