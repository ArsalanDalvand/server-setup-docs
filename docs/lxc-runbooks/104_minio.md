# LXC 104: MinIO S3 Object Storage (`storage-s3`)

This document serves as the atomic provisioning log, configuration mapping, and operations runbook for the MinIO object storage container.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `104` | Unique index for the storage backend. |
| **Hostname** | `storage-s3` | Internal hostname for S3 endpoints. |
| **Baseline OS** | Debian 12 Standard | Aligns with the core virtualization layer. |
| **Storage Target** | `sas-storage` (SAS) | Uses high-capacity SAS HDD storage for static assets. |
| **Disk Size** | 450 GB | Large thin-provisioned storage mount. |
| **CPU Allocation** | 2 Cores | Pinned to NUMA Node 1 (CPU 2) to offload object processing. |
| **RAM Allocation** | 2048 MB (2 GB) | Minimal memory needed; caching handled by disk controllers. |
| **Static IP Address** | `192.168.1.104/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Implements nesting for backend data volumes. |

---

## 2. Provisioning & Implementation Plan

### Phase 1: Container Creation
Orchestrated on PVE host using the local Debian template and attaching the SAS thinpool:
```bash
pct create 104 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 2 -memory 2048 -swap 0 -hostname storage-s3 -ostype debian \
  -storage sas-storage -rootfs sas-storage:450 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.104/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
```

### Phase 2: MinIO Setup & Buckets
1.  Download and install MinIO server package.
2.  Configure system variables for user accounts and data directories (`/mnt/data`).
3.  Configure Stalwart bucket templates inside MinIO.
