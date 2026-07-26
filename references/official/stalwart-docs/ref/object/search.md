---
title: "Search"
source: "https://stalw.art/docs/ref/object/search"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/search"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Search"
---
# Search

      

Configures full-text search indexing for emails, calendars, contacts, and tracing.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Search

## Fields

[Section titled “Fields”](#fields)

##### indexBatchSize

[Section titled “indexBatchSize”](#indexbatchsize)

Type: `UnsignedInt` · default: `100` · min: 1

Number of items to process in each batch during indexing operations

##### defaultLanguage

[Section titled “defaultLanguage”](#defaultlanguage)

Type: [[ref/enum/locale|`Locale`]] · default: `"en_US"`

Default language to use when language detection is not possible

##### supportedLanguages

[Section titled “supportedLanguages”](#supportedlanguages)

Type: [[ref/enum/locale|`Locale`]]`[]` · default: `["en_US"]`

List of languages to enable for full-text search

##### indexCalendar

[Section titled “indexCalendar”](#indexcalendar)

Type: `Boolean` · default: `true`

Enable full-text search indexing for calendar data

##### indexCalendarFields

[Section titled “indexCalendarFields”](#indexcalendarfields)

Type: [`SearchCalendarField`](#searchcalendarfield)`[]` · default: `["title","description","location","owner","attendee","start","uid"]`

List of calendar fields to index

##### indexContacts

[Section titled “indexContacts”](#indexcontacts)

Type: `Boolean` · default: `true`

Enable full-text search indexing for contacts data

##### indexContactFields

[Section titled “indexContactFields”](#indexcontactfields)

Type: [`SearchContactField`](#searchcontactfield)`[]` · default: `["member","kind","name","nickname","organization","email","phone","onlineService","address","note","uid"]`

List of contact fields to index

##### indexEmail

[Section titled “indexEmail”](#indexemail)

Type: `Boolean` · default: `true`

Enable full-text search indexing for email content and metadata

##### indexEmailFields

[Section titled “indexEmailFields”](#indexemailfields)

Type: [`SearchEmailField`](#searchemailfield)`[]` · default: `["from","to","cc","bcc","subject","body","attachment","receivedAt","sentAt","size","hasAttachment"]`

List of email fields to index

##### indexTelemetry

[Section titled “indexTelemetry”](#indextelemetry)

Type: `Boolean` · [[server/enterprise|enterprise]] · default: `true`

Enable full-text search indexing for tracing data

##### indexTracingFields

[Section titled “indexTracingFields”](#indextracingfields)

Type: [`SearchTracingField`](#searchtracingfield)`[]` · [[server/enterprise|enterprise]] · default: `["eventType","queueId","keywords"]`

List of tracing fields to index

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Search singleton is available via the `urn:stalwart:jmap` capability.

### x:Search/get

[Section titled “x:Search/get”](#xsearchget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSearchGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Search/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Search/set

[Section titled “x:Search/set”](#xsearchset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSearchUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Search/set",          {            "update": {              "singleton": {                "indexBatchSize": 100              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Search
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Search --field indexBatchSize=100
```

## Enums

[Section titled “Enums”](#enums)

### SearchCalendarField

[Section titled “SearchCalendarField”](#searchcalendarfield)
ValueLabel`title`Event Title`description`Event Description`location`Event Location`owner`Event Owner`attendee`Event Attendee`start`Event Start Date`uid`Event UID

### SearchContactField

[Section titled “SearchContactField”](#searchcontactfield)
ValueLabel`member`Group Member`kind`Contact Kind`name`Contact Name`nickname`Contact Nickname`organization`Contact Organization`email`Contact Email`phone`Contact Phone`onlineService`Contact Online Service`address`Contact Address`note`Contact Note`uid`Contact UID

### SearchEmailField

[Section titled “SearchEmailField”](#searchemailfield)
ValueLabel`from`From Address`to`To Address`cc`Cc Address`bcc`Bcc Address`subject`Subject`body`Body Content`attachment`Attachment Content`receivedAt`Received Date`sentAt`Sent Date`size`Message Size`hasAttachment`Has Attachment`headers`Email Headers

### SearchTracingField

[Section titled “SearchTracingField”](#searchtracingfield)
ValueLabel`eventType`Event Type`queueId`Queue ID`keywords`Keywords