---
title: "URN Namespace"
source: "https://stalw.art/docs/development/urn"
source_format: official-html-to-markdown
language: "en"
doc_slug: "development/urn"
category: "development"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "URN Namespace"
---
# URN Namespace

      

## Overview

[Section titled “Overview”](#overview)

This document describes the ‘stalwart’ Uniform Resource Name (URN) namespace, which provides persistent, location-independent identifiers for resources related to the Stalwart and Collaboration Server.

The namespace identifier (NID) is: `stalwart`

## Purpose

[Section titled “Purpose”](#purpose)

The ‘stalwart’ URN namespace is used to provide unique identifiers for:

Custom JMAP extensions (RFC 8620)
WebDAV lock tokens (RFC 4918)
WebDAV synchronization tokens (RFC 6578)

## Syntax

[Section titled “Syntax”](#syntax)

URNs in the ‘stalwart’ namespace have the following structure:

```
urn:stalwart:{resource-type}:{specific-identifier}
```

Where:

`urn:` is the URI scheme
`stalwart` is the namespace identifier (NID)
`{resource-type}` identifies the category of resource
`{specific-identifier}` is the unique identifier for that resource

### Resource Types

[Section titled “Resource Types”](#resource-types)

Version 1 of this specification defines the following resource types:

jmap - Used for JMAP protocol extensions
davlock - Used for WebDAV lock tokens
davsync - Used for WebDAV synchronization tokens

### Specific Identifier Format

[Section titled “Specific Identifier Format”](#specific-identifier-format)

The format of the specific identifier depends on the resource type:

Resource TypeSpecific Identifier FormatExamplejmapText string identifying the extension`mailfilter`, `calendar-settings`davlock128-bit number encoded as hexadecimal`a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6`davsync64-bit number encoded as hexadecimal`a1b2c3d4e5f6a7b8`

## Examples

[Section titled “Examples”](#examples)

Here are examples of valid URNs in the ‘stalwart’ namespace:

`urn:stalwart:jmap:mailfilter`
`urn:stalwart:jmap:calendar-settings`
`urn:stalwart:davlock:a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6`
`urn:stalwart:davsync:a1b2c3d4e5f6a7b8`

## URN Equivalence

[Section titled “URN Equivalence”](#urn-equivalence)

URN equivalence within the ‘stalwart’ namespace follows the rules defined in RFC 8141, with the addition that string comparison of the Namespace Specific String (NSS) is case-insensitive.

This means:

`urn:stalwart:jmap:MailFilter` and `urn:stalwart:jmap:mailfilter` are equivalent
`urn:stalwart:davlock:A1B2C3D4E5F6A7B8C9D0E1F2A3B4C5D6` and `urn:stalwart:davlock:a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6` are equivalent

## Assignment

[Section titled “Assignment”](#assignment)

URNs in the ‘stalwart’ namespace are assigned by Stalwart Labs LLC. Assignment follows these guidelines:

JMAP Extensions (jmap):

Assigned based on the functionality provided by the extension
Names follow a convention similar to standard JMAP capabilities
Names should be concise, descriptive, and use lowercase letters and hyphens

WebDAV Lock Tokens (davlock):

Generated as random 128-bit numbers
Encoded as 32 character hexadecimal strings
Generated on demand when locks are created

WebDAV Synchronization Tokens (davsync):

Generated as 64-bit numbers
Encoded as 16 character hexadecimal strings
Generated when a resource changes to indicate its current version

## Security and Privacy Considerations

[Section titled “Security and Privacy Considerations”](#security-and-privacy-considerations)

WebDAV lock tokens should be treated as private information, although they do not grant exclusive access to resources.
When WebDAV sync tokens or lock tokens are used across networks, they should be transmitted using secure protocols to prevent unauthorized interception.
JMAP extension identifiers may reveal information about specific server capabilities, which could be useful to potential attackers in fingerprinting server installations.

## Usage Policy

[Section titled “Usage Policy”](#usage-policy)

Use of the ‘stalwart’ URN namespace within the scope of projects related to or derived from the Stalwart and Collaboration Server is permitted according to the terms of the server’s license.

## Version History

[Section titled “Version History”](#version-history)

Version 1 (2025-03-23): Initial definition with jmap, davlock, and davsync resource types.

## Contact Information

[Section titled “Contact Information”](#contact-information)

For questions or concerns regarding the ‘stalwart’ URN namespace, please contact:

Organization: Stalwart Labs LLC
Email: [[email protected]](/cdn-cgi/l/email-protection#5c34393030331c2f283d302b723d2e28)
Website: [https://github.com/stalwartlabs/stalwart](https://github.com/stalwartlabs/stalwart)