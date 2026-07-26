---
title: "API Endpoints"
source: "https://stalw.art/docs/development/api"
source_format: official-html-to-markdown
language: "en"
doc_slug: "development/api"
category: "development"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "API Endpoints"
---
# API Endpoints

      

The Stalwart API is deliberately small. The bulk of server
configuration and mailbox data is accessed through JMAP (under `/jmap`), which is a separate API with its own documentation;
the endpoints below are helpers for authentication, account introspection,
configuration schema retrieval and live (Server-Sent Events) telemetry.

All paths are relative to the server’s base URL, e.g.
`https://mail.example.com`.

## Authentication

[Section titled “Authentication”](#authentication)

Requests to the Management API are authenticated with one of:

SchemeHow to sendWhen to useBearer`Authorization: Bearer <access_token>`Normal programmatic access with an OAuth2 access token.Basic`Authorization: Basic <base64(user:secret)>`Simple scripts or initial bootstrap.Live token`?token=<short_lived_token>` in the URL queryServer-Sent Events streams from browsers where an `Authorization` header cannot be set on `EventSource`. The token is obtained from `/api/token/{kind}` and expires after 60 seconds.

Anonymous endpoints (`/api/auth`, `/api/discover/{email}`) are rate-limited.

Failed authentication returns `401 Unauthorized` with a
`WWW-Authenticate: Bearer realm="Stalwart Server"` header and an
`application/problem+json` body (RFC 7807).

## POST /api/auth

[Section titled “POST /api/auth”](#post-apiauth)

Authenticate a user and obtain an OAuth authorization code. Used by the web UI
and by device-flow clients to complete login. Anonymous, rate-limited.

Request body — tagged union discriminated by `type`:

### authCode — authorization-code flow (optionally with PKCE)

[Section titled “authCode — authorization-code flow (optionally with PKCE)”](#authcode--authorization-code-flow-optionally-with-pkce)

```
{  "type": "authCode",  "accountName": "[email protected]",  "accountSecret": "s3cret",  "mfaToken": null,  "clientId": "stalwart-webui",  "redirectUri": "https://mail.example.com/admin/oauth/callback",  "nonce": null,  "scope": null,  "codeChallenge": "E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM",  "codeChallengeMethod": "S256",  "state": null}
```

FieldTypeRequiredNotes`accountName`stringyesAccount name or email.`accountSecret`stringyesAccount password.`mfaToken`stringnoProvided on a retry after a previous `mfaRequired` response.`clientId`stringyesOAuth client identifier.`redirectUri`string (URI)noMust use `https://` unless the server is in recovery or dev mode.`nonce`stringnoOpenID Connect nonce.`scope`stringnoRequested OAuth scope.`codeChallenge`stringnoPKCE code challenge (RFC 7636).`codeChallengeMethod`stringno`plain` or `S256`. Defaults to `plain` when a challenge is present.`state`stringnoOpaque value echoed back to the client.

### authDevice — device-flow completion

[Section titled “authDevice — device-flow completion”](#authdevice--device-flow-completion)

```
{  "type": "authDevice",  "accountName": "[email protected]",  "accountSecret": "s3cret",  "code": "BDWP-HQPK"}
```

FieldTypeRequiredNotes`accountName`stringyes`accountSecret`stringyes`mfaToken`stringno`code`stringyesUser-facing device code issued by `POST /auth/device`.

Response body — tagged union discriminated by `type`:

`type`Extra fieldsMeaning`authenticated``clientCode` (string)Credentials accepted. Exchange `clientCode` at `POST /auth/token`.`verified`–Device-flow verification succeeded.`mfaRequired`–Multi-factor is required; retry with a valid `mfaToken`.`failure`–Credentials rejected.

Example success:

```
{ "type": "authenticated", "clientCode": "3F7A9C1E4B2D8E6F" }
```

## GET /api/discover/{email}

[Section titled “GET /api/discover/{email}”](#get-apidiscoveremail)

Return the OpenID Connect discovery document for the directory that owns the
domain of `email`. If the domain is not bound to an external OIDC directory,
the server’s own discovery document (equivalent to
`/.well-known/openid-configuration`) is returned.

Anonymous, rate-limited.

Path parameters

NameInTypeDescription`email`pathstringEmail address or account name.

Response — `200 OK`, `application/json`. Standard OIDC discovery metadata
(issuer, `authorization_endpoint`, `token_endpoint`, etc. per RFC 8414).

## GET /api/account

[Section titled “GET /api/account”](#get-apiaccount)

Return the authenticated account’s effective permissions, server edition and
preferred locale. Used by the web UI to tailor the interface.

Response — `200 OK`, `application/json`:

```
{  "permissions": [    "authenticate",    "jmap-email-get",    "sys-account-settings-get"  ],  "edition": "oss",  "locale": "en-US"}
```

FieldTypeNotes`permissions`string[]Effective permissions, filtered to exclude internal/system-only entries. In bootstrap mode, limited to `sys-bootstrap-get`/`-update`.`edition`stringOne of `oss`, `community`, `enterprise`.`locale`stringBCP 47–style language tag.

## GET /api/schema

[Section titled “GET /api/schema”](#get-apischema)

Redirect (`302`) to `/api/schema/{hash}`, where `{hash}` is the SHA-256 hex
digest of the current configuration schema. Callers that do not already know
the current hash should follow this redirect; callers that have cached a
schema at a specific hash can fetch it directly — that URL is immutable.

## GET /api/schema/{hash}

[Section titled “GET /api/schema/{hash}”](#get-apischemahash)

Return the JSON Schema describing the full Stalwart configuration tree. The
response is always `Content-Encoding: gzip` and is served with an immutable
cache policy: the schema at a given hash never changes. If the supplied hash
does not match the server’s current schema, the server redirects to the
correct URL.

Path parameters

NameInTypeDescription`hash`pathstringSHA-256 hex digest of the schema document.

Response — `200 OK`, `application/json` (gzip-encoded) containing a JSON
Schema document.

## GET /api/token/{kind}

[Section titled “GET /api/token/{kind}”](#get-apitokenkind)

Issue a short-lived (60 second) bearer token for a specific Server-Sent Events
stream. The returned token is used as the `?token=` query parameter on the
matching `/api/live/...` endpoint, which lets browsers authenticate an
`EventSource` connection without being able to set request headers.

Path parameters

`kind`Permission requiredEnterprise only`delivery``LiveDeliveryTest`no`tracing``LiveTracing`yes`metrics``LiveMetrics`yes

Response — `200 OK`, `text/plain` — the opaque token string, valid for 60
seconds and bound to the matching `/api/live/{kind}` path.

Requesting `tracing` or `metrics` on a non-Enterprise build returns
`404 Not Found`.

## GET /api/live/delivery/{target}

[Section titled “GET /api/live/delivery/{target}”](#get-apilivedeliverytarget)

Stream outbound-delivery diagnostics as Server-Sent Events. Opens a
long-running `text/event-stream` that reports each stage of an outbound
delivery attempt to `target` (MX lookup, MTA-STS fetch, DANE/TLSA validation,
SMTP conversation, etc.) and ends with a `completed` event.

Requires the `LiveDeliveryTest` permission. May be authenticated via a normal
`Authorization` header or via `?token=` using a token from
`/api/token/delivery`.

Path parameters

NameInTypeDescription`target`pathstringDomain or email address to diagnose.

Query parameters

NameTypeDefaultDescription`timeout`integer`30`Maximum stream lifetime in seconds (≥ 1).`token`string–Short-lived token from `/api/token/delivery`.

Response — `200 OK`, `Content-Type: text/event-stream`. Each frame has the
form:

```
event: eventdata: [<DeliveryStage JSON>]
```

…where `<DeliveryStage JSON>` is an object tagged by `type` — for example
`{"type":"mxLookupStart","domain":"example.com"}`,
`{"type":"mxLookupSuccess","mxs":[...],"elapsed":42}`, and so on. The final
frame is always `{"type":"completed"}`.

## GET /api/live/tracing (Enterprise)

[Section titled “GET /api/live/tracing (Enterprise)”](#get-apilivetracing-enterprise)

Stream live server trace events as Server-Sent Events. Requires the
`LiveTracing` permission and may be authenticated via `?token=` using a token
from `/api/token/tracing`. Returns `404 Not Found` on non-Enterprise builds.

Response — `200 OK`, `Content-Type: text/event-stream`.

## GET /api/live/metrics (Enterprise)

[Section titled “GET /api/live/metrics (Enterprise)”](#get-apilivemetrics-enterprise)

Stream live server metrics as Server-Sent Events. Requires the `LiveMetrics`
permission and may be authenticated via `?token=` using a token from
`/api/token/metrics`. Returns `404 Not Found` on non-Enterprise builds.

Response — `200 OK`, `Content-Type: text/event-stream`.

## Error responses

[Section titled “Error responses”](#error-responses)

All error responses use [RFC 7807](https://www.rfc-editor.org/rfc/rfc7807)
`application/problem+json`:

```
{  "type": "about:blank",  "title": "Unauthorized",  "status": 401,  "detail": "Invalid or missing credentials."}
```

StatusMeaning`400`Malformed request body or invalid parameters.`401`Missing or invalid credentials. Response includes `WWW-Authenticate`.`403`Authenticated, but lacking the required permission.`404`Resource not found, or Enterprise-only endpoint on an OSS build.`429`Anonymous-request rate limit exceeded.