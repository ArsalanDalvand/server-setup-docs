---
title: "Encrypt private keys"
source: "https://redis.io/docs/latest/operate/rs/security/encryption/pem-encryption/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/encryption/pem-encryption"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Encrypt private keys"
---
# Encrypt private keys

Enable PEM encryption to encrypt all private keys on disk.

            Redis Software
          

Enable PEM encryption to automatically encrypt all private keys on disk. Public keys (`.cert` files) are not encrypted.

When certificates are rotated, the encrypted private keys are also rotated.

## Enable PEM encryption

To enable PEM encryption and encrypt private keys on the disk, use [`rladmin`](/docs/latest/operate/rs/references/cli-utilities/rladmin/) or the [REST API](/docs/latest/operate/rs/references/rest-api/).

[`rladmin cluster config`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/config/):

```sh
rladmin cluster config encrypt_pkeys enabled
```

[Update cluster settings](/docs/latest/operate/rs/references/rest-api/requests/cluster/#put-cluster) REST API request:

```sh
PUT /v1/cluster
{ "encrypt_pkeys": true }
```

## Deactivate PEM encryption

To deactivate PEM encryption and decrypt private keys on the disk, use [`rladmin`](/docs/latest/operate/rs/references/cli-utilities/rladmin/) or the [REST API](/docs/latest/operate/rs/references/rest-api/).

[`rladmin cluster config`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/config/):

```sh
rladmin cluster config encrypt_pkeys disabled
```

[Update cluster settings](/docs/latest/operate/rs/references/rest-api/requests/cluster/#put-cluster) REST API request:

```sh
PUT /v1/cluster
{ "encrypt_pkeys": false }
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page