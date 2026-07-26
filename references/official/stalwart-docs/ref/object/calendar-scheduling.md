---
title: "CalendarScheduling"
source: "https://stalw.art/docs/ref/object/calendar-scheduling"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/calendar-scheduling"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "CalendarScheduling"
---
# CalendarScheduling

      

Configures calendar scheduling, iTIP messaging, and HTTP RSVP settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Calendar & Contacts › Scheduling

## Fields

[Section titled “Fields”](#fields)

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Enables the scheduling features for calendar events, allowing users to send and receive invitations

##### httpRsvpEnable

[Section titled “httpRsvpEnable”](#httprsvpenable)

Type: `Boolean` · default: `true`

Enables the HTTP RSVP feature for calendar invitations, allowing users to respond via a web interface.

##### httpRsvpLinkExpiry

[Section titled “httpRsvpLinkExpiry”](#httprsvplinkexpiry)

Type: `Duration` · default: `"90d"`

Sets the expiration duration for HTTP RSVP links, after which they will no longer be valid.

##### httpRsvpUrl

[Section titled “httpRsvpUrl”](#httprsvpurl)

Type: `Uri?`

Specifies a custom URL for the HTTP RSVP endpoint, where users can respond to calendar invitations.

##### autoAddInvitations

[Section titled “autoAddInvitations”](#autoaddinvitations)

Type: `Boolean` · default: `false`

Automatically adds incoming invitations to the user’s calendar.

##### itipMaxSize

[Section titled “itipMaxSize”](#itipmaxsize)

Type: `Size` · default: `"512kb"` · min: 100

Sets the maximum iCalendar object size for incoming iTIP messages.

##### maxRecipients

[Section titled “maxRecipients”](#maxrecipients)

Type: `UnsignedInt` · default: `100` · min: 1

Sets the maximum number of recipients for outbound iTIP messages.

##### emailTemplate

[Section titled “emailTemplate”](#emailtemplate)

Type: `Html?` · [[server/enterprise|enterprise]]

Specifies the HTML template used for rendering iMIP invitations.

##### httpRsvpTemplate

[Section titled “httpRsvpTemplate”](#httprsvptemplate)

Type: `Html?` · [[server/enterprise|enterprise]]

Specifies the HTML template used for rendering HTTP RSVP confirmations.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The CalendarScheduling singleton is available via the `urn:stalwart:jmap` capability.

### x:CalendarScheduling/get

[Section titled “x:CalendarScheduling/get”](#xcalendarschedulingget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysCalendarSchedulingGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:CalendarScheduling/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:CalendarScheduling/set

[Section titled “x:CalendarScheduling/set”](#xcalendarschedulingset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysCalendarSchedulingUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:CalendarScheduling/set",          {            "update": {              "singleton": {                "enable": true              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get CalendarScheduling
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update CalendarScheduling --field enable=true
```