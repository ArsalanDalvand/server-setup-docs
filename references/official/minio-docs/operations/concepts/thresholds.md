---
title: "Thresholds and Limits — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/concepts/thresholds.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/concepts/thresholds"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Thresholds and Limits — SILO (MinIO Object Storage)"
---
# Thresholds and Limits

This page reflects limits and thresholds that apply to MinIO.

Refer to the [[operations/checklists/hardware#minio-hardware-checklist|hardware]] and [[operations/checklists/software#minio-software-checklists|software]] for related recommendations and requirements.

## S3 API Limits

Item

Specification

Maximum object size

50 TiB

Minimum object size

0 B

Maximum object size per PUT operation

5 TiB for non-multipart upload
50 TiB for multipart upload

Maximum number of parts per upload

10,000

Part size range

5 MiB to 5 GiB. Last part can be 0 B to 5 GiB

Maximum number of parts returned per list parts request

10,000

Maximum number of objects returned per list objects request

1,000

Maximum number of multipart uploads returned per list multipart uploads request

1,000

Maximum length for bucket names

63

Maximum length for object names

1024

Maximum length for each `/` separated segment of an object name

255

Maximum number of object versions for a unique object

10000 (Configurable)

## Erasure Code Limits

Item

Specification

Maximum number of servers per cluster

no limit

Minimum number of servers

1

Minimum number of drives per server when server count is 1

1 (for SNSD deployments, which do not provide additional reliability or availability)

Minimum number of drives per server when server count is 2 or more

1

Maximum number of drives per server

no limit

Read quorum

\(N/2\)

Write quorum

\((N/2)+1\)

## Object Name Limitations

### Filesystem and Operating System Restrictions

Object Names in MinIO are restricted primarily by the local operating system and filesystem.
Windows and some other operating systems restrict file systems with certain special characters, such as `^`, `*`, `|`, `\`, `/`, `&`, `"`, or `;`.

This list is not exhaustive and may not apply to your operating system and filesystem combination.

On Unix-like operating systems, objects with a path name of `.`, `..`, or `/` return an error of `file access denied`.

Consult your operating system vendor or filesystem documentation for a comprehensive list for your situation.

MinIO recommends using a Linux operating system with an XFS based filesystem for production workloads.

### Conflicting Objects

Applications must assign non-conflicting, unique keys for all objects.
This includes avoiding creating objects where the name can collide with that of a parent or sibling object.
MinIO returns an empty set for LIST operations at the location of the collision.

For example, the following operations create a namespace conflicts

```
PUT data/invoices/2024/january/vendors.csv
PUT data/invoices/2024/january <- collides with existing object prefix
```

```
PUT data/invoices/2024/january
PUT data/invoices/2024/january/vendors.csv <- collides with existing object
```

While you can perform GET or HEAD operations against these objects, the name collision causes LIST operations to return an empty result set at the `/invoices/2024/january` path.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).