---
title: "Overview"
source: "https://stalw.art/docs/auth/principals"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/principals"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

Principals are the entities that represent individuals, groups, resources, or other organizational elements within Stalwart. They play a key role in managing access, permissions, and interactions with the mail system. Principals in Stalwart follow the [JMAP Sharing](https://datatracker.ietf.org/doc/draft-ietf-jmap-sharing/09/) specification (as outlined in the RFC), which provides a standardized way to define and share resources among users. Each principal type represents a different category of entity and serves a distinct function within the system.

## Types

[Section titled “Types”](#types)

Stalwart supports a range of principal types, some defined in the JMAP Sharing RFC and some internal to Stalwart’s architecture:

[[auth/principals/individual|Individual]]: a single person who can send, receive, and manage email within the system.
[[auth/principals/group|Group]]: a collection of individuals or other principals, used to manage permissions and access rights for multiple users at once.
[[auth/authorization/tenants|Tenant]]: an organisational or logical division within the system, used for multi-tenant deployments.
[[auth/authorization/roles|Role]]: a named set of permissions that can be assigned to individuals or groups.
Resource: a physical or digital resource, such as a projector, a meeting room, or shared file storage.
Location: a physical location, such as an office or a conference room.
Other: any principal that does not fall into the predefined categories.

Several entities that are closely related to authentication are no longer modelled as principals in v0.16 and have their own top-level configuration instead: email [[domains|domains]], [[email/management/mailing-lists|mailing lists]], [[auth/authentication/api-key|API keys]], and [[auth/oauth/oauth-client|OAuth clients]].

## JMAP Sharing

[Section titled “JMAP Sharing”](#jmap-sharing)

The [JMAP Sharing specification](https://datatracker.ietf.org/doc/draft-ietf-jmap-sharing/09/) defines a fixed set of principal types:

Individual: a single person.
Group: a group of other principals.
Resource: a physical or digital resource (for example, a projector).
Location: a physical location (for example, a conference room).
Other: a principal that does not fall into any of the above categories.

When Stalwart principals are accessed over the JMAP Sharing protocol, the Tenant and Role types (which are internal to Stalwart) are mapped to the Other principal type, preserving compatibility with the specification while keeping the extended functionality available to Stalwart’s own APIs.