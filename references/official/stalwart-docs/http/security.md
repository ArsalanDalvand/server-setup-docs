---
title: "Security"
source: "https://stalw.art/docs/http/security"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/security"
category: "http"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Security"
---
# Security

      

The HTTP server exposes two transport-security controls on the [[ref/object/http|Http]] singleton (found in the WebUI under  Settings ›  Network › HTTP › General,  Settings ›  Network › HTTP › Security): HTTP Strict Transport Security (HSTS) and a permissive CORS policy. These controls enforce secure communication practices and determine how cross-origin requests are handled.

## Strict Transport Security

[Section titled “Strict Transport Security”](#strict-transport-security)

HTTP Strict Transport Security (HSTS) is a web security policy mechanism that instructs compliant user agents to interact with the server only over HTTPS, protecting against protocol-downgrade attacks and cookie hijacking. Enabling HSTS guarantees that every subsequent request is encrypted and reduces the risk of active eavesdropping or session hijacking.

HSTS is enabled by setting [[ref/object/http#enablehsts|`enableHsts`]] to `true`. The default is `false`.

## Permissive CORS Policy

[Section titled “Permissive CORS Policy”](#permissive-cors-policy)

Cross-Origin Resource Sharing (CORS) is a browser security feature that controls how a web page loaded from one origin can request resources from another origin. By default, browsers block these cross-origin requests; the server must opt in by returning the appropriate CORS headers.

A permissive CORS policy can be useful when the Stalwart server is managed through a WebUI hosted on a different domain. Setting [[ref/object/http#usepermissivecors|`usePermissiveCors`]] to `true` instructs the server to accept requests from any origin. The default is `false`.

This is the broadest possible CORS setting. When enabling it, ensure that every sensitive endpoint is protected by appropriate authentication and authorization checks, because browsers will no longer restrict cross-origin access to the server’s resources.