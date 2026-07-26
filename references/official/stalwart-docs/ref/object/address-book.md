---
title: "AddressBook"
source: "https://stalw.art/docs/ref/object/address-book"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/address-book"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AddressBook"
---
# AddressBook

      

Configures address book and contact storage settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Calendar & Contacts › Address Book

## Fields

[Section titled “Fields”](#fields)

##### defaultDisplayName

[Section titled “defaultDisplayName”](#defaultdisplayname)

Type: `String?` · default: `"Stalwart Address Book"`

Specifies the default display name for a contact when it is created

##### defaultHrefName

[Section titled “defaultHrefName”](#defaulthrefname)

Type: `String?` · default: `"default"`

Specifies the default href name for a contact when it is created

##### maxVCardSize

[Section titled “maxVCardSize”](#maxvcardsize)

Type: `Size` · default: `524288`

Specifies the maximum size of a vCard file that can be uploaded to the server

##### maxAddressBooks

[Section titled “maxAddressBooks”](#maxaddressbooks)

Type: `UnsignedInt?` · default: `250` · min: 1

The default maximum number of address books a user can create

##### maxContacts

[Section titled “maxContacts”](#maxcontacts)

Type: `UnsignedInt?` · min: 1

The default maximum number of contact cards a user can create

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The AddressBook singleton is available via the `urn:stalwart:jmap` capability.

### x:AddressBook/get

[Section titled “x:AddressBook/get”](#xaddressbookget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysAddressBookGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AddressBook/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AddressBook/set

[Section titled “x:AddressBook/set”](#xaddressbookset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysAddressBookUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AddressBook/set",          {            "update": {              "singleton": {                "defaultDisplayName": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get AddressBook
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update AddressBook --field defaultDisplayName='updated value'
```