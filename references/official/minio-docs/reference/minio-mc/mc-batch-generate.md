---
title: "mc batch generate — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-batch-generate.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-batch-generate"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc batch generate — SILO (MinIO Object Storage)"
---
# mc batch generate

Changed in version MinIO: RELEASE.2022-10-08T20-11-00Z or later

## Syntax

The [`mc batch generate`](#command-mc.batch.generate) command creates a basic YAML-formatted template file for the specified job type.

After MinIO creates the file, open it in your preferred text editor tool to further customize.
You can define one job task definition per batch file.

See [job types](#minio-batch-job-types) for the supported jobs you can generate.

EXAMPLE

The following command creates a basic YAML file for a replicate job on the `mybucket` bucket of the `myminio` alias.

```
mc batch generate myminio replicate
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] batch generate \
                       ALIAS   \
                       JOBTYPE
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.batch.generate.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] used to generate the YAML template file.
The specified `alias` does not restrict the deployment(s) where you can use the generated file.

For example:

```
mc batch generate myminio replicate
```

JOBTYPE[](#mc.batch.generate.JOBTYPE)
Required

The type of job to generate a YAML document for.

Supports the following values:

[replicate](#minio-mc-batch-generate-replicate-job)

[keyrotate](#minio-mc-batch-generate-keyrotate-job)

[expire](#minio-mc-batch-generate-expire-job) (Added `mc.RELEASE.2023-12-02T11-24-10Z`)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Generate a yaml File for a Replicate Job Type

The following command generates a YAML blueprint for a replicate type batch job and names the file `replicate` with the `.yaml` extension:

```
mc batch generate alias replicate > replicate.yaml
```

Replace `alias` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] to use to generate the yaml file.

Replace `replicate` with the type of job to generate a yaml file for.

:mc:`mc batch` supports the `replicate` and `keyrotate` job types.

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Job Types

[[reference/minio-mc/mc-batch#command-mc.batch|`mc batch`]] currently supports the following job task types:

[replicate](#minio-mc-batch-generate-replicate-job)

Replicate objects between two MinIO deployments.
Provides similar functionality to [[administration/bucket-replication#minio-bucket-replication|bucket replication]] as a batch job rather than continual scanning function.

[keyrotate](#minio-mc-batch-generate-keyrotate-job)

New in version MinIO: RELEASE.2023-04-07T05-28-58Z

Rotate the sse-s3 or sse-kms keys for objects at rest on a MinIO deployment.

[expire](#minio-mc-batch-generate-expire-job)

New in version MinIO: RELEASE.2023-12-02T10-51-33Z

Expire objects based using similar semantics as [[administration/object-management/create-lifecycle-management-expiration-rule#minio-lifecycle-management-create-expiry-rule|Automatic Object Expiration]].

### replicate

You can use the following example configuration as the starting point for building your own custom replication batch job:

```
replicate:
  apiVersion: v1
  # source of the objects to be replicated
  source:
    type: TYPE # valid values are "s3" or "minio"
    bucket: BUCKET
    prefix: PREFIX # 'PREFIX' is optional
    # If your source is the 'local' alias specified to 'mc batch start', then the 'endpoint' and 'credentials' fields are optional and can be omitted
    # Either the 'source' or 'remote' *must* be the "local" deployment
    endpoint: "http[s]://HOSTNAME:PORT" 
    # path: "on|off|auto" # "on" enables path-style bucket lookup. "off" enables virtual host (DNS)-style bucket lookup. Defaults to "auto"
    credentials:
      accessKey: ACCESS-KEY # Required
      secretKey: SECRET-KEY # Required
    # sessionToken: SESSION-TOKEN # Optional only available when rotating credentials are used
    snowball: # automatically activated if the source is local
      disable: false # optionally turn-off snowball archive transfer
      batch: 100 # upto this many objects per archive
      inmemory: true # indicates if the archive must be staged locally or in-memory
      compress: false # S2/Snappy compressed archive
      smallerThan: 5MiB # create archive for all objects smaller than 5MiB
      skipErrs: false # skips any source side read() errors

  # target where the objects must be replicated
  target:
    type: TYPE # valid values are "s3" or "minio"
    bucket: BUCKET
    prefix: PREFIX # 'PREFIX' is optional
    # If your source is the 'local' alias specified to 'mc batch start', then the 'endpoint' and 'credentials' fields are optional and can be omitted

    # Either the 'source' or 'remote' *must* be the "local" deployment
    endpoint: "http[s]://HOSTNAME:PORT"
    # path: "on|off|auto" # "on" enables path-style bucket lookup. "off" enables virtual host (DNS)-style bucket lookup. Defaults to "auto"
    credentials:
      accessKey: ACCESS-KEY
      secretKey: SECRET-KEY
    # sessionToken: SESSION-TOKEN # Optional only available when rotating credentials are used

  # NOTE: All flags are optional
  # - filtering criteria only applies for all source objects match the criteria
  # - configurable notification endpoints
  # - configurable retries for the job (each retry skips successfully previously replaced objects)
  flags:
    filter:
      newerThan: "7d" # match objects newer than this value (e.g. 7d10h31s)
      olderThan: "7d" # match objects older than this value (e.g. 7d10h31s)
      createdAfter: "datetime" # match objects created after this date and time in RFC3339 format
      createdBefore: "datetime" # match objects created before this date and time in RFC3339 format

      ## NOTE: tags are not supported when "source" is remote.
      # tags:
      #   - key: "name"
      #     value: "pick*" # match objects with tag 'name', with all values starting with 'pick'

      # metadata:
      #   - key: "content-type"
      #     value: "image/*" # match objects with 'content-type', with all values starting with 'image/'

    notify:
      endpoint: "https://notify.endpoint" # notification endpoint to receive job status events
      token: "Bearer xxxxx" # optional authentication token for the notification endpoint

    retry:
      attempts: 10 # number of retries for the job before giving up
      delay: "500ms" # least amount of delay between each retry
```

See [[administration/batch-framework-job-replicate#minio-batch-framework-replicate-job-ref|Replicate Batch Job Reference]] for more complete documentation on each key.

### keyrotate

You can use the following example configuration as the starting point for building your own custom key rotation batch job:

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

See [[administration/batch-framework-job-keyrotate#minio-batch-framework-keyrotate-job-ref|Key Rotate Batch Job Reference]] for more complete documentation on each key.

### expire

You can use the following example configuration as a starting point for building your own custom expiration batch job:

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

See [[administration/batch-framework-job-expire#minio-batch-framework-expire-job-ref|Expire Batch Job Reference]] for more complete documentation on each key.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).