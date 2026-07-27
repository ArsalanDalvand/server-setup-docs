---
title: "Glossary — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/glossary.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "glossary"
category: "minio"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Glossary — SILO (MinIO Object Storage)"
---
# Glossary

access keys[](#term-access-keys)

A MinIO deployment or tenant user account with limited account typically used with API calls.
Access Keys were previously referred to as “Service Accounts”

active-active[](#term-active-active)

A method of [replication](#term-replication) that provides bidirectional mirroring of data.
With active-active configuration, changing the data at at any storage location also changes the data at the other storage location(s).

See also: [active-passive](#term-active-passive).

active-passive[](#term-active-passive)

A method of [replication](#term-replication) that provides one-way mirroring of data.
With the active-passive configuration, changing data at the originating location also changes the data at the destination.
However, changing data at the destination does not affect the data on the origin.

See also: [active-active](#term-active-active).

alias[](#term-alias)

A locally defined reference to a MinIO Deployment used in most command line interface operations.
See [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]].

audit logs[](#term-audit-logs)

Granular descriptions of each operation on a MinIO deployment.
[[operations/monitoring/minio-logging#minio-logging|Audit logs]] support security standards and regulations which require detailed tracking of operations.

See also: [server logs](#term-server-logs).

bit rot[](#term-bit-rot)

Data corruption that occurs without the user’s knowledge.

MinIO combats bit rot with [hashing](#term-hashing) and [erasure coding](#term-erasure-coding).

bit rot healing[](#term-bit-rot-healing)

Objects corrupted due to bit rot are automatically [healed](#term-healing) during any `GET` or `HEAD` operation.
MinIO captures and heals corrupted objects on the fly with its [hashing](#term-hashing) implementation.

bucket[](#term-bucket)buckets[](#term-buckets)

A grouping of [objects](#term-objects) and associated configurations.

cluster[](#term-cluster)

A group of drives and one or more MinIO server processes pooled into a single storage resource.

See also: [tenant](#term-tenant).

cluster registration[](#term-cluster-registration)

Cluster registration links a MinIO deployment to a [SUBNET](#term-SUBNET) [subscription](https://min.io/pricing?jmp=docs).
An organization may have more than one MinIO clusters registered to the same SUBNET subscription.

Console[](#term-Console)MinIO Console[](#term-MinIO-Console)

Graphical User Interface (GUI) for interacting with a MinIO deployment or [tenant](#term-tenant).

data[](#term-data)

One of the two types of blocks MinIO writes when doing [erasure coding](#term-erasure-coding).
Data blocks contain the contents of a file.

[Parity](#term-parity) blocks support data reconstruction should data blocks become corrupt or go missing.

decommission[](#term-decommission)

Process of removing a pool of drives from a [distributed](#term-distributed) deployment.
When initiated, the objects on the decommission pool drain by moving to other pools on the deployment.

The process is not reversible.

deployment[](#term-deployment)

A specific instance of MinIO containing a set of [buckets](#term-buckets) and [objects](#term-objects).

disk encryption[](#term-disk-encryption)

The conversion of all of the contents written to a disk to values that cannot be easily deciphered by an unauthorized entity.
Disk encryption can be used in conjunction with other encryption technologies to create a robust data security system.

enclave[](#term-enclave)

A description of an isolated area within a stateful Key Encryption Service (KES) server.
A KES server may have one enclave or multiple enclaves.
Each enclave within a KES server holds separate keys, policies, and administration identity.
An enclave cannot see or make use of any other enclave on the server.

For example, you might use multiple enclaves to hold completely separate key stores for multiple MinIO tenants within a single stateful KES server.

encryption at rest[](#term-encryption-at-rest)

A method of encryption that stores an object in an encrypted state.
The object remains encrypted while not moving from one location to another.

Objects can be encrypted by the the server using one of key management methods:
[SSE-KMS](#term-SSE-KMS), [SSE-S3](#term-SSE-S3), or [SSE-C](#term-SSE-C).

encryption in transit[](#term-encryption-in-transit)

A method of encryption that protects an object when moving it from one location to another, such as during a GET request.
The object may or may not be encrypted on the origin or destination storage devices.

erasure coding[](#term-erasure-coding)

A technology that splits [objects](#term-objects) into multiple shards and writes the shards to multiple, separate drives.

Depending on the [topology](#term-topology) used, erasure coding allows for loss of drives or nodes within a MinIO deployment without losing read or write access.

erasure set[](#term-erasure-set)

A group of drives within MinIO that support [erasure coding](#term-erasure-coding).
MinIO divides the number of drives in a deployment’s server pool into groups of 4 to 16 drives that make up each erasure set.
When writing objects, [data](#term-data) and [parity](#term-parity) blocks write randomly to the drives in the erasure set.

hashing[](#term-hashing)

The use of an algorithm to create a unique, fixed-length string (a value) to identify a piece of data.

healing[](#term-healing)

Restoration of data from partial loss due to bit rot, drive failure, or site failure.

health diagnostics[](#term-health-diagnostics)

A suite of MinIO [[operations/monitoring/healthcheck-probe#minio-healthcheck-api|API endpoints]] available to check whether a server is

online

available for writing data

available for reading data

available for maintenance without affecting the cluster’s read and write operations

host bus adapter[](#term-host-bus-adapter)HBA[](#term-HBA)

A circuit board or integrated circuit adapter that connects a host system to a storage device.
The HBA handles processing to reduce load on the host system’s processor.

IAM integration[](#term-IAM-integration)

MinIO only allows access to data for authenticated users.
MinIO provides a built-in identity management solution to create authorized credentials.
Optionally, MinIO users can authenticate with credentials from a 3rd party identify provider (IDP), including either OpenID or LDAP providers.

JBOD[](#term-JBOD)

Initialism for “Just A Bunch of Drives”.
JBOD is a storage device enclosure that holds many hard drives.
These drives can combine into one logical drive unit.

See also: [RAID](#term-RAID)

lifecycle management[](#term-lifecycle-management)ILM[](#term-ILM)

Rules to determine when [objects](#term-objects) should move or expire.

locking[](#term-locking)

A rule that prevents removal or deletion of an object until an authorized agent removes the rule or it expires.

monitoring[](#term-monitoring)

The act of reviewing the status, activity, and availability of a MinIO cluster, deployment, tenant, or server.
MinIO provides the following tools:

[Prometheus](https://prometheus.io/) compatible metrics and alerts

[Audit logs](#term-audit-logs)

[server logs](#term-server-logs)

[[operations/monitoring/healthcheck-probe#minio-healthcheck-api|Healthcheck API endpoints]]

[[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket notifications]]

multi-node multi-drive[](#term-multi-node-multi-drive)MNMD[](#term-MNMD)distributed[](#term-distributed)

A system [topology](#term-topology) that uses more than one server and more than one drive per server to host a MinIO instance.
MinIO recommends Kubernetes for distributed deployments.

multipart upload[](#term-multipart-upload)

Multipart upload is a client-initiated [S3 function](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html) that splits a single object into multiple parts for moving from one location to another.
The client uploads each part independently to MinIO, and MinIO manages reconstructing those received parts into the original object.

Multipart uploads provide benefits such as improved throughput and resiliency to network errors.
Use multipart uploads for objects greater than 100MB in actual or estimated size for best results.

See [Amazon AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html) for more details.

network encryption[](#term-network-encryption)

A method of securing data during transit from one location to another, such as server-server or client-server.
MinIO supports [[operations/network-encryption#minio-tls|Transport Layer Security (TLS)]], version 1.2 and later, for both incoming and outgoing traffic.

object[](#term-object)objects[](#term-objects)

An item of data MinIO interacts with using an S3-compatible API.
Objects can be grouped into [buckets](#term-buckets).

Operator[](#term-Operator)Operator Console[](#term-Operator-Console)

The Graphical User Interface (GUI) to deploy and manage the MinIO [tenants](#term-tenants) in a distributed deployment environment.

parity[](#term-parity)

The portion of blocks written for an object by MinIO to support data reconstruction due to missing or corrupt data blocks.
The number of parity blocks indicates the number of drives in the [erasure set](#term-erasure-set) that a deployment can lose while still retaining read and write operations.

prefix[](#term-prefix)

Prefixes organize the [objects](#term-objects) in a [bucket](#term-bucket) by assigning the same string of characters to objects that should share a similar hierarchy or structure.
Use a delimiter character, typically a / to add layers to the hierarchy.
While prefixed objects may resemble a directory structure in some file systems, prefixes are not directories.

MinIO itself does not limit the number of objects that any specific prefix can contain.
However, hardware and network conditions may show performance impacts with large prefixes.

Deployments with modest or budget-focused hardware should architect their workloads to target 10,000 objects per prefix as a baseline.
Increase this target based on benchmarking and monitoring of real world workloads up to what the hardware can meaningfully handle.

Deployments with high-performance or enterprise-grade [[operations/checklists/hardware#deploy-minio-distributed-recommendations|hardware]] can typically handle prefixes with millions of objects or more.

SUBNET| Enterprise accounts can utilize yearly architecture reviews as part of the deployment and maintenance strategy to ensure long-term performance and success of your MinIO-dependent projects.

RAID[](#term-RAID)

Initialism for “Redundant Array of Independent Disks”.
The technology merges multiple separate physical disks into a single storage unit or array.
Some RAID levels provide data redundancy or fault tolerance by duplicating data, striping data, or mirroring data across physical disks.

See also: [JBOD](#term-JBOD).

read quorum[](#term-read-quorum)

The minimum number of object shards necessary to reconstruct the full object for read operations.
See [[operations/concepts/erasure-coding#minio-ec-basics|Erasure Coding Basics]] for more information.

replication[](#term-replication)mirror[](#term-mirror)

The replication of a [[administration/bucket-replication#minio-bucket-replication|bucket]] or entire [[operations/replication/multi-site-replication#minio-site-replication-overview|site]] to another location.

scanner[](#term-scanner)MinIO Scanner[](#term-MinIO-Scanner)

One of several low-priority processes MinIO runs to check:

lifecycle management rules requiring object transition

bucket or site replication status

object [bit rot](#term-bit-rot) and [healing](#term-healing)

usage data

For more, see [[operations/concepts/scanner#minio-concepts-scanner|Object Scanner]].

self signed certificates[](#term-self-signed-certificates)

A self-signed certificate is one created by, issued by, and signed by the company or developer responsible for the content the certificate secures.
Self-signed certificates are not issued by or signed by a publicly trusted, third-party Certificate Authority (CA).
These types of certificates do not expire or require periodic review, and they cannot be revoked.

server logs[](#term-server-logs)

Records the `minio server` operations logged to the system console.
[[operations/monitoring/minio-logging#minio-logging|Server logs]] support general monitoring and troubleshooting of operations.

For more detailed logging information, see [audit logs](#term-audit-logs).

server pool[](#term-server-pool)pool[](#term-pool)

A set of `minio server` nodes which combine their drives and resources to support object storage and retrieval requests.

For more information, see [[operations/concepts#minio-intro-server-pool|How does MinIO manage multiple virtual or physical servers?]].

service account[](#term-service-account)

Renamed to [access keys](#term-access-keys).
A MinIO deployment or tenant user account with limited account typically used with API calls.

shard[](#term-shard)shards[](#term-shards)

A portion of an object after being [erasure coded](#term-erasure-coding) by MinIO.
Each “shard” represents either data or parity for MinIO to use for reconstructing objects on read requests.

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

For more detailed information, see [[operations/concepts/erasure-coding#minio-erasure-coding|Erasure Coding]].

single-node multi-drive[](#term-single-node-multi-drive)SNMD[](#term-SNMD)

A system [topology](#term-topology) that deploys MinIO on one compute resource with more than one attached volume.

single-node single-drive[](#term-single-node-single-drive)SNSD[](#term-SNSD)filesystem[](#term-filesystem)

A system [topology](#term-topology) that deploys MinIO on a single compute resource with a single drive.
This adds S3-type functionality to an otherwise standard filesystem.

SSE-C[](#term-SSE-C)

A method of [encryption at rest](#term-encryption-at-rest) that encrypts an object at the time of writing with an encryption key included with the write request.
To retrieve the object, you must provide the same encryption key provided when originally writing the object.
Additionally, you must self-manage the encryption key(s) used.

See also: [SSE-KMS](#term-SSE-KMS), [SSE-S3](#term-SSE-S3), [encryption at rest](#term-encryption-at-rest), [network encryption](#term-network-encryption).

SSE-KMS[](#term-SSE-KMS)

A method of [encryption at rest](#term-encryption-at-rest) that encrypts each object at the time of writing with separate keys managed by a service provider.
Use keys at either the bucket level (default) or at the object level.
MinIO recommends the SSE-KMS method for key management of encryption.

See also: [SSE-S3](#term-SSE-S3), [SSE-C](#term-SSE-C), [encryption at rest](#term-encryption-at-rest), [network encryption](#term-network-encryption).

SSE-S3[](#term-SSE-S3)

A method of [encryption at rest](#term-encryption-at-rest) that encrypts each object at the time of writing with a single key for all objects on a deployment.
A deployment uses a single external key to decrypt any object throughout the deployment.

See also: [SSE-KMS](#term-SSE-KMS), [SSE-C](#term-SSE-C), [encryption at rest](#term-encryption-at-rest), [network encryption](#term-network-encryption).

standalone deployment[](#term-standalone-deployment)

A [single-node single-drive](#term-single-node-single-drive) (SNSD) MinIO deployment.
This term previously referred to the deprecated [[operations/deployments/baremetal-migrate-fs-gateway#minio-gateway-migration|Gateway or Filesystem Mode]] deployment types.

SUBNET[](#term-SUBNET)

[MinIO’s Subscription Network](https://min.io/pricing?jmp=docs) tracks support tickets and provides 24 hour direct-to-engineer access for subscribed accounts.

tenant[](#term-tenant)tenants[](#term-tenants)

In a [distributed](#term-distributed) mode, a specific MinIO deployment.
One instance of the MinIO Operator may have multiple tenants.

topology[](#term-topology)

The hardware configuration used for a deployment.
MinIO works with three topologies:

[multi-node multi-drive](#term-multi-node-multi-drive)

[single-node multi-drive](#term-single-node-multi-drive)

[single-node single-drive](#term-single-node-single-drive)

versioning[](#term-versioning)

The retention of multiple iterations of an [object](#term-object) as it changes over time.

webhook[](#term-webhook)

A [[administration/monitoring/publish-events-to-webhook#minio-bucket-notifications-publish-webhook|webhook]] is a method for altering the behavior of a web page or web application with a custom callback.
The format is typically JSON sent as an HTTP POST request.

WORM[](#term-WORM)

Write Once Read Many (WORM) is a data retention methodology that functions as part of object locking.
Many requests can retrieve can view a WORM-locked object (`read many`), but no write requests can change the object (`write once`).

write quorum[](#term-write-quorum)

The minimum number of object shards MinIO must successfully write to an [[operations/concepts/erasure-coding#minio-ec-erasure-set|erasure set]] for write operations.
See [[operations/concepts/erasure-coding#minio-ec-basics|Erasure Coding Basics]] for more information

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).