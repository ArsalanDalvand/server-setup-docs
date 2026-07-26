# LXC Container Configuration & Runbooks

This ledger records the deployment, provisioning, and tuning of all Linux Containers (LXC) hosted on Proxmox VE.

---

## 1. LXC 102: FoundationDB Core Database (`fdb-db`)

### Configuration Profile
*   **Container ID:** `102`
*   **Hostname:** `fdb-db`
*   **Baseline OS:** Debian 12 Standard (`debian-12-standard_12.12-1_amd64.tar.zst`)
*   **Hardware Specifications:**
    *   **CPU Cores:** `2` (Pinned to CPU 1 via NUMA policy)
    *   **Memory (RAM):** `8192 MB` (8 GB)
    *   **Swap:** `0 MB` (Disabled to protect Lexar SSD from write wear)
    *   **Storage (Disk):** 20 GB Virtual Disk (allocated from the `local-lvm` NVMe pool)
*   **Network Interface (`eth0`):**
    *   **IP Address:** `192.168.1.102/24` (Static on home subnet)
    *   **Gateway:** `192.168.1.1`
    *   **Firewall:** `Enabled`
*   **Security Flags:**
    *   `Unprivileged`: `true` (Enforces host kernel process isolation)
    *   `Nesting`: `true` (Enabled via Proxmox features to support systemd inside Debian 12 container environment)
*   **Availability:**
    *   `Start on Boot`: `true` (Auto-starts when host boots)

---

### Step-by-Step Provisioning Log

#### 1. Container Creation & Basic Tweaks (July 2026)
The container was created via CLI on the Proxmox host using `pct create`.
Due to Debian 12's systemd 252 architecture, a warning was triggered requiring nesting to be enabled to allow correct namespace mounting:
```bash
# Enable systemd nesting feature
pct set 102 -features nesting=1
# Boot container
pct start 102
```
The container is verified to be in `running` state.

#### 2. Database Installation & Tuning (July 2026)
Apple FoundationDB **Version 7.3.77** (Latest Stable) was successfully compiled and installed inside `LXC 102`.
The configuration was tuned for optimal performance under a 32GB host RAM limit:
*   **Memory Allocations:**
    *   Host process resident memory (`memory`): **4 GiB** (per process)
    *   Cache memory (`cache-memory`): **2 GiB** (per process)
    *   *Rationale:* This prevents the `ERROR: --memory has to be larger than --cache-memory` crash, while ensuring the entire cluster (4 processes) uses at most 16 GB RAM in peak execution.
*   **Process Layout (`/etc/foundationdb/foundationdb.conf`):**
    *   `fdbserver.4500` (Class: `storage`)
    *   `fdbserver.4501` (Class: `storage`)
    *   `fdbserver.4502` (Class: `stateless`)
    *   `fdbserver.4503` (Class: `stateless`)
*   **Storage Engine Migration:**
    *   The engine was migrated from volatile memory to the Redwood B-Tree storage engine:
    ```bash
    fdbcli --exec "configure storage_migration_type=aggressive"
    fdbcli --exec "configure ssd-redwood-1"
    ```
*   **Verified Status:**
    All 4 processes are running, listening on ports 4500-4503, database configured to `ssd-redwood-1` (Disk-backed SSD optimization), redundancy `single`, and status is `Healthy`.

---

## 2. LXC 101: Stalwart Mail Server (`mail-server`)
*(Status: Planned)*
*   **Baseline OS:** Debian 12
*   **Hardware Specifications:** 4GB RAM, 4 CPU Cores (NUMA CPU 2 pinning).

---

## 3. LXC 103: Redis Cache (`cache-redis`)
*(Status: Planned)*
*   **Baseline OS:** Debian 12
*   **Hardware Specifications:** 4GB RAM, 2 CPU Cores (NUMA CPU 1 pinning).

---

## 4. LXC 104: MinIO S3 Object Storage (`storage-s3`)
*(Status: Planned)*
*   **Baseline OS:** Debian 12
*   **Hardware Specifications:** 2GB RAM, 2 CPU Cores (NUMA CPU 2 pinning).
