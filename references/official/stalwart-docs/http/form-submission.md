---
title: "Form Handling"
source: "https://stalw.art/docs/http/form-submission"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/form-submission"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Form Handling"
---
# Form Handling

      

Stalwart can accept HTTP form submissions on the `/form` endpoint and turn each submission into an email message delivered to one or more local recipients. The feature is typically used for web forms such as contact or feedback forms, where submissions need to be forwarded to a designated group of local recipients on the mail server.

Deliveries are limited to local recipients only. External recipients are rejected to prevent the form endpoint from being used as an open relay.

## Security Features

[Section titled “Security Features”](#security-features)

Two anti-abuse mechanisms protect the form endpoint:

Per-IP rate limiting throttles how many submissions a single client can send within a given window, so that legitimate traffic is not crowded out by automated floods.
A honeypot field is a hidden form field that is invisible to human users but visible to bots. Legitimate users will not fill it out; bots that attempt to fill every input will, and the server discards any submission in which the honeypot field is populated. This avoids the friction of CAPTCHAs while still filtering out automated spam.

## Configuration

[Section titled “Configuration”](#configuration)

Form handling is configured through the [[ref/object/http-form|HttpForm]] singleton (found in the WebUI under  Settings ›  Network › HTTP › Contact Form). The relevant fields are:

[[ref/object/http-form#enable|`enable`]]: turns the feature on or off. When `false`, the server returns an error for any request to `/form`. Default `false`.
[[ref/object/http-form#maxsize|`maxSize`]]: maximum size of a single submission. Default `"100kb"`.
[[ref/object/http-form#validatedomain|`validateDomain`]]: whether the server validates the domain of the sender’s email address. Default `true`.
[[ref/object/http-form#ratelimit|`rateLimit`]]: per-IP submission rate, as a count over a time period. Default five submissions per hour.
[[ref/object/http-form#deliverto|`deliverTo`]]: the list of local email addresses that receive the generated message.
[[ref/object/http-form#fieldemail|`fieldEmail`]]: the name of the form field that carries the sender’s email address, used as the message `From` address.
[[ref/object/http-form#defaultfromaddress|`defaultFromAddress`]]: fallback `From` address used when the submission does not include one. Default `"postmaster@localhost"`.
[[ref/object/http-form#fieldhoneypot|`fieldHoneyPot`]]: the name of the hidden honeypot field; a populated value flags the submission as spam.
[[ref/object/http-form#fieldname|`fieldName`]]: the name of the form field that carries the sender’s name, used in the `From` header.
[[ref/object/http-form#defaultname|`defaultName`]]: fallback name used when the submission does not include one. Default `"Anonymous"`.
[[ref/object/http-form#fieldsubject|`fieldSubject`]]: the name of the form field that carries the message subject.
[[ref/object/http-form#defaultsubject|`defaultSubject`]]: fallback subject line used when the submission does not include one. Default `"Contact form submission"`.

Example configuration:

```
{  "enable": true,  "maxSize": 10240,  "validateDomain": true,  "rateLimit": {"count": 5, "period": "1h"},  "deliverTo": ["[email protected]", "[email protected]"],  "fieldEmail": "email",  "defaultFromAddress": "[email protected]",  "fieldHoneyPot": "subject",  "fieldName": "name",  "defaultName": "Anonymous",  "fieldSubject": "subject",  "defaultSubject": "Contact Form"}
```