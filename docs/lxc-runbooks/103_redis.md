# LXC 103: Redis Cache (`cache-redis`)

This document serves as the canonical, atomic provisioning ledger and operations runbook for the Redis cache container (LXC 103). It details the system specifications, configuration logic, and installation steps.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `103` | Unique index for the Redis store in PVE. |
| **Hostname** | `cache-redis` | Internal hostname for routing. |
| **Baseline OS** | Debian 12 Standard | Ensures library and kernel namespace alignment with host. |
| **Storage Target** | `local-lvm` (NVMe) | High-speed cache snapshot dump writes. |
| **Disk Size** | 4 GB | Minimal disk allocation; Redis keeps all active data in memory. |
| **CPU Allocation** | 1 Core | Pinned to NUMA Node 0 (CPU 1). Redis is single-threaded; 1 core is optimal. |
| **RAM Allocation** | 512 MB | **Resource Optimization:** Mapped to handle transient rate limits and locks. Low footprint leaves RAM for databases. |
| **Swap** | 0 MB | Disabled to protect NVMe wear profiles. |
| **Static IP Address** | `192.168.1.103/24` | Static local address; gateway set to `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Implements system isolation. Nesting enabled (`nesting=1`). |
| **Availability** | Start on Boot (`true`) | Auto-starts on host power-up. |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Orchestration & Bootstrap (July 2026)
Orchestrated on PVE host using CLI tools:
```bash
pct create 103 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 1 -memory 512 -swap 0 -hostname cache-redis -ostype debian \
  -storage local-lvm -rootfs local-lvm:4 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.103/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
pct start 103
```

### Phase 2: Redis Installation & Optimizations (July 2026)
Installed and tuned the Redis server package. The memory rules were optimized to match the 512MB RAM cap:
```bash
apt update && apt install -y redis-server
```

#### Applied Configuration Changes (`/etc/redis/redis.conf`):
1.  **Network Bind:** Allowed binding on all interfaces to permit communication from other LXC instances:
    `bind 0.0.0.0`
2.  **Memory Caps:** Set max memory limits to prevent out-of-memory container crashes:
    `maxmemory 256mb` (reserves 256MB of RAM for database cache keys, leaving 256MB for OS/networking overhead).
3.  **Eviction Policy:** Enforced volatile least-recently-used key recycling:
    `maxmemory-policy volatile-lru`
4.  **Protected Mode:** Disabled local interface restriction since container ingress is secured via Proxmox Firewall rules:
    `protected-mode no`

```bash
systemctl restart redis-server
```

### Verification
Running `redis-cli ping` returns:
*   **Result:** `PONG` (Active & responding).
