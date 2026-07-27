# S3 Object Storage Deployment Strategy

This document details the selection, architectural rationale, and execution path for deploying S3-compatible Object Storage on the mechanical SAS storage tier.

---

## 1. Technology Evaluation & Decision Matrix

Due to licensing transitions and feature modifications in common S3 engines, a thorough review of the target storage systems was conducted:

| Storage Engine | License | Web UI Status | Inode Efficiency | Deployment Complexity | Verdict |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **MinIO (Latest)** | AGPLv3 | 🔴 Deprecated | Low (1 file/object) | Low | **Rejected** (CLI-only admin, AGPL constraints) |
| **Silo (Pigsty)** | AGPLv3 | 🟢 Active | Low (1 file/object) | Low | **Approved & Deployed** (Fully open-source community maintained fork) |
| **SeaweedFS** | Apache 2.0 | 🟡 Basic | 🟢 Extreme (Merged blocks) | High | **Approved for Future Scale-out** |

### Rationale Behind S3 Engine Selections

#### Silo (Pigsty Fork) - Deployed Option
*   **The Pros:** Silo is the community-maintained fork of MinIO by the Pigsty project (`pgsty/minio`). It explicitly restores the embedded administrative web UI console reference that MinIO Inc. removed from its community editions, and it is actively patched for security vulnerabilities (e.g. CVE-2026-34986, CVE-2026-39883).
*   **The Verdict:** We are using **Silo (Pigsty Fork) Version RELEASE.2026-03-25T00-00-00Z** (Latest Stable) packaged as a native `.deb`. This provides the newest security patches, a fully unlocked administrative web console, and 100% S3 compatibility without being locked into commercial license tiers.

#### SeaweedFS (Target for Phase 3)
*   *Why not now:* SeaweedFS requires running multiple daemons (Master, Volume, Filer, and S3 gateway). This introduces configuration complexity during the MVP staging phase.
*   *Why in the future:* SeaweedFS merges small object writes into large sequential blocks. Since Stalwart writes thousands of small raw email payloads, SeaweedFS prevents **Inode Exhaustion** on the mechanical SAS drive, which is a major long-term failure vector under MinIO/Silo.

---

## 2. Silo (Pigsty Fork) Container Configuration (`storage-s3`)

We deploy Silo inside an independent LXC container using the dedicated SAS storage pool.

### Container Profile (LXC 104)
*   **Target Storage Pool:** `sas-storage` (Thinpool configured on `/dev/sda` hardware RAID).
*   **Disk Size:** `450 GB` (ext4 filesystem).
*   **CPU Allocation:** `2 Cores` (Pinned to NUMA Node 1 / CPU 2).
*   **RAM Allocation:** `1024 MB` (1 GB) (Optimal for metadata cache; static writes do not require excessive heap allocations).
*   **Static IP Address:** `192.168.1.104/24`

---

## 3. Provisioning Runbook

### Step 1: Create the Container on Proxmox Host
Execute on the PVE host via SSH:
```bash
pct create 104 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 2 -memory 1024 -swap 0 -hostname storage-s3 -ostype debian \
  -storage sas-storage -rootfs sas-storage:450 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.104/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
pct start 104
```

### Step 2: Download & Install Silo (Pigsty Fork) DEB Package
Execute inside the container (`pct enter 104`):
```bash
apt update && apt install -y wget curl
wget https://github.com/pgsty/minio/releases/download/RELEASE.2026-03-25T00-00-00Z/minio_20260325000000.0.0_amd64.deb -O /tmp/minio_silo.deb
dpkg -i /tmp/minio_silo.deb
rm -f /tmp/minio_silo.deb
mkdir -p /mnt/s3-data
```

### Step 3: Configure Environment Variables & Daemon (`/etc/default/minio`)
Create the configuration file:
```env
MINIO_VOLUMES="/mnt/s3-data"
MINIO_OPTS="--address :9000 --console-address :9001"
MINIO_ROOT_USER="minioadmin"
MINIO_ROOT_PASSWORD="minioadmin"
```

### Step 4: User Setup & Activation
```bash
groupadd -r minio-user
useradd -M -r -g minio-user minio-user
chown -R minio-user:minio-user /mnt/s3-data
systemctl daemon-reload
systemctl enable minio
systemctl start minio
```

---

## 4. Operational Health & Verification
Running `systemctl status minio` inside LXC 104 returns:
*   **Service Status:** `active (running)`
*   **Version:** `RELEASE.2026-03-25T00-00-00Z (Silo - Community maintained fork of MinIO)`
*   **API Listener:** `http://192.168.1.104:9000` (S3 active)
*   **Console UI Web Listener:** `http://192.168.1.104:9001` (Admin Web GUI active and responding)

