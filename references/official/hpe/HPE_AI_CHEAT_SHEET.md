---
title: HPE ProLiant Gen10 — AI Cheatsheet & CLI Reference
tags: [hpe, ilo, uefi, bios, sysadmin, storage, smart-array]
---

# HPE ProLiant Gen10 — AI Cheatsheet & CLI Reference

> [!success] Comprehensive technical summary for HPE iLO 5, UEFI/BIOS and RAID configurations.

---

## 1. Out-of-Band Management: iLO 5 SSH CLI Reference

When connected to iLO 5 via SSH, the **SMASH CLP** (Systems Management Architecture for Server Hardware Command Line Protocol) is used.

### Basic CLI Syntax

*   **View Host Power Status:**
    ```bash
    show /system1
    ```
*   **Power On Server:**
    ```bash
    start /system1
    ```
*   **Graceful Shutdown (Momentary Press):**
    ```bash
    stop /system1
    ```
*   **Hard Power Off (Press and Hold):**
    ```bash
    stop /system1 -force
    ```
*   **Reset Server (Warm Boot):**
    ```bash
    reset /system1
    ```
*   **Show Temperature Sensors:**
    ```bash
    show /system1/sensor1
    ```
*   **Reset iLO Management Processor (Without affecting host):**
    ```bash
    reset /map1
    ```

### iLO Hardening & Best Practices

1.  **VLAN Segregation:** Place the iLO network interface on an isolated VLAN. Do not expose iLO ports (443, 22, 23, 623) to the public internet.
2.  **Encryption and Console Locks:** Enable Remote Console encryption in `iLO Web Interface > Administration > Access Settings`.

---

## 2. UEFI / BIOS Configurations for Virtualization & Databases

To configure CPU, memory, and virtualization settings on the DL360 Gen10 (2x Xeon Gold 6248) for running Proxmox VE 9.x:

### Core Configuration Parameters (UEFI F9 Menu)

*   **Virtualization Options:**
    *   `Intel VT-x` -> **Enabled** (Required for hardware virtualization).
    *   `Intel VT-d` -> **Enabled** (Required for PCIe / NVMe passthrough to VM/LXC).
*   **Processor Options:**
    *   `Hyper-Threading` -> **Enabled** (Doubles execution threads, essential for asynchronous Stalwart sockets).
    *   `Processor x2APIC Support` -> **Enabled** (Improves interrupt steering on multi-socket CPU topologies).
*   **Memory / NUMA Configuration:**
    *   `Node Interleaving` -> **Disabled** (Crucial. Keeps NUMA nodes distinct so memory allocations remain bound to local CPU sockets).
    *   `Channel Interleaving` -> **Enabled** (Optimizes memory throughput).
*   **Power Regulator Settings:**
    *   `HPE Power Regulator` -> **Static High Performance** (Disables CPU throttling / dynamic voltage scaling. Prevents database write/fsync latency spikes on FoundationDB/Redis).

---

## 3. RAID Storage Controller Management: `ssacli`

If the DL360 Gen10 is equipped with an HPE Smart Array (P408i-a), it is managed in Linux using `ssacli` (Smart Storage Administrator CLI).

### Common `ssacli` Commands

*   **Show Detailed Controller and Drive Info:**
    ```bash
    ssacli ctrl all show config detail
    ```
*   **Show Battery/Capacitor Status (Smart Storage Battery):**
    ```bash
    ssacli ctrl slot=0 show status
    ```
*   **Create Hardware RAID 1 (Mirrored OS Drives):**
    ```bash
    ssacli ctrl slot=0 create type=ld drives=1I:1:1,1I:1:2 rar=1 size=max
    ```
*   **Create Hardware RAID 0 (For SAS scratch/temporary storage):**
    ```bash
    ssacli ctrl slot=0 create type=ld drives=1I:1:3 rar=0 size=max
    ```
*   **Enable Cache (Write-Back) on a Specific Logical Drive:**
    ```bash
    ssacli ctrl slot=0 ld 1 modify arrayaccelerator=enable
    ```
*   **Disable cache on a physical controller (Passthrough/HBA mode):**
    ```bash
    ssacli ctrl slot=0 modify drivewritecache=disable
    ```

---

## 4. Hardware Sizing & Redundancy

*   **DIMM Population Rules:** For Intel Xeon Gold 6248 (6 memory channels), install RAM in sets of 6 or 12 DIMMs per socket to optimize memory channel bandwidth.
*   **Dual PSU Redundancy:** DL360 Gen10 dual power supplies operate in active-passive load balancing by default. If cycling power completely, you must unplug **both** PSU cables.
