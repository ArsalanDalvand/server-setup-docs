---
title: "Disable basic authentication for the REST API"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/disable-basic-authentication/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/disable-basic-authentication"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Disable basic authentication for the REST API"
---
# Disable basic authentication for the REST API

Disable Basic and Digest authentication for the cluster management REST API and require certificate or JWT authentication instead.

            Redis Software
          

By default, the Redis Software cluster management REST API accepts HTTP Basic authentication using a cluster username and password. Starting with Redis Software 8.2.0, you can disable Basic and Digest authentication cluster-wide with the `control_plane_basic_authentication` setting.

When you disable basic authentication, the cluster rejects Basic and Digest authentication on all inbound REST API requests, and clients must authenticate with a client certificate (mTLS) or a JSON Web Token (JWT) instead. This reduces the cluster's attack surface by removing password-based access to the management API.

Note:
  This setting applies only to the cluster management REST API. It does not change how clients authenticate to databases.

## Before you begin

Warning:
  Before you disable basic authentication, make sure every client and tool that calls the REST API can still authenticate another way. If you disable basic authentication without another working method in place, you lose REST API access to the cluster.

When basic authentication is disabled, other configured authentication methodsâsuch as certificate-based (mTLS), JWT, and LDAPâcontinue to work. Set up at least one before you disable basic authentication:

Certificate-based (mTLS) authentication (recommended) â see [Certificate-based authentication](/docs/latest/operate/rs/security/certificates/certificate-based-authentication/).

JWT authentication â obtain a token with an [authorize user](/docs/latest/operate/rs/references/rest-api/requests/users/authorize/) request, then send it as a bearer token on subsequent requests.

Some cluster-management flows require certificate-based authentication specifically when basic authentication is disabledâthey don't use JWT or LDAP:

Joining a node to the cluster and Active-Active (CRDB) management. Configure these flows to use certificate credentials (client certificate, client key, and trusted CA) instead of a username and password. The client certificate's signing CA must be present in the cluster's `mtls_trusted_ca`. See [Certificate-based authentication](/docs/latest/operate/rs/security/certificates/certificate-based-authentication/).

## Disable basic authentication

Basic authentication is enabled by default (`control_plane_basic_authentication` is `true`). To disable it, use one of the following methods:

        REST API
      

        rladmin
      

To disable basic authentication using the REST API, use an [update cluster settings](/docs/latest/operate/rs/references/rest-api/requests/cluster/#put-cluster) request:

```sh
PUT https://<host>:<port>/v1/cluster
{
    "control_plane_basic_authentication": false
}
```

To disable basic authentication using [`rladmin tune cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/tune/):

```sh
rladmin tune cluster control_plane_basic_authentication disabled
```

## Verify

After you disable basic authentication, a request that uses Basic authentication returns `401 Unauthorized`:

```sh
curl -k -u "<username>:<password>" https://<host>:<port>/v1/cluster
# HTTP 401 Unauthorized
```

A request that uses a client certificate or a JWT succeeds. For example, using a client certificate:

```sh
curl -k --cert <client-cert> --key <client-key> https://<host>:<port>/v1/cluster
```

## Re-enable basic authentication

Note:
  If disabling basic authentication left you without cluster access, use the `rladmin` method below. It runs locally on a cluster node and doesn't require REST API access.

To re-enable basic authentication, use one of the following methods:

        REST API
      

        rladmin
      

```sh
PUT https://<host>:<port>/v1/cluster
{
    "control_plane_basic_authentication": true
}
```

```sh
rladmin tune cluster control_plane_basic_authentication enabled
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page