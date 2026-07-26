---
title: "Dynamic Registration"
source: "https://stalw.art/docs/auth/oauth/client-registration"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/oauth/client-registration"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Dynamic Registration"
---
# Dynamic Registration

      

The OAuth Dynamic Client Registration Protocol ([RFC 7591](https://www.rfc-editor.org/rfc/rfc7591.html)) extends OAuth 2.0 so that clients can register with an authorization server without administrator intervention. In a pre-registration model, each client id and secret is created manually; dynamic registration is well suited to environments where many applications or devices need to connect, such as cloud services or IoT deployments.

A client that wants to register sends an HTTP request to the OAuth server’s registration endpoint, supplying metadata about itself (name, redirect URIs, and the types of grants it intends to use). If the request is accepted, the server returns a client id and, when applicable, a client secret that the client uses to authenticate during subsequent OAuth flows.

The overall sequence is as follows:

Registration request: the client sends an HTTP POST to the OAuth server’s registration endpoint with its metadata (name, redirect URIs, supported flows, and so on).
Client registration: the server validates the request and, if acceptable, returns a client id and optional secret.
Client use of credentials: the client uses the returned credentials to drive OAuth flows and obtain access and refresh tokens.

Stalwart supports the Dynamic Client Registration Protocol against its built-in OAuth server.

## Enabling registration

[Section titled “Enabling registration”](#enabling-registration)

By default, Stalwart does not require clients to register before requesting access tokens: any client id is accepted. To enforce registration, set [[ref/object/oidc-provider#requireclientregistration|`requireClientRegistration`]] on the [[ref/object/oidc-provider|OidcProvider]] singleton (found in the WebUI under  Settings ›  Authentication › OIDC Provider) to `true`. When enabled, every client must register and obtain credentials before the server issues tokens to it.

## Client authentication

[Section titled “Client authentication”](#client-authentication)

Before a client can register, it must itself authenticate. The recommended approach is to create an [[auth/authentication/api-key|API Key]] for the client. The key should be granted the `authenticate` and `oauth-client-registration` [[auth/authorization/permissions|permissions]], allowing the client to authenticate and to perform the registration call.

## Anonymous registrations

[Section titled “Anonymous registrations”](#anonymous-registrations)

Clients can also be allowed to register without any prior authentication, although this is not recommended because it widens the attack surface. Anonymous registration is controlled by [[ref/object/oidc-provider#anonymousclientregistration|`anonymousClientRegistration`]] on the [[ref/object/oidc-provider|OidcProvider]] singleton. The default, `false`, should be kept unless there is a specific reason to allow untrusted clients to self-register.

## Managing clients

[Section titled “Managing clients”](#managing-clients)

Once dynamic registration is enabled and a client registers successfully, the client is stored as an [[ref/object/o-auth-client|OAuthClient]] object (found in the WebUI under  Management ›  Directory › OAuth Clients). The relevant fields are [[ref/object/o-auth-client#clientid|`clientId`]], [[ref/object/o-auth-client#description|`description`]], [[ref/object/o-auth-client#contacts|`contacts`]], [[ref/object/o-auth-client#redirecturis|`redirectUris`]], [[ref/object/o-auth-client#secret|`secret`]], [[ref/object/o-auth-client#logo|`logo`]], [[ref/object/o-auth-client#expiresat|`expiresAt`]], and [[ref/object/o-auth-client#membertenantid|`memberTenantId`]]. Administrators can modify or delete registered clients from the [[management/webui|WebUI]] or the JMAP API.

## WebUI client

[Section titled “WebUI client”](#webui-client)

The Stalwart [[management/webui|WebUI]], which includes the Self-Service Portal for end users, authenticates through OAuth. When `requireClientRegistration` is set to `true`, the WebUI stops working unless it is itself registered, because its default client id (`stalwart-webui`) is not registered automatically.

### Manual registration

[Section titled “Manual registration”](#manual-registration)

To keep the WebUI functional after enforcing client registration, create an OAuthClient object with:

[[ref/object/o-auth-client#clientid|`clientId`]]: `stalwart-webui`
[[ref/object/o-auth-client#redirecturis|`redirectUris`]]: the OAuth callback for each base path the WebUI is served under, built from the server’s public URL: `["https://<server>/admin/oauth/callback", "https://<server>/account/oauth/callback"]`. The `/admin` path is the administration interface and `/account` is the Self-Service Portal; adjust these prefixes if `server.url-prefix` has been customized.

With the WebUI registered as an OAuth client, administrators and users continue to reach the WebUI and the Self-Service Portal without interruption.

### Alternative

[Section titled “Alternative”](#alternative)

When administrators prefer not to register the WebUI explicitly, they can instead grant the `oauth-client-override` [[auth/authorization/permissions|permission]] to administrative accounts. This permission allows the holder to use any client id, bypassing the registration requirement.

The `oauth-client-override` permission should not be granted to regular accounts. Doing so effectively disables the registration requirement for those accounts and should be reserved for administrators who need to manage the server or troubleshoot access.