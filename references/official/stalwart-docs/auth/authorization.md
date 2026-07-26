---
title: "Overview"
source: "https://stalw.art/docs/auth/authorization"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authorization"
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

      

Authorization determines what actions an authenticated user or entity may perform within the server. Where [[auth/authentication|authentication]] establishes identity, authorization controls what that identity is allowed to do once verified.

Stalwart supports fine-grained access control. [[auth/authorization/permissions|Permissions]] can be assigned at several levels: directly to [[auth/principals/individual|individuals]] or [[auth/principals/group|groups]], through [[auth/authorization/roles|roles]], or at the [[auth/authorization/tenants|tenant]] level for multi-tenant deployments. The combination of these layers determines the effective permissions of a given principal.

The sections that follow cover the available permission types and how they are configured to match a deployment’s access policy.