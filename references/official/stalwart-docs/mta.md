---
title: "Overview"
source: "https://stalw.art/docs/mta"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta"
category: "stalwart"
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

      

A Message Transfer Agent (MTA) is a core component of email infrastructure responsible for transferring email messages from one server to another using the Simple Mail Transfer Protocol (SMTP). MTAs handle message routing, queuing, delivery attempts, and message handoff between different servers.

SMTP is the protocol used for the transmission of electronic mail across IP networks. It operates by establishing a connection between servers to transfer messages, handling aspects such as message envelopes, routing paths, and error handling. Modern MTAs also incorporate security, authentication, and policy enforcement mechanisms.

Stalwart’s MTA is a standards-compliant SMTP server designed to serve both inbound and outbound email flows. It includes native support for email authentication standards such as DMARC, DKIM, SPF, and ARC. To secure message transport, Stalwart implements DANE, MTA-STS, and SMTP TLS reporting, allowing verifiable transmission over TLS.

Beyond authentication and transport security, Stalwart provides mechanisms for managing incoming traffic: inbound connection throttling, configurable filtering rules, support for Sieve scripting, MTA-level hooks, and milter integration.

For outbound delivery, Stalwart uses a distributed virtual queue architecture with support for delayed and priority delivery, delivery quotas, routing rules, and sender- or domain-specific throttling. Envelope rewriting and message modification are also supported, so operators can adapt message content and routing to deployment-specific requirements.

This documentation provides a detailed guide to configuring and operating the Stalwart MTA.

## Configuration

[Section titled “Configuration”](#configuration)

The SMTP service is enabled by creating one or more listeners on the [[ref/object/network-listener|NetworkListener]] object (found in the WebUI under  Settings ›  Network › Listeners). A listener is an entry point for SMTP or LMTP connections on a specific address and port. Multiple listeners may be created, each serving a different role or catering to different types of connections. There is no built-in limit on the number of listeners.

Conventional SMTP configurations associate certain ports with specific types of connections: port 25 for cleartext SMTP between servers, port 587 for SMTP submission, and port 465 for SMTP submission with implicit TLS. Stalwart does not enforce these associations; each listener’s role is defined by its configuration. See the main [[server/listener|listeners]] section for the complete listener configuration.

A listener is created by setting its [[ref/object/network-listener#bind|`bind`]] addresses and selecting the appropriate [[ref/object/network-listener#protocol|`protocol`]] variant. The relevant protocol variants for MTA use are `smtp` and `lmtp`. Implicit TLS is enabled by setting the [[ref/object/network-listener#tlsimplicit|`tlsImplicit`]] field to `true` on the listener. The SMTP banner shown to connecting clients is not configured on the listener itself; it is defined on the [[ref/object/mta-stage-connect|MtaStageConnect]] singleton via the [[ref/object/mta-stage-connect#smtpgreeting|`smtpGreeting`]] expression, which can branch on the listener identifier to serve different banners per listener.

### SMTP port

[Section titled “SMTP port”](#smtp-port)

Cleartext SMTP connections between servers are typically received on port 25. A listener configured with [[ref/object/network-listener#bind|`bind`]] set to `[::]:25` and the SMTP [[ref/object/network-listener#protocol|`protocol`]] variant enables this role.

### Submissions port (TLS)

[Section titled “Submissions port (TLS)”](#submissions-port-tls)

SMTP submissions with implicit TLS are typically received on port 465. A listener configured with [[ref/object/network-listener#bind|`bind`]] set to `[::]:465`, the SMTP protocol variant, and [[ref/object/network-listener#tlsimplicit|`tlsImplicit`]] set to `true` serves this role.

### Submissions port

[Section titled “Submissions port”](#submissions-port)

SMTP submissions without implicit TLS, upgradable via `STARTTLS`, are typically received on port 587. A listener configured with [[ref/object/network-listener#bind|`bind`]] set to `[::]:587` and the SMTP [[ref/object/network-listener#protocol|`protocol`]] variant enables this role.

### LMTP port

[Section titled “LMTP port”](#lmtp-port)

LMTP is a protocol used to deliver email to a mail server. It is similar to SMTP but designed for local handoff rather than transport between organisations. LMTP is typically received on port 24. A listener configured with [[ref/object/network-listener#bind|`bind`]] set to `[::]:24` and the LMTP [[ref/object/network-listener#protocol|`protocol`]] variant enables this role. LMTP is only required when Stalwart is not acting as the final mail store.