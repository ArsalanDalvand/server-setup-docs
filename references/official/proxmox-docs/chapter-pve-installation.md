---
title: "Installing Proxmox VE"
source: "https://pve.proxmox.com/pve-docs/chapter-pve-installation.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "chapter-pve-installation"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "Installing Proxmox VE"
---
[â](#)

Proxmox VE is based on Debian GNU/Linux. The install disk images (ISO files)
provided by Proxmox include a complete Debian system as well as all necessary
Proxmox VE packages. [](#chapter_installation)

See the [[chapter-pve-faq.html#faq-support-table|support table in the FAQ]] for the
relationship between Proxmox VE releases and Debian releases.

The installer guides you through the setup, allowing you to partition the
local disk(s), apply basic system configurations (for example, timezone,
language, network), and install all required packages. This process should not
take more than a few minutes. Installing with the provided ISO is the
recommended method for new and existing users.

Alternatively, Proxmox VE can be installed on top of an existing Debian system. This
option is only recommended for advanced users because detailed knowledge about
Proxmox VE is required.

## System Requirements

We recommend using high quality server hardware, when running Proxmox VE in
production. To further decrease the impact of a failed host, you can run Proxmox VE in
a cluster with highly available (HA) virtual machines and containers.

Proxmox VE can use local storage (DAS), SAN, NAS, and distributed storage like Ceph
RBD. For details see [[chapter-pvesm.html#chapter_storage|chapter storage]].

### Minimum Requirements, for Evaluation

These minimum requirements are for evaluation purposes only and should not be
used in production.

CPU: 64bit (Intel 64 or AMD64)

Intel VT/AMD-V capable CPU/motherboard for KVM full virtualization support

RAM: 1 GB RAM, plus additional RAM needed for guests

Hard drive

One network card (NIC)

### Recommended System Requirements

Intel 64 or AMD64 with Intel VT/AMD-V CPU flag.

Memory: Minimum 2 GB for the OS and Proxmox VE services, plus designated memory for
  guests. For Ceph and ZFS, additional memory is required; approximately 1GB of
  memory for every TB of used storage.

Fast and redundant storage, best results are achieved with SSDs.

OS storage: Use a hardware RAID with battery protected write cache (“BBU”)
  or non-RAID with ZFS (optional SSD for ZIL).

VM storage:

For local storage, use either a hardware RAID with battery backed write cache
  (BBU) or non-RAID for ZFS and Ceph. Neither ZFS nor Ceph are compatible with a
  hardware RAID controller.

Shared and distributed storage is possible.

SSDs with Power-Loss-Protection (PLP) are recommended for good performance.
  Using consumer SSDs is discouraged.

Redundant (Multi-)Gbit NICs, with additional NICs depending on the preferred
  storage technology and cluster setup.

For PCI(e) passthrough, the CPU and motherboard need to support IOMMU
  (Intel VT-d, AMD-Vi).

IOMMU usually also needs to be enabled in the BIOS/UEFI. See the
[[chapter-qm.html#qm_pci_passthrough|PCI(e) Passthrough]] chapter for details.

### Simple Performance Overview

To get an overview of the CPU and hard disk performance on an installed Proxmox VE
system, run the included pveperf tool.

This is just a very quick and general benchmark. More detailed tests are
recommended, especially regarding the I/O performance of your system.

### Supported Web Browsers for Accessing the Web Interface

To access the web-based user interface, we recommend using one of the following
browsers:

Firefox, a release from the current year, or the latest Extended Support Release

Chrome, a release from the current year

Microsoft’s currently supported version of Edge

Safari, a release from the current year

When accessed from a mobile device, Proxmox VE will show a lightweight, touch-based
interface.

## Prepare Installation Media

Download the installer ISO image from: [https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)

The Proxmox VE installation media is a hybrid ISO image. It works in two ways:

An ISO image file ready to burn to a CD or DVD.

A raw sector (IMG) image file ready to copy to a USB flash drive (USB stick).

Using a USB flash drive to install Proxmox VE is the recommended way because it is
the faster option.

### Prepare a USB Flash Drive as Installation Medium

The flash drive needs to be larger than the ISO image.

Do not use UNetbootin. It does not work with the Proxmox VE installation image.

Make sure that the USB flash drive is not mounted and does not
contain any important data.

### Instructions for GNU/Linux

On Unix-like operating system use the dd command to copy the ISO image to the
USB flash drive. First find the correct device name of the USB flash drive (see
below). Then run the dd command.

```bash
# dd bs=1M conv=fdatasync if=./proxmox-ve_*.iso of=/dev/XYZ
```

Be sure to replace /dev/XYZ with the correct device name and adapt the
input filename (if) path.

Be very careful, and do not overwrite the wrong disk!

#### Find the Correct USB Device Name

There are two ways to find out the name of the USB flash drive. The first one is
to compare the last lines of the dmesg command output before and after
plugging in the flash drive. The second way is to compare the output of the
lsblk command. Open a terminal and run:

```bash
# lsblk
```

Then plug in your USB flash drive and run the command again:

```bash
# lsblk
```

A new device will appear. This is the one you want to use. To be on the extra
safe side check if the reported size matches your USB flash drive.

### Instructions for macOS

Open the terminal (query Terminal in Spotlight).

Convert the .iso file to .dmg format using the convert option of hdiutil,
for example:

```bash
# hdiutil convert proxmox-ve_*.iso -format UDRW -o proxmox-ve_*.dmg
```

macOS tends to automatically add .dmg to the output file name.

To get the current list of devices run the command:

```bash
# diskutil list
```

Now insert the USB flash drive and run this command again to determine which
device node has been assigned to it. (e.g., /dev/diskX).

```bash
# diskutil list
# diskutil unmountDisk /dev/diskX
```

replace X with the disk number from the last command.

```bash
# sudo dd if=proxmox-ve_*.dmg bs=1M of=/dev/rdiskX
```

rdiskX, instead of diskX, in the last command is intended. It will
increase the write speed.

### Instructions for Windows

#### Using Etcher

Etcher works out of the box. Download Etcher from [https://etcher.io](https://etcher.io). It will
guide you through the process of selecting the ISO and your USB flash drive.

#### Using Rufus

Rufus is a more lightweight alternative, but you need to use the DD mode to
make it work. Download Rufus from [https://rufus.ie/](https://rufus.ie/). Either install it or use
the portable version. Select the destination drive and the Proxmox VE ISO file.

Once you Start you have to click No on the dialog asking to
download a different version of GRUB. In the next dialog select the DD mode.

## Using the Proxmox VE Installer

The installer ISO image includes the following:

Complete operating system (Debian GNU/Linux, 64-bit)

The Proxmox VE installer, which partitions the local disk(s) with ext4, XFS,
  BTRFS (technology preview), or ZFS and installs the operating system

Proxmox VE Linux kernel with KVM and LXC support

Complete toolset for administering virtual machines, containers, the host
  system, clusters, and all necessary resources

Web-based management interface

All existing data on the selected drives will be removed during the
installation process. The installer does not add boot menu entries for other
operating systems.

Please insert the [[chapter-pve-installation.html#installation_prepare_media|prepared installation media]]
(for example, USB flash drive or CD-ROM) and boot from it.

Make sure that booting from the installation medium (for example, USB) is
enabled in your server’s firmware settings. Secure boot needs to be disabled
when booting an installer prior to Proxmox VE version 8.1.

[](images/screenshot/pve-grub-menu.png)

After choosing the correct entry (for example, Boot from USB), the Proxmox VE menu
appears and you can select one of the following options:

Install Proxmox VE (Graphical)

Starts the normal installation.

It’s possible to use the installation wizard with a keyboard only. Buttons
can be clicked by pressing the ALT key combined with the underlined character
from the respective button. For example, ALT + N to press a Next button.

Install Proxmox VE (Terminal UI)

Starts the terminal-mode installation wizard. It provides the same overall
installation experience as the graphical installer, but has generally better
compatibility with very old and very new hardware.

Install Proxmox VE (Terminal UI, Serial Console)

Starts the terminal-mode installation wizard, additionally setting up the Linux
kernel to use the (first) serial port of the machine for in- and output. This
can be used if the machine is completely headless and only has a serial console
available.

[](images/screenshot/pve-tui-installer.png)

Both modes use the same code base for the actual installation process to
benefit from more than a decade of bug fixes and ensure feature parity.

The Terminal UI option can be used in case the graphical installer does
not work correctly, for example due to driver issues. See also
[[chapter-pve-installation.html#nomodeset_kernel_param|adding the nomodeset kernel parameter]].

Advanced Options: Install Proxmox VE (Graphical, Debug Mode)

Starts the installation in debug mode. A console will be opened at several
installation steps. This helps to debug the situation if something goes wrong.
To exit a debug console, press CTRL-D. This option can be used to boot a live
system with all basic tools available. You can use it, for example, to
[[chapter-sysadmin.html#chapter_zfs|repair a degraded ZFS rpool]] or fix the
[[chapter-sysadmin.html#sysboot|bootloader]] for an existing Proxmox VE setup.

Advanced Options: Install Proxmox VE (Terminal UI, Debug Mode)

Same as the graphical debug mode, but preparing the system to run the
terminal-based installer instead.

Advanced Options: Install Proxmox VE (Serial Console Debug Mode)

Same as the terminal-based debug mode, but additionally sets up the Linux kernel
to use the (first) serial port of the machine for in- and output.

Advanced Options: Install Proxmox VE (Automated)

Starts the installer in unattended mode, even if the ISO has not been
appropriately prepared for an automated installation. This option can be used to
gather hardware details or might be useful to debug an automated installation
setup. See [[chapter-pve-installation.html#installation_unattended|Unattended Installation]] for more
information.

Advanced Options: Rescue Boot

With this option you can boot an existing installation. It searches all attached
hard disks. If it finds an existing installation, it boots directly into that
disk using the Linux kernel from the ISO. This can be useful if there are
problems with the bootloader (GRUB/systemd-boot) or the BIOS/UEFI is unable to
read the boot block from the disk.

Advanced Options: Test Memory (memtest86+)

Runs memtest86+. This is useful to check if the memory is functional and free
of errors. Secure Boot must be turned off in the UEFI firmware setup utility to
run this option.

You normally select Install Proxmox VE (Graphical) to start the installation.

[](images/screenshot/pve-select-target-disk.png)

The first step is to read the EULA (End User License Agreement). Following this,
you can select the target hard disk(s) for the installation.

By default, the whole server is used and all existing data is removed.
Make sure there is no important data on the server before proceeding with the
installation.

The Options button lets you select the target file system, which
defaults to ext4. The installer uses LVM if you select
ext4 or xfs as a file system, and offers additional options to
restrict LVM space (see [[chapter-pve-installation.html#advanced_lvm_options|below]]).

Proxmox VE can also be installed on ZFS. As ZFS offers several software RAID levels,
this is an option for systems that don’t have a hardware RAID controller. The
target disks must be selected in the Options dialog. More ZFS-specific
settings can be changed under [[chapter-pve-installation.html#advanced_zfs_options|Advanced Options]].

ZFS on top of any hardware RAID is not supported and can result in data
loss.

[](images/screenshot/pve-select-location.png)

The next page asks for basic configuration options like your location, time
zone, and keyboard layout. The location is used to select a nearby download
server, in order to increase the speed of updates. The installer is usually able
to auto-detect these settings, so you only need to change them in rare
situations when auto-detection fails, or when you want to use a keyboard layout
not commonly used in your country.

[](images/screenshot/pve-set-password.png)

You must also specify the password of the superuser (root) and an email
address. The password must consist of at least 8 characters. It’s highly
recommended to use a stronger password. Some guidelines are:

Use a minimum password length of at least 12 characters.

Include lowercase and uppercase alphabetic characters, numbers, and symbols.

Avoid character repetition, keyboard patterns, common dictionary words,
  letter or number sequences, usernames, relative or pet names, romantic links
  (current or past), and biographical information (for example ID numbers,
  ancestors' names or dates).

The email address is used to send notifications to the system administrator.
For example:

Information about available package updates.

Error messages from periodic cron jobs.

[](images/screenshot/pve-setup-network.png)

The system sends all notification emails to the specified address.

The last step is the network configuration. Network interfaces that are UP
show a filled circle in front of their name in the drop-down menu. Please note
that during installation you can either specify an IPv4 or IPv6 address, but not
both. To configure a dual-stack node, add additional IP addresses after the
installation.

[](images/screenshot/pve-install-summary.png)

The next step shows a summary of the previously selected options. Please
re-check every setting and use the Previous button if a setting needs to be
changed.

After clicking Install, the installer begins to format the disks and copy
packages to the target disk(s). Please wait until this step has finished; then
remove the installation medium and restart your system.

[](images/screenshot/pve-installation.png)

Copying the packages usually takes several minutes, mostly depending on the
speed of the installation medium and the target disk performance.

When copying and setting up the packages has finished, you can reboot the
server. This will be done automatically after a few seconds by default.

Installation Failure

If the installation failed, check out specific errors on the second TTY
(CTRL + ALT + F2) and ensure that the system meets the
[[chapter-pve-installation.html#install_minimal_requirements|minimum requirements]].

If the installation is still not working, look at the
[[chapter-pve-intro.html#getting_help|how to get help chapter]].

### Accessing the Management Interface Post-Installation

[](images/screenshot/gui-login-window.png)

After a successful installation and reboot of the system you can use the Proxmox VE
web interface for further configuration.

Point your browser to the IP address given during the installation and port
  8006, for example: [https://youripaddress:8006](https://youripaddress:8006)

Log in using the root (realm PAM) username and the password chosen during
  installation.

Upload your subscription key to gain access to the Enterprise repository.
  Otherwise, you will need to set up one of the public, less tested package
  repositories to get updates for security fixes, bug fixes, and new features.

Check the IP configuration and hostname.

Check the timezone.

Check your [[chapter-pve-firewall.html#chapter_pve_firewall|Firewall settings]].

### Advanced LVM Configuration Options

The installer creates a Volume Group (VG) called pve, and additional Logical
Volumes (LVs) called root, data, and swap, if ext4 or xfs is used. To
control the size of these volumes use:

hdsize

Defines the total hard disk size to be used. This way you can reserve free space
on the hard disk for further partitioning (for example for an additional PV and
VG on the same hard disk that can be used for LVM storage).

swapsize

Defines the size of the swap volume. The default is the size of the installed
memory, minimum 4 GB and maximum 8 GB. The resulting value cannot be greater
than one eighth of the size of the hard drive (hdsize / 8).

If set to 0, no swap volume will be created.

maxroot

Defines the maximum size of the root volume, which stores the operating
system. With more than 48 GiB storage available, the default is a quarter of
the size of the hard drive (hdsize / 4) with a maximum of 96 GiB.
With less than 48 GiB of storage available, the root volume size is at least
half the size of the hard drive (hdsize / 2).

maxvz

Defines the maximum size of the data volume. The actual size of the data
volume is:

datasize = hdsize - rootsize - swapsize - minfree

Where datasize cannot be bigger than maxvz.

In case of LVM thin, the data pool will only be created if datasize is
bigger than 4GB.

If set to 0, no data volume will be created and the storage
configuration will be adapted accordingly.

minfree

Defines the amount of free space that should be left in the LVM volume group
pve. With more than 128GB storage available, the default is 16GB, otherwise
hdsize/8 will be used.

LVM requires free space in the VG for snapshot creation (not required for
lvmthin snapshots).

### Advanced ZFS Configuration Options

The installer creates the ZFS pool rpool, if ZFS is used. No swap space is
created but you can reserve some unpartitioned space on the install disks for
swap. You can also create a swap zvol after the installation, although this can
lead to problems (see [[chapter-sysadmin.html#zfs_swap|ZFS swap notes]]).

ashift

Defines the ashift value for the created pool. The ashift needs to be set at
least to the sector-size of the underlying disks (2 to the power of ashift is
the sector-size), or any disk which might be put in the pool (for example the
replacement of a defective disk).

compress

Defines whether compression is enabled for rpool.

checksum

Defines which checksumming algorithm should be used for rpool.

copies

Defines the copies parameter for rpool. Check the zfs(8) manpage for the
semantics, and why this does not replace redundancy on disk-level.

ARC max size

Defines the maximum size the ARC can grow to and thus limits the amount of
memory ZFS will use. See also the section on
[[chapter-sysadmin.html#sysadmin_zfs_limit_memory_usage|how to limit ZFS memory usage]] for more
details.

hdsize

Defines the total hard disk size to be used. This is useful to save free space
on the hard disk(s) for further partitioning (for example to create a
swap-partition). hdsize is only honored for bootable disks, that is only the
first disk or mirror for RAID0, RAID1 or RAID10, and all disks in RAID-Z[123].

### Advanced BTRFS Configuration Options

No swap space is created when BTRFS is used but you can reserve some
unpartitioned space on the install disks for swap. You can either create a
separate partition, BTRFS subvolume or a swapfile using the btrfs filesystem
mkswapfile command.

compress

Defines whether compression is enabled for the BTRFS subvolume. Different
compression algorithms are supported: on (equivalent to zlib), zlib, lzo
and zstd. Defaults to off.

hdsize

Defines the total hard disk size to be used. This is useful to save free space
on the hard disk(s) for further partitioning (for example, to create a
swap partition).

### ZFS Performance Tips

ZFS works best with a lot of memory. If you intend to use ZFS make sure to have
enough RAM available for it. A good calculation is 4GB plus 1GB RAM for each TB
RAW disk space.

ZFS can use a dedicated drive as write cache, called the ZFS Intent Log (ZIL).
Use a fast drive (SSD) for it. It can be added after installation with the
following command:

```bash
# zpool add <pool-name> log </dev/path_to_fast_ssd>
```

### Adding the nomodeset Kernel Parameter

Problems may arise on very old or very new hardware due to graphics drivers. If
the installation hangs during boot, you can try adding the nomodeset
parameter. This prevents the Linux kernel from loading any graphics drivers and
forces it to continue using the BIOS/UEFI-provided framebuffer.

On the Proxmox VE bootloader menu, navigate to Install Proxmox VE (Terminal UI) and
press e to edit the entry. Using the arrow keys, navigate to the line starting
with linux, move the cursor to the end of that line and add the
parameter nomodeset, separated by a space from the pre-existing last
parameter.

Then press Ctrl-X or F10 to boot the configuration.

## Unattended Installation

The automated installation method allows installing Proxmox VE
in an unattended manner. This enables you to fully automate the setup
process on bare-metal. Once the installation is complete and the host
has booted up, automation tools like Ansible can be used to further
configure the installation.

The necessary options for the installer must be provided in an answer
file. This file allows using filter rules to determine which disks and
network cards should be used.

To use the automated installation, first choose a source from which to
fetch the answer file and then prepare an installation ISO with that
choice.

Once the ISO is prepared, its initial boot menu will show a new boot
entry named Automated Installation which gets automatically selected
after a 10-second timeout.

[Visit the wiki](https://pve.proxmox.com/wiki/Automated_Installation) for more
details and information on the unattended installation.

## Install Proxmox VE on Debian

Proxmox VE ships as a set of Debian packages and can be installed on top of a standard
Debian installation.
[[chapter-sysadmin.html#sysadmin_package_repositories|After configuring the repositories]] you need
to run the following commands:

```bash
# apt-get update
# apt-get install proxmox-ve
```

Installing on top of an existing Debian installation looks easy, but it presumes
that the base system has been installed correctly and that you know how you want
to configure and use the local storage. You also need to configure the network
manually.

In general, this is not trivial, especially when LVM or ZFS is used.

A detailed step-by-step how-to can be found on the
[wiki](https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_13_Trixie).