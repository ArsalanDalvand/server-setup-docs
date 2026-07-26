---
title: "SieveUserInterpreter"
source: "https://stalw.art/docs/ref/object/sieve-user-interpreter"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/sieve-user-interpreter"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SieveUserInterpreter"
---
# SieveUserInterpreter

      

Configures the user-level Sieve script interpreter settings and limits.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Sieve › User Interpreter

## Fields

[Section titled “Fields”](#fields)

##### defaultExpiryDuplicate

[Section titled “defaultExpiryDuplicate”](#defaultexpiryduplicate)

Type: `Duration` · default: `"7d"`

Default expiration time for IDs stored by the duplicate extension from user scripts

##### defaultExpiryVacation

[Section titled “defaultExpiryVacation”](#defaultexpiryvacation)

Type: `Duration` · default: `"30d"`

Default expiration time for IDs stored by the vacation extension

##### disableCapabilities

[Section titled “disableCapabilities”](#disablecapabilities)

Type: [`SieveCapability`](#sievecapability)`[]`

List of capabilities to disable in the user interpreter

##### allowedNotifyUris

[Section titled “allowedNotifyUris”](#allowednotifyuris)

Type: `String[]` · default: `["mailto"]`

List of allowed URIs for the notify extension

##### protectedHeaders

[Section titled “protectedHeaders”](#protectedheaders)

Type: `String[]` · default: `["Original-Subject","Original-From","Received","Auto-Submitted"]`

List of headers that cannot be deleted or added using the editheader extension

##### defaultSubject

[Section titled “defaultSubject”](#defaultsubject)

Type: `String` · default: `"Automated reply"`

Default subject of vacation responses

##### defaultSubjectPrefix

[Section titled “defaultSubjectPrefix”](#defaultsubjectprefix)

Type: `String` · default: `"Auto: "`

Default subject prefix of vacation responses

##### maxCpuCycles

[Section titled “maxCpuCycles”](#maxcpucycles)

Type: `UnsignedInt` · default: `5000` · min: 1

Maximum number CPU cycles a script can use

##### maxHeaderSize

[Section titled “maxHeaderSize”](#maxheadersize)

Type: `UnsignedInt` · default: `1024` · min: 1

Maximum size of a header

##### maxIncludes

[Section titled “maxIncludes”](#maxincludes)

Type: `UnsignedInt` · default: `3` · min: 1

Maximum number of includes

##### maxLocalVars

[Section titled “maxLocalVars”](#maxlocalvars)

Type: `UnsignedInt` · default: `128` · min: 1

Maximum number of local variables

##### maxMatchVars

[Section titled “maxMatchVars”](#maxmatchvars)

Type: `UnsignedInt` · default: `30` · min: 1

Maximum number of match variables

##### maxScriptNameLength

[Section titled “maxScriptNameLength”](#maxscriptnamelength)

Type: `UnsignedInt` · default: `512` · min: 1

Maximum length of a script name

##### maxNestedBlocks

[Section titled “maxNestedBlocks”](#maxnestedblocks)

Type: `UnsignedInt` · default: `15` · min: 1

Maximum number of nested blocks

##### maxNestedForEvery

[Section titled “maxNestedForEvery”](#maxnestedforevery)

Type: `UnsignedInt` · default: `3` · min: 1

Maximum number of nested foreach blocks

##### maxNestedIncludes

[Section titled “maxNestedIncludes”](#maxnestedincludes)

Type: `UnsignedInt` · default: `3` · min: 1

Maximum number of nested includes

##### maxNestedTests

[Section titled “maxNestedTests”](#maxnestedtests)

Type: `UnsignedInt` · default: `15` · min: 1

Maximum number of nested tests

##### maxOutMessages

[Section titled “maxOutMessages”](#maxoutmessages)

Type: `UnsignedInt` · default: `3`

Maximum number of outgoing messages

##### maxReceivedHeaders

[Section titled “maxReceivedHeaders”](#maxreceivedheaders)

Type: `UnsignedInt` · default: `50` · min: 1

Maximum number of received headers

##### maxRedirects

[Section titled “maxRedirects”](#maxredirects)

Type: `UnsignedInt` · default: `1`

Maximum number of redirects

##### maxScriptSize

[Section titled “maxScriptSize”](#maxscriptsize)

Type: `Size` · default: `"100kb"` · min: 1

Maximum size of a script

##### maxStringLength

[Section titled “maxStringLength”](#maxstringlength)

Type: `UnsignedInt` · default: `4096` · min: 1

Maximum length of a string

##### maxVarNameLength

[Section titled “maxVarNameLength”](#maxvarnamelength)

Type: `UnsignedInt` · default: `32` · min: 1

Maximum length of a variable name

##### maxVarSize

[Section titled “maxVarSize”](#maxvarsize)

Type: `UnsignedInt` · default: `4096` · min: 1

Maximum size of a variable

##### maxScripts

[Section titled “maxScripts”](#maxscripts)

Type: `UnsignedInt?` · default: `100` · min: 1

The default maximum number of sieve scripts a user can create

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SieveUserInterpreter singleton is available via the `urn:stalwart:jmap` capability.

### x:SieveUserInterpreter/get

[Section titled “x:SieveUserInterpreter/get”](#xsieveuserinterpreterget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSieveUserInterpreterGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserInterpreter/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SieveUserInterpreter/set

[Section titled “x:SieveUserInterpreter/set”](#xsieveuserinterpreterset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSieveUserInterpreterUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserInterpreter/set",          {            "update": {              "singleton": {                "defaultSubject": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SieveUserInterpreter
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SieveUserInterpreter --field defaultSubject='updated value'
```

## Enums

[Section titled “Enums”](#enums)

### SieveCapability

[Section titled “SieveCapability”](#sievecapability)
ValueLabel`envelope`Envelope Test`envelope-dsn`Envelope DSN (Delivery Status Notification)`envelope-deliverby`Envelope Deliver-By`fileinto`File Into Mailbox`encoded-character`Encoded Character Support`comparator-elbonia`Elbonia Comparator`comparator-i;octet`Octet Comparator`comparator-i;ascii-casemap`ASCII Case Map Comparator`comparator-i;ascii-numeric`ASCII Numeric Comparator`body`Body Test`convert`Message Conversion`copy`Copying Without Side Effects`relational`Relational Tests`date`Date Tests`index`Header Index`duplicate`Duplicate Delivery Detection`variables`Variables`editheader`Edit Header`foreverypart`For Every MIME Part`mime`MIME Part Tests`replace`MIME Part Replacement`enclose`MIME Part Enclosure`extracttext`Extract Text from MIME Parts`enotify`Email Notifications`redirect-dsn`Redirect with DSN`redirect-deliverby`Redirect with Deliver-By`environment`Environment Information`reject`Reject Message`ereject`Extended Reject (SMTP-level)`extlists`Externally Stored Lists`subaddress`Subaddress (Detail) Matching`vacation`Vacation Auto-Reply`vacation-seconds`Vacation with Seconds Parameter`fcc`File Carbon Copy`mailbox`Mailbox Existence Check`mailboxid`Mailbox by OBJECTID`mboxmetadata`Mailbox Metadata Access`servermetadata`Server Metadata Access`special-use`Special-Use Mailboxes`imap4flags`IMAP Flags`ihave`Conditional Extension Test`imapsieve`IMAP Events in Sieve`include`Script Inclusion`regex`Regular Expression Matching`spamtest`Spam Test`spamtestplus`Extended Spam Test`virustest`Virus Test`vnd.stalwart.while`While Loop (Stalwart)`vnd.stalwart.expressions`Expression Evaluation (Stalwart)