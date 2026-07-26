---
title: "ManageSieve"
source: "https://stalw.art/docs/sieve/managesieve"
source_format: official-html-to-markdown
language: "en"
doc_slug: "sieve/managesieve"
category: "sieve"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ManageSieve"
---
# ManageSieve

      

[ManageSieve](https://datatracker.ietf.org/doc/html/rfc5804) is a protocol for remotely managing Sieve scripts on a mail server. Sieve scripts filter and sort incoming email based on rules defined by the user or the administrator. ManageSieve provides a standard way to upload, download, edit, and delete those scripts from a remote client, typically a supported email client or a web interface.

Stalwart includes a ManageSieve server, so Sieve scripts can be managed over the network without direct access to the server’s file system.

## Listener

[Section titled “Listener”](#listener)

To enable ManageSieve access, create a [[server/listener|network listener]] with its protocol set to `manageSieve`. Listeners are configured through the [[ref/object/network-listener|NetworkListener]] object (found in the WebUI under  Settings ›  Network › Listeners). The relevant fields are [[ref/object/network-listener#bind|`bind`]], [[ref/object/network-listener#protocol|`protocol`]], and [[ref/object/network-listener#tlsimplicit|`tlsImplicit`]].

For example, a listener named `sieve` that binds to port 4190 on all interfaces with implicit TLS:

```
{  "name": "sieve",  "bind": ["[::]:4190"],  "protocol": "manageSieve",  "tlsImplicit": true}
```

Once the listener is active, users can manage their Sieve scripts through any compatible ManageSieve client or through the WebUI.