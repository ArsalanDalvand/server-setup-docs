# Target Service & Microservice Architecture

To support 100,000 active concurrent users under a 32GB RAM footprint, the network and microservices are containerized using **LXC (Linux Containers)** to eliminate the virtualization overhead associated with full VM kernels.

## Base Operating System Selection: Debian 12 (Bookworm)
All LXC containers utilize **Debian 12** as their baseline distribution. The architectural rationale for this choice is:
1. **Host-Container Kernel Alignment:** Proxmox VE 9.x is built directly on Debian 12. Aligning the container base OS with the host kernel optimizes shared library linking and guarantees 100% systemd syscall compatibility, minimizing execution faults.
2. **Library Compatibility (glibc vs musl):** While Alpine Linux is lighter, it uses `musl libc`. Critical services like Apple FoundationDB and Rust-based Stalwart Mail Server rely on compiled `glibc` libraries. Running them on Debian eliminates translation layers and performance overhead.
3. **RAM Optimization:** Debian 12 provides a bare minimum memory footprint (under 30MB RAM in idle states), leaving maximum capacity for caching in FoundationDB and Redis.
4. **Lifecycle & Security:** Debian's conservative release cycles prioritize security and long-term package stability over cutting-edge features, matching enterprise SLA standards.

## Container Network Architecture (Proposed)
*   **Virtual Bridge:** All containers communicate via Proxmox's default bridge `vmbr0`.
*   **SSL Gateway (NGINX / HAProxy):** Acts as the ingress controller. Exposes ports `25` (SMTP), `143` (IMAP), `993` (IMAPS), `465` (SMTPS), and `443` (HTTPS) to the public interface. Proxies unencrypted traffic inside the internal bridge.

## Microservices Topology (32GB Profile)

| Container ID | Service | Type | RAM | Disk Target | NUMA CPU Pinning |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **LXC 100** | NGINX / Gateway | Ingress | 512 MB | NVMe | CPU 1 (Cores 0-19) |
| **LXC 101** | Stalwart Mail Server | Async MTA/IMAP | 4 GB | NVMe | CPU 2 (Cores 20-39) |
| **LXC 102** | FoundationDB (FDB) | Core Database | 8 GB | NVMe | CPU 1 (Cores 0-19) |
| **LXC 103** | Redis | In-Memory Store | 4 GB | NVMe | CPU 1 (Cores 0-19) |
| **LXC 104** | MinIO | S3 Storage | 2 GB | SAS | CPU 2 (Cores 20-39) |

## NUMA Resource Partitioning & Hardware Affinity

### NUMA Node 0 (CPU 1 + Memory 16GB + PCIe Slot 1/NVMe)
*   **Services:** FoundationDB (`LXC 102`) and Redis (`LXC 103`).
*   **Design Logic:** Both databases demand high I/O throughput and extremely low memory access latency. Pining them to the CPU socket directly attached to the NVMe host bus adapter completely bypasses UPI (Ultra Path Interconnect) cross-socket bus penalties, reducing database transaction response times.

### NUMA Node 1 (CPU 2 + Memory 16GB + SAS Storage)
*   **Services:** Stalwart Mail Server (`LXC 101`) and MinIO (`LXC 104`).
*   **Design Logic:** Stalwart manages parallel SSL handshake execution and network socket pooling. It is compute-bound rather than memory-latency bound. MinIO handles large file writes routed through the SAS controller. Isolating these services on CPU 2 prevents processing spikes from throttling the critical database transaction queues on CPU 1.
