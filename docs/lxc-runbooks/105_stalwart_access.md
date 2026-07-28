# LXC 105: Stalwart User Access Layer (`stalwart-access`)

This document serves as the canonical, atomic provisioning ledger for the Stalwart User Access node handling IMAP, JMAP, and CalDAV/CardDAV protocols.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `105` | Dedicated index for client access protocols (IMAP, JMAP, DAV). |
| **Hostname** | `stalwart-access` | Internal routing hostname. |
| **Baseline OS** | Debian 12 Standard | Ensures systemd/glibc compatibility with host environment. |
| **Storage Target** | `local-lvm` (NVMe) | High IOPS for mailbox indexing and IMAP IDLE connection states. |
| **Disk Size** | 20 GB | Queue data and transient buffer storage. |
| **CPU Allocation** | 2 Cores | Pinned to NUMA Node 0 (CPU 1) close to FoundationDB. |
| **RAM Allocation** | 3072 MB (3 GB) | Consolidated memory pool to serve concurrent IMAP, JMAP, and DAV sessions efficiently. |
| **Swap** | 0 MB | Disabled to preserve flash memory wear profiles. |
| **Static IP Address** | `192.168.1.105/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Strict isolation context. Nesting enabled (`nesting=1`). |
| **Availability** | Start on Boot (`true`) | Auto-starts on host boot. |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Orchestration (July 2026)
Created via Proxmox CLI:
```bash
pct create 105 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 2 -memory 3072 -swap 0 -hostname stalwart-access -ostype debian \
  -storage local-lvm -rootfs local-lvm:20 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.105/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
pct start 105
```
