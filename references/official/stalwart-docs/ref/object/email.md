---
title: "Email"
source: "https://stalw.art/docs/ref/object/email"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/email"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Email"
---
# Email

      

Configures email message limits, encryption, compression, and default folder settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Email › Defaults Settings ›  Email › Encryption Settings ›  Email › Limits Settings ›  Email › Storage

## Fields

[Section titled “Fields”](#fields)

##### maxAttachmentSize

[Section titled “maxAttachmentSize”](#maxattachmentsize)

Type: `Size` · default: `50000000` · min: 1

Specifies the maximum size for an email attachment

##### maxMessageSize

[Section titled “maxMessageSize”](#maxmessagesize)

Type: `Size` · default: `75000000` · min: 1

Determines the maximum size for an email message

##### maxMailboxDepth

[Section titled “maxMailboxDepth”](#maxmailboxdepth)

Type: `UnsignedInt` · default: `10` · min: 1

Restricts the maximum depth of nested mailboxes a user can create

##### maxMailboxNameLength

[Section titled “maxMailboxNameLength”](#maxmailboxnamelength)

Type: `UnsignedInt` · default: `255` · min: 1

Establishes the maximum length of a mailbox name

##### encryptOnAppend

[Section titled “encryptOnAppend”](#encryptonappend)

Type: `Boolean` · default: `false`

Encrypt messages that are manually appended by the user using JMAP or IMAP

##### encryptAtRest

[Section titled “encryptAtRest”](#encryptatrest)

Type: `Boolean` · default: `true`

Allow users to configure encryption at rest for their data

##### compressionAlgorithm

[Section titled “compressionAlgorithm”](#compressionalgorithm)

Type: [`CompressionAlgo`](#compressionalgo) · default: `"lz4"`

Algorithm to use to compress e-mail data

##### defaultFolders

[Section titled “defaultFolders”](#defaultfolders)

Type: `Map<`[`SpecialUse`](#specialuse)`,`[`EmailFolder`](#emailfolder)`>`

Default special-use folders to create for new users

##### maxMessages

[Section titled “maxMessages”](#maxmessages)

Type: `UnsignedInt?` · min: 1

The default maximum number of emails a user can create

##### maxSubmissions

[Section titled “maxSubmissions”](#maxsubmissions)

Type: `UnsignedInt?` · default: `500` · min: 1

The default maximum number of email submissions a user can create

##### maxIdentities

[Section titled “maxIdentities”](#maxidentities)

Type: `UnsignedInt?` · default: `20` · min: 1

The default maximum number of identities a user can create

##### maxMailboxes

[Section titled “maxMailboxes”](#maxmailboxes)

Type: `UnsignedInt?` · default: `250` · min: 1

The default maximum number of mailboxes a user can create

##### maxMaskedAddresses

[Section titled “maxMaskedAddresses”](#maxmaskedaddresses)

Type: `UnsignedInt?` · [[server/enterprise|enterprise]] · default: `5` · min: 1

The default maximum number of masked email addresses a user can create

##### maxPublicKeys

[Section titled “maxPublicKeys”](#maxpublickeys)

Type: `UnsignedInt?` · default: `5` · min: 1

The default maximum number of encryption-at-rest public keys a user can create

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Email singleton is available via the `urn:stalwart:jmap` capability.

### x:Email/get

[Section titled “x:Email/get”](#xemailget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysEmailGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Email/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Email/set

[Section titled “x:Email/set”](#xemailset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysEmailUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Email/set",          {            "update": {              "singleton": {                "maxAttachmentSize": 50000000              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Email
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Email --field maxAttachmentSize=50000000
```

## Nested types

[Section titled “Nested types”](#nested-types)

### EmailFolder

[Section titled “EmailFolder”](#emailfolder)

Defines a default email folder configuration.

##### name

[Section titled “name”](#name)

Type: `String` · required

Default name for the folder

##### create

[Section titled “create”](#create)

Type: `Boolean` · default: `true`

Whether to create the folder automatically

##### subscribe

[Section titled “subscribe”](#subscribe)

Type: `Boolean` · default: `true`

Whether to subscribe to the folder automatically

##### aliases

[Section titled “aliases”](#aliases)

Type: `String[]`

List of folder aliases

## Enums

[Section titled “Enums”](#enums)

### CompressionAlgo

[Section titled “CompressionAlgo”](#compressionalgo)
ValueLabel`lz4`LZ4`none`None

### SpecialUse

[Section titled “SpecialUse”](#specialuse)
ValueLabel`inbox`Inbox`trash`Trash`junk`Junk`drafts`Drafts`archive`Archive`sent`Sent`shared`Shared Folders`important`Important`memos`Memos`scheduled`Scheduled`snoozed`Snoozed