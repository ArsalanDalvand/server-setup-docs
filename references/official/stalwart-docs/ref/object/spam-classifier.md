---
title: "SpamClassifier"
source: "https://stalw.art/docs/ref/object/spam-classifier"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-classifier"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamClassifier"
---
# SpamClassifier

      

Configures the spam classifier model, training parameters, and auto-learning settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › Classifier

## Fields

[Section titled “Fields”](#fields)

##### model

[Section titled “model”](#model)

Type: [`SpamClassifierModel`](#spamclassifiermodel) · required

The spam classifier model to use.

##### learnHamFromCard

[Section titled “learnHamFromCard”](#learnhamfromcard)

Type: `Boolean` · default: `true`

Whether to automatically learn ham messages from senders in the user’s address book.

##### learnSpamFromRblHits

[Section titled “learnSpamFromRblHits”](#learnspamfromrblhits)

Type: `UnsignedInt` · default: `2` · max: 100

Number of DNSBL servers that list the sender to auto-learn as spam

##### learnSpamFromTraps

[Section titled “learnSpamFromTraps”](#learnspamfromtraps)

Type: `Boolean` · default: `true`

Train as spam messages sent to spam trap addresses

##### holdSamplesFor

[Section titled “holdSamplesFor”](#holdsamplesfor)

Type: `Duration` · default: `"180d"`

Duration to hold training samples for

##### minHamSamples

[Section titled “minHamSamples”](#minhamsamples)

Type: `UnsignedInt` · default: `100` · max: 10000 · min: 1

Minimum number of ham samples required for training

##### minSpamSamples

[Section titled “minSpamSamples”](#minspamsamples)

Type: `UnsignedInt` · default: `100` · max: 10000 · min: 1

Minimum number of spam samples required for training

##### reservoirCapacity

[Section titled “reservoirCapacity”](#reservoircapacity)

Type: `UnsignedInt` · default: `1024` · max: 100000 · min: 100

The capacity of the training sample reservoir

##### trainFrequency

[Section titled “trainFrequency”](#trainfrequency)

Type: `Duration?` · default: `"12h"`

Frequency to train the spam classifier

##### learnHamFromReply

[Section titled “learnHamFromReply”](#learnhamfromreply)

Type: `Boolean` · default: `true`

Whether to automatically learn ham messages that are replies to messages sent by the recipient.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamClassifier singleton is available via the `urn:stalwart:jmap` capability.

### x:SpamClassifier/get

[Section titled “x:SpamClassifier/get”](#xspamclassifierget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSpamClassifierGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamClassifier/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamClassifier/set

[Section titled “x:SpamClassifier/set”](#xspamclassifierset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSpamClassifierUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamClassifier/set",          {            "update": {              "singleton": {                "model": {                  "@type": "FtrlFh"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamClassifier
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SpamClassifier --field model='{"@type":"FtrlFh"}'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SpamClassifierModel

[Section titled “SpamClassifierModel”](#spamclassifiermodel)

Defines the model used for spam classification.

`FtrlFh`: FTRL-Proximal + Feature Hashing. Carries the fields of [`SpamClassifierFtrlFh`](#spamclassifierftrlfh).
`FtrlCcfh`: FTRL-Proximal + Cuckoo Feature Hashing. Carries the fields of [`SpamClassifierFtrlCcfh`](#spamclassifierftrlccfh).
`Disabled`: Disabled. No additional fields.

#### SpamClassifierFtrlFh

[Section titled “SpamClassifierFtrlFh”](#spamclassifierftrlfh)

Defines a spam classifier model using FTRL-Proximal optimization with feature hashing.

##### parameters

[Section titled “parameters”](#parameters)

Type: [`FtrlParameters`](#ftrlparameters) · default: `{"numFeatures":"20"}`

Hyperparameters for the FTRL-FH model

##### featureL2Normalize

[Section titled “featureL2Normalize”](#featurel2normalize)

Type: `Boolean` · default: `true`

Whether to L2-normalize feature values in the spam classifier

##### featureLogScale

[Section titled “featureLogScale”](#featurelogscale)

Type: `Boolean` · default: `true`

Whether to apply sublinear scaling to feature values in the spam classifier

##### FtrlParameters

[Section titled “FtrlParameters”](#ftrlparameters)

Hyperparameters for the FTRL-Proximal optimization algorithm.

##### alpha

[Section titled “alpha”](#alpha)

Type: `Float` · default: `2` · min: 0

The alpha parameter for the FTRL-Proximal algorithm

##### beta

[Section titled “beta”](#beta)

Type: `Float` · default: `1` · min: 0

The beta parameter for the FTRL-Proximal algorithm

##### numFeatures

[Section titled “numFeatures”](#numfeatures)

Type: [`ModelSize`](#modelsize) · default: `"20"`

The number of parameters (2^n)

##### l1Ratio

[Section titled “l1Ratio”](#l1ratio)

Type: `Float` · default: `0.001` · min: 0

The L1 regularization parameter for the FTRL-Proximal algorithm

##### l2Ratio

[Section titled “l2Ratio”](#l2ratio)

Type: `Float` · default: `0.0001` · min: 0

The L2 regularization parameter for the FTRL-Proximal algorithm

#### SpamClassifierFtrlCcfh

[Section titled “SpamClassifierFtrlCcfh”](#spamclassifierftrlccfh)

Defines a spam classifier model using FTRL-Proximal optimization with cuckoo feature hashing.

##### indicatorParameters

[Section titled “indicatorParameters”](#indicatorparameters)

Type: [`FtrlParameters`](#ftrlparameters) · default: `{"numFeatures":"18"}`

Hyperparameters for the indicator features in the FTRL-CCFH model

##### parameters

[Section titled “parameters”](#parameters-1)

Type: [`FtrlParameters`](#ftrlparameters) · default: `{"numFeatures":"20"}`

Hyperparameters for the FTRL-FH model

##### featureL2Normalize

[Section titled “featureL2Normalize”](#featurel2normalize-1)

Type: `Boolean` · default: `true`

Whether to L2-normalize feature values in the spam classifier

##### featureLogScale

[Section titled “featureLogScale”](#featurelogscale-1)

Type: `Boolean` · default: `true`

Whether to apply sublinear scaling to feature values in the spam classifier

## Enums

[Section titled “Enums”](#enums)

### ModelSize

[Section titled “ModelSize”](#modelsize)
ValueLabel`16`65k`17`131k`18`262k`19`524k`20`1M`21`2M`22`4M`23`8M`24`16M`25`33M`26`67M`27`134M`28`268M