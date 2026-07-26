---
title: "Tokens"
source: "https://stalw.art/docs/auth/oauth/tokens"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/oauth/tokens"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Tokens"
---
# Tokens

      

OAuth tokens are the mechanism by which OAuth 2.0 grants access to protected resources. Instead of sending a username and password on every request, clients present tokens issued by the authorization server; each token has a defined scope and lifespan, which bounds what a compromised token can do.

OAuth defines two primary token types: access tokens and refresh tokens. Each has a distinct role in the authorization process.

## Access Tokens

[Section titled “Access Tokens”](#access-tokens)

An access token is a short-lived token issued by the authorization server (Stalwart’s built-in OAuth server) after a successful authorization request. The client presents the token to the resource server, which validates it before serving the request.

Access tokens have deliberately short lifetimes (typically minutes to a few hours) to limit the impact of a leak. When the token expires, the client must obtain a new one, either by presenting a refresh token or by repeating the authorization flow.

Access tokens are issued with a scope that determines which resources and actions the token grants. A token issued for message submission, for example, does not authorise reading a user’s inbox or changing their account settings.

## Refresh Tokens

[Section titled “Refresh Tokens”](#refresh-tokens)

Refresh tokens let the client obtain a new access token without involving the user again. When the current access token expires, the client presents its refresh token to the authorization server and receives a new access token.

Refresh tokens live longer than access tokens and are therefore more sensitive, because anyone holding a valid refresh token can mint new access tokens. They should be stored securely by the client and must not be shared. Depending on server policy, each use of a refresh token may either return the same refresh token or a rotated one; Stalwart rotates the refresh token when the remaining lifetime drops below a configured threshold.

Refresh tokens are typically issued alongside access tokens during the initial authorization flow.

## Configuration

[Section titled “Configuration”](#configuration)

OAuth token behaviour is configured through the [[ref/object/oidc-provider|OidcProvider]] singleton (found in the WebUI under  Settings ›  Authentication › OIDC Provider).

### Encryption

[Section titled “Encryption”](#encryption)

Tokens issued by Stalwart are cryptographically signed using a construction inspired by [PASETO security tokens](https://paseto.io/):

An encryption key is derived via [BLAKE3](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE3), combining the principal encryption key configured through [[ref/object/oidc-provider#encryptionkey|`encryptionKey`]] with the device id, the expiration time, account details, and an [Argon2](https://en.wikipedia.org/wiki/Argon2) hash of the account’s password.
A nonce is derived via BLAKE3 from the user details and the token’s expiration time.
The derived key and nonce are used to seal the token with [AES-256-GCM-SIV](https://www.rfc-editor.org/rfc/rfc8452).

The [[ref/object/oidc-provider#encryptionkey|`encryptionKey`]] field accepts a `SecretKey` variant: a direct value, a reference to an environment variable, or a reference to a file. Reading the key from a file or environment variable avoids committing it to configuration objects.

A 512-bit key is generated during installation. To replace it, generate a new key (for example with `LC_ALL=C tr -dc '[:alpha:]' < /dev/urandom | head -c 64` on Linux) and update `encryptionKey`, for example:

```
{  "encryptionKey": {    "@type": "Value",    "secret": "sDRvvROsJmRUnjIfcUiDUSaAxdQpfixuLvdwlSffptaxUnSQZALenZSYUPQQByUI"  }}
```

To read the key from an environment variable such as `OAUTH_KEY`:

```
{  "encryptionKey": {    "@type": "EnvironmentVariable",    "variableName": "OAUTH_KEY"  }}
```

### Expiration

[Section titled “Expiration”](#expiration)

Token and code lifetimes are configured through the following fields on the [[ref/object/oidc-provider|OidcProvider]] singleton:

[[ref/object/oidc-provider#accesstokenexpiry|`accessTokenExpiry`]]: lifetime of an access token. Default `"1h"`.
[[ref/object/oidc-provider#refreshtokenexpiry|`refreshTokenExpiry`]]: lifetime of a refresh token. Default `"30d"`.
[[ref/object/oidc-provider#refreshtokenrenewal|`refreshTokenRenewal`]]: remaining lifetime of a refresh token below which the server issues a new one to the client. Default `"4d"`.
[[ref/object/oidc-provider#usercodeexpiry|`userCodeExpiry`]]: lifetime of a user code issued by the device-authorization flow. Default `"30m"`.
[[ref/object/oidc-provider#authcodeexpiry|`authCodeExpiry`]]: lifetime of an authorization code issued by the authorization-code flow. Default `"10m"`.
[[ref/object/oidc-provider#idtokenexpiry|`idTokenExpiry`]]: lifetime of an OpenID Connect ID token. Default `"15m"`.

The maximum number of failed login attempts against an authorization code is configured through [[ref/object/oidc-provider#authcodemaxattempts|`authCodeMaxAttempts`]]; beyond this count the code is invalidated and the user must restart the flow. Default `3`.

### Revocation

[Section titled “Revocation”](#revocation)

Access and refresh tokens can be revoked by changing the owning account’s password, which invalidates every token previously issued against it. This is available to account owners and to administrators.