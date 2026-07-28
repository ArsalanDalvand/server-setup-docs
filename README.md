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
*   [**08_datacenter_transition.md**](docs/08_datacenter_transition.md)
    *   *Focus:* Detailed hardware colocation request specifications and IP configuration modification guides.
*   [**LXC Runbooks Directory**](docs/lxc-runbooks/)
    *   *Focus:* Atomic, dedicated runbooks for provisioning and managing individual containers:
        *   [**100_nats.md**](docs/lxc-runbooks/100_nats.md) - NATS Cluster Coordination Broker.
        *   [**101_stalwart.md**](docs/lxc-runbooks/101_stalwart.md) - Stalwart MTA Port 25 Inbound/Outbound.
        *   [**108_stalwart_submit.md**](docs/lxc-runbooks/108_stalwart_submit.md) - Stalwart Authenticated Submissions (Ports 587/465).
        *   [**105_stalwart_access.md**](docs/lxc-runbooks/105_stalwart_access.md) - Stalwart User Access (IMAP, JMAP, DAV).
        *   [**102_foundationdb.md**](docs/lxc-runbooks/102_foundationdb.md) - FoundationDB Core Database.
        *   [**103_redis.md**](docs/lxc-runbooks/103_redis.md) - Redis Cache Cluster.
        *   [**104_minio.md**](docs/lxc-runbooks/104_minio.md) - MinIO S3 Object Storage.

---
*Created: July 2026*
*Author: Agent Arsalan*
