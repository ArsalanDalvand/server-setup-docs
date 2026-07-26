---
title: "Overview"
source: "https://stalw.art/docs/domains"
source_format: official-html-to-markdown
language: "en"
doc_slug: "domains"
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

      

A Domain represents a local mail domain that the server recognises as its own. Without a matching Domain object, the server treats messages addressed to that domain as external and refuses delivery for them. The Domain is therefore the anchor for every downstream decision that depends on domain ownership: local-recipient resolution, DKIM signing, TLS certificate issuance, and the publication of DNS policy records.

Domains are configured through the [[ref/object/domain|Domain]] object (found in the WebUI under  Management ›  Domains › Domains). Each record identifies a primary name and carries the settings that govern how the server handles mail and infrastructure for that name.

## Identity and aliases

[Section titled “Identity and aliases”](#identity-and-aliases)

The primary identifier is [[ref/object/domain#name|`name`]], the canonical domain name. Additional names that should be treated as equivalent to the primary name are listed in [[ref/object/domain#aliases|`aliases`]]; these are recognised as local for the purpose of accepting incoming mail and do not require a separate Domain object. Administrators group alias names under the primary record rather than duplicating configuration across multiple Domains, which keeps DKIM, DNS, and TLS settings in a single place.

A domain can be disabled without deletion by clearing [[ref/object/domain#isenabled|`isEnabled`]]; disabled domains are retained in the database but are no longer accepted as local. The [[ref/object/domain#description|`description`]] field carries an operator-facing note, and [[ref/object/domain#logo|`logo`]] holds a branding image used by the WebUI when a user signs in under the domain. In multi-tenant deployments, [[ref/object/domain#membertenantid|`memberTenantId`]] assigns ownership to a [[ref/object/tenant|Tenant]], and [[ref/object/domain#directoryid|`directoryId`]] points to the [[ref/object/directory|Directory]] in which accounts for the domain are looked up. Leaving the directory unset falls back to the internal directory.

## Mail handling

[Section titled “Mail handling”](#mail-handling)

Recipient resolution for the domain is shaped by three fields. [[ref/object/domain#catchalladdress|`catchAllAddress`]] names a fallback mailbox that receives messages addressed to unknown local recipients; leaving it unset causes unknown recipients to be rejected at SMTP time. [[ref/object/domain#subaddressing|`subAddressing`]] controls plus-addressing behaviour: the `Enabled` variant accepts `user+tag@domain` and delivers to `user@domain`, the `Disabled` variant turns the feature off, and the `Custom` variant runs an expression against the recipient to decide how the local part is canonicalised. [[ref/object/domain#allowrelaying|`allowRelaying`]] opts the domain into relaying for recipients that are not local, which is required in split-delivery or smart-host setups and should remain disabled otherwise.

The [[ref/object/domain#reportaddressuri|`reportAddressUri`]] field sets the mailbox that receives DMARC aggregate, TLS-RPT, and CAA incident reports generated for the domain. The default is `mailto:postmaster`; setting the field to null suppresses report generation for the domain.

## Linked infrastructure

[Section titled “Linked infrastructure”](#linked-infrastructure)

Three fields on the Domain link it to the objects that manage its externally visible infrastructure:

[[ref/object/domain#dnsmanagement|`dnsManagement`]] selects manual or automatic DNS record publication. The `Automatic` variant references a [[ref/object/dns-server|DnsServer]] through [[ref/object/domain#dnsserverid|`dnsServerId`]] and controls which record types the server keeps in sync. See [[domains/dns-records|DNS records]] for the record management workflow.
[[ref/object/domain#certificatemanagement|`certificateManagement`]] selects manual or ACME-driven TLS management. The `Automatic` variant references an [[ref/object/acme-provider|AcmeProvider]] through [[ref/object/domain#acmeproviderid|`acmeProviderId`]]; the `Manual` variant relies on [[ref/object/certificate|Certificate]] objects supplied by the operator. See [[domains/tls-certificates|TLS certificates]] for the details.
[[ref/object/domain#dkimmanagement|`dkimManagement`]] selects manual or automatic DKIM key generation and rotation. The `Automatic` variant drives the creation, publication, and retirement of [[ref/object/dkim-signature|DkimSignature]] records for the domain. See [[domains/dkim-rotation|DKIM key rotation]] for the rotation lifecycle.

The read-only [[ref/object/domain#dnszonefile|`dnsZoneFile`]] field exposes the current set of DNS records the server expects to be published for the domain. When DNS management is manual, administrators copy this zone data into the external DNS provider; when DNS management is automatic, the server reconciles the same records against the configured [[ref/object/dns-server|DnsServer]].