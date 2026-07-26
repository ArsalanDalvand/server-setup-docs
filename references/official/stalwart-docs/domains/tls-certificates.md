---
title: "TLS certificates"
source: "https://stalw.art/docs/domains/tls-certificates"
source_format: official-html-to-markdown
language: "en"
doc_slug: "domains/tls-certificates"
category: "domains"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TLS certificates"
---
# TLS certificates

      

Every Domain that terminates TLS (for SMTP on submission and port 25, IMAP, JMAP, HTTP services, and so on) requires a certificate whose Subject Alternative Names cover the host names used for those services. The Domain object decides whether that certificate is supplied by the operator or obtained automatically from an ACME certificate authority.

## Selecting the management mode

[Section titled “Selecting the management mode”](#selecting-the-management-mode)

The mode is set through the [[ref/object/domain#certificatemanagement|`certificateManagement`]] field on the [[ref/object/domain|Domain]] object. Two variants are supported:

Manual: the server expects one or more [[ref/object/certificate|Certificate]] objects (found in the WebUI under  Settings ›  TLS › Certificates) to be provisioned by the operator. The server does not request, renew, or delete certificates in this mode; the operator is responsible for the certificate lifecycle.
Automatic: the server obtains and renews certificates through an [[ref/object/acme-provider|AcmeProvider]] (found in the WebUI under  Settings ›  TLS › ACME Providers). The variant carries the fields that describe which provider is used and which names should be included in the issued certificate.

## Manual certificates

[Section titled “Manual certificates”](#manual-certificates)

A Certificate object stores a PEM-encoded public certificate in [[ref/object/certificate#certificate|`certificate`]] and the matching private key in [[ref/object/certificate#privatekey|`privateKey`]]. Both fields accept inline values, an environment variable reference, or a file path, which allows secrets to be injected from the deployment environment rather than stored inline in the database.

Once a Certificate is saved, the server parses it and populates read-only metadata: [[ref/object/certificate#subjectalternativenames|`subjectAlternativeNames`]], [[ref/object/certificate#notvalidbefore|`notValidBefore`]], [[ref/object/certificate#notvalidafter|`notValidAfter`]], and [[ref/object/certificate#issuer|`issuer`]]. Manual mode does not bind a Domain to a specific Certificate id; instead, the TLS listener matches an incoming connection’s Server Name Indication against the Subject Alternative Names of every installed Certificate and presents the newest certificate that matches. Renewal therefore consists of adding a replacement Certificate with the same SAN set before the current one expires, or updating the [[ref/object/certificate#certificate|`certificate`]] and [[ref/object/certificate#privatekey|`privateKey`]] fields on the existing record.

## Automatic certificates

[Section titled “Automatic certificates”](#automatic-certificates)

In automatic mode, [[ref/object/domain#certificatemanagement|`certificateManagement`]] carries two fields:

[[ref/object/domain#acmeproviderid|`acmeProviderId`]] references the AcmeProvider responsible for issuing certificates for the domain. The provider object holds the ACME directory URL, the registered account key, the contact address, the challenge type, and the renewal threshold.
[[ref/object/domain#subjectalternativenames|`subjectAlternativeNames`]] lists additional host names to include alongside the domain name itself. Leaving the list empty requests a wildcard certificate where the provider supports it, or uses the default SAN set derived from the Domain.

The AcmeProvider carries the lifecycle settings that apply to every domain bound to it. [[ref/object/acme-provider#challengetype|`challengeType`]] selects between `TlsAlpn01`, `Http01`, `Dns01`, and `DnsPersist01`; the DNS-based challenges require the Domain to have automatic DNS management enabled so the server can write the `_acme-challenge` records. [[ref/object/acme-provider#renewbefore|`renewBefore`]] expresses how early renewal is attempted, as a fraction of the remaining validity window (the default `R23` triggers renewal once two thirds of the validity period has elapsed). [[ref/object/acme-provider#maxretries|`maxRetries`]] caps the retry count for failed challenges, and [[ref/object/acme-provider#contact|`contact`]] supplies the email address the CA uses for expiry and revocation notifications.

## Renewal

[Section titled “Renewal”](#renewal)

Renewal in automatic mode is performed by the ACME renewal task, which is a variant of the [[ref/object/task|Task]] object (the `AcmeRenewal` variant). The first `AcmeRenewal` task is scheduled when a Domain is first configured with automatic certificate management. Each subsequent renewal schedules the next one: when an `AcmeRenewal` task completes, it creates a follow-up task timed from the AcmeProvider’s [[ref/object/acme-provider#renewbefore|`renewBefore`]] relative to the new certificate’s expiry. The task can also be triggered on demand when an immediate renewal is required, for example after adding a Subject Alternative Name. A successful renewal installs the new certificate into the store and replaces the one presented on subsequent TLS handshakes.

## Report generation

[Section titled “Report generation”](#report-generation)

CAA incident reports generated by the server are delivered to the address in [[ref/object/domain#reportaddressuri|`reportAddressUri`]] on the Domain, alongside DMARC and TLS-RPT reports. The default is `mailto:postmaster`; setting the field to null suppresses report generation for the domain.