---
title: "Batch Replication — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/batch-framework-job-replicate.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/batch-framework-job-replicate"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Batch Replication — SILO (MinIO Object Storage)"
---
# Batch Replication

New in version MinIO: RELEASE.2022-10-08T20-11-00Z

The Batch Framework was introduced with the `replicate` job type in the [[reference/minio-mc#command-mc|`mc`]] [RELEASE.2022-10-08T20-11-00Z](https://github.com/minio/mc/releases/tag/RELEASE.2022-10-08T20-11-00Z).

The MinIO Batch Framework allows you to create, manage, monitor, and execute jobs using a YAML-formatted job definition file (a “batch file”).
The batch jobs run directly on the MinIO deployment to take advantage of the server-side processing power without constraints of the local machine where you run the [[reference/minio-mc#minio-client|MinIO Client]].

The `replicate` batch job replicates objects from one MinIO deployment (the `source` deployment) to another MinIO deployment (the `target` deployment).
Either the `source` or the `target` must be the [[administration/batch-framework#minio-batch-local|local]] deployment.

Batch Replication between MinIO deployments have the following advantages over using [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]]:

Removes the client to cluster network as a potential bottleneck

A user only needs access to starting a batch job with no other permissions, as the job runs entirely server side on the cluster

The job provides for retry attempts in event that objects do not replicate

Batch jobs are one-time, curated processes allowing for fine control replication

(MinIO to MinIO only) The replication process copies object versions from source to target

Starting with the MinIO Server `RELEASE.2023-05-04T21-44-30Z`, the other deployment can be either another MinIO deployment or any S3-compatible location using a realtime storage class.
Use filtering options in the replication `YAML` file to exclude objects stored in locations that require rehydration or other restoration methods before serving the requested object.
Batch replication to these types of remotes uses `mc mirror` behavior.

## Behavior

### Access Control and Requirements

Batch replication shares similar access and permission requirements as [[administration/bucket-replication/bucket-replication-requirements#minio-bucket-replication-requirements|bucket replication]].

The credentials for the “source” deployment must have a policy similar to the following:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "admin:SetBucketTarget",
                "admin:GetBucketTarget",
                "admin:ListBatchJobs",
                "admin:DescribeBatchJob",
                "admin:StartBatchJob",
                "admin:CancelBatchJob"
            ],
            "Effect": "Allow",
            "Sid": "EnableRemoteBucketConfiguration"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ListBucket",
                "s3:ListBucketMultipartUploads",
                "s3:GetBucketLocation",
                "s3:GetBucketVersioning",
                "s3:GetObjectRetention",
                "s3:GetObjectLegalHold",
                "s3:PutReplicationConfiguration"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ],
            "Sid": "EnableReplicationRuleConfiguration"
        }
    ]
}
```

The credentials for the “remote” deployment must have a policy similar to the following:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ListBucket",
                "s3:ListBucketMultipartUploads",
                "s3:GetBucketLocation",
                "s3:GetBucketVersioning",
                "s3:GetBucketObjectLockConfiguration",
                "s3:GetEncryptionConfiguration"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ],
            "Sid": "EnableReplicationOnBucket"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ReplicateTags",
                "s3:AbortMultipartUpload",
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:GetObjectVersionTagging",
                "s3:PutObject",
                "s3:PutObjectRetention",
                "s3:PutBucketObjectLockConfiguration",
                "s3:PutObjectLegalHold",
                "s3:DeleteObject",
                "s3:ReplicateObject",
                "s3:ReplicateDelete"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ],
            "Sid": "EnableReplicatingDataIntoBucket"
        }
    ]
}
```

See [[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]], [[reference/minio-mc-admin/mc-admin-user-svcacct#command-mc.admin.user.svcacct|`mc admin user svcacct`]], and [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] for more complete documentation on adding users, access keys, and policies to a MinIO deployment.

MinIO deployments configured for [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP]] or [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect]] user management can instead create dedicated [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]] for supporting batch replication.

### Filter Replication Targets

The batch job definition file can limit the replication by bucket, prefix, and/or filters to only replicate certain objects.
The access to objects and buckets for the replication process may be restricted by the credentials you provide in the YAML for either the source or target destinations.

Changed in version MinIO: Server RELEASE.2023-04-07T05-28-58Z

You can replicate from a remote MinIO deployment to the local deployment that runs the batch job.

For example, you can use a batch job to perform a one-time replication sync to push objects from a bucket on a local deployment at `minio-local/invoices/` to a bucket on a remote deployment at `minio-remote/invoices`.
You can also pull objects from the remote deployment at `minio-remote/invoices` to the local deployment at `minio-local/invoices`.

### Small File Optimization

Starting with [RELEASE.2023-12-09T18-17-51Z](https://github.com/minio/minio/releases/tag/RELEASE.2023-12-09T18-17-51Z), batch replication by default automatically batches and compresses objects smaller than 5MiB to efficiently transfer data between the source and remote.
The remote MinIO deployment can check and immediately apply lifecycle management tiering rules to batched objects.
The functionality resembles that offered by S3 Snowball Edge small file batching.

You can modify the compression settings in the [[reference/minio-mc/mc-batch-generate#minio-batch-job-types|replicate]] job configuration.

## Replicate Batch Job Reference

The YAML must define the source and target deployments.
If the source deployment is remote, then the target deployment must be `local`.
Optionally, the YAML can also define flags to filter which objects replicate, send notifications for the job, or define retry attempts for the job.

Changed in version MinIO: RELEASE.2023-04-07T05-28-58Z

You can replicate from a remote MinIO deployment to the local deployment that runs the batch job.

Changed in version MinIO: RELEASE.2024-08-03T04-33-23Z

This release introduces a new version of the Batch Job Replicate API, `v2`.
The updated API allows you to list multiple prefixes on the source to replicate from.
To replicate multiple prefixes from a source, specify `replicate.apiVersion` as `v2`.

```
replicate:
  apiVersion: v1
  source:
    type: minio
    bucket: mybucket
    prefix:
      - prefix1
      - prefix2
...
```

For the source deployment

Required information

`type:`

Must be `minio`.

`bucket:`

The bucket on the deployment.

Optional information

`prefix:`

The prefix on the object(s) that should replicate.
Beginning with MinIO Server `RELEASE.2024-08-03T04-33-23Z`, v2 of the Batch Job Replicate API allows you to list multiple prefixes.
Specify `replicate.apiVersion` as `v2` to replicate from multiple prefixes.

`endpoint:`

Location of the deployment to use for either the source or the target of a replication batch job.
For example, `https://minio.example.net`.

If the deployment is the [[reference/minio-mc/mc-alias-set#alias|mc alias set]] specified to the command, omit this field to direct MinIO to use that alias for the endpoint and credentials values.
Either the source deployment or the remote deployment must be the [[administration/batch-framework#minio-batch-local|“local”]] alias.
The non-“local” deployment must specify the `endpoint` and `credentials`.

`path:`

Directs MinIO to use Path or Virtual Style (DNS) lookup of the bucket.

- Specify `on` for Path style
- Specify `off` for Virtual style
- Specify `auto` to let MinIO determine the correct lookup style.

Defaults to `auto`.

`credentials:`

The `accesskey:` and `secretKey:` or the `sessionToken:` that grants access to the object(s).
Only specify for the deployment that is not the [[administration/batch-framework#minio-batch-local|local]] deployment.

`snowball`

version added: RELEASE.2023-12-09T18-17-51Z

Configuration options for controlling the batch-and-compress functionality.

`snowball.disable`

Specify `true` to disable the batch-and-compress functionality during replication.
Defaults to `false`.

`snowball.batch`

Specify the maximum integer number of objects to batch for compression.
Defaults to `100`.

`snowball.inmemory`

Specify `false` to stage archives using local storage or `true` to stage to memory (RAM).
Defaults to `true`.

`snowball.compress`

Specify `true` to generate compress batched objects over the wire using the [S2/Snappy compression algorithm](https://en.wikipedia.org/wiki/Snappy_(compression)).
Defaults to `false` or no compression.

`snowball.smallerThan`

Specify the size of object in Megabits (MiB) under which MinIO should batch objects.
Defaults to `5MiB`.

`snowball.skipErrs`

Specify `false` to direct MinIO to halt on any object which produces errors on read.
Defaults to `true`.

For the target deployment

Required information

`type:`

Must be `minio`.

`bucket:`

The bucket on the deployment.

Optional information

`prefix:`

The prefix on the object(s) to replicate.

`endpoint:`

The location of the target deployment.

If the target is the [[reference/minio-mc/mc-alias-set#alias|alias]] specified to the command, you can omit this and the `credentials` fields.
If the target is “local”, the source must specify the remote deployment with `endpoint` and `credentials`.

`credentials:`

The `accesskey` and `secretKey` or the `sessionToken` that grants access to the object(s).

For filters

`newerThan:`

A string representing a length of time in `#d#h#s` format.

Only objects newer than the specified length of time replicate.
For example, `7d`, `24h`, `5d12h30s` are valid strings.

`olderThan:`

A string representing a length of time in `#d#h#s` format.

Only objects older than the specified length of time replicate.

`createdAfter:`

A date in `YYYY-MM-DDTHH:MM:SSZ` [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339.html)  date and time format.

Only objects created after the date replicate.

`createdBefore:`

A date in `YYYY-MM-DDTHH:MM:SSZ` [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339.html)  date and time format.

Only objects created prior to the date replicate.

For notifications

`endpoint:`

The predefined endpoint to send events for notifications.

`token:`

An optional JWT <JSON Web Token> to access the `endpoint`.

For retry attempts

If something interrupts the job, you can define how many attempts to retry the job batch.
For each retry, you can also define how long to wait between attempts.

`attempts:`

Number of tries to complete the batch job before giving up.

`delay:`

The least amount of time to wait between each attempt.

## Sample YAML Description File for a replicate Job Type

Use [[reference/minio-mc/mc-batch-generate#command-mc.batch.generate|`mc batch generate`]] to create a basic `replicate` batch job for further customization.

For the [[administration/batch-framework#minio-batch-local|local]] deployment, do not specify the endpoint or credentials.
Either delete or comment out those lines for the source or the target section, depending on which is the `local`.

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

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).