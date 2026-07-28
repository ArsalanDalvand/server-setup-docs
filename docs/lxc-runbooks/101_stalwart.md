# LXC 101: Stalwart MTA Inbound/Outbound (`stalwart-mta`)

This document serves as the canonical, atomic provisioning ledger for the Stalwart MTA node handling Port 25 traffic.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `101` | Dedicated index for Port 25 MTA traffic. |
| **Hostname** | `stalwart-mta` | Internal routing hostname. |
| **Baseline OS** | Debian 12 Standard | Ensures systemd/glibc compatibility with host environment. |
| **Storage Target** | `local-lvm` (NVMe) | High IOPS for raw email queue buffering. |
| **Disk Size** | 20 GB | Queue data and transient buffer storage. |
| **CPU Allocation** | 2 Cores | Pinned to NUMA Node 1 (CPU 2). |
| **RAM Allocation** | 2048 MB (2 GB) | Optimized for receiving external SMTP traffic. |
| **Swap** | 0 MB | Disabled to preserve flash memory wear profiles. |
| **Static IP Address** | `192.168.1.101/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Strict isolation context. Nesting enabled (`nesting=1`). |
| **Availability** | Start on Boot (`true`) | Auto-starts on host boot. |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Orchestration (July 2026)
Created via Proxmox CLI:
```bash
pct create 101 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 2 -memory 2048 -swap 0 -hostname stalwart-mta -ostype debian \
  -storage local-lvm -rootfs local-lvm:20 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.101/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
pct start 101
```
