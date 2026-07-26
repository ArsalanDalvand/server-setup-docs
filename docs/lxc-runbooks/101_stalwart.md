# LXC 101: Stalwart Mail Server (`mail-server`)

This document serves as the atomic provisioning log, configuration mapping, and operations runbook for the Stalwart Mail Server container.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `101` | Unique index for the mail server in PVE. |
| **Hostname** | `mail-server` | Hostname mapped to mail delivery records. |
| **Baseline OS** | Debian 12 Standard | Ensures library alignment with host environment. |
| **Storage Target** | `local-lvm` (NVMe) | High-speed processing for active email queues. |
| **Disk Size** | 20 GB | Baseline for configuration and mail directories. |
| **CPU Allocation** | 4 Cores | Pinned to NUMA Node 1 (CPU 2) to isolate TLS/SMTP network processing. |
| **RAM Allocation** | 4096 MB (4 GB) | Optimized for memory footprint. |
| **Static IP Address** | `192.168.1.101/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Strict isolation context. Nesting enabled (`nesting=1`). |

---

## 2. Provisioning & Implementation Plan

### Phase 1: Container Creation
Orchestrated on PVE host using the local Debian template:
```bash
pct create 101 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 4 -memory 4096 -swap 0 -hostname mail-server -ostype debian \
  -storage local-lvm -rootfs local-lvm:20 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.101/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
```

### Phase 2: Stalwart Installation & FDB Client Integration
1.  Stalwart binary download and installation using the official bootstrap script:
    ```bash
    sh install.sh --fdb
    ```
2.  Importing the FDB dynamic client library (`libfdb_c.so`) inside the mail-server container to enable native communication with `LXC 102` (FoundationDB).
3.  Mapping the `/etc/foundationdb/fdb.cluster` file from the database node to enable remote clustering.
