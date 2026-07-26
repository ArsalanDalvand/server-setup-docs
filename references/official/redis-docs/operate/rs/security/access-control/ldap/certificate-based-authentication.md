---
title: "Certificate-based authentication for LDAP"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/ldap/certificate-based-authentication/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/ldap/certificate-based-authentication"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Certificate-based authentication for LDAP"
---
# Certificate-based authentication for LDAP

Learn how to set up certificate-based authentication for LDAP in Redis Software.

You can configure Redis Software to use certificate-based authentication in combination with LDAP to authenticate and authorize users.

Here's how it works:

Users authenticate to the database using client certificates.

Redis Software extracts the user identity from the certificate.

Redis Software looks up the user in LDAP.

LDAP group mappings and database ACLs determine authorization.

## Prerequisites

Before enabling certificate-based authentication for LDAP, you must complete the following prerequisites:

[[operate/rs/security/encryption/tls/enable-tls|Enable mutual TLS]] for the relevant databases to set up certificate-based authentication.

[[operate/rs/security/access-control/ldap/enable-role-based-ldap#set-up-ldap-connection|Set up LDAP for the cluster]].

[Map LDAP users to appropriate cluster roles](/docs/latest/operate/rs/security/access-control/ldap/map-ldap-groups-to-roles/).

[[operate/rs/security/access-control/ldap/update-database-acls|Configure database ACLs]] to authorize LDAP users.

## Set up certificate-based authentication for LDAP

To set up certificate-based authentication for LDAP:

Enable certificate-based authentication in the cluster LDAP configuration using an [update LDAP configuration](/docs/latest/operate/rs/references/rest-api/api-reference/#tag/Cluster/operation/cluster_update_ldap_config) REST API request.

To perform LDAP lookup for certificate-authenticated users:

Redis Software must extract an identity from the client certificate's subject line, using either the subject CN or a custom OID. Extracting user identity from SAN is not currently supported.

The certificate subject must follow [RFC 4514](https://datatracker.ietf.org/doc/html/rfc4514) formatting.

Do not create local user accounts for LDAP users.

        Subject CN
      

        Subject OID
      

To enable certificate-based authentication and use the certificate subject CN as the LDAP identifier:

```sh
PUT https://<host>:<port>/v1/cluster/ldap
{
  "cba": true,
  "cba_identity_source": "subject_cn",
  "control_plane": true,
  "data_plane": true
}
```

To enable certificate-based authentication and use a custom subject OID as the LDAP identifier:

```sh
PUT https://<host>:<port>/v1/cluster/ldap
{
  "cba": true,
  "cba_identity_source": "subject_oid",
  "cba_identity_oid": "1.2.3.4",
  "control_plane": true,
  "data_plane": true
}
```

The certificate subject must include the OID.

Enable external certificate-based authentication in cluster settings using an [update cluster settings](/docs/latest/operate/rs/references/rest-api/requests/cluster/#put-cluster) REST API request:

```sh
PUT https://<host>:<port>/v1/cluster
{
  "dmc_external_cba_authentication": true
}
```

## Verify your setup

After configuration is complete, try to [connect to the database with certificate-based authentication](/docs/latest/operate/rs/security/certificates/certificate-based-authentication/#authenticate-database-connections) and verify your setup.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page