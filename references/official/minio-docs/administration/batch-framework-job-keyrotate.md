---
title: "Batch Key Rotation — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/batch-framework-job-keyrotate.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/batch-framework-job-keyrotate"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Batch Key Rotation — SILO (MinIO Object Storage)"
---
# Batch Key Rotation

New in version MinIO: RELEASE.2023-04-07T05-28-58Z

The MinIO Batch Framework allows you to create, manage, monitor, and execute jobs using a YAML-formatted job definition file (a “batch file”).
The batch jobs run directly on the MinIO deployment to take advantage of the server-side processing power without constraints of the local machine where you run the [[reference/minio-mc#minio-client|MinIO Client]].

The `keyrotate` batch job type cycles the [[operations/server-side-encryption#minio-sse-data-encryption|sse-s3 or sse-kms keys]] for encrypted objects on a MinIO deployment.

The YAML configuration supports filters to restrict key rotation to a specific set of objects by creation date, tags, metadata, or kms key.
You can also define retry attempts or set a notification endpoint and token.

## Key Rotate Batch Job Reference

New in version MinIO: RELEASE.2023-04-07T05-28-58Z

Use the `keyrotate` job type to create a batch job that cycles the [[operations/server-side-encryption#minio-sse-data-encryption|sse-s3 or sse-kms keys]] for encrypted objects.

### Required Fields

`type:`

Either `sse-s3` or `sse-kms`.

`key:`

Only for use with the `sse-kms` type.
The key to use to unseal the key vault.

### Optional Fields

For flag based filters

`newerThan:`

A string representing a length of time in `#d#h#s` format.

Keys rotate only for objects newer than the specified length of time.
For example, `7d`, `24h`, `5d12h30s` are valid strings.

`olderThan:`

A string representing a length of time in `#d#h#s` format.

Keys rotate only for objects older than the specified length of time.

`createdAfter:`

A date in `YYYY-MM-DDTHH:MM:SSZ` [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339.html)  date and time format.

Keys rotate only for objects created after the date.

`createdBefore:`

A date in `YYYY-MM-DDTHH:MM:SSZ` [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339.html)  date and time format.

Keys rotate only for objects created prior to the date.

`context:`

Only for use with the `sse-kms` type.
The context within which to perform actions.

`tags:`

Rotate keys only for objects with tags that match the specified `key:` and `value:`.

`metadata:`

Rotate keys only for objects with metadata that match the specified `key:` and `value:`.

`kmskey:`

Rotate keys only for objects with a KMS key-id that match the specified value.
This is only applicable for the `sse-kms` type.

For notifications

`endpoint:`

The predefined endpoint to send events for notifications.

`token:`

An optional JSON Web Token (JWT) to access the `endpoint`.

For retry attempts

If something interrupts the job, you can define a maximum number of retry attempts.
For each retry, you can also define how long to wait between attempts.

`attempts:`

Number of tries to complete the batch job before giving up.

`delay:`

The amount of time to wait between each attempt.

## Sample YAML Description File for a keyrotate Job Type

Use [[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]] to create a basic `keyrotate` batch job for further customization:

```
keyrotate:
  apiVersion: v1
  bucket: BUCKET
  prefix: PREFIX
  encryption:
    type: sse-s3 # valid values are sse-s3 and sse-kms
    key: <new-kms-key> # valid only for sse-kms
    context: <new-kms-key-context> # valid only for sse-kms

  # optional flags based filtering criteria
  # for all objects
  flags:
    filter:
      newerThan: "7d" # match objects newer than this value (e.g. 7d10h31s)
      olderThan: "7d" # match objects older than this value (e.g. 7d10h31s)
      createdAfter: "date" # match objects created after this date and time in RFC3339 format
      createdBefore: "date" # match objects created before this date and time in RFC3339 format
      tags:
        - key: "name"
          value: "pick*" # match objects with tag 'name', with all values starting with 'pick'
      metadata:
        - key: "content-type"
          value: "image/*" # match objects with 'content-type', with all values starting with 'image/'
      kmskey: "key-id" # match objects with KMS key-id (applicable only for sse-kms)
    notify:
      endpoint: "https://notify.endpoint" # notification endpoint to receive job status events
      token: "Bearer xxxxx" # optional authentication token for the notification endpoint
    retry:
      attempts: 10 # number of retries for the job before giving up
      delay: "500ms" # least amount of delay between each retry
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).