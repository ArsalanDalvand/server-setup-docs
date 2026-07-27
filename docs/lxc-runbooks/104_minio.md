# S3 Object Storage Deployment Strategy

This document details the selection, architectural rationale, and execution path for deploying S3-compatible Object Storage on the mechanical SAS storage tier.

---

## 1. Technology Evaluation & Decision Matrix

Due to licensing transitions and feature modifications in common S3 engines, a thorough review of the target storage systems was conducted:

| Storage Engine | License | Web UI Status | Inode Efficiency | Deployment Complexity | Verdict |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **MinIO (Latest)** | AGPLv3 | 🔴 Deprecated | Low (1 file/object) | Low | **Rejected** (CLI-only admin, AGPL constraints) |
| **Silo (Pigsty)** | AGPLv3 | 🟢 Active | Low (1 file/object) | Low | **Approved (Option A)** |
| **MinIO (RELEASE.2025-01-01)** | AGPLv3 | 🟢 Active | Low (1 file/object) | Low | **Approved (Option B - Recommended)** |
| **SeaweedFS** | Apache 2.0 | 🟡 Basic | 🟢 Extreme (Merged blocks) | High | **Approved for Future Scale-out** |

### Rationale Behind S3 Engine Selections

#### Option A: Silo (Pigsty Fork)
*   **The Pros:** Silo is a community-maintained fork of MinIO by the Pigsty project. It explicitly restores the embedded administrative web UI console reference that MinIO Inc. removed from its community editions. It retains 100% S3 API compatibility.
*   **The Cons:** Being a relatively fresh fork, long-term security patching frequency is dependent on the Pigsty community.

#### Option B: MinIO pinned to `RELEASE.2025-01-01T16-13-09Z` (Recommended for Home/Testing)
*   **The Pros:** Relies on the mature, fully tested, upstream MinIO codebase. Pining the installation to a release tag prior to the UI extraction ensures that the fully-featured administrative web GUI remains available and free.
*   **The Cons:** Pinned versions will not receive subsequent upstream security patches automatically. However, since the storage k8s/LXC interface is locked inside the private virtual bridge (`vmbr0`) and accessible only via local proxied interfaces, external network vulnerability vectors are fully mitigated.

#### Option C: SeaweedFS (Target for Phase 3)
*   *Why not now:* SeaweedFS requires running multiple daemons (Master, Volume, Filer, and S3 gateway). This introduces configuration complexity during the MVP staging phase.
*   *Why in the future:* SeaweedFS merges small object writes into large sequential blocks. Since Stalwart writes thousands of small raw email payloads, SeaweedFS prevents **Inode Exhaustion** on the mechanical SAS drive, which is a major long-term failure vector under MinIO/Silo.

---

## 2. MinIO (Pinned UI) Container Configuration (`storage-s3`)

We will deploy Option B (pinned stable MinIO) inside an independent LXC container using the dedicated SAS storage pool.

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

### Step 2: Download & Install Pinned MinIO Binary
Execute inside the container (`pct enter 104`):
```bash
apt update && apt install -y wget curl
# Download the specific release binary containing the administrative web UI
wget https://dl.min.io/server/minio/release/linux-amd64/archive/minio.RELEASE.2025-01-01T16-13-09Z -O /usr/local/bin/minio
chmod +x /usr/local/bin/minio
```

### Step 3: Configure Environment Variables & Daemon (`/etc/default/minio`)
Create the configuration file:
```env
MINIO_VOLUMES="/mnt/s3-data"
MINIO_OPTS="--address :9000 --console-address :9001"
MINIO_ROOT_USER="minioadmin"
MINIO_ROOT_PASSWORD="your_secure_password"
```

### Step 4: Systemd Service Integration
Create `/etc/systemd/system/minio.service` to manage the process lifetime, mount `/mnt/s3-data`, and run the daemon under the unprivileged system user.
