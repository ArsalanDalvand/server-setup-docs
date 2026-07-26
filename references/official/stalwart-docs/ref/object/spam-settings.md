---
title: "SpamSettings"
source: "https://stalw.art/docs/ref/object/spam-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamSettings"
---
# SpamSettings

      

Configures global spam filter thresholds, greylisting, and trust settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › General

## Fields

[Section titled “Fields”](#fields)

##### trustContacts

[Section titled “trustContacts”](#trustcontacts)

Type: `Boolean` · default: `true`

Never classify messages as spam if they are sent from addresses present in the user’s address book.

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable the spam filter

##### greylistFor

[Section titled “greylistFor”](#greylistfor)

Type: `Duration?`

Time to keep an IP address in the grey list. The grey list is used to delay messages from unknown senders.

##### scoreDiscard

[Section titled “scoreDiscard”](#scorediscard)

Type: `Float` · default: `0` · max: 100 · min: -100

Discard messages with a score above this threshold

##### scoreReject

[Section titled “scoreReject”](#scorereject)

Type: `Float` · default: `0` · max: 100 · min: -100

Reject messages with a score above this threshold

##### scoreSpam

[Section titled “scoreSpam”](#scorespam)

Type: `Float` · default: `5` · max: 100 · min: -100

Mark as Spam messages with a score above this threshold

##### trustReplies

[Section titled “trustReplies”](#trustreplies)

Type: `Boolean` · default: `true`

Never classify messages as spam if they are replies to messages sent by the recipient.

##### spamFilterRulesUrl

[Section titled “spamFilterRulesUrl”](#spamfilterrulesurl)

Type: `Uri?` · default: `"https://github.com/stalwartlabs/spam-filter/releases/latest/download/spam-filter-rules.json.gz"`

URL to download spam filter rules from

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:SpamSettings/get

[Section titled “x:SpamSettings/get”](#xspamsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSpamSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamSettings/set

[Section titled “x:SpamSettings/set”](#xspamsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSpamSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamSettings/set",          {            "update": {              "singleton": {                "trustContacts": true              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SpamSettings --field trustContacts=true
```