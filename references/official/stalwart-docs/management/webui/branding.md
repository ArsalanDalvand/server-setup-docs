---
title: "Branding"
source: "https://stalw.art/docs/management/webui/branding"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/webui/branding"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Branding"
---
# Branding

      

Branding allows administrators to customise the appearance of the WebUI by changing the logo displayed to users. Custom branding can be applied to the entire system or scoped to specific tenants and domains, which is useful for multi-tenant deployments.

## Logo selection order

[Section titled “Logo selection order”](#logo-selection-order)

The logo displayed by the WebUI is chosen based on the hostname of the incoming HTTP request. Stalwart consults the following sources in order and uses the first one that yields a valid logo URL:

Domain logo. Stalwart looks up the hostname against the configured [[ref/object/domain|Domain]] objects. If a matching Domain has the [[ref/object/domain#logo|`logo`]] field set, that logo is used.
Tenant logo. If the matching Domain has no logo but is linked to a [[ref/object/tenant|Tenant]] through [[ref/object/domain#membertenantid|`memberTenantId`]], and the Tenant has its own [[ref/object/tenant#logo|`logo`]] set, that logo is displayed instead. This provides tenant-wide branding for domains that do not set their own logo.
System-wide default. If neither the Domain nor the Tenant sets a logo, Stalwart falls back to the system-wide default carried by the [[ref/object/enterprise|Enterprise]] singleton (found in the WebUI under  Settings ›  Enterprise) through its [[ref/object/enterprise#logourl|`logoUrl`]] field.
Built-in logo. If no custom logo is set anywhere, the WebUI displays Stalwart’s built-in default.