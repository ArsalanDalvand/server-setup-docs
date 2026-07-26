# LXC 102: FoundationDB Core Database (`fdb-db`)

This document serves as the canonical, atomic provisioning ledger and operations runbook for the FoundationDB database container (LXC 102). It records the system profile, installation logs, performance tuning parameters, troubleshooting histories, and the complete engineering rationale behind each decision.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `102` | Unique index for the FDB node in PVE. |
| **Hostname** | `fdb-db` | Resolves locally on the PVE bridge interface. |
| **Baseline OS** | Debian 12 Standard | Matches host OS libraries, ensuring systemd/glibc compatibility and minimizing CPU context switching. |
| **Storage Target** | `local-lvm` (NVMe) | Redwood engine demands SSD IOPS. Utilizing the fast NVMe Lexar drive prevents transactional bottlenecks. |
| **Disk Size** | 20 GB | Baseline allocation for system and database state; dynamically expandable on-the-fly via LVM-Thin. |
| **CPU Allocation** | 2 Cores | Pinned to NUMA Node 0 (CPU 1) alongside database storage controllers. |
| **RAM Allocation** | 8192 MB (8 GB) | Fixed layout. Swap is disabled (`swap=0`) to protect the NVMe flash cells from excessive write wear. |
| **Static IP Address** | `192.168.1.102/24` | Static local address; gateway set to `192.168.1.1` (Home router). |
| **Security Flags** | Unprivileged (`true`) | Enforces host kernel namespace isolation for container security. Nesting enabled (`nesting=1`). |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Orchestration (July 2026)
The container was created via the CLI on the Proxmox host using `pct create`. 

#### 🔴 Incident: systemd 252 Namespace Warning
During creation, Proxmox triggered a warning stating: `WARN: Systemd 252 detected. You may need to enable nesting.`
*   **Root Cause:** Debian 12 utilizes systemd v252, which makes use of filesystem namespaces and cgroups for service isolation. In an unprivileged container, systemd cannot mount these filesystems without explicit permission from the host hypervisor.
*   **Resolution:** Nesting features were explicitly enabled on the container config, followed by a boot command:
    ```bash
    pct set 102 -features nesting=1
    pct start 102
    ```
*   **Verification:** Container verified in `running` state.

---

### Phase 2: FDB Installation (July 2026)

#### 🔴 Incident: Stalwart Compatibility Version Misalignment
*   **Context:** Initial deployment attempted to use FoundationDB Version `7.1.33`. 
*   **Root Cause:** Research into Stalwart Mail Server's codebase (version `v0.16` and newer) revealed that it targets the newer **FoundationDB API Version 740**, which corresponds to the **FDB 7.3.x** release branch. Deploying FDB 7.1.x would result in runtime panic or transaction API mismatch errors.
*   **Resolution:** The FDB 7.1.33 download was aborted. Official Debian packages for the latest stable version **`7.3.77`** (compiled with AVX instructions enabled to match the Intel Xeon Gold 6248 architecture) were retrieved and installed instead:
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

#### 🔴 Incident: Process Startup Crash (Memory Conflict)
*   **Symptoms:** After installing FDB 7.3.77 and configuring the process layouts, the processes failed to start, logging:
    `ERROR: --memory has to be larger than --cache-memory`
*   **Root Cause:** The default cache allocation for each FDB process is 2GiB. The total memory limit for each process (`memory`) was initially set to 2GiB in our configuration. Because there was no memory overhead left for the server execution beyond the cache-memory, the `fdbserver` processes immediately terminated.
*   **Resolution:** The configuration limits were adjusted to meet the Apple FDB guidelines:
    *   `memory` (Total RAM limit): **`4GiB`**
    *   `cache-memory` (Disk cache): **`2GiB`**
    This allows 2GiB for transaction caching and 2GiB of overhead for the database processes to operate safely. 

*   **Process Division:** 4 processes are spawned on ports 4500-4503:
    *   `fdbserver.4500` & `4501` (Class: `storage` - reads and writes to disk).
    *   `fdbserver.4502` & `4503` (Class: `stateless` - coordinates transactions, no disk writes).

#### Active Configuration File (`/etc/foundationdb/foundationdb.conf`):
```ini
[fdbmonitor]
user = foundationdb
group = foundationdb

[general]
restart-delay = 60
cluster-file = /etc/foundationdb/fdb.cluster

[fdbserver]
command = /usr/sbin/fdbserver
public-address = auto:$ID
listen-address = public
datadir = /var/lib/foundationdb/data/$ID
logdir = /var/log/foundationdb
memory = 4GiB
cache-memory = 2GiB

[fdbserver.4500]
class = storage
[fdbserver.4501]
class = storage
[fdbserver.4502]
class = stateless
[fdbserver.4503]
class = stateless

[backup_agent]
command = /usr/lib/foundationdb/backup_agent/backup_agent
logdir = /var/log/foundationdb

[backup_agent.1]
```

---

### 2. Redwood Storage Engine Migration

#### 🔴 Incident: Memory Engine Incompatibility
*   **Symptoms:** FDB default installation booted with `memory` storage engine.
*   **Root Cause:** The memory storage engine keeps all key-value states in volatile RAM. This would lead to complete mail state loss upon container or server reboot.
*   **Resolution:** Configured the database to migrate aggressively to the SSD B-Tree optimized engine (**`ssd-redwood-1`**):
    ```bash
    fdbcli --exec "configure storage_migration_type=aggressive"
    fdbcli --exec "configure ssd-redwood-1"
    ```
*   **Why Redwood:** Unlike SQLite storage engine, Redwood is optimized for high concurrent transaction rates, offering lower write amplification and structured key prefix compression.

---

## 4. Operational Health & Verification
Running `fdbcli --exec "status details"` returns:
*   **Database Health:** `Healthy`
*   **Redundancy Mode:** `single` (optimized for single-node development)
*   **Active Processes:** 4 processes listening on ports `4500-4503`
*   **Storage Engine:** `ssd-redwood-1` (disk-based B-tree active)
*   **Coordinators:** `127.0.0.1:4500` (reachable)
