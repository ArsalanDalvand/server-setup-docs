# LXC 100: NATS Message Broker (`nats-broker`)

This document serves as the canonical, atomic provisioning ledger and operations runbook for the NATS Server container (LXC 100). It details the system specifications, configuration logic, and installation steps.

---

## 1. Container Profile

| Property | Value | Architectural Rationale |
| :--- | :--- | :--- |
| **Container ID** | `100` | Dedicated index for the cluster coordination broker. |
| **Hostname** | `nats-broker` | Internal hostname for DNS routing across Stalwart nodes. |
| **Baseline OS** | Debian 12 Standard | Ensures kernel namespace alignment with host environment. |
| **Storage Target** | `local-lvm` (NVMe) | Low latency for runtime configurations and state tracking. |
| **Disk Size** | 2 GB | Minimal disk allocation; NATS runs entirely in memory without disk persistence requirements. |
| **CPU Allocation** | 1 Core | Pinned to NUMA Node 0 (CPU 1). |
| **RAM Allocation** | 256 MB | **Resource Optimization:** Extremely lightweight footprint (sub-20MB active execution). |
| **Swap** | 0 MB | Disabled to preserve flash memory wear profiles. |
| **Static IP Address** | `192.168.1.107/24` | Static local IP, gateway `192.168.1.1`. |
| **Security Flags** | Unprivileged (`true`) | Strict isolation context. Nesting enabled (`nesting=1`). |
| **Availability** | Start on Boot (`true`) | Auto-starts on host boot. |

---

## 2. Step-by-Step Provisioning Log

### Phase 1: Container Creation (July 2026)
Orchestrated on PVE host using the local Debian template:
```bash
pct create 100 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  -cores 1 -memory 256 -swap 0 -hostname nats-broker -ostype debian \
  -storage local-lvm -rootfs local-lvm:2 \
  -net0 name=eth0,bridge=vmbr0,firewall=1,ip=192.168.1.107/24,gw=192.168.1.1 \
  -unprivileged 1 -features nesting=1 -onboot 1
pct start 100
```

### Phase 2: Latest Stable NATS Server Installation (July 2026)
Installed **NATS Server Version `v2.14.3`** from the official upstream releases:
```bash
apt update && apt install -y wget curl
wget https://github.com/nats-io/nats-server/releases/download/v2.14.3/nats-server-v2.14.3-amd64.deb -O /tmp/nats.deb
dpkg -i /tmp/nats.deb
rm -f /tmp/nats.deb
```

### Phase 3: Configuration & Systemd Integration

#### 1. Configuration File (`/etc/nats/nats-server.conf`):
```config
host: "0.0.0.0"
port: 4222
max_connections: 10000
ping_interval: "60s"
max_payload: 8MB
```

#### 2. Systemd Service (`/etc/systemd/system/nats-server.service`):
```ini
[Unit]
Description=NATS Server
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/nats-server -c /etc/nats/nats-server.conf
Restart=on-failure
RestartSec=5s
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable nats-server
systemctl start nats-server
```

---

## 3. Operational Health & Verification
Running `systemctl status nats-server` inside LXC 100 returns:
*   **Service Status:** `active (running)`
*   **Version:** `v2.14.3`
*   **Active Memory Footprint:** `1.9 MB` (Extremely lightweight)
*   **Listener Address:** `0.0.0.0:4222` (Listening for Stalwart node connections)
