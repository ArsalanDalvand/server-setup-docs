# LXC 103: Redis Cache (`cache-redis`)

This document serves as the atomic provisioning log, configuration mapping, and operations runbook for the Redis caching container.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `103` | Unique index for the Redis store in PVE. |
| **Hostname** | `cache-redis` | Internal hostname for DNS caching requests. |
| **Baseline OS** | Debian 12 Standard | Matches database tier library conventions. |
| **Storage Target** | `local-lvm` (NVMe) | Rapid disk writes for Redis snapshot dumping. |
| **Disk Size** | 10 GB | Small allocation; memory-first architecture. |
| **CPU Allocation** | 2 Cores | Pinned to NUMA Node 0 (CPU 1) for minimal memory latency. |
| **RAM Allocation** | 4096 MB (4 GB) | Allocated for in-memory operations and rate-limiting keys. |
| **Static IP Address** | `192.168.1.103/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Standard process containment. Nesting enabled (`nesting=1`). |

---

## 2. Provisioning & Implementation Plan

### Phase 1: Container Creation
Orchestrated on PVE host using the local Debian template:
```bash
pct create 103 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 2 -memory 4096 -swap 0 -hostname cache-redis -ostype debian \
  -storage local-lvm -rootfs local-lvm:10 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.103/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
```

### Phase 2: Redis Installation & Optimizations
1.  Installation of Redis server from stable Debian packages:
    ```bash
    apt update && apt install -y redis-server
    ```
2.  Memory configuration tuning (`/etc/redis/redis.conf`):
    *   Set `maxmemory 3gb` (reserves 1GB for system execution).
    *   Set `maxmemory-policy volatile-lru` (automatically evicts expired cache items).
    *   Bind exclusively to internal network addresses.
