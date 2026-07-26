# Proxmox VE 9.2 Host Setup

## Network Mapping (Home Stage)
*   **Active Port:** `nic0` (Embedded LOM 1 Port 1, driver: `tg3`).
*   **IP Address:** `192.168.1.100/24` (static).
*   **Gateway:** `192.168.1.1` (home router).
*   **DNS Resolver:** `192.168.1.1`.
*   **Hostname:** `pve.arsalandalvand.tech`.

## Software Repositories Setup
Proxmox Enterprise repositories are disabled to prevent authorization failures. Free community repositories are activated:

1.  **Main PVE Repository:** Disabled `pve-enterprise` and enabled `pve-no-subscription`.
2.  **Ceph Repository:** Disabled `ceph-squid` enterprise repository to prevent unauthorized errors. No-subscription Ceph is omitted as a single-node setup does not utilize clustering.
3.  **Update Verification:** System updated successfully to Kernel version `7.0.14-6-pve`.

---

## Post-Installation Host System Configuration

### 1. Subscription Warning Banner Removal
To disable the default "No Valid Subscription" GUI pop-up:
```bash
sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{\/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
systemctl restart pveproxy.service
```

### 2. Physical System Beep Removal
To prevent physical PC speakers from sounding warnings during kernel events in the staging space:
```bash
echo 'blacklist pcspkr' >> /etc/modprobe.d/nobeep.conf
```

### 3. SAS Disk LVM-Thin CLI Initialization
To bypass GUI limitations when creating LVM-Thin pools on raw disks:
```bash
vgcreate vg_sas /dev/sda
lvcreate -L 500G -T vg_sas/sas-storage
pvesm add lvmthin sas-storage --vgname vg_sas --thinpool sas-storage
```
This registers `sas-storage` as a 500GB pool, leaving reserve space in `vg_sas` for future volume expansions.
