---
title: "Overview"
source: "https://stalw.art/docs/http/jmap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/jmap"
category: "http"
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

      

JMAP (JSON Meta Application Protocol) is a modern, stateful protocol for synchronising mail, calendars, and contacts between a client and a server. It operates over HTTP and uses JSON as its data format, which makes it easy to implement across platforms. JMAP is designed to handle large volumes of data efficiently, offers a consistent interface across data types, and provides built-in push updates so that changes propagate to every connected device as soon as they occur.

## Enabling JMAP

[Section titled “Enabling JMAP”](#enabling-jmap)

Accepting JMAP connections requires an HTTP [[server/listener|listener]] defined on the [[ref/object/network-listener|NetworkListener]] object (found in the WebUI under  Settings ›  Network › Listeners) with the [[ref/object/network-listener#protocol|`protocol`]] set to `http`. In most installations this listener is created automatically during setup, so no further action is required.

## Accessing JMAP

[Section titled “Accessing JMAP”](#accessing-jmap)

Most JMAP clients discover the JMAP endpoint through the well-known resource at `/.well-known/jmap`, which returns the URL of the JMAP endpoint and other session details. The endpoint itself is served at `/jmap` and is the primary access point for client operations, including retrieving messages, managing mailboxes, and synchronising data.

## Disabling JMAP

[Section titled “Disabling JMAP”](#disabling-jmap)

By default, JMAP access is available as soon as an HTTP listener is configured. Some deployments may need to restrict or disable it entirely for security, compliance, or policy reasons.

The most direct way to disable JMAP access server-wide is to add an [[http/access-control|HTTP access control rule]] that blocks any path under `/jmap`. This denies access to every JMAP endpoint regardless of per-user settings.

For finer control, JMAP access can be restricted on a per-user, per-group, or per-tenant basis by removing the relevant [[auth/authorization/permissions|JMAP permissions]] from the account or entity, so that only authorised principals can use the service.