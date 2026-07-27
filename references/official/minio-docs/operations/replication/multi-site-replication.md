---
title: "Site Replication Overview — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/replication/multi-site-replication.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/replication/multi-site-replication"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Site Replication Overview — SILO (MinIO Object Storage)"
---
# Site Replication Overview

Site replication configures multiple independent MinIO deployments as a cluster of replicas called peer sites.

A site replication deployment with two peer sites.
A load balancer manages routing operations to either of the two sites.
Data written to one site automatically replicates to the other peer site.[](#id1)

Site replication assumes the use of either the included MinIO identity provider (IDP) or an external IDP.
All configured deployments must use the same IDP.
Deployments using an external IDP must use the same configuration across sites.

For more information on site replication architecture and deployment concepts, see [[operations/concepts/architecture#minio-deployment-architecture-replicated|Deployment Architecture: Replicated MinIO Deployments]].

MinIO does not recommend using MacOS, Windows, or non-orchestrated Containerized deployments for site replication outside of early development, evaluation, or general experimentation.
For production, use [Linux](https://silo.pigsty.io/minio/linux/operations/install-deploy-manage/multi-site-replication.html) or [Kubernetes](https://silo.pigsty.io/minio/kubernetes/upstream/operations/install-deploy-manage/multi-site-replication.html)

## Overview

### What Replicates Across All Sites

Each MinIO deployment (“peer site”) synchronizes the following changes across the other peer sites:

Creation, modification, and deletion of buckets and objects, including

Bucket and Object Configurations

[[administration/identity-access-management/policy-based-access-control#minio-policy|Policies]]

[[reference/minio-mc/mc-tag-set#command-mc.tag.set|`mc tag set`]]

[[administration/object-management/object-retention#minio-object-locking|Locks]], including retention and legal hold configurations

[[administration/server-side-encryption#minio-encryption-overview|Encryption settings]]

Creation and deletion of IAM users, groups, policies, and policy mappings to users or groups (for LDAP users or groups)

Creation of Security Token Service (STS) credentials for session tokens verifiable from the local `root` credentials

Creation and deletion of [[reference/minio-mc-admin/mc-admin-user-svcacct#minio-mc-admin-user-svcacct|access keys]] (except those owned by the `root` user)

Site replication enables [[administration/object-management/object-versioning#minio-bucket-versioning|bucket versioning]] for all new and existing buckets on all replicated sites.

New in version mc: RELEASE.2023-12-02T02-03-28Z

You can choose to replicate ILM expiration rules across peer sites.
For new site replication configurations, use the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.add|`mc admin replicate add`]]  with the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.add.-replicate-ilm-expiry|`--replicate-ilm-expiry`]] flag.
For existing site replication configurations, you can enable or disable the behavior using [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.update|`mc admin replicate update`]] with either the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.update.-enable-ilm-expiry-replication|`--enable-ilm-expiry-replication`]] or [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.update.-disable-ilm-expiry-replication|`--disable-ilm-expiry-replication`]] flag, as appropriate.

### What Does Not Replicate Across Sites

MinIO deployments in a site replication configuration do not replicate the creation or modification of the following items:

[[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket notifications]]

[[administration/object-management/object-lifecycle-management#minio-lifecycle-management|Lifecycle management (ILM) configurations]]

[[reference/minio-mc-admin/mc-admin-config#minio-mc-admin-config|Site configuration settings]]

### Initial Site Replication Process

After enabling site replication, identity and access management (IAM) settings sync in the following order:

MinIO IDP

Policies

User accounts (for local users)

Groups

Access Keys

Access Keys for `root` do not sync.

Policy mapping for synced user accounts

Policy mapping for [[developers/security-token-service#minio-security-token-service|Security Token Service (STS) users]]

OIDC

Policies

Access Keys associated to OIDC accounts with a valid [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy]]. `root` access keys do not sync.

Policy mapping for synced user accounts

Policy mapping for [[developers/security-token-service#minio-security-token-service|Security Token Service (STS) users]]

LDAP

Policies

Groups

Access Keys associated to LDAP accounts with a valid [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy]]. `root` access keys do not sync.

Policy mapping for synced user accounts

Policy mapping for [[developers/security-token-service#minio-security-token-service|Security Token Service (STS) users]]

After the initial synchronization of data across peer sites, MinIO continually replicates and synchronizes [replicable data](#minio-site-replication-what-replicates) among all sites as they occur on any site.

### Site Healing

Any MinIO deployment in the site replication configuration can resynchronize damaged [replica-eligible data](#minio-site-replication-what-replicates) from the peer with the most updated (“latest”) version of that data.

Changed in version RELEASE.2023-07-18T17-49-40Z: Site replication operations retry up to three (3) times.

MinIO dequeues replication operations that fail to replicate after three attempts.
The [[operations/concepts/scanner#minio-concepts-scanner|scanner]] picks up those affected objects at a later time and requeues them for replication.

Changed in version RELEASE.2022-08-11T04-37-28Z: Failed or pending replications requeue automatically when performing any `GET` or `HEAD` API method.
For example, using [[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]], [[reference/minio-mc/mc-cat#command-mc.cat|`mc cat`]], or [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] commands after a site comes back online prompts healing to requeue.

Changed in version RELEASE.2022-12-02T23-48-47Z: If one site loses data for any reason, resynchronize the data from another healthy site with [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.resync|`mc admin replicate resync`]].
This launches an active process that resynchronizes the data without waiting for the passive [[operations/concepts/scanner#minio-concepts-scanner|MinIO scanner]] to recognize the missing data.

You can adjust how MinIO balances the scanner performance with read/write operations using either the [[reference/minio-server/settings/core#envvar.MINIO_SCANNER_SPEED|`MINIO_SCANNER_SPEED`]] environment variable or the [[reference/minio-server/settings/core#mc-conf.scanner.speed|`scanner speed`]] configuration setting.

### Synchronous vs Asynchronous Replication

MinIO supports specifying either asynchronous (default) or synchronous replication for a given remote target.

With asynchronous replication, MinIO completes the originating `PUT` operation before placing the object into a [[administration/bucket-replication#minio-replication-process|replication queue]].
The originating client may therefore see a successful `PUT` operation before the object is replicated.
While this may result in stale or missing objects on the remote, it mitigates the risk of slow write operations due to replication load.

With synchronous replication, MinIO attempts to replicate the object prior to completing the originating `PUT` operation.
MinIO returns a successful `PUT` operation whether or not the replication attempt succeeds.
This reduces the risk of slow write operations at a possible cost of stale or missing objects on the remote location.

MinIO strongly recommends using the default asynchronous site replication.
Synchronous site replication performance depends strongly on latency between sites, where higher latency can result in lower PUT performance and replication lag.
To configure synchronous site replication use [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.update|`mc admin replicate update`]] with the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.update.-mode|`--mode`]] option.

### Proxy to Other Sites

MinIO peer sites can proxy `GET/HEAD` requests for an object to other peers to check if it exists.
This allows a site that is healing or lagging behind other peers to still return an object persisted to other sites.

For example:

A client issues `GET("data/invoices/january.xls")` to `Site1`

`Site1` cannot locate the object

`Site1` proxies the request to `Site2`

`Site2` returns the latest version of the requested object

`Site1` returns the proxied object to the client

For `GET/HEAD` requests that do not include a unique version ID, the proxy request returns the latest version of that object on the peer site.
This may result in retrieval of a non-current version of an object, such as if the responding peer site is also experiencing replication lag.

MinIO does not proxy `LIST`, `DELETE`, and `PUT` operations.

## Prerequisites

### Back Up Cluster Settings First

Use the [[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`mc admin cluster bucket export`]] and [[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`mc admin cluster iam export`]] commands to take a snapshot of the bucket metadata and IAM configurations respectively prior to configuring Site Replication.
You can use these snapshots to restore bucket/IAM settings in the event of misconfiguration during site replication configuration.

### One Site with Data at Setup

Only one site can have data at the time of setup.
The other sites must be empty of buckets and objects.

After configuring site replication, any data on the first deployment replicates to the other sites.

### All Sites Must Use the Same IDP

All sites must use the same [[administration/identity-access-management#minio-authentication-and-identity-management|Identity Provider]].
Site replication supports the included MinIO IDP, OIDC, or LDAP.

### All Sites Must use the Same MinIO Server Version

All sites must have a matching and consistent MinIO Server version.
Configuring replication between sites with mismatched MinIO Server versions may result in unexpected or undesired replication behavior.

You should also ensure the [[reference/minio-mc#command-mc|`mc`]] version used to configure replication closely matches the server version.

### Access to the Same Encryption Service

For [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]] or [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]] encryption via Key Management Service (KMS), all sites must have access to a central KMS deployment.

You can achieve this with a central KES server or multiple KES servers (say one per site) connected via a central supported [[administration/server-side-encryption#minio-sse|key vault server]].

### Replication Requires Versioning

Site replication requires [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]] and enables it for all created buckets automatically.
You cannot disable versioning in site replication deployments.

MinIO cannot replicate objects in prefixes in the bucket that you excluded from versioning.

### Load Balancers Installed on Each Site

Specify the URL or IP address of the site’s load balancer, reverse proxy, or similar network control plane component.
Requests are automatically routed to nodes in the deployment.

MinIO recommends against using a single node hostname for a peer site.
This creates a single point of failure: if that node goes offline, replication fails.

### Switch to Site Replication from Bucket Replication

[[administration/bucket-replication#minio-bucket-replication|Bucket replication]] and multi-site replication are mutually exclusive.
You cannot use both replication methods on the same deployments.

If you previously set up bucket replication and wish to now use site replication, you must first delete all of the bucket replication rules on the deployment that has data when initializing site replication.
Use [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]] on the command line to remove bucket replication rules.

Only one site can have data when setting up site replication.
All other sites must be empty.

## Tutorials

### Configure Site Replication

The following steps create a new site replication configuration for three [[operations/deployments/installation#deploy-minio-distributed|distributed deployments]].
One of the sites contains [replicable data](#minio-site-replication-what-replicates).

The three sites use aliases, `minio1`, `minio2`, and `minio3`, and only `minio1` contains any data.

[[operations/deployments/installation#deploy-minio-distributed|Deploy]] three or more separate MinIO sites, using the same [[administration/identity-access-management#minio-authentication-and-identity-management|IDP]]

Start with empty sites or have no more than one site with any [replicable data](#minio-site-replication-what-replicates).

Configure an alias for each site

Specify the URL or IP address of the site’s load balancer, reverse proxy, or similar network control plane component.
Requests are automatically routed to nodes in the deployment.

MinIO recommends against using a single node hostname for a peer site.
This creates a single point of failure: if that node goes offline, replication fails.

For example, for three MinIO sites, you might create aliases `minio1`, `minio2`, and `minio3`.

Use [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] to define the hostname or IP of the load balancer managing connections to the site.

```
mc alias set minio1 https://minio1.example.com:9000 adminuser adminpassword
mc alias set minio2 https://minio2.example.com:9000 adminuser adminpassword
mc alias set minio3 https://minio3.example.com:9000 adminuser adminpassword
```

or define environment variables

```
export MC_HOST_minio1=https://adminuser:[email protected]
export MC_HOST_minio2=https://adminuser:[email protected]
export MC_HOST_minio3=https://adminuser:[email protected]
```

Add site replication configuration

```
mc admin replicate add minio1 minio2 minio3
```

If all sites are empty, the order of the aliases does not matter.
If one of the sites contains any [replicable data](#minio-site-replication-what-replicates), you must list it first.

No more than one site can contain any replicable data.

Query the site replication configuration to verify

```
mc admin replicate info minio1
```

You can use the alias for any peer site in the site replication configuration.

Query the site replication status to confirm any initial data has replicated to all peer sites.

```
mc admin replicate status minio1
```

You can use the alias for any of the peer sites in the site replication configuration.
The output should say that all [replicable data](#minio-site-replication-what-replicates) is in sync.

The output could resemble the following:

```
Bucket replication status:
●  1/1 Buckets in sync

Policy replication status:
●  5/5 Policies in sync

User replication status:
No Users present

Group replication status:
No Groups present
```

For more on reviewing site replication, see the [Site Replication Status tutorial](#minio-site-replication-status-tutorial).

### Expand Site Replication

You can add more sites to an existing site replication configuration.

The new site must meet the following requirements:

Site is fully deployed and accessible by hostname or IP

Shares the IDP configuration as all other sites in the configuration

Uses the same root user credentials as other configured sites

Contains no bucket or object data

Deploy the new MinIO peer site(s) following the stated requirements

Configure an alias for the new site

Specify the URL or IP address of the site’s load balancer, reverse proxy, or similar network control plane component.
Requests are automatically routed to nodes in the deployment.

MinIO recommends against using a single node hostname for a peer site.
This creates a single point of failure: if that node goes offline, replication fails.

To check the existing aliases, use [[reference/minio-mc/mc-alias-list#command-mc.alias.list|`mc alias list`]].

Use [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] to define the hostname or IP of the load balancer managing connections to the new site(s).

```
mc alias set minio4 https://minio4.example.com:9000 adminuser adminpassword
```

or define environment variables

```
export MC_HOST_minio4=https://adminuser:[email protected]
```

Add site replication configuration

Use the [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.add|`mc admin replicate add`]] command to expand the site replication configuration with the new peer site.
Specify the alias of all existing peer sites, then the alias of the new site to add.

For example, the following command adds the new peer site `minio4` to an existing site replication configuration that includes the existing sites `minio1`, `minio2`, and `minio3`.

```
mc admin replicate add minio1 minio2 minio3 minio4
```

Note

If any of the sites are unreachable or permanently lost, you must first remove the unreachable site(s) with [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.rm|`mc admin replicate rm`]] before expanding with the new site.

Query the site replication configuration to verify

```
mc admin replicate info minio1
```

### Modify a Site’s Endpoint

If a peer site changes its hostname, you can modify the replication configuration to reflect the new hostname.

Obtain the site’s Deployment ID with [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.info|`mc admin replicate info`]]

```
mc admin replicate info <ALIAS>
```

Update the site’s endpoint with [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.update|`mc admin replicate update`]]

```
mc admin replicate update ALIAS --deployment-id [DEPLOYMENT-ID] --endpoint [NEW-ENDPOINT]
```

Replace [DEPLOYMENT-ID] with the deployment ID of the site to update.

Replace [NEW-ENDPOINT] with the new endpoint for the site.

Specify the URL or IP address of the site’s load balancer, reverse proxy, or similar network control plane component.
Requests are automatically routed to nodes in the deployment.

MinIO recommends against using a single node hostname for a peer site.
This creates a single point of failure: if that node goes offline, replication fails.

### Remove a Site from Replication

You can remove a site from replication at any time.
You can re-add the site at a later date, but you must first completely wipe bucket and object data from the site.

Use [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.rm|`mc admin replicate rm`]]:

```
mc admin replicate rm ALIAS PEER_TO_REMOVE --force
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of any peer site in the replication configuration.

Replace `PEER_TO_REMOVE` with the alias of the peer site to remove.

All healthy peers in the site replication configuration update to remove the specified peer automatically.

MinIO requires the `--force` flag to remove the peer from the site replication configuration.

### Review Replication Status

MinIO provides information on replication across the sites for users, groups, policies, or buckets.

The summary information includes the number of Synced and Failed items for each category.

Use [[reference/minio-mc-admin/mc-admin-replicate#mc.admin.replicate.status|`mc admin replicate status`]]:

```
mc admin replicate status <ALIAS> --<flag> <value>
```

For example:

`mc admin replicate status minio3 --bucket images`

Displays the replication status for the `images` bucket on the `minio3` site.

The output resembles the following:

```
●  Bucket config replication summary for: images

Bucket          | MINIO2          | MINIO3          | MINIO4
Tags            |                 |                 |
Policy          |                 |                 |
Quota           |                 |                 |
Retention       |                 |                 |
Encryption      |                 |                 |
Replication     | ✔               | ✔               | ✔
```

`mc admin replicate status minio3 --all`

Displays the replication status summary for all replication sites of which `minio3` is part.

The output resembles the following:

```
Bucket replication status:
●  1/1 Buckets in sync

Policy replication status:
●  5/5 Policies in sync

User replication status:
●  1/1 Users in sync

Group replication status:
●  0/2 Groups in sync

Group           | MINIO2          | MINIO3          | MINIO4
ittechs         | ✗  in-sync      |                 | ✗  in-sync
managers        | ✗  in-sync      |                 | ✗  in-sync
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).