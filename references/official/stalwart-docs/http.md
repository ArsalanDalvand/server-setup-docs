---
title: "Overview"
source: "https://stalw.art/docs/http"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http"
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

      

Stalwart includes an HTTP service that is enabled by default. It supports JMAP access, WebDAV access, API management, ACME certificate issuance, autoconfig/autodiscover protocols, well-known resources, metrics collection, and OAuth authentication.

Multiple HTTP services can be defined, each with custom [[http/access-control|access control rules]] expressed on the [[ref/object/http|Http]] singleton (found in the WebUI under  Settings ›  Network › HTTP › General,  Settings ›  Network › HTTP › Security). Rules can filter requests by IP address, resource, method name, or listener identity.

## HTTP Endpoints

[Section titled “HTTP Endpoints”](#http-endpoints)

The following endpoints are available through the HTTP service:

`/jmap`: The JMAP endpoint provides access to the [[http/jmap|JMAP API]], allowing clients to interact with mailboxes, messages, and other resources.
`/dav/*`: The WebDAV endpoint provides access to [[http/webdav|WebDAV]] resources, allowing clients to manage calendar, contact and file resources.
`/calendar/rsvp`: The Calendar Scheduling [[collaboration/scheduling#http-rsvp|RSVP endpoint]] allows participants to respond to calendar invitations using a simple web interface.
`/.well-known/*`: The [well-known endpoint](#well-known-resources) provides access to resources that are commonly used by clients to discover service information.
`/api/*`: Provides access to the [[development/api|HTTP API]], a small set of endpoints for authentication, account introspection, configuration schema retrieval, and live telemetry. Server configuration and mailbox data are accessed through JMAP, not through this endpoint.
`/auth/device`: The device authorization endpoint is used for [[auth/oauth|OAuth device authorization]].
`/auth/token`: The token endpoint is used for [[auth/oauth|OAuth token exchange]].
`/auth/introspect`: The token introspection endpoint is used for [[auth/oauth/endpoints#authintrospect|OAuth token introspection]].
`/auth/userinfo`: The user information endpoint is used for [[auth/openid/endpoints#authuserinfo|OpenID user information]].
`/auth/jwks.json`: The JSON Web Key Set (JWKS) endpoint provides public keys for [[auth/openid/endpoints#authjwksjson|JWT verification]].
`/mail/config-v1.1.xml`: The [[server/autoconfig|autodiscover]] endpoint provides email client configuration information.
`/autodiscover/autodiscover.xml`: The [[server/autoconfig|autodiscover]] endpoint provides email client configuration information.
`/metrics/prometheus`: The [[telemetry/metrics/prometheus|Prometheus]] metrics endpoint provides metrics for monitoring the server’s performance.
`/form`: The form endpoint is used for [[http/form-submission|Form submissions]].
`/healthz/ready`: The health check endpoint indicates whether the server is ready to accept requests.
`/healthz/live`: The health check endpoint indicates whether the server is live and operational.
`/robots.txt`: The robots.txt file provides instructions to web crawlers and other user agents about the server’s content.
`/*`: The default endpoint serves static files from the [[management/webui|WebUI bundle]].

## Well-known Resources

[Section titled “Well-known Resources”](#well-known-resources)

The following well-known resources are available through the HTTP service:

`/.well-known/jmap`: The JMAP well-known resource provides information about the [[http/jmap|JMAP]] endpoint.
`/.well-known/caldav`: The CalDAV well-known resource provides information about the [[collaboration/calendar|CalDAV]] endpoint.
`/.well-known/carddav`: The CardDAV well-known resource provides information about the [[collaboration/contact|CardDAV]] endpoint.
`/.well-known/oauth-authorization-server`: The OAuth authorization server well-known resource provides information about the [[auth/oauth|OAuth]] authorization server.
`/.well-known/openid-configuration`: The OIDC discovery well-known resource provides information about the [[auth/openid|OIDC]] discovery endpoint.
`/.well-known/acme-challenge`: The ACME challenge well-known resource provides the HTTP-01 challenge for [[server/tls/acme/challenges#http-01|ACME certificate issuance]].
`/.well-known/mta-sts.txt`: The MTA-STS well-known resource provides information about the [[mta/transport-security/mta-sts|MTA-STS]] policy.
`/.well-known/mail-v1.xml`: The mail configuration well-known resource provides information about the [[server/autoconfig|configuration endpoint]].
`/.well-known/autoconfig/mail/config-v1.1.xml`: The autoconfig mail configuration well-known resource provides information about the [[server/autoconfig|autconfig]] mail configuration endpoint.