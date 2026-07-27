---
title: "Drive Failure Recovery — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/data-recovery/recover-after-drive-failure.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/data-recovery/recover-after-drive-failure"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Drive Failure Recovery — SILO (MinIO Object Storage)"
---
# Drive Failure Recovery

MinIO supports hot-swapping failed drives with new healthy drives.
MinIO detects and heals those drives without requiring any node or deployment-level restart.
[[operations/concepts/healing#minio-concepts-healing|MinIO healing]] occurs only on the replaced drive(s) and in most cases has minimal or negligible impact on deployment performance.

MinIO healing ensures consistency and correctness of all data restored onto the drive.

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

The following steps provide a more detailed walkthrough of drive replacement.
These steps assume a MinIO deployment where each node manages drives using `/etc/fstab` with per-drive labels as per the [[operations/deployments/installation#minio-installation|documented prerequisites]].

## 1) Unmount the failed drive(s)

Unmount each failed drive using `umount`. For example, the following
command unmounts the drive at `/dev/sdb`:

```
umount /dev/sdb
```

## 2) Replace the failed drive(s)

Remove the failed drive(s) from the node hardware and replace it with known
healthy drive(s). Replacement drives must meet the following requirements:

[[operations/checklists/hardware#minio-hardware-checklist-storage|XFS formatted]] and empty.

Same drive type (e.g. HDD, SSD, NVMe).

Equal or greater performance.

Equal or greater capacity.

Using a replacement drive with greater capacity does not increase the total
cluster storage. MinIO uses the smallest drive’s capacity as the ceiling for
all drives in the [[operations/concepts#minio-intro-server-pool|Server Pool]].

The following command formats a drive as XFS and assigns it a label to match
the failed drive.

```
mkfs.xfs /dev/sdb -L DRIVE1
```

MinIO strongly recommends using label-based mounting to ensure consistent
drive order that persists through system restarts.

## 3) Review and Update fstab

Review the `/etc/fstab` file and update as needed such that the entry for
the failed drive points to the newly formatted replacement.

If using label-based drive assignment, ensure that each label points to the
correct newly formatted drive.

If using UUID-based drive assignment, update the UUID for each point based on
the newly formatted drive. You can use `lsblk` to view drive UUIDs.

For example, consider

```
$ cat /etc/fstab

  # <file system>  <mount point>  <type>  <options>         <dump>  <pass>
  LABEL=DRIVE1     /mnt/drive1    xfs     defaults,noatime  0       2
  LABEL=DRIVE2     /mnt/drive2    xfs     defaults,noatime  0       2
  LABEL=DRIVE3     /mnt/drive3    xfs     defaults,noatime  0       2
  LABEL=DRIVE4     /mnt/drive4    xfs     defaults,noatime  0       2
```

Note

Cloud environment instances which depend on mounted external storage may encounter boot failure if one or more of the remote file mounts return errors or failure.
For example, an AWS ECS instances with mounted persistent EBS volumes may fail to boot with the standard `/etc/fstab` configuration if one or more EBS volumes fail to mount.

You can set the `nofail` option to silence error reporting at boot and allow the instance to boot with one or more mount issues.

You should not use this option on systems which have locally attached disks, as silencing drive errors prevents both MinIO and the OS from responding to those errors in a normal fashion.

Given the previous example command, no changes are required to
`fstab` since the replacement drive at `/mnt/drive1` uses the same
label `DRIVE1` as the failed drive.

## 4) Remount the Replaced Drive(s)

Use `mount -a` to remount the drives unmounted at the beginning of this
procedure:

```
mount -a
```

The command should result in remounting of all of the replaced drives.

## 5) Monitor MinIO for Drive Detection and Healing Status

Use [[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]] command or `journalctl -u minio` for
`systemd`-managed installations to monitor the server log output after
remounting drives. The output should include messages identifying each formatted
and empty drive.

Use [[reference/minio-mc-admin/mc-admin-heal#command-mc.admin.heal|`mc admin heal`]] to monitor the overall [[operations/concepts/healing#minio-concepts-healing|healing]] status on the
deployment. MinIO aggressively heals replaced drive(s) to ensure rapid recovery
from the degraded state.

## 6) Next Steps

Monitor the cluster for any further drive failures. Some drive batches may fail
in close proximity to each other. Deployments seeing higher than expected drive
failure rates should schedule dedicated maintenance around replacing the known
bad batch. Consider using [MinIO SUBNET](https://min.io/pricing?jmp=docs) to
coordinate with MinIO engineering around guidance for any such operations.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).