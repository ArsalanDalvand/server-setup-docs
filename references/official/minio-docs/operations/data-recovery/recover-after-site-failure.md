---
title: "Site Failure Recovery — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/data-recovery/recover-after-site-failure.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/data-recovery/recover-after-site-failure"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Site Failure Recovery — SILO (MinIO Object Storage)"
---
# Site Failure Recovery

MinIO can make the loss of an entire site, while significant, a relatively minor incident.
Site recovery depends on the replication option you use for the site.

Site Replication

Total restoration of IAM configurations, bucket configurations, and data from the healthy peer site(s)

Bucket Replication

Data restoration of objects and metadata from a healthy remote location for each bucket configured for replication

[[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]]

Data restoration of objects only from a healthy remote location with no versioning

Site replication healing automatically adds IAM settings, buckets, bucket configurations, and objects from the existing site(s) to the new site with no further action required.

You cannot configure site replication if any bucket replication rules remain in place on other healthy sites.
Bucket replication is mutually exclusive with site replication.

If you are switching from using bucket replication to using site replication, you must first remove all bucket replication rules from the healthy site prior to setting up site replication.

## Restore an Unhealthy Peer to Site Replication

Important

The [RELEASE.2023-01-02T09-40-09Z](https://github.com/minio/minio/releases/tag/RELEASE.2023-01-02T09-40-09Z) MinIO server release includes important fixes for removing a downed site in replication configurations containing three or more peer sites.

For deployments configured for site replication, plan to [[operations/deployments/baremetal-upgrade-minio-deployment#minio-upgrade|test and upgrade]] all peer sites to the specified release.
In the event of a site failure, you can update the remaining healthy sites to the specified version and use this procedure.

[[operations/replication/multi-site-replication#minio-site-replication-overview|Site replication]] keeps two or more MinIO deployments in sync with IAM policies, buckets, bucket configurations, objects, and object metadata.
If a peer site fails, such as due to a major disaster or long power outage, you can use the remaining healthy site(s) to restore the [[operations/replication/multi-site-replication#minio-site-replication-what-replicates|replicable data]].

The following procedure can restore data in scenarios where [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]] was active prior to the site loss.
This procedure assumes a total loss of one or more peer sites versus replication lag or delays due to latency or transient deployment downtime.

Remove the failed site from the MinIO site replication configuration using the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.rm|`mc admin replicate rm`]] command with the `--force` option.

The following command force-removes an unhealthy peer site from the replication configuration:

```
mc admin replicate rm HEALTHY_PEER UNHEALTHY_PEER --force
```

Replace `HEALTHY_PEER` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of any healthy peer in the replication configuration

Replace `UNHEALTHY_PEER` with the alias of the unhealthy peer site

All healthy peers in the site replication configuration update to remove the unhealthy peer automatically.
You can use the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.info|`mc admin replicate info`]] command to verify the new site replication configuration.

Deploy a new MinIO site following the [[operations/replication/multi-site-replication#minio-expand-site-replication|site replication requirements]].

Do not upload any data or otherwise configure the deployment beyond the stated requirements.

Validate that the new MinIO deployment functions normally and has bidirectional connectivity to the other peer sites.

Ensure the new site matches the server version on the existing peer sites

Warning

The [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.rm.-force|`mc admin replicate rm --force`]] command only operates on the online or healthy nodes in the site replication configuration.
The removed offline MinIO deployment retains its original replication configuration, such that if the deployment resumes normal operations it would continue replication operations to its configured peer sites.

If you plan to re-use the hardware for the site replication configuration, you must completely wipe the drives for the deployment before re-initializing MinIO and adding the site back to the replication configuration.

[[operations/replication/multi-site-replication#minio-expand-site-replication|Add the replacement peer site]] to the replication configuration.

Use the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.add|`mc admin replicate add`]] command to update the replication configuration with the new site:

```
mc admin replicate add HEALTHY_PEER NEW_PEER
```

Replace `HEALTHY_PEER` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of any healthy peer in the replication configuration

Replace `NEW_PEER` with the alias of the new peer

All healthy peers in the site replication configuration update for the new peer automatically.
You can use the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.info|`mc admin replicate info`]] command to verify the new site replication configuration.

Resynchronize the new peer with [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.resync|`mc admin replicate resync`]].

