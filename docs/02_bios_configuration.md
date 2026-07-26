# BIOS / UEFI Platform Options (RBSU)

## Performance Profile
*   **Workload Profile:** `Static High Performance` (or `Virtualization - Max Performance`).
    *   *Why:* Disables processor C-States and P-States. Prevents CPU cores from dropping frequency to save power. Vital for Redis and FoundationDB to eliminate microsecond-level query response latency.
*   **Intel Turbo Boost Technology:** `Enabled`.

## CPU & Memory Topology
*   **Intel(R) Hyper-Threading:** `Enabled`.
    *   *Why:* Translates 40 physical processor cores into 80 virtual cores (vCPUs) visible to Proxmox. Maximizes parallel transaction handling in Stalwart and matrix operations in the ONNX Face Engine.
*   **Node Interleaving:** `Disabled`.
    *   *Why:* **Enforces NUMA.** Keeps memory access local to the CPU socket. This allows us to run FoundationDB directly on CPU 1 (which handles the NVMe controller) without cross-socket bus penalties.
*   **Processor x2APIC Support:** `Enabled`.
    *   *Why:* Optimizes interrupt handling for high-density networking and high-concurrency systems.

## Security & Boot
*   **Boot Mode:** `UEFI Mode`.
    *   *Why:* Mandated for GPT partition tables and Secure Boot compliance on modern Proxmox VE 9.x kernels.
*   **Secure Boot:** `Enabled`.
    *   *Why:* Validates bootloader integrity and removes the critical security warning in the HPE iLO dashboard.

## Availability & Thermal
*   **ASR (Automatic System Recovery) Status:** `Enabled` (Timeout: 10 minutes).
    *   *Why:* Automatically power cycles the server if the Linux kernel freezes or panics in the datacenter.
*   **Automatic Power-On:** `Always Power On` (with 60s delay).
    *   *Why:* Automatically boots up the server after power outages.
*   **Thermal Configuration:** `Optimal Cooling` (reduces fan noise during home staging).
