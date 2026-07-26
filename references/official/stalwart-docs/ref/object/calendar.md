---
title: "Calendar"
source: "https://stalw.art/docs/ref/object/calendar"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/calendar"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Calendar"
---
# Calendar

      

Configures calendar settings including iCalendar limits and default names.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Calendar & Contacts › Calendar

## Fields

[Section titled “Fields”](#fields)

##### defaultDisplayName

[Section titled “defaultDisplayName”](#defaultdisplayname)

Type: `String?` · default: `"Stalwart Calendar"`

Specifies the default display name for a calendar when it is created

##### defaultHrefName

[Section titled “defaultHrefName”](#defaulthrefname)

Type: `String?` · default: `"default"`

Specifies the default href name for a calendar when it is created

##### maxAttendees

[Section titled “maxAttendees”](#maxattendees)

Type: `UnsignedInt` · default: `20` · max: 100000

Specifies the maximum number of attendees that can be included in a single iCalendar instance

##### maxRecurrenceExpansions

[Section titled “maxRecurrenceExpansions”](#maxrecurrenceexpansions)

Type: `UnsignedInt` · default: `3000`

Specifies the maximum number of instances that can be generated from a recurring iCalendar event

##### maxICalendarSize

[Section titled “maxICalendarSize”](#maxicalendarsize)

Type: `Size` · default: `"512kb"`

Specifies the maximum size of an iCalendar file that can be uploaded to the server

##### maxCalendars

[Section titled “maxCalendars”](#maxcalendars)

Type: `UnsignedInt?` · default: `250` · min: 1

The default maximum number of calendars a user can create

##### maxEvents

[Section titled “maxEvents”](#maxevents)

Type: `UnsignedInt?` · min: 1

The default maximum number of calendar events a user can create

##### maxParticipantIdentities

[Section titled “maxParticipantIdentities”](#maxparticipantidentities)

Type: `UnsignedInt?` · default: `100` · min: 1

The default maximum number of participant identities a user can create

##### maxEventNotifications

[Section titled “maxEventNotifications”](#maxeventnotifications)

Type: `UnsignedInt?` · min: 1

The default maximum number of notifications a user can create for calendar events

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Calendar singleton is available via the `urn:stalwart:jmap` capability.

### x:Calendar/get

[Section titled “x:Calendar/get”](#xcalendarget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysCalendarGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Calendar/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Calendar/set

[Section titled “x:Calendar/set”](#xcalendarset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysCalendarUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Calendar/set",          {            "update": {              "singleton": {                "defaultDisplayName": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Calendar
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Calendar --field defaultDisplayName='updated value'
```