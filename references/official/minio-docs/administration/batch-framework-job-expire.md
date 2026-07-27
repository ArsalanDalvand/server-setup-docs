---
title: "Batch Expiration — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/batch-framework-job-expire.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/batch-framework-job-expire"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Batch Expiration — SILO (MinIO Object Storage)"
---
# Batch Expiration

New in version MinIO: RELEASE.2023-12-02T10-51-33Z

The MinIO Batch Framework allows you to create, manage, monitor, and execute jobs using a YAML-formatted job definition file (a “batch file”).
The batch jobs run directly on the MinIO deployment to take advantage of the server-side processing power without constraints of the local machine where you run the [[reference/minio-mc#minio-client|MinIO Client]].

The `expire` batch job applies [[administration/object-management/create-lifecycle-management-expiration-rule#minio-lifecycle-management-create-expiry-rule|Automatic Object Expiration]] behavior to a single bucket.
The job determines expiration eligibility based on the provided configuration, independent of any configured expiration rules.

## Behavior

### Immediate Expiration of Objects

Batch expiration occurs immediately as part of the batch job, as compared to the [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|passive scanner-based application of expiration rules]].
Specifically, batch expiration does not yield to application I/O and may impact performance of regular read/write operations on the deployment.

### Expiration Eligibility Determined at Batch-Run

The batch expiration works per-bucket and runs once to completion.
The job determines expiration eligibility at the time the job runs, and does not rescan or recheck for new objects periodically.

To capture any new objects eligible for expiration, re-run the batch job.

### Expiry Rules Check Latest Object Only

The batch expiration job only checks the latest or “current” version of each object against each batch expiration rule.

## Expire Batch Job Reference

Field

Description

`expire`

Required

Top-level field for the expiration job type.

`apiVersion`

Required

Set to `v1`.

`bucket`

Required

Specify the name of the bucket in which the job runs.

`prefix`

Optional

Specify the bucket prefix in which the job runs.

`rules`

Required

An array of one or more expiration rules to apply to objects in the specified `bucket` and `prefix` (if any).

`rules.[n].type`

Required

Supports one of the following two values:

`object` - Applies only to objects which do not have a `DeleteMarker` as the current version.

`deleted` - Applies only to objects which do Have a `DeleteMarker` as the current version.

See [[administration/object-management/object-delete#minio-object-delete|Object Deletion]] for more complete documentation on `DeleteMarker` or delete operations in versioned buckets.

`rules.[n].name`

Optional

Specify a match string to use for filtering objects.

Supports glob-style wildcards (`*`, `?`).

`rules.[n].olderThan`

Optional

Specify the age of objects for filtering objects.
The rule applies to only those objects older than the specified unit of time.

For example, `72h` or `3d` selects objects older than three days.

`rules.[n].createdBefore`

Optional

Specify an [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339.html) date and time for filtering objects.

The rule applies to only those objects created before the specified timestamp.

`rules.[n].tags`

Optional

Specify an array of key-value pairs describing object tags to use for filtering objects.
The `value` entry supports glob-style wildcards (`*`, `?`).

For example, the following filters the rule to only objects with matching tags:

```
tags:
  - key: archive
    value: True
```

This key is incompatible with `rules.[n].type: deleted`.

`rules.[n].metadata`

Optional

Specify an array of key-value pairs describing object metadata to use for filtering objects.
The `value` key supports glob-style wildcards (`*`, `?`).

For example, the following filters the rule to only objects with matching metadata:

```
metadata:
  - key: content-type
    value: image/*
```

This key is incompatible with `rules.[n].type: deleted`.

`rules.[n].size`

Optional

Specify the range of object sizes for filtering objects.

`lessThan` - matches objects with size less than the specified amount (e.g. `MiB`, `GiB`).

`greaterThan` - matches objects with size greater than the specified amount (e.g. `MiB`, `GiB`).

`rules.[n].purge.retainVersions`

Optional

Specify the number of object versions to retain when applying expiration.

Defaults to `0` for deleting all object versions (fastest).

`notify.endpoint`

Optional

The predefined endpoint to send events for notifications.

`notify.token`

Optional

An optional JSON Web Token (JWT) to access the `notify.endpoint`.

`retry.attempts`

Optional

The number of tries to complete the batch job before giving up.

`retry.delay`

Optional

The amount of time to wait between each attempt (`ms`).

## Sample YAML Description for an expire Job Type

Use [[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]] to create a basic `expire` batch job for further customization.

```
expire:
  apiVersion: v1
  bucket: mybucket # Bucket where this job will expire matching objects from
  prefix: myprefix # (Optional) Prefix under which this job will expire objects matching the rules below.
  rules:
    - type: object  # objects with zero ore more older versions
      name: NAME # match object names that satisfy the wildcard expression.
      olderThan: 70h # match objects older than this value
      createdBefore: "2006-01-02T15:04:05.00Z" # match objects created before this date and time in RFC3339 format
      tags:
        - key: name
          value: pick* # match objects with tag 'name', all values starting with 'pick'
      metadata:
        - key: content-type
          value: image/* # match objects with 'content-type', all values starting with 'image/'
      size:
        lessThan: 10MiB # match objects with size less than this value (e.g. 10MiB)
        greaterThan: 1MiB # match objects with size greater than this value (e.g. 1MiB)
      purge:
          # retainVersions: 0 # (default) delete all versions of the object. This option is the fastest.
          # retainVersions: 5 # keep the latest 5 versions of the object.

    - type: deleted # objects with delete marker as their latest version
      name: NAME # match object names that satisfy the wildcard expression.
      olderThan: 10h # match objects older than this value (e.g. 7d10h31s)
      createdBefore: "2006-01-02T15:04:05.00Z" # match objects created before this date and time in RFC3339 format
      purge:
          # retainVersions: 0 # (default) delete all versions of the object. This option is the fastest.
          # retainVersions: 5 # keep the latest 5 versions of the object including delete markers.

  notify:
    endpoint: https://notify.endpoint # notification endpoint to receive job completion status
    token: Bearer xxxxx # optional authentication token for the notification endpoint

  retry:
    attempts: 10 # number of retries for the job before giving up
    delay: 500ms # least amount of delay between each retry
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).