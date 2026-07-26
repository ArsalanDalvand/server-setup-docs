---
title: "Rules"
source: "https://stalw.art/docs/spamfilter/rules"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/rules"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Rules"
---
# Rules

      

Stalwart supports custom spam filter rules that analyse incoming messages and assign tags dynamically. Rules use [[configuration/expressions|expressions]] to evaluate specific parts of a message (headers, body, sender information, attachments, and so on) and to decide which tags to apply.

Stalwart ships with a set of preconfigured rules that cover common spam patterns. Administrators can also create custom rules tailored to local requirements.

## Configuration

[Section titled “Configuration”](#configuration)

Each rule is a [[ref/object/spam-rule|SpamRule]] object (found in the WebUI under  Settings ›  Spam Filter › Rules). SpamRule is a multi-variant object: the variant selects which part of the message the rule evaluates (`Url`, `Domain`, `Email`, `Ip`, `Header`, `Body`, or `Any`).

Each rule instance carries:

[[ref/object/spam-rule#enable|`enable`]]: whether the rule is active. Disabled rules are skipped during filtering.
[[ref/object/spam-rule#condition|`condition`]]: an [[ref/object/spam-rule#expression|`Expression`]] that evaluates the rule. If the expression returns a tag, that tag is assigned to the message; if it evaluates to `false`, the rule is skipped.
[[ref/object/spam-rule#priority|`priority`]]: the order in which rules are evaluated. Lower values run first. Default 500.

Example reproducing the freemail/disposable provider rule, using the `Email` variant:

```
{  "@type": "Email",  "name": "STWT_FREE_OR_DISP",  "enable": true,  "priority": 63,  "condition": {    "match": [      {"if": "!contains(['env_from', 'from', 'reply_to', 'to', 'cc', 'bcc', 'dnt'], location) || is_empty(sld)", "then": "false"},      {"if": "key_exists('freemail-providers', sld)", "then": "'FREEMAIL_' + to_uppercase(location)"},      {"if": "key_exists('disposable-providers', sld)", "then": "'DISPOSABLE_' + to_uppercase(location)"}    ],    "else": "false"  }}
```

## Core rules

[Section titled “Core rules”](#core-rules)

Core rules are predefined spam filter rules maintained by Stalwart Labs. They are identified by a name starting with `STWT_` and cover common spam patterns and behaviours. Core rules are updated regularly to remain effective against evolving tactics.

The latest version of the core rules is maintained in the [Spam Filter repository](https://github.com/stalwartlabs/spam-filter). Stalwart can be configured to download and apply rule [[spamfilter/settings/general#updates|updates]] automatically.

Core rules should not be modified directly, because any changes are overwritten on the next update. The only safe in-place modification is toggling [[ref/object/spam-rule#enable|`enable`]] on a core rule, which is preserved across updates. To customise behaviour beyond enabling or disabling a core rule, disable the core rule and create a custom rule with the desired values.

For example, to disable a core rule and add a custom one:

```
{  "@type": "Header",  "name": "STWT_SOME_SYSTEM_RULE",  "enable": false}
```

```
{  "@type": "Header",  "name": "MY_CUSTOM_RULE",  "enable": true,  "priority": 20,  "condition": {"else": "custom expression here"}}
```

This keeps customisations intact while still benefiting from ongoing updates to the core rules.