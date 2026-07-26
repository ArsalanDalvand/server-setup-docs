---
title: "QueuedMessage"
source: "https://stalw.art/docs/ref/object/queued-message"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/queued-message"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "QueuedMessage"
---
# QueuedMessage

      

Represents a queued email message pending delivery.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Emails › Queued

## Fields

[Section titled “Fields”](#fields)

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

When the message was received and queued

##### nextRetry

[Section titled “nextRetry”](#nextretry)

Type: `UTCDateTime?`

When the next delivery attempt is scheduled

##### nextNotify

[Section titled “nextNotify”](#nextnotify)

Type: `UTCDateTime?` · server-set

When the next DSN notification is scheduled

##### blobId

[Section titled “blobId”](#blobid)

Type: `BlobId` · server-set

Reference to the stored message content

##### returnPath

[Section titled “returnPath”](#returnpath)

Type: `String` · server-set

Envelope sender address (MAIL FROM)

##### recipients

[Section titled “recipients”](#recipients)

Type: `Map<EmailAddress,`[`QueuedRecipient`](#queuedrecipient)`>`

List of envelope recipients and their delivery status

##### receivedFromIp

[Section titled “receivedFromIp”](#receivedfromip)

Type: `IpAddr` · server-set

IP address of the client that submitted the message

##### receivedViaPort

[Section titled “receivedViaPort”](#receivedviaport)

Type: `UnsignedInt` · server-set · default: `25` · min: 1 · max: 65535

Local port on which the message was received

##### flags

[Section titled “flags”](#flags)

Type: [`MessageFlag`](#messageflag)`[]` · server-set

Classification flags for the message

##### envId

[Section titled “envId”](#envid)

Type: `String?`

SMTP ENVID parameter for delivery status notifications

##### priority

[Section titled “priority”](#priority)

Type: `Integer` · default: `0` · min: -100 · max: 100

Message priority (lower values = higher priority)

##### size

[Section titled “size”](#size)

Type: `UnsignedInt` · server-set · default: `0`

Size of the message in bytes

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The QueuedMessage object is available via the `urn:stalwart:jmap` capability.

### x:QueuedMessage/get

[Section titled “x:QueuedMessage/get”](#xqueuedmessageget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysQueuedMessageGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:QueuedMessage/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:QueuedMessage/set

[Section titled “x:QueuedMessage/set”](#xqueuedmessageset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysQueuedMessageCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:QueuedMessage/set",          {            "create": {              "new1": {                "recipients": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysQueuedMessageUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:QueuedMessage/set",          {            "update": {              "id1": {                "envId": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysQueuedMessageDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:QueuedMessage/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:QueuedMessage/query

[Section titled “x:QueuedMessage/query”](#xqueuedmessagequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysQueuedMessageQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:QueuedMessage/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:QueuedMessage/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`to`text`due`date`queueName`text`returnPath`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get QueuedMessage id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create QueuedMessage \  --field 'recipients={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query QueuedMessagestalwart-cli query QueuedMessage --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update QueuedMessage id1 --field envId='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete QueuedMessage --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### QueuedRecipient

[Section titled “QueuedRecipient”](#queuedrecipient)

Delivery status and scheduling for a message recipient.

##### retryCount

[Section titled “retryCount”](#retrycount)

Type: `UnsignedInt` · default: `0`

Number of delivery attempts made

##### retryDue

[Section titled “retryDue”](#retrydue)

Type: `UTCDateTime` · required

When the next delivery attempt is scheduled

##### notifyCount

[Section titled “notifyCount”](#notifycount)

Type: `UnsignedInt` · default: `0`

Number of DSN notifications sent

##### notifyDue

[Section titled “notifyDue”](#notifydue)

Type: `UTCDateTime` · required

When the next DSN notification is scheduled

##### expires

[Section titled “expires”](#expires)

Type: [`QueueExpiry`](#queueexpiry) · required

Message expiry information for this recipient

##### queueName

[Section titled “queueName”](#queuename)

Type: `String` · server-set · max length: 8

Queue name for this recipient

##### status

[Section titled “status”](#status)

Type: [`RecipientStatus`](#recipientstatus) · required

Current delivery status

##### flags

[Section titled “flags”](#flags-1)

Type: [`RecipientFlag`](#recipientflag)`[]` · server-set

Status flags for this recipient

##### orcpt

[Section titled “orcpt”](#orcpt)

Type: `String?`

Original recipient address (SMTP ORCPT parameter)

#### QueueExpiry

[Section titled “QueueExpiry”](#queueexpiry)

Message expiry strategy for a queued recipient.

`Ttl`: TTL-based expiry. Carries the fields of [`QueueExpiryTtl`](#queueexpiryttl).
`Attempts`: Attempt-based expiry. Carries the fields of [`QueueExpiryAttempts`](#queueexpiryattempts).

##### QueueExpiryTtl

[Section titled “QueueExpiryTtl”](#queueexpiryttl)

TTL-based message expiry settings.

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime` · required

Absolute time when the message expires

##### QueueExpiryAttempts

[Section titled “QueueExpiryAttempts”](#queueexpiryattempts)

Attempt-based message expiry settings.

##### expiresAttempts

[Section titled “expiresAttempts”](#expiresattempts)

Type: `UnsignedInt` · default: `0`

Maximum number of delivery attempts before the message expires

#### RecipientStatus

[Section titled “RecipientStatus”](#recipientstatus)

Delivery status for a queued message recipient.

`Scheduled`: Delivery is pending. No additional fields.
`Completed`: Message was successfully delivered. Carries the fields of [`ServerResponse`](#serverresponse).
`TemporaryFailure`: Delivery failed temporarily, will retry. Carries the fields of [`DeliveryError`](#deliveryerror).
`PermanentFailure`: Delivery failed permanently, will not retry. Carries the fields of [`DeliveryError`](#deliveryerror).

##### ServerResponse

[Section titled “ServerResponse”](#serverresponse)

SMTP server response details.

##### responseHostname

[Section titled “responseHostname”](#responsehostname)

Type: `String?`

Hostname of the remote server that responded

##### responseCode

[Section titled “responseCode”](#responsecode)

Type: `UnsignedInt?` · min: 100 · max: 599

SMTP response code from the remote server

##### responseEnhanced

[Section titled “responseEnhanced”](#responseenhanced)

Type: `String?`

SMTP enhanced status code (e.g., “4.7.1”)

##### responseMessage

[Section titled “responseMessage”](#responsemessage)

Type: `String?`

SMTP response message from the remote server

##### DeliveryError

[Section titled “DeliveryError”](#deliveryerror)

Details of a delivery error including the triggering command.

##### errorType

[Section titled “errorType”](#errortype)

Type: [`DeliveryErrorType`](#deliveryerrortype) · required

Type of delivery error encountered

##### errorMessage

[Section titled “errorMessage”](#errormessage)

Type: `String?`

Detailed error message

##### errorCommand

[Section titled “errorCommand”](#errorcommand)

Type: `String?`

SMTP command that triggered an unexpected response

##### responseHostname

[Section titled “responseHostname”](#responsehostname-1)

Type: `String?`

Hostname of the remote server that responded

##### responseCode

[Section titled “responseCode”](#responsecode-1)

Type: `UnsignedInt?` · min: 100 · max: 599

SMTP response code from the remote server

##### responseEnhanced

[Section titled “responseEnhanced”](#responseenhanced-1)

Type: `String?`

SMTP enhanced status code (e.g., “4.7.1”)

##### responseMessage

[Section titled “responseMessage”](#responsemessage-1)

Type: `String?`

SMTP response message from the remote server

## Enums

[Section titled “Enums”](#enums)

### DeliveryErrorType

[Section titled “DeliveryErrorType”](#deliveryerrortype)
ValueLabel`dnsError`DNS resolution failed`unexpectedResponse`Remote server returned an unexpected SMTP response`connectionError`Failed to establish connection to remote server`tlsError`TLS handshake or encryption failed`daneError`DANE validation failed`mtaStsError`MTA-STS policy validation failed`rateLimited`Delivery delayed due to rate limiting`concurrencyLimited`Delivery delayed due to connection limits`io`Network I/O error occurred

### RecipientFlag

[Section titled “RecipientFlag”](#recipientflag)
ValueLabel`dsnSent`A delivery status notification has been sent for this recipient`spamPayload`Message content was classified as spam

### MessageFlag

[Section titled “MessageFlag”](#messageflag)
ValueLabel`authenticated`Message was received from an authenticated session`unauthenticated`Message was received without authentication`unauthenticatedDmarc`Message was received without authentication but passed DMARC`dsn`Message is a Delivery Status Notification`report`Message is an automated report (DMARC, TLS-RPT, etc.)`autogenerated`Message was automatically generated by the server