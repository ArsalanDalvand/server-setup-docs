---
title: "Security Token Service (STS) — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/security-token-service.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/security-token-service"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Security Token Service (STS) — SILO (MinIO Object Storage)"
---
# Security Token Service (STS)

The MinIO Security Token Service (STS) APIs allow applications to generate temporary credentials for accessing the MinIO deployment.

The STS API is required for MinIO deployments configured to use external identity managers, as the API allows conversion of the external IDP credentials into AWS Signature v4-compatible credentials.

## STS API Endpoints

MinIO supports the following STS API endpoints:

Endpoint

Supported IDP

Description

[[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]]

OpenID Connect

Generates an access key and secret key using the JWT token returned by the OIDC provider

[[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]]

Active Directory / LDAP

Generates an access key and secret key using the AD/LDAP credentials specified to the API endpoint.

[[developers/security-token-service/AssumeRoleWithCustomToken#minio-sts-assumerolewithcustomtoken|AssumeRoleWithCustomToken]]

MinIO Identity Plugin

Generates a token for use with an external identity provider and the [[administration/identity-access-management/pluggable-authentication#minio-external-identity-management-plugin|MinIO Identity Plugin]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).