---
title: "OpenID Connect"
source: "https://stalw.art/docs/auth/backend/oidc"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/backend/oidc"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "OpenID Connect"
---
# OpenID Connect

      

Stalwart can authenticate users against a third-party OpenID Connect (OIDC) provider. This allows the server to delegate authentication to an existing identity system, for example Google, Microsoft Entra ID, or any OIDC-compliant provider.

As Stalwart is primarily a mail server, it handles OIDC somewhat differently from a typical web application. Clients authenticate over IMAP, POP3, SMTP, or JMAP using the `OAUTHBEARER` SASL mechanism, which carries an access token already obtained from the identity provider. Stalwart does not initiate the OIDC flow itself and does not consume an ID token directly; it validates the access token and uses it to resolve the user’s identity.

## Authentication flow

[Section titled “Authentication flow”](#authentication-flow)

Clients (such as mail applications) present an access token issued by the external OIDC provider using the `OAUTHBEARER` SASL mechanism. Because Stalwart is not part of the OIDC flow, it does not see the [[auth/openid/id-tokens|ID token]]. To identify the user associated with an access token, Stalwart validates the token against the OIDC provider.

Validation is performed automatically through the provider’s OIDC discovery document (the `/.well-known/openid-configuration` endpoint). From that document Stalwart resolves the provider’s signing keys and the userinfo endpoint. The server first attempts offline validation of the access token using the JWT signing keys; when offline validation cannot be completed (for example, opaque tokens that are not JWTs), Stalwart queries the userinfo endpoint with the token to resolve the user’s identity. In both cases the user’s claims are read from the resulting payload. Token introspection with client credentials is not used.

## Limitations

[Section titled “Limitations”](#limitations)

### Offline access

[Section titled “Offline access”](#offline-access)

Stalwart learns about an account only after the first time that account authenticates. OIDC does not provide an offline directory lookup, so an account that exists in the identity provider but has not yet signed in is unknown to the server. Mail addressed to such an account is rejected because the address does not resolve to a local recipient.

To avoid this, create accounts in Stalwart before users sign in for the first time. Accounts can be created through the [[management/webui|WebUI]], the JMAP API, or the [[management/cli|CLI]]. Pre-creating the accounts ensures that inbound mail is accepted from the start, even for users who have not yet authenticated via OIDC.

### OAUTHBEARER SASL

[Section titled “OAUTHBEARER SASL”](#oauthbearer-sasl)

Many widely deployed mail clients, including Outlook, Thunderbird, and Apple Mail, do not support the `OAUTHBEARER` or `XOAUTH2` SASL mechanism with third-party OAuth providers. Users of those clients cannot authenticate directly with OIDC. The standard workaround is to provision [[auth/authentication/app-password|App Passwords]] for those users, as described in the [[auth/oauth/interoperability|interoperability]] section of the OAuth documentation.

## Configuration

[Section titled “Configuration”](#configuration)

An OIDC integration is configured through the OIDC variant of the [[ref/object/directory|Directory]] object (found in the WebUI under  Settings ›  Authentication › Directories). The relevant fields are:

[[ref/object/directory#issuerurl|`issuerUrl`]]: the base URL of the OIDC provider, for example `https://accounts.example.org/realms/myrealm`. Stalwart uses this URL to discover the provider’s endpoints.
[[ref/object/directory#requireaudience|`requireAudience`]]: if set, access tokens whose `aud` claim does not include this value are rejected. The default is `"stalwart"`. Set this to the client id or resource identifier registered for Stalwart at the provider.
[[ref/object/directory#requirescopes|`requireScopes`]]: if set, access tokens must include every listed scope. Default `["openid", "email"]`.
[[ref/object/directory#claimusername|`claimUsername`]]: the claim used to derive the account login name. Default `"preferred_username"`. If the claim value is not already an email address and [[ref/object/directory#usernamedomain|`usernameDomain`]] is set, the domain is appended automatically.
[[ref/object/directory#usernamedomain|`usernameDomain`]]: the domain to append to the username claim when it does not already contain an `@`. When unset, the server falls back to the `email` claim.
[[ref/object/directory#claimname|`claimName`]]: the claim used for the user’s display name. Default `"name"`.
[[ref/object/directory#claimgroups|`claimGroups`]]: the claim used for group memberships. Typical values are `"groups"` or `"roles"`, depending on the provider.

Example integration with a Keycloak-style provider that issues `preferred_username` as a bare username and exposes groups on the `groups` claim:

```
{  "@type": "Oidc",  "description": "External IdP",  "issuerUrl": "https://accounts.example.org/realms/myrealm",  "requireAudience": "stalwart",  "requireScopes": ["openid", "email"],  "claimUsername": "preferred_username",  "usernameDomain": "example.org",  "claimName": "name",  "claimGroups": "groups"}
```

Once configured, the Directory object is selected as the active authentication source by setting [[ref/object/authentication#directoryid|`directoryId`]] on the [[ref/object/authentication|Authentication]] singleton (found in the WebUI under  Settings ›  Authentication › General) to its id.