```
mc admin replicate resync start HEALTHY_PEER NEW_PEER
```

Replace `HEALTHY_PEER` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of any healthy peer in the replication configuration

Replace `NEW_PEER` with the alias of the new peer

Validate the replication status.

Use the following commands to track the replication status:

[[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.status|`mc admin replicate status`]] - provides overall status and progress of replication

[[reference/minio-mc/mc-replicate-status#command-mc.replicate.status|`mc replicate status`]] - provides bucket-level and global replication status

## Active Bucket Replication Resynchronization

For scenarios where [[administration/bucket-replication#minio-bucket-replication|bucket replication]] was in place prior to the failure, you can use [[reference/minio-mc/mc-replicate-resync#command-mc.replicate.resync|`mc replicate resync`]] to restore data to a new site.
Create a new site to replace the failed deployment, then synchronize the data from an existing, healthy, bucket replication-enabled deployment to the new site.

Deploy a new MinIO site.

Set up IAM and users as needed.

On the site with data, create a new `remote target` using the [[reference/deprecated/mc-admin-bucket-remote#mc.admin.bucket.remote.add|`mc admin bucket remote add`]] command and record the ARN from the output.

From the site with the data, use the [[reference/minio-mc/mc-replicate-resync#mc.replicate.resync.start|`mc replicate resync start`]] command with the ARN from the previous command to rebuild the bucket on the new site.

Wait for re-synchronization to complete (use [[reference/minio-mc/mc-replicate-resync#mc.replicate.resync.status|`mc replicate resync status`]] to check).

Set up bucket replication rule(s) from the new MinIO site to the existing target bucket(s).

(Optional) Delete the bucket replication rules from the target deployment(s) to restore an active-passive replication scenario.

## Passive Bucket Replication Resynchronization

[[administration/bucket-replication#minio-bucket-replication|Bucket replication]] can directly restore the site contents by performing a replication from the target bucket(s) to a new MinIO site.

As a passive process, bucket replication may not perform as quickly as desired for a site recovery scenario.

Bucket replication relies on the standard replication [[operations/concepts/scanner#minio-concepts-scanner|scanner]] queue, which does not take priority over other processes.
For recovery procedures with stricter SLA/SLO, use the active bucket replication process with [[reference/minio-mc/mc-replicate-resync#command-mc.replicate.resync|`mc replicate resync`]] command as described above.

Bucket replication rules copy the object, its version ID, versions, and other metadata to the target bucket.
MinIO can restore the object with all of these attributes to a new MinIO site if bucket replication had already been in use prior to the site loss.

Deploy a new MinIO site.

Set up IAM and users as needed.

On the remaining target bucket deployment(s), create bucket replication rule(s) for each bucket to the new MinIO site.

Wait for replication to complete.

Set up bucket replication rule(s) from the new MinIO site to the existing target bucket(s).

(Optional) Delete the bucket replication rules from the target deployment(s) to restore an active-passive replication scenario.

Do not delete the bucket replication rules from the deployments used to recover data if you prefer to keep an active-active replication between the buckets.
In active-active replication, changes to the objects at either location affect the objects at the other location.

## Mirroring

MinIO’s mirroring copies an object from any S3 compatible storage system.

Mirroring only copies the latest version of each object and does not include versioning metadata, regardless of the source.
You cannot restore those attributes with this method.

Use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]] in situations where you need to restore only the latest version of an object.
Use bucket replication or site replication where those methods were already in use if you are copying from another MinIO deployment and wish to restore the object’s version history and version metadata.

Deploy a new MinIO site.

Set up IAM and users as needed.

Create buckets on the new site.

Use the [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] CLI command to copy the contents from the mirror location to the new MinIO site.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).