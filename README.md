# HPE ProLiant DL360 Gen10 Architecture Ledger

This ledger acts as the central index and entry point for all architectural configurations, deployment strategies, and runbooks applied to Arsalan's HPE ProLiant DL360 Gen10 server.

## Directory Structure & Ledger Mapping

To maintain modularity and atomic documentation, the configurations are divided into focused directories under `/docs/`:

*   [**01_hardware_inventory.md**](docs/01_hardware_inventory.md)
    *   *Focus:* Complete physical inventory, memory placements, and PCI-E slot mappings.
*   [**02_bios_configuration.md**](docs/02_bios_configuration.md)
    *   *Focus:* Setup of UEFI boot parameters, hardware profiles, NUMA configuration, Hyper-Threading, and cooling policies.
*   [**03_ilo_hardening.md**](docs/03_ilo_hardening.md)
    *   *Focus:* OOB management settings, SNMPv3, HTTPS-only rules, and console locking.
*   [**04_storage_architecture.md**](docs/04_storage_architecture.md)
    *   *Focus:* Detailed disk design, ext4/LVM-Thin configuration logic, and the FoundationDB fsync mitigation strategy.
*   [**05_virtualization_layer.md**](docs/05_virtualization_layer.md)
    *   *Focus:* Proxmox VE installation settings, host networking, repository setup, and licensing adjustments.
*   [**06_service_architecture.md**](docs/06_service_architecture.md)
    *   *Focus:* Network design for the Stalwart, FoundationDB, Redis, and MinIO microservices, including NUMA pinning targets.
*   [**07_lxc_containers.md**](docs/07_lxc_containers.md)
    *   *Focus:* Detailed provisioning runbooks, resource profiles, and installation status for individual LXC containers.

---
*Created: July 2026*
*Author: Agent Arsalan*
