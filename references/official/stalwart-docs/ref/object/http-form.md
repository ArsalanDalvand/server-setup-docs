---
title: "HttpForm"
source: "https://stalw.art/docs/ref/object/http-form"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/http-form"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "HttpForm"
---
# HttpForm

      

Configures the contact form submission endpoint.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › HTTP › Contact Form

## Fields

[Section titled “Fields”](#fields)

##### deliverTo

[Section titled “deliverTo”](#deliverto)

Type: `EmailAddress[]`

List of local e-mail addresses to deliver the contact form to.

##### defaultFromAddress

[Section titled “defaultFromAddress”](#defaultfromaddress)

Type: `String` · default: `"postmaster@localhost"`

The default e-mail address to use when the sender does not provide one.

##### fieldEmail

[Section titled “fieldEmail”](#fieldemail)

Type: `String?`

The name of the field in the contact form that contains the e-mail address of the sender.

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `false`

Whether to enable contact form submissions.

##### fieldHoneyPot

[Section titled “fieldHoneyPot”](#fieldhoneypot)

Type: `String?`

The name of the field in the contact form that is used as a honey pot to catch spam bots.

##### maxSize

[Section titled “maxSize”](#maxsize)

Type: `Size` · default: `"100kb"`

Maximum size of the contact form submission in bytes.

##### defaultName

[Section titled “defaultName”](#defaultname)

Type: `String` · default: `"Anonymous"`

The default name to use when the sender does not provide one.

##### fieldName

[Section titled “fieldName”](#fieldname)

Type: `String?`

The name of the field in the contact form that contains the name of the sender.

##### rateLimit

[Section titled “rateLimit”](#ratelimit)

Type: [`Rate`](#rate)`?` · default: `{"count":5,"period":"1h"}`

Maximum number of contact form submissions that can be made in a timeframe by a given IP address.

##### defaultSubject

[Section titled “defaultSubject”](#defaultsubject)

Type: `String` · default: `"Contact form submission"`

The default subject to use when the sender does not provide one.

##### fieldSubject

[Section titled “fieldSubject”](#fieldsubject)

Type: `String?`

The name of the field in the contact form that contains the subject of the message.

##### validateDomain

[Section titled “validateDomain”](#validatedomain)

Type: `Boolean` · default: `true`

Whether to validate the domain of the sender’s email address.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The HttpForm singleton is available via the `urn:stalwart:jmap` capability.

### x:HttpForm/get

[Section titled “x:HttpForm/get”](#xhttpformget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysHttpFormGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpForm/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:HttpForm/set

[Section titled “x:HttpForm/set”](#xhttpformset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysHttpFormUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:HttpForm/set",          {            "update": {              "singleton": {                "defaultFromAddress": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get HttpForm
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update HttpForm --field defaultFromAddress='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Rate

[Section titled “Rate”](#rate)

Defines a rate limit as a count over a time period.

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0` · min: 1 · max: 1000000

Count

##### period

[Section titled “period”](#period)

Type: `Duration` · default: `"0s"` · min: 1

Period