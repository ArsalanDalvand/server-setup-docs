# Physical Hardware Inventory

## Server Base
*   **Product Name:** HPE ProLiant DL360 Gen10
*   **Form Factor:** 1U Rackmount
*   **Chassis Type:** 8SFF (Small Form Factor) Front Drive Bays

## Processing & Memory
*   **Processors (CPU):** 2x Intel(R) Xeon(R) Gold 6248 CPU @ 2.50GHz (20 Cores, 40 Threads each; Total 40 Cores, 80 Threads).
*   **Memory (RAM):** 32 GB Total Configuration:
    *   `PROC 1 DIMM 8`: 16 GiB RDIMM DDR4-2666 (Smart Memory)
    *   `PROC 2 DIMM 8`: 16 GiB RDIMM DDR4-2666 (Smart Memory)
    *   *NUMA Architecture:* Active. 16GB allocated directly to each processor socket.

## Storage Controller & Accessories
*   **Smart Array:** HPE Smart Array P408i-a SR Gen10 Controller (8 Internal Lanes, 2GB Cache).
*   **Battery:** HPE 96W Smart Storage Lithium-ion Battery with 145mm Cable Kit (protects Write-Back cache memory).

## Physical Disks & Expansion Slots
*   **System Disk (PCIe Slot 1):** Lexar NM620 M.2 256GB NVMe SSD mounted on a single-slot PCIe x4 adapter (wired to CPU 1).
*   **Data Disk (SFF Front Bay):** 1x HPE 600GB SAS 10K SFF HDD.
*   **Rail Kit:** HP DL360 GEN10 1U SFF Easy Install Rail Kit.

## Network Interfaces
*   **10G NIC (FlexibleLOM Slot):** HPE Ethernet 10Gb 2-port 562FLR-SFP+ Adapter (driver: `i40e`, MACs: `48:df:37:b6:8e:b0`, `48:df:37:b6:8e:b8`).
*   **1G NIC (Embedded LOM):** HPE Ethernet 1Gb 4-port 331i Adapter (driver: `tg3`, MACs: `08:f1:ea:7e:19:1c` through `1f`). Port 1 is the active link for home testing.
