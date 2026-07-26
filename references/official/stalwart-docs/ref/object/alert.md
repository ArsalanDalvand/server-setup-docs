---
title: "Alert"
source: "https://stalw.art/docs/ref/object/alert"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/alert"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Alert"
---
# Alert

      

Defines an alert rule triggered by metric conditions.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Telemetry › Alerts

## Fields

[Section titled “Fields”](#fields)

##### condition

[Section titled “condition”](#condition)

Type: [`Expression`](#expression) · required

The condition that triggers the alert.

##### emailAlert

[Section titled “emailAlert”](#emailalert)

Type: [`AlertEmail`](#alertemail) · required

Email notification settings

##### eventAlert

[Section titled “eventAlert”](#eventalert)

Type: [`AlertEvent`](#alertevent) · required

Event notification settings

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Enable or disable the alert

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Alert object is available via the `urn:stalwart:jmap` capability.

### x:Alert/get

[Section titled “x:Alert/get”](#xalertget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysAlertGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Alert/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Alert/set

[Section titled “x:Alert/set”](#xalertset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysAlertCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Alert/set",          {            "create": {              "new1": {                "condition": {                  "else": "Example",                  "match": {}                },                "emailAlert": {                  "@type": "Disabled"                },                "eventAlert": {                  "@type": "Disabled"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysAlertUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Alert/set",          {            "update": {              "id1": {                "condition": {                  "else": "Example",                  "match": {}                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysAlertDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Alert/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Alert/query

[Section titled “x:Alert/query”](#xalertquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysAlertQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Alert/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Alert id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Alert \  --field 'condition={"else":"Example","match":{}}' \  --field 'emailAlert={"@type":"Disabled"}' \  --field 'eventAlert={"@type":"Disabled"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Alert
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Alert id1 --field condition='{"else":"Example","match":{}}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Alert --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Expression

[Section titled “Expression”](#expression)

A conditional expression with match rules and a default value.

##### match

[Section titled “match”](#match)

Type: [`ExpressionMatch`](#expressionmatch)`[]`

List of conditions and their corresponding results

##### else

[Section titled “else”](#else)

Type: `String` · required

Else condition

#### ExpressionMatch

[Section titled “ExpressionMatch”](#expressionmatch)

A single condition-result pair in an expression.

##### if

[Section titled “if”](#if)

Type: `String` · required

If condition

##### then

[Section titled “then”](#then)

Type: `String` · required

Then clause

### AlertEmail

[Section titled “AlertEmail”](#alertemail)

Defines email notification settings for alerts.

`Disabled`: Disabled. No additional fields.
`Enabled`: Enabled. Carries the fields of [`AlertEmailProperties`](#alertemailproperties).

#### AlertEmailProperties

[Section titled “AlertEmailProperties”](#alertemailproperties)

Alert email notification settings.

##### body

[Section titled “body”](#body)

Type: `Text` · required

The body of the email

##### fromAddress

[Section titled “fromAddress”](#fromaddress)

Type: `EmailAddress` · required

The email address of the sender

##### fromName

[Section titled “fromName”](#fromname)

Type: `String?`

The name of the sender

##### subject

[Section titled “subject”](#subject)

Type: `String` · required

The subject of the email

##### to

[Section titled “to”](#to)

Type: `EmailAddress[]` · min items: 1

The email address of the recipient(s)

### AlertEvent

[Section titled “AlertEvent”](#alertevent)

Defines event notification settings for alerts.

`Disabled`: Disabled. No additional fields.
`Enabled`: Enabled. Carries the fields of [`AlertEventProperties`](#alerteventproperties).

#### AlertEventProperties

[Section titled “AlertEventProperties”](#alerteventproperties)

Alert event notification settings.

##### eventMessage

[Section titled “eventMessage”](#eventmessage)

Type: `Text?`

The message of the event to trigger