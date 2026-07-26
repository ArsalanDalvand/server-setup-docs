# LXC 102: FoundationDB Core Database (`fdb-db`)

This document serves as the atomic provisioning log, configuration mapping, and operations runbook for the FoundationDB database container.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `102` | Unique index for the FDB node in PVE. |
| **Hostname** | `fdb-db` | Resolves locally on PVE bridge interface. |
| **Baseline OS** | Debian 12 Standard | Matches host OS libraries, ensures systemd/glibc compatibility. |
| **Storage Target** | `local-lvm` (NVMe) | High-speed, direct PCIe access. Redwood engine demands SSD IOPS. |
| **Disk Size** | 20 GB | Conservative allocation; dynamically expandable on-the-fly. |
| **CPU Allocation** | 2 Cores | Pinned to NUMA Node 0 (CPU 1) alongside database storage controllers. |
| **RAM Allocation** | 8192 MB (8 GB) | Fixed layout. No Swap config to protect NVMe cells from degradation. |
| **Static IP Address** | `192.168.1.102/24` | Configured on home subnet; gateway pointed to `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Strict user namespace containment. Nesting enabled (`nesting=1`). |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Orchestration (July 2026)
Created the unprivileged container using the Debian template. Systemd namespace requirements under PVE 9.x necessitated explicit nesting enablement:
```bash
# Set nesting capability
pct set 102 -features nesting=1
# Boot node
pct start 102
```

### Phase 2: FDB Installation (July 2026)
Downloaded and installed official Apple FoundationDB packages (Version `7.3.77` - Latest Stable):
```bash
apt update && apt install -y wget
wget https://github.com/apple/foundationdb/releases/download/7.3.77/foundationdb-clients_7.3.77-1_amd64.deb
wget https://github.com/apple/foundationdb/releases/download/7.3.77/foundationdb-server_7.3.77-1_amd64.deb
dpkg -i foundationdb-clients_7.3.77-1_amd64.deb
dpkg -i foundationdb-server_7.3.77-1_amd64.deb
```

---

## 3. Database Engine & Process Configurations

### 1. Multi-Process Layout & Limits (`/etc/foundationdb/foundationdb.conf`)
To avoid cache-to-host memory crashes (`ERROR: --memory has to be larger than --cache-memory`), the layout was rewritten to spawn 4 processes while pinning memory usage to a maximum of 16GB:
*   `fdbserver.4500` & `4501` (Class: `storage`)
*   `fdbserver.4502` & `4503` (Class: `stateless`)

Configuration details:
```ini
[fdbserver]
command = /usr/sbin/fdbserver
public-address = auto:$ID
listen-address = public
datadir = /var/lib/foundationdb/data/$ID
logdir = /var/log/foundationdb
memory = 4GiB
cache-memory = 2GiB
```

### 2. Redwood Storage Engine Migration
The database engine was migrated from volatile in-memory structure to disk-backed, SSD-optimized B-Tree Redwood:
```bash
fdbcli --exec "configure storage_migration_type=aggressive"
fdbcli --exec "configure ssd-redwood-1"
```

### 3. Verification
Verified operational status:
```bash
fdbcli --exec "status details"
```
*   **Result:** 4 processes online, engine running on `ssd-redwood-1`, status is `Healthy`.
