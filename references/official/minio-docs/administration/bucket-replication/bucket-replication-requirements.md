---
title: "Requirements to Set Up Bucket Replication — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/bucket-replication/bucket-replication-requirements.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/bucket-replication/bucket-replication-requirements"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Requirements to Set Up Bucket Replication — SILO (MinIO Object Storage)"
---
# Requirements to Set Up Bucket Replication

Bucket replication uses rules to synchronize the contents of a bucket on one MinIO deployment to a bucket on a remote MinIO deployment.

Replication can be done in any of the following ways:

[[administration/bucket-replication/enable-server-side-one-way-bucket-replication#minio-bucket-replication-serverside-oneway|Active-Passive]]
Eligible objects replicate from the source bucket to the remote bucket.
Any changes on the remote bucket do not replicate back.

[[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|Active-Active]]
Changes to eligible objects of either bucket replicate to the other bucket in a two-way direction.

[[administration/bucket-replication/enable-server-side-multi-site-bucket-replication#minio-bucket-replication-serverside-multi|Multi-Site Active-Active]]
Changes to eligible objects on any bucket set up for bucket replication replicate to all of the other buckets.

Ensure you meet the following prerequisites before you set up any of these replication configurations.

## Permissions Required for Setting Up Bucket Replication

Bucket replication requires specific permissions on the source and destination deployments to configure and enable replication rules.

Replication Admin

The following policy provides permissions for configuring and enabling replication on a deployment.

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

The `"EnableRemoteBucketConfiguration"` statement grants permission for creating a remote target for supporting replication.

The `"EnableReplicationRuleConfiguration"` statement grants permission for creating replication rules on a bucket.
The `"arn:aws:s3:::*` resource applies the replication permissions to any bucket on the source deployment.
You can restrict the user policy to specific buckets as-needed.

The following code creates a [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed user]] with the necessary policy. Replace the `TARGET`  with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment on which you are configuring replication:

```
wget -O - https://silo.pigsty.io/examples/ReplicationAdminPolicy.json | \
mc admin policy create TARGET ReplicationAdminPolicy /dev/stdin
mc admin user add TARGET ReplicationAdmin LongRandomSecretKey
mc admin policy attach TARGET ReplicationAdminPolicy --user=ReplicationAdmin
```

MinIO deployments configured for [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP]] or [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect]] user management should instead create a dedicated [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]] for bucket replication.

Replication Remote User

The following policy provides permissions for enabling synchronization of replicated data into the deployment.

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

The `"EnableReplicationOnBucket"` statement grants permission for a remote target to retrieve bucket-level configuration for supporting replication operations on all buckets in the MinIO deployment.
To restrict the policy to specific buckets, specify those buckets as an element in the `Resource` array similar to `"arn:aws:s3:::bucketName"`.

The `"EnableReplicatingDataIntoBucket"` statement grants permission for a remote target to synchronize data into any bucket in the MinIO deployment.
To restrict the policy to specific buckets, specify those buckets as an element in the `Resource` array similar to `"arn:aws:s3:::bucketName/*"`.

The following code creates a [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed user]] with the necessary policy.
Replace `TARGET`  with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment on which you are configuring replication:

```
wget -O - https://silo.pigsty.io/examples/ReplicationRemoteUserPolicy.json | \
mc admin policy create TARGET ReplicationRemoteUserPolicy /dev/stdin
mc admin user add TARGET ReplicationRemoteUser LongRandomSecretKey
mc admin policy attach TARGET ReplicationRemoteUserPolicy --user=ReplicationRemoteUser
```

MinIO deployments configured for [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP]] or [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect]] user management should instead create a dedicated [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]] for bucket replication.

See [[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]], [[reference/minio-mc-admin/mc-admin-user-svcacct#command-mc.admin.user.svcacct|`mc admin user svcacct`]], and [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] for more complete documentation on adding users, access keys, and policies to a MinIO deployment.

## Matching Object Encryption Settings for Bucket Replication

MinIO supports replication of objects encrypted using [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]] and [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]]:

For objects encrypted using SSE-KMS, MinIO requires that the target bucket support SSE-KMS encryption of objects using the same key names used to encrypt objects on the source bucket.

For objects encrypted using [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]], MinIO requires that the target bucket also support SSE-S3 encryption of objects regardless of key name.

As part of the replication process, MinIO decrypts the object on the source bucket and transmits the unencrypted object over the network.
The destination MinIO deployment then re-encrypts the object using the encryption settings from the target.
MinIO therefore strongly recommends [[operations/network-encryption#minio-tls|enabling TLS]] on both source and destination deployments to ensure the safety of objects during transmission.

MinIO does not support replicating client-side encrypted objects (SSE-C).

## Bucket Replication Requires MinIO Deployments

MinIO server-side replication only works between MinIO deployments.
Both the source and destination deployments must run MinIO Server with matching versions.

To configure replication between arbitrary S3-compatible services, use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].

## Replication Requires Versioning

MinIO relies on the immutability protections provided by [[administration/object-management/object-versioning#minio-bucket-versioning|versioning]] to support replication and resynchronization.

Use [[reference/minio-mc/mc-version-info#command-mc.version.info|`mc version info`]] to validate the versioning status of both the source and remote buckets.
Use the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command to enable versioning as necessary.

If you exclude a prefix or folder from versioning within the source bucket, MinIO cannot replicate objects within that folder or prefix.

## Matching Object Locking State With Bucket Replication

MinIO supports replicating objects held under [[administration/object-management/object-retention#minio-object-locking|WORM Locking]].
Both replication buckets must have object locking enabled for MinIO to replicate the locked object.
For active-active configuration, MinIO recommends using the same retention rules on both buckets to ensure consistent behavior across sites.

You must enable object locking during bucket creation as per S3 behavior.
You can then configure object retention rules at any time.
Configure the necessary rules on the unhealthy target bucket prior to beginning this procedure.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).