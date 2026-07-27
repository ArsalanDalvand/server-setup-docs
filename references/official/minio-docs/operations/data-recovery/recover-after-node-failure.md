---
title: "Node Failure Recovery — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/data-recovery/recover-after-node-failure.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/data-recovery/recover-after-node-failure"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Node Failure Recovery — SILO (MinIO Object Storage)"
---
# Node Failure Recovery

If a MinIO node suffers complete hardware failure (e.g. loss of all drives,
data, etc.), the node begins [[operations/concepts/healing#minio-concepts-healing|healing operations]] once it rejoins the deployment.
MinIO healing occurs only on the replaced hardware and does not typically impact
deployment performance.

MinIO healing ensures consistency and correctness of all data restored onto the
drive.

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

The replacement node hardware should be substantially similar to the failed
node. There are no negative performance implications to using improved hardware.

The replacement drive hardware should be substantially similar to the failed
drive. For example, replace a failed SSD with another SSD drive of the same
capacity. While you can use drives with larger capacity, MinIO uses the
smallest drive’s capacity as the ceiling for all drives in the
[[operations/concepts#minio-intro-server-pool|Server Pool]].

The following steps provide a more detailed walkthrough of node replacement.
These steps assume a MinIO deployment where each node has a DNS hostname
as per the [[operations/deployments/installation#minio-installation|documented prerequisites]].

## 1) Start the Replacement Node

Ensure the new node has received all necessary security, firmware, and OS
updates as per industry, regulatory, or organizational standards and
requirements.

The new node software configuration must match that of the other nodes in the
deployment, including but not limited to the OS and Kernel versions and
configurations. Heterogeneous software configurations may result in unexpected
or undesired behavior in the deployment.

## 2) Update Hostname for the New Node

Optional This step is only required if the replacement node has a
different IP address from the failed host.

Ensure the hostname associated to the failed node now resolves to the new node.

For example, if `https://minio-1.example.net` previously resolved to the
failed host, it should now resolve to the new host.

## 3) Download and Prepare the MinIO Server

Follow the [[operations/deployments/installation#minio-installation|deployment procedure]] to download
and run the MinIO server using a matching configuration as all other nodes
in the deployment.

The MinIO server version must match across all nodes

The MinIO service and environment file configurations must match across
all nodes.

## 4) Rejoin the node to the deployment

Start the MinIO server process on the node and monitor the process output
using [[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]] or by monitoring the MinIO service logs using
`journalctl -u minio` for `systemd` managed installations.

The server output should indicate that it has detected the other nodes
in the deployment and begun [[operations/concepts/healing#minio-concepts-healing|healing operations]].

Use [[reference/minio-mc-admin/mc-admin-heal#command-mc.admin.heal|`mc admin heal`]] to monitor overall healing status on the
deployment. MinIO aggressively heals the node to ensure rapid recovery
from the degraded state.

## 5) Next Steps

Continue monitoring the deployment until healing completes. Deployments with
persistent and repeated node failures should schedule dedicated maintenance to
identify the root cause. Consider using
[MinIO SUBNET](https://min.io/pricing?jmp=docs) to coordinate with MinIO
engineering around guidance for any such operations.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).