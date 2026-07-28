# Datacenter Colocation Setup Guide & Request Specifications

This document defines the exact technical specifications and requirements to submit to the hosting provider (datacenter) when requesting a 1U colocation slot for the HPE ProLiant DL360 Gen10 server. It also outlines the post-deployment IP mapping instructions.

---

## 1. Datacenter Colocation Request Template ( Persian / English )

Copy and submit the following specifications to your datacenter sales/technical support department:

```text
موضوع: درخواست کولوکیشن ۱ یونیت (Colocation 1U) - سرور HPE Gen10

با سلام و احترام،
احتراماً جهت میزبانی (کولوکیشن) یک عدد سرور فیزیکی خود، درخواست تخصیص فضا با مشخصات فنی زیر را دارم:

1. مشخصات فیزیکی سرور:
   - مدل: HPE ProLiant DL360 Gen10 (تک یونیت - 1U)
   - ریل‌کیت: ریل کیت اورجینال ۱ یونیت HP (توسط اینجانب تحویل داده خواهد شد).

2. توان مصرفی (Power Allocation):
   - حداکثر برق مصرفی مورد نیاز: 500 وات (یا تخصیص حداقل 2.5 آمپر جریان برق). سرور مجهز به یک عدد پاور 800W می‌باشد.

3. اتصالات شبکه و پورت‌های سوییچ (Uplink Ports & Transceivers):
   - کارت شبکه ۱۰ گیگابایت اصلی سرور: مدل HPE Ethernet 10Gb 2-port 562FLR-SFP+ Adapter
   - نیاز به ۲ عدد ماژول 10G SFP+ (ترجیحاً Cisco Compatible سازگار با سوییچ دیتاسنتر و سرور) جهت اتصال فیبر نوری/مسی سوییچ به سرور.

4. آدرس‌های آی‌پی درخواستی (IP Addresses):
   - تخصیص یک ساب‌نت /30 یا ۴ عدد آدرس IP پابلیک استاتیک (Static Public IPs):
     * آی‌پی اول: جهت هاست اصلی Proxmox VE (هایپروایزر).
     * آی‌پی دوم: جهت سرویس اصلی ایمیل سرور و گیت‌وی (MTA Inbound/Outbound).
     * آی‌پی سوم: جهت سرویس‌های احراز هویت و دسترسی کلاینت‌ها (Submission / Access Layer).
     * آی‌پی چهارم: جهت پورت مدیریت مجزا iLO 5 (یا تخصیص در رنج پرایوت/VPN دیتاسنتر جهت حفظ امنیت).

خواهشمند است رنج آی‌پی‌های تخصیص یافته به همراه مشخصات Gateway و Netmask را جهت کانفیگ اولیه ارسال فرمایید.

با تشکر.
```

---

### 3. Switch Interface Uplink Requirements (SFP+ 10G)

For the server to successfully connect to the datacenter's 10G network backbone via the **HPE Ethernet 10Gb 2-port 562FLR-SFP+ Adapter (Intel X710)**, the physical link must be populated with optical transceivers. 

*   **Total Transceivers Required:** **`2x` SFP+ Modules**
    *   *Transceiver 1:* Plugged into Port 1 of the server's HPE 562FLR-SFP+ Adapter.
    *   *Transceiver 2:* Plugged into the corresponding uplink port on the Datacenter's Top-of-Rack (ToR) Switch.
*   **Target Specifications:**
    *   **Form Factor:** `SFP+` (10 Gigabit)
    *   **Standard:** `10GBASE-SR` (Short Range)
    *   **Wavelength / Media:** `850nm` Multi-Mode Fiber (MMF)
    *   **Connector Type:** `LC Duplex`
    *   **Compatibility Encoding:**
        *   *Server-Side Transceiver:* **HPE Compatible** (P/N: `455883-B21`). The HPE Gen10 firmware rejects generic/non-HP coded modules.
        *   *Switch-Side Transceiver:* Must match the datacenter's switch vendor code (e.g., Cisco, Juniper, Arista, etc.). Inform the datacenter to provide or confirm their switch-side module vendor compatibility.

---

## 2. iLO & Proxmox IP Transition Runbook

Once the datacenter allocates the IP addresses, you must update the temporary staging IPs (`192.168.1.x`) to the production datacenter IPs.

### Step 1: iLO 5 IP Configuration
Ensure iLO is accessible over the datacenter management network:
1. Log in to iLO Web GUI (`https://192.168.1.2`).
2. Navigate to **iLO Dedicated Network Port -> IPv4**.
3. Set **DHCPv4** to `Disabled`.
4. Enter the new Datacenter Management IP, Subnet Mask, and Gateway.
5. Click **Submit**. (Note: iLO network stack will restart; future connections must use the new IP).

### Step 2: Proxmox VE Host Network Configuration
To update the hypervisor host to the new public IP:
1. SSH into the Proxmox host: `ssh root@192.168.1.100` (or use the iLO HTML5 CLI console if already moved).
2. Edit the network configuration:
   ```bash
   nano /etc/network/interfaces
   ```
3. Locate the `vmbr0` (Virtual Bridge) section and replace the home configuration with datacenter values:
   ```text
   auto vmbr0
   iface vmbr0 inet static
           address <DATACENTER_PUBLIC_IP>/<CIDR_NETMASK>
           gateway <DATACENTER_GATEWAY_IP>
           bridge-ports eno1
           bridge-stp off
           bridge-fd 0
   ```
4. Save and exit (`Ctrl+O`, `Ctrl+X`).
5. Update local hostname maps:
   ```bash
   nano /etc/hosts
   ```
   Ensure the entry for `pve.arsalandalvand.tech` points to the new `<DATACENTER_PUBLIC_IP>`.
6. Update DNS resolvers:
   ```bash
   nano /etc/resolv.conf
   ```
   Replace `nameserver 192.168.1.1` with the datacenter DNS or public DNS:
   ```text
   nameserver 8.8.8.8
   nameserver 1.1.1.1
   ```
7. Reboot the server to apply all changes:
   ```bash
   reboot
   ```
