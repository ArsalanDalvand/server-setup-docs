# Target Service & Microservice Architecture

To support 100,000 active concurrent users under a 32GB RAM footprint, the network and microservices are containerized using **LXC (Linux Containers)** to eliminate the virtualization overhead associated with full VM kernels.

## Base Operating System Selection: Debian 12 (Bookworm)
All LXC containers utilize **Debian 12** as their baseline distribution. The architectural rationale for this choice is:
1. **Host-Container Kernel Alignment:** Proxmox VE 9.x is built directly on Debian 12. Aligning the container base OS with the host kernel optimizes shared library linking and guarantees 100% systemd syscall compatibility, minimizing execution faults.
2. **Library Compatibility (glibc vs musl):** While Alpine Linux is lighter, it uses `musl libc`. Critical services like Apple FoundationDB and Rust-based Stalwart Mail Server rely on compiled `glibc` libraries. Running them on Debian eliminates translation layers and performance overhead.
3. **RAM Optimization:** Debian 12 provides a bare minimum memory footprint (under 30MB RAM in idle states), leaving maximum capacity for caching in FoundationDB and Redis.
4. **Lifecycle & Security:** Debian's conservative release cycles prioritize security and long-term package stability over cutting-edge features, matching enterprise SLA standards.

## Microservices Service-Specific Topology (Final Provisioned Topology)

| Container ID | Service Name | Role & Protocols | RAM Limit | Disk Pool | Static IP | NUMA Pinning |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **LXC 100** | `nats-broker` | Cluster Coordination Broker (NATS v2.14.3) | 256 MB | NVMe (`local-lvm`) | `192.168.1.107` | CPU 1 (NUMA 0) |
| **LXC 101** | `stalwart-mta` | MTA Relay (Port 25 Inbound/Outbound) | 2048 MB | NVMe (`local-lvm`) | `192.168.1.101` | CPU 2 (NUMA 1) |
| **LXC 108** | `stalwart-submit` | Authenticated Submission (Ports 587/465) | 4096 MB | NVMe (`local-lvm`) | `192.168.1.108` | CPU 2 (NUMA 1) |
| **LXC 105** | `stalwart-access` | Client Access Layer (IMAP, JMAP, CalDAV/CardDAV) | 3072 MB | NVMe (`local-lvm`) | `192.168.1.105` | CPU 1 (NUMA 0) |
| **LXC 102** | `fdb-db` | Core Transactional Database (FoundationDB 7.3.77) | 8192 MB | NVMe (`local-lvm`) | `192.168.1.102` | CPU 1 (NUMA 0) |
| **LXC 103** | `cache-redis` | In-Memory Rate Limiter & Token Cache (Redis) | 512 MB | NVMe (`local-lvm`) | `192.168.1.103` | CPU 1 (NUMA 0) |
| **LXC 104** | `storage-s3` | S3 Object Storage (Silo/MinIO 2026) | 1024 MB | SAS (`sas-storage`) | `192.168.1.104` | CPU 2 (NUMA 1) |

---

## NUMA Resource Partitioning & Hardware Affinity

### NUMA Node 0 (CPU 1 + Memory 16GB + PCIe Slot 1/NVMe)
*   **Services:** `fdb-db` (LXC 102), `cache-redis` (LXC 103), `nats-broker` (LXC 100), and `stalwart-access` (LXC 105).
*   **Design Logic:** FoundationDB, Redis, NATS, and the IMAP/JMAP access layer require maximum memory access speeds and minimum disk read latency. Pining them to CPU Socket 1 (which hosts the PCIe NVMe riser bus) completely eliminates cross-socket UPI (Ultra Path Interconnect) interconnect latencies.

### NUMA Node 1 (CPU 2 + Memory 16GB + SAS Storage)
*   **Services:** `stalwart-mta` (LXC 101), `stalwart-submit` (LXC 108), and `storage-s3` (LXC 104).
*   **Design Logic:** `stalwart-submit` and `stalwart-mta` handle CPU-intensive TLS encryption/decryption handshakes, DKIM signing, and DMARC verifications. Placing them on CPU Socket 2 isolates computational processing spikes from the primary database transaction bus on CPU Socket 1. MinIO routes large payload writes through the SAS Smart Array controller on this socket.
