# Storage Architecture Design

## Layout & Drive Mappings
```
Physical Storage Setup
  ├── Lexar NM620 M.2 256GB NVMe (PCI-E Slot 1) ──► ext4 ──► local-lvm (PVE Host & Databases)
  └── HPE 600GB SAS 10K HDD (SFF Front Bay) ──────► ext4 ──► sas-storage (MinIO & Backups)
```

## Storage Pool Configuration
1.  **System Pool (`local-lvm` on NVMe):**
    *   *Path:* `/dev/nvme0n1p3`
    *   *Logical Size:* 140.9 GB
    *   *Target:* Host system config, templates, and high-performance databases (FoundationDB & Redis).
2.  **Data Pool (`sas-storage` on SAS):**
    *   *Path:* `/dev/sda` (Single physical drive configured as a single-drive RAID 0 logical volume in Smart Array SSA).
    *   *Logical Size:* 500 GB (LVM-Thin Pool).
    *   *Target:* S3 Object Storage (MinIO) for email attachments and local disaster recovery snapshots (`fdbbackup`).

## Database-to-Storage Mapping Decisions

### The "Lying Disk" / FDB Torn Pages Mitigation
*   **The Problem:** FoundationDB (FDB) Redwood engine relies heavily on atomic transactions using the `fsync` command. Consumer-grade SSDs (like the Lexar NM620) lack **PLP (Power Loss Protection)** capacitors. They "lie" to the OS, stating that data is written while it still resides in volatile cache. A sudden power loss in the home staging phase can corrupt the database metadata (Torn Pages).
*   **The Mitigation:**
    1.  During home testing, continuous streaming backups (`fdbbackup`) are routed from the NVMe to the SAS storage directory (`/mnt/sas-storage/fdb-backups`).
    2.  For production datacenter deployment, the Lexar NM620 must be replaced by an enterprise-grade U.3/M.2 NVMe (e.g., Samsung PM9A3) featuring hardware-level PLP.

### Filesystem Strategy: `ext4` over `ZFS` / `Btrfs`
*   **Write Amplification Reduction:** Both ZFS and Btrfs utilize Copy-on-Write (CoW) trees. FDB updates database states in small transactional commits. Running FDB on ZFS creates massive write amplification, degrading I/O performance and prematurely killing the Lexar SSD.
*   **Double Journaling Conflict:** Redwood engine handles transaction logging natively. Layering it on top of a journaling/CoW filesystem like ZFS/Btrfs causes redundant write operations.
*   **Memory Efficiency:** ZFS caching (ARC) consumes heavy RAM resources. Under a 32GB RAM envelope, using standard LVM-Thin over `ext4` ensures near-zero virtualization memory overhead.
*   **Partition Flexibility:** Unlike XFS, `ext4` partitions can be dynamically shrunk (reduced in size) using standard `resize2fs` tools if storage adjustments are needed.
