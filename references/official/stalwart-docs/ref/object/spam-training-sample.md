---
title: "SpamTrainingSample"
source: "https://stalw.art/docs/ref/object/spam-training-sample"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-training-sample"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamTrainingSample"
---
# SpamTrainingSample

      

Stores an email sample used for spam classifier training.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Spam Samples

## Fields

[Section titled “Fields”](#fields)

##### from

[Section titled “from”](#from)

Type: `EmailAddress` · read-only · server-set

Email address of the sender of the message associated with this training sample

##### subject

[Section titled “subject”](#subject)

Type: `String` · server-set

Subject of the message associated with this training sample

##### blobId

[Section titled “blobId”](#blobid)

Type: `BlobId` · read-only

Reference to the stored message content

##### isSpam

[Section titled “isSpam”](#isspam)

Type: `Boolean` · read-only · default: `false`

Indicates whether the sample is spam (true) or ham (false)

##### accountId

[Section titled “accountId”](#accountid)

Type: `Id<`[[ref/object/account|`Account`]]`>?` · read-only

Identifier of the account associated with this training sample

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime` · server-set

Timestamp when the training sample is scheduled to expire

##### deleteAfterUse

[Section titled “deleteAfterUse”](#deleteafteruse)

Type: `Boolean` · read-only · default: `false`

Indicates whether the training sample should be deleted after being used for training

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamTrainingSample object is available via the `urn:stalwart:jmap` capability.

### x:SpamTrainingSample/get

[Section titled “x:SpamTrainingSample/get”](#xspamtrainingsampleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSpamTrainingSampleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTrainingSample/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamTrainingSample/set

[Section titled “x:SpamTrainingSample/set”](#xspamtrainingsampleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSpamTrainingSampleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTrainingSample/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSpamTrainingSampleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTrainingSample/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSpamTrainingSampleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTrainingSample/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamTrainingSample/query

[Section titled “x:SpamTrainingSample/query”](#xspamtrainingsamplequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSpamTrainingSampleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamTrainingSample/query",          {            "filter": {              "accountId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SpamTrainingSample/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`accountId`id of Account

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamTrainingSample id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SpamTrainingSample
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SpamTrainingSamplestalwart-cli query SpamTrainingSample --where accountId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SpamTrainingSample id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SpamTrainingSample --ids id1
```