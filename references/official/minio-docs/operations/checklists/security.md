---
title: "Security Checklist — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/checklists/security.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/checklists/security"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Security Checklist — SILO (MinIO Object Storage)"
---
# Security Checklist

Use the following checklist when planning the security configuration for a production, distributed MinIO deployment.

## Required Steps

Define group policies either on MinIO or the selected 3rd party Identity Provider (LDAP/Active Directory or OpenID)

Define individual access policies on MinIO or the selected 3rd party Identity Provider

(For Kubernetes deployments only) Configure the tenant(s) to use the selected 3rd party Identity Provider

Grant firewall access for TCP traffic to the MinIO Server S3 API Listen Port (Default: `9000`).

Grant firewall access for TCP traffic to the [[administration/minio-console#minio-console-port-assignment|MinIO Server Console Listen Port]] (Recommended Default: `9090`).

## Encryption-at-Rest

MinIO supports the following external KMS providers through Key Encryption Service (KES):

[[operations/server-side-encryption/configure-minio-kes#minio-sse-vault|HashiCorp Vault Root KMS]]

[[operations/server-side-encryption/configure-minio-kes#minio-sse-aws|AWS Root KMS]]

[[operations/server-side-encryption/configure-minio-kes#minio-sse-gcp|Google Cloud Platform Secret Manager Root KMS]]

[[operations/server-side-encryption/configure-minio-kes#minio-sse-azure|Azure Key Vault Root KMS]]

Download and install the MinIO Key Encryption Service (KES)

Enable TLS

Generate private and public keys for KES

Generate private and public keys for MinIO

Create a KES configuration file and start the service

Generate an external key for the key management service (KMS)

Connect MinIO to the KES

Enable server side encryption

## Encryption-in-Transit (“In flight”)

[[operations/network-encryption#minio-tls|Enable TLS]]

Add separate certificates and keys for each internal and external domain that accesses MinIO

Generate public and private TLS keys using a supported cipher for TLS 1.3 or TLS 1.2

Configure trusted Certificate Authority (CA) store(s)

Expose your Kubernetes service, such as with NGINX

(Optional) Validate certificates, such as with [https://www.sslchecker.com/certdecoder](https://www.sslchecker.com/certdecoder)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).