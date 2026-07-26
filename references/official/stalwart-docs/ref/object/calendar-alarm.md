---
title: "CalendarAlarm"
source: "https://stalw.art/docs/ref/object/calendar-alarm"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/calendar-alarm"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "CalendarAlarm"
---
# CalendarAlarm

      

Configures calendar alarm email notifications.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Calendar & Contacts › Alarms

## Fields

[Section titled “Fields”](#fields)

##### allowExternalRcpts

[Section titled “allowExternalRcpts”](#allowexternalrcpts)

Type: `Boolean` · default: `false`

Allows calendar alarms to be sent to external recipients, enabling notifications to users outside the server

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Enables the calendar alarms feature, allowing users to set alarms for events and receive notifications via e-mail

##### fromEmail

[Section titled “fromEmail”](#fromemail)

Type: `EmailAddress?`

Specifies the e-mail address that will appear in the ‘From’ field of calendar alarm e-mails, ensuring that users can reply to or contact the sender

##### fromName

[Section titled “fromName”](#fromname)

Type: `String` · default: `"Stalwart Calendar"`

Specifies the name that will appear in the ‘From’ field of calendar alarm e-mails, providing a recognizable sender name for users

##### minTriggerInterval

[Section titled “minTriggerInterval”](#mintriggerinterval)

Type: `Duration` · default: `"1h"`

Specifies the minimum interval for calendar alarms, ensuring that alarms are not triggered too frequently

##### template

[Section titled “template”](#template)

Type: `Html?` · [[server/enterprise|enterprise]]

Specifies the HTML template used for rendering calendar alarm e-mails, allowing customization of the alarm notification format

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The CalendarAlarm singleton is available via the `urn:stalwart:jmap` capability.

### x:CalendarAlarm/get

[Section titled “x:CalendarAlarm/get”](#xcalendaralarmget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysCalendarAlarmGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:CalendarAlarm/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:CalendarAlarm/set

[Section titled “x:CalendarAlarm/set”](#xcalendaralarmset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysCalendarAlarmUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:CalendarAlarm/set",          {            "update": {              "singleton": {                "fromName": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get CalendarAlarm
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update CalendarAlarm --field fromName='updated value'
```