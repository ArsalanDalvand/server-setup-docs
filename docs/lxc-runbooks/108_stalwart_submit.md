# LXC 108: Stalwart Authenticated Submissions (`stalwart-submit`)

This document serves as the canonical, atomic provisioning ledger for the Stalwart Submission node handling Ports 587/465 traffic.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `108` | Dedicated index for authenticated user submissions. |
| **Hostname** | `stalwart-submit` | Internal routing hostname. |
| **Baseline OS** | Debian 12 Standard | Ensures systemd/glibc compatibility with host environment. |
| **Storage Target** | `local-lvm` (NVMe) | High IOPS for client transaction encryption & queues. |
| **Disk Size** | 20 GB | Queue data and transient buffer storage. |
| **CPU Allocation** | 4 Cores | High CPU allocation for parallel TLS handshakes and authentication checks. |
| **RAM Allocation** | 4096 MB (4 GB) | Extended memory profile to guarantee zero throttling during peak user authentication traffic. |
| **Swap** | 0 MB | Disabled to preserve flash memory wear profiles. |
| **Static IP Address** | `192.168.1.108/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Strict isolation context. Nesting enabled (`nesting=1`). |
| **Availability** | Start on Boot (`true`) | Auto-starts on host boot. |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Orchestration (July 2026)
Created via Proxmox CLI:
```bash
pct create 108 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 4 -memory 4096 -swap 0 -hostname stalwart-submit -ostype debian \
  -storage local-lvm -rootfs local-lvm:20 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.108/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
pct start 108
```
