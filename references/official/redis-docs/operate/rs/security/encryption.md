---
title: "Encryption in Redis Software"
source: "https://redis.io/docs/latest/operate/rs/security/encryption/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/encryption"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Encryption in Redis Software"
---
# Encryption in Redis Software

Encryption in Redis Software.

            Redis Software
          

Redis Software uses encryption to secure communications between clusters, nodes, databases, and clients and to protect [data in transit](https://en.wikipedia.org/wiki/Data_in_transit), [at rest](https://en.wikipedia.org/wiki/Data_at_rest), and [in use](https://en.wikipedia.org/wiki/Data_in_use).

## Encrypt data in transit

### TLS

Redis Software uses [[operate/rs/security/encryption/tls|Transport Layer Security (TLS)]] to encrypt communications for the following:

Cluster Manager UI

Command-line utilities

REST API

Internode communication

You can also [[operate/rs/security/encryption/tls/enable-tls|enable TLS authentication]] for the following:

Communication from clients or applications to your database

Communication from your database to other clusters for replication using [Replica Of](/docs/latest/operate/rs/databases/import-export/replica-of/)

Communication to and from your database to other clusters for [Active-Active](/docs/latest/operate/rs/databases/active-active/) synchronization

### Internode encryption

[[operate/rs/security/encryption/internode-encryption|Internode encryption]] uses TLS to encrypt data in transit between cluster nodes.

By default, internode encryption is enabled for the control plane, which manages the cluster and databases. If you also want to encrypt replication and proxy communications between database shards on different nodes, [[operate/rs/security/encryption/internode-encryption#enable-data-internode-encryption|enable data internode encryption]].

### Require HTTPS for REST API endpoints

By default, the Redis Software API supports communication over HTTP and HTTPS. However, you can [turn off HTTP support](/docs/latest/operate/rs/references/rest-api/encryption/) to ensure that API requests are encrypted.

## Encrypt data at rest

### File system encryption

To encrypt data stored on disk, use file system-based encryption capabilities available on Linux operating systems before you install Redis Software.

### Private key encryption

Enable PEM encryption to [[operate/rs/security/encryption/pem-encryption|encrypt all private keys]] on disk.

## Encrypt data in use

### Client-side encryption

Use client-side encryption to encrypt the data an application stores in a Redis database. The application decrypts the data when it retrieves it from the database.

You can add client-side encryption logic to your application or use built-in client functions.

Client-side encryption has the following limitations:

Operations that must operate on the data, such as increments, comparisons, and searches will not function properly.

Increases management overhead.

Reduces performance.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page