---
title: "Overview"
source: "https://stalw.art/docs/mta/authentication/dkim"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/authentication/dkim"
category: "mta"
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

      

DomainKeys Identified Mail (DKIM) is an email-authentication method that verifies the authenticity of an email message using public-key cryptography. A digital signature is added to the message header; the signature can be verified against a public key published in DNS under the sending domain. DKIM prevents email spoofing and allows recipients to confirm that a message genuinely originated from the claimed domain.

With DKIM enabled, outgoing messages carry one or more signatures that help build sender reputation, reduce the chance of legitimate messages being marked as spam, and improve overall email security.

DKIM signatures are defined as [[ref/object/dkim-signature|DkimSignature]] objects (found in the WebUI under  Management ›  Domains › DKIM Signatures). Each signature is a multi-variant object: the `Dkim1Ed25519Sha256` variant is used for Ed25519 signatures and the `Dkim1RsaSha256` variant for RSA signatures. See the [[mta/authentication/dkim/sign|signing]] and [[mta/authentication/dkim/verify|verifying]] pages for details.