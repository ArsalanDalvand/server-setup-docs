---
title: "SpamLlm"
source: "https://stalw.art/docs/ref/object/spam-llm"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-llm"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamLlm"
---
# SpamLlm

      

Configures the LLM-based spam classifier.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › LLM Classifier

## Fields

[Section titled “Fields”](#fields)

SpamLlm is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Disable"

[Section titled “@type: "Disable"”](#type-disable)

Disabled

### @type: "Enable"

[Section titled “@type: "Enable"”](#type-enable)

Enabled

##### categories

[Section titled “categories”](#categories)

Type: `String[]` · default: `["Unsolicited","Commercial","Harmful","Legitimate"]` · min items: 2

The expected categories in the LLM response

##### confidence

[Section titled “confidence”](#confidence)

Type: `String[]` · default: `["High","Medium","Low"]`

The expected confidence levels in the LLM response

##### responsePosCategory

[Section titled “responsePosCategory”](#responseposcategory)

Type: `UnsignedInt` · default: `0`

The position of the category field in the LLM response.

##### responsePosConfidence

[Section titled “responsePosConfidence”](#responseposconfidence)

Type: `UnsignedInt?` · default: `1`

The position of the confidence field in the LLM response.

##### responsePosExplanation

[Section titled “responsePosExplanation”](#responseposexplanation)

Type: `UnsignedInt?` · default: `2`

The position of the explanation field in the LLM response.

##### modelId

[Section titled “modelId”](#modelid)

Type: `Id<`[[ref/object/ai-model|`AiModel`]]`>` · required

The AI model to use for the LLM classifier

##### prompt

[Section titled “prompt”](#prompt)

Type: `Text` · required

The prompt to use for the LLM classifier

##### separator

[Section titled “separator”](#separator)

Type: `String` · default: `","`

The separator character used to parse the LLM response.

##### temperature

[Section titled “temperature”](#temperature)

Type: `Float` · default: `0.5` · max: 1 · min: 0

The temperature to use for the LLM classifier

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamLlm singleton is available via the `urn:stalwart:jmap` capability.

### x:SpamLlm/get

[Section titled “x:SpamLlm/get”](#xspamllmget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSpamLlmGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamLlm/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamLlm/set

[Section titled “x:SpamLlm/set”](#xspamllmset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSpamLlmUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamLlm/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamLlm
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SpamLlm --field description='updated value'
```