---
title: "Contacts"
source: "https://stalw.art/docs/collaboration/contact"
source_format: official-html-to-markdown
language: "en"
doc_slug: "collaboration/contact"
category: "collaboration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Contacts"
---
# Contacts

      

Stalwart includes built-in support for contact management and synchronization through JMAP for Contacts and CardDAV, covering storage, access, and maintenance of address book data across devices and applications.

## JMAP for Contacts

[Section titled “JMAP for Contacts”](#jmap-for-contacts)

JMAP for Contacts is a modern alternative to CardDAV, built on the same JSON-based foundation as other JMAP specifications. It allows clients to manage address books and contact data through a schema-defined JSON representation known as JSContact.

Where CardDAV relies on XML-based WebDAV extensions and the vCard format, JMAP for Contacts replaces both layers with JSON objects and operations.

## CardDAV

[Section titled “CardDAV”](#carddav)

CardDAV is a standardized extension of WebDAV that allows clients to interact with contact data stored on a server. It provides an interoperable way to manage personal and shared address books, and is supported by contact applications on desktop and mobile platforms including Apple Contacts and Thunderbird.

Through CardDAV, contacts can be created and edited, organised into address books, and synchronised across devices. Stalwart’s CardDAV implementation integrates with the rest of the collaboration stack so that the same underlying data is available to JMAP clients.

### Accessing contacts

[Section titled “Accessing contacts”](#accessing-contacts)

CardDAV clients can access address book data on Stalwart using standardized URLs. The recommended method for client configuration is through the autodiscovery endpoint located at `/.well-known/carddav`. When a client queries this path, the server redirects the request to the appropriate CardDAV resource associated with the authenticated account.

Alternatively, CardDAV address books can be accessed directly via the path `/dav/card/<account_name>`, where `<account_name>` is the username of the account. For example, the address book for the account `alice` is available at `/dav/card/alice`.

Both methods provide access to the same contact data and are fully compatible with the CardDAV protocol.

## Limits

[Section titled “Limits”](#limits)

Address-book-wide limits are configured on the [[ref/object/address-book|AddressBook]] singleton (found in the WebUI under  Settings ›  Calendar & Contacts › Address Book).

### vCard object size

[Section titled “vCard object size”](#vcard-object-size)

[[ref/object/address-book#maxvcardsize|`maxVCardSize`]] defines the maximum accepted size of a single vCard object submitted to the server. The default is `524288` (512 KB). Raise or lower the limit to suit expected client behaviour.

### JMAP parsing limit

[Section titled “JMAP parsing limit”](#jmap-parsing-limit)

The JMAP `ContactCard/parse` method is bounded by [[ref/object/jmap#parselimitcontact|`parseLimitContact`]] on the [[ref/object/jmap|Jmap]] singleton (found in the WebUI under  Settings ›  Network › JMAP › Limits,  Settings ›  Network › JMAP › Push,  Settings ›  Network › JMAP › WebSocket), which sets the maximum number of vCard items that can be parsed in a single request. The default is `10`.

## Default address book

[Section titled “Default address book”](#default-address-book)

To improve client compatibility and allow contact operations to proceed without manual setup, Stalwart automatically creates a default address book when an account is accessed and no address books currently exist for it. The URL path of the auto-created address book is controlled by [[ref/object/address-book#defaulthrefname|`defaultHrefName`]] on the AddressBook singleton. The default value is `"default"`, so for an account named `john` the default address book is created at `/dav/card/john/default`.

### Disabling automatic creation

[Section titled “Disabling automatic creation”](#disabling-automatic-creation)

Automatic creation can be suppressed by clearing [[ref/object/address-book#defaulthrefname|`defaultHrefName`]] (leaving it unset). In that case, Stalwart does not create a default address book and clients that connect to an account with no address books receive an appropriate error response.

### Default display name

[Section titled “Default display name”](#default-display-name)

The display name assigned to the auto-created address book is controlled by [[ref/object/address-book#defaultdisplayname|`defaultDisplayName`]]. The default is `"Stalwart Address Book"`; the value can be adjusted to match organisational branding or language preferences.

## Per-account limits

[Section titled “Per-account limits”](#per-account-limits)

The AddressBook singleton also sets default caps on per-account address-book resources: [[ref/object/address-book#maxaddressbooks|`maxAddressBooks`]] caps the number of address books an account can create (default `250`), and [[ref/object/address-book#maxcontacts|`maxContacts`]] caps the number of contact cards. These apply unless overridden at the account or tenant level.

## Spam filter integration

[Section titled “Spam filter integration”](#spam-filter-integration)

Stalwart’s contact management system is integrated with the [[spamfilter|built-in spam filter]] to improve filtering accuracy and reduce false positives. When enabled, the spam filter references an account’s address book to identify trusted senders so that their messages are not marked as spam.

In addition to [[spamfilter/settings/general#address-book-integration|bypassing spam classification]] for known contacts, the address book can be used to [[spamfilter/classifier/autolearn#address-books|improve the spam classifier]]. When a message from a known contact is incorrectly flagged as spam, the classifier can learn from the mistake and recognise similar messages in future.

For details on configuring these features, see the [[spamfilter|spam filter documentation]].