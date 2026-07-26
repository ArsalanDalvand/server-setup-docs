---
title: "TaskManager"
source: "https://stalw.art/docs/ref/object/task-manager"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/task-manager"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TaskManager"
---
# TaskManager

      

Configures task execution settings including retry strategies.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Task Manager

## Fields

[Section titled “Fields”](#fields)

##### maxAttempts

[Section titled “maxAttempts”](#maxattempts)

Type: `UnsignedInt` · default: `3` · min: 1

Maximum number of attempts for retrying a task

##### strategy

[Section titled “strategy”](#strategy)

Type: [`TaskRetryStrategy`](#taskretrystrategy) · required

Strategy to use for retrying failed tasks

##### totalDeadline

[Section titled “totalDeadline”](#totaldeadline)

Type: `Duration` · default: `"6h"`

Total deadline for retrying a task before it is marked as failed

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The TaskManager singleton is available via the `urn:stalwart:jmap` capability.

### x:TaskManager/get

[Section titled “x:TaskManager/get”](#xtaskmanagerget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysTaskManagerGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TaskManager/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:TaskManager/set

[Section titled “x:TaskManager/set”](#xtaskmanagerset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysTaskManagerUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TaskManager/set",          {            "update": {              "singleton": {                "maxAttempts": 3              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get TaskManager
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update TaskManager --field maxAttempts=3
```

## Nested types

[Section titled “Nested types”](#nested-types)

### TaskRetryStrategy

[Section titled “TaskRetryStrategy”](#taskretrystrategy)

Retry strategy for failed tasks.

`ExponentialBackoff`: Exponential backoff. Carries the fields of [`TaskRetryStrategyBackoff`](#taskretrystrategybackoff).
`FixedDelay`: Fixed delay. Carries the fields of [`TaskRetryStrategyFixed`](#taskretrystrategyfixed).

#### TaskRetryStrategyBackoff

[Section titled “TaskRetryStrategyBackoff”](#taskretrystrategybackoff)

Exponential backoff retry strategy settings.

##### factor

[Section titled “factor”](#factor)

Type: `Float` · default: `2.0` · min: 1

Backoff factor for calculating retry delays

##### initialDelay

[Section titled “initialDelay”](#initialdelay)

Type: `Duration` · default: `"1m"`

Initial delay before retrying a failed task

##### maxDelay

[Section titled “maxDelay”](#maxdelay)

Type: `Duration` · default: `"30m"`

Maximum delay between retry attempts

##### jitter

[Section titled “jitter”](#jitter)

Type: `Boolean` · default: `true`

Whether to apply jitter to the retry delay to avoid thundering herd problem

#### TaskRetryStrategyFixed

[Section titled “TaskRetryStrategyFixed”](#taskretrystrategyfixed)

Fixed delay retry strategy settings.

##### delay

[Section titled “delay”](#delay)

Type: `Duration` · default: `"5m"`

Fixed delay before retrying a failed task