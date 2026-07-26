---
title: "8.8.3. Using GRUB with EFI andSecure Boot"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.config-bootloader.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.config-bootloader.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.8.3. Using GRUB with EFI andSecure Boot"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.config-printing.en.md)The Debian Administrator's Handbook[Next](sect.config-misc.en.md)

## 8.8.Â Configuring the Bootloader

			It is probably already functional, but it is always good to know how to configure and install the bootloader in case it disappears from the Master Boot Record. This can occur after installation of another operating system, such as Windows. The following information can also help you to modify the bootloader configuration if needed.
		

BACK TO BASICS Master boot record

			The Master Boot Record (MBR) occupies the first 512 bytes of the first hard disk, and is the first thing loaded by the BIOS to hand over control to a program capable of booting the desired operating system. In general, a bootloader gets installed in the MBR, removing its previous content.
		

### 8.8.1.Â Identifying the Disks

CULTURE udev and /dev/

				The /dev/ directory traditionally houses so-called âspecialâ files, intended to represent system peripherals (see sidebar [BACK TO BASICS Device access permissions](sect.creating-accounts.en.md#sidebar.special-files)). Once upon a time, it used to contain all special files that could potentially be used. This approach had a number of drawbacks among which the fact that it restricted the number of devices that one could use (due to the hardcoded list of names), and that it was impossible to know which special files were actually useful.
			
				Nowadays, the management of special files is entirely dynamic and matches better the nature of hot-swappable computer devices. The kernel cooperates with udev ([SectionÂ 9.11.3, âHow udev Worksâ](sect.hotplug.en.md#sect.udev)) to create and delete them as needed when the corresponding devices appear and disappear. For this reason, /dev/ doesn't need to be persistent and is thus a RAM-based filesystem that starts empty and contains only the relevant entries.
			
				The kernel communicates lots of information about any newly added device and hands out a pair of major/minor numbers to identify it. With this udevd can create the special file under the name and with the permissions that it wants. It can also create aliases and perform additional actions (such as initialization or registration tasks). udevd's behavior is driven by a large set of (customizable) rules usually found in /lib/udev/rules.d/ and /etc/udev/rules.d/.
			
				With dynamically assigned names, you can thus keep the same name for a given device, regardless of the connector used or the connection order, which is especially useful when you use various USB peripherals. The first partition on the first hard drive can then be called /dev/sda1 for backwards compatibility, or /dev/root-partition if you prefer, or even both at the same time since udevd can be configured to automatically create a symbolic link.
			
				In ancient times, some kernel modules did automatically load when you tried to access the corresponding device file. This is no longer the case, and the peripheral's special file no longer exists prior to loading the module; this is no big deal, since most modules are loaded on boot thanks to automatic hardware detection. But for undetectable peripherals (such as very old disk drives or PS/2 mice), this doesn't work. Consider adding the modules, floppy, psmouse and mousedev to /etc/modules or /etc/modules-load.d/ in order to force loading them on boot.
			
				Configuration of the bootloader must identify the different hard drives and their partitions. Linux uses âblockâ special files stored in the /dev/ directory, for this purpose. Since Debian Squeeze, the naming scheme for hard drives has been unified by the Linux kernel, and all hard drives (IDE/PATA, SATA, SCSI, USB, IEEEÂ 1394) are now represented by /dev/sd*.
			
				Each partition is represented by its number on the disk on which it resides: for instance, /dev/sda1 is the first partition on the first disk, and /dev/sdb3 is the third partition on the second disk.
			
				The PC architecture (or âi386â, including its younger cousin âamd64â) has long been limited to using the âMS-DOSâ partition table format, which only allows four âprimaryâ partitions per disk. To go beyond this limitation under this scheme, one of them has to be created as an âextendedâ partition, and it can then contain additional âsecondaryâ partitions. These secondary partitions are numbered from 5. Thus the first secondary partition could be /dev/sda5, followed by /dev/sda6, etc.
			
				Another restriction of the MS-DOS partition table format is that it only allows disks up to 2Â TiB in size, which is becoming a real problem with recent disks.
			
				A new partition table format called GPT (GUID Partition Table) loosens these constraints on the number of partitions (it allows up to 128 partitions when using standard settings) and on the size of the disks (up to 8Â ZiB, which is more than 8 billion terabytes). If you intend to create many physical partitions on the same disk, you should therefore ensure that you are creating the partition table in the GPT format when partitioning your disk.
			

QUICK LOOK The Globally Unique Identifier (GUID)

				Using GPT the partition type is represented as a 16 bytes long universally unique identifier, also known as Globally Unique Identifier (GUID), which is part of the UEFI standard (see [NOTE UEFI, a modern replacement to the BIOS](sect.computer-layers.en.md#sidebar.bios-uefi)). Compared to the hex code used to determine the partition type in the MS-DOS table format, you will have a hard time trying to remember these identifiers. Fortunately there are lists with all the IDs which you can use to create partition recipes to use with sfdisk or for preseeding the Debian installer. âÂ [https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_type_GUIDs](https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_type_GUIDs)

				It is not always easy to remember what disk is connected to which SATA controller, or in third position in the SCSI chain, especially since the naming of hotplugged hard drives (which includes among others most SATA disks and external disks) can change from one boot to another. Fortunately, udev creates, in addition to /dev/sd*, symbolic links with a fixed name, which you could then use if you wished to identify a hard drive in a non-ambiguous manner. These symbolic links are stored in /dev/disk/by-id/. On a machine with two physical disks, for example, one could find the following:
			

```bash
mirexpress:/dev/disk/by-id# ls -l
total 0
lrwxrwxrwx 1 root root  9 23 jul. 08:58 ata-STM3500418AS_9VM3L3KP -> ../../sda
lrwxrwxrwx 1 root root 10 23 jul. 08:58 ata-STM3500418AS_9VM3L3KP-part1 -> ../../sda1
lrwxrwxrwx 1 root root 10 23 jul. 08:58 ata-STM3500418AS_9VM3L3KP-part2 -> ../../sda2
[...]
lrwxrwxrwx 1 root root  9 23 jul. 08:58 ata-WDC_WD5001AALS-00L3B2_WD-WCAT00241697 -> ../../sdb
lrwxrwxrwx 1 root root 10 23 jul. 08:58 ata-WDC_WD5001AALS-00L3B2_WD-WCAT00241697-part1 -> ../../sdb1
lrwxrwxrwx 1 root root 10 23 jul. 08:58 ata-WDC_WD5001AALS-00L3B2_WD-WCAT00241697-part2 -> ../../sdb2
[...]
lrwxrwxrwx 1 root root  9 23 jul. 08:58 scsi-SATA_STM3500418AS_9VM3L3KP -> ../../sda
lrwxrwxrwx 1 root root 10 23 jul. 08:58 scsi-SATA_STM3500418AS_9VM3L3KP-part1 -> ../../sda1
lrwxrwxrwx 1 root root 10 23 jul. 08:58 scsi-SATA_STM3500418AS_9VM3L3KP-part2 -> ../../sda2
[...]
lrwxrwxrwx 1 root root  9 23 jul. 08:58 scsi-SATA_WDC_WD5001AALS-_WD-WCAT00241697 -> ../../sdb
lrwxrwxrwx 1 root root 10 23 jul. 08:58 scsi-SATA_WDC_WD5001AALS-_WD-WCAT00241697-part1 -> ../../sdb1
lrwxrwxrwx 1 root root 10 23 jul. 08:58 scsi-SATA_WDC_WD5001AALS-_WD-WCAT00241697-part2 -> ../../sdb2
[...]
lrwxrwxrwx 1 root root  9 23 jul. 16:48 usb-LaCie_iamaKey_3ed00e26ccc11a-0:0 -> ../../sdc
lrwxrwxrwx 1 root root 10 23 jul. 16:48 usb-LaCie_iamaKey_3ed00e26ccc11a-0:0-part1 -> ../../sdc1
lrwxrwxrwx 1 root root 10 23 jul. 16:48 usb-LaCie_iamaKey_3ed00e26ccc11a-0:0-part2 -> ../../sdc2
[...]
lrwxrwxrwx 1 root root  9 23 jul. 08:58 wwn-0x5000c50015c4842f -> ../../sda
lrwxrwxrwx 1 root root 10 23 jul. 08:58 wwn-0x5000c50015c4842f-part1 -> ../../sda1
[...]
mirexpress:/dev/disk/by-id#
```

				Note that some disks are listed several times (because they behave simultaneously as ATA disks and SCSI disks), but the relevant information is mainly in the model and serial numbers of the disks, from which you can find the peripheral file. While the links in /dev/disk/by-id/ are created using the device' serial number and physical path, there are more convenience links in e.g. /dev/disk/by-label/ (based on given labels), /dev/disk/by-uuid/ (based on unique identifiers, which can change when reformatting a device using mkfs.* or mkswap), /dev/disk/by-path/ (based on shortest physical path), and /dev/disk/by-partlabel/ and /dev/disk/by-partuuid/ (only partitions with GPT labels and their unique identifiers). If you use these links, e.g. in /etc/fstab, always prefer unique identifiers over labels. You can also obtain and change this information for each partition or device using the lsblk and blkid commands.
			
				The example configuration files given in the following sections are based on the same setup: a single SATA disk, where the first partition is an old Windows installation and the second contains Debian GNU/Linux.
			

ALTERNATIVE The LILO bootloader

LILO (LInux LOader) is the oldest bootloader â solid but rustic. It writes the physical address of the kernel to boot on the MBR, which is why each update to LILO (or its configuration file) must be followed by the command lilo. Forgetting to do so will render a system unable to boot if the old kernel was removed or replaced as the new one will not be in the same location on the disk.
			
				LILO's configuration file is /etc/lilo.conf; a simple file for standard configuration is illustrated in the example below.
			

ExampleÂ 8.4.Â LILO configuration file

```bash
# The disk on which LILO should be installed.
# By indicating the disk and not a partition.
# you order LILO to be installed on the MBR.
boot=/dev/sda
# the partition that contains Debian
root=/dev/sda2
# the item to be loaded by default
default=Linux

# the most recent kernel image
image=/vmlinuz
  label=Linux
  initrd=/initrd.img
  read-only

# Old kernel (if the newly installed kernel doesn't boot)
image=/vmlinuz.old
  label=LinuxOLD
  initrd=/initrd.img.old
  read-only
  optional

# only for Linux/Windows dual boot
other=/dev/sda1
  label=Windows
```

				As per request of the maintainer and upstream author of lilo, the bootloader has been removed from Debian 11 Bullseye and it is unlikely to make a comeback.
			

### 8.8.2.Â GRUBÂ 2 Configuration

GRUB (GRand Unified Bootloader) is more recent. It is not necessary to invoke it after each update of the kernel; GRUB knows how to read the filesystems and find the position of the kernel on the disk by itself. To install it on the MBR of the first disk, simply type grub-install /dev/sda. This will overwrite the MBR, so be careful not to overwrite the wrong location. While it is also possible to install GRUB into a partition boot record, beware that it is usually a mistake and doing grub-install /dev/sda1 has not the same meaning as grub-install /dev/sda.
			

NOTE Disk names for GRUB

				GRUB can only identify hard drives based on information provided by the BIOS. (hd0) corresponds to the first disk thus detected, (hd1) the second, etc. In most cases, this order corresponds exactly to the usual order of disks under Linux, but problems can occur when you associate SCSI and IDE disks. GRUB used to store the correspondences that it detected in the file /boot/grub/device.map. Nowadays it avoids this problem using universally unique identifier (UUIDs) or file system labels when generating grub.cfg. However, the device map file is not obsolete yet, since it can be used to override when the current environment is different from the one on boot. If you find errors there (because you know that your BIOS detects drives in a different order), correct them manually and run grub-install again. grub-mkdevicemap can help creating a device.map file from which to start.
			
				Partitions also have a specific name in GRUB. When you use âclassicalâ partitions in MS-DOS format, the first partition on the first disk is labeled, (hd0,msdos1), the second (hd0,msdos2), etc.
			
				GRUBÂ 2 configuration is stored in /boot/grub/grub.cfg, but this file (in Debian) is generated from others. Be careful not to modify it by hand, since such local modifications will be lost the next time update-grub is run (which may occur upon update of various packages). The most common modifications of the /boot/grub/grub.cfg file (to add command line parameters to the kernel or change the duration that the menu is displayed, for example) are made through the variables in /etc/default/grub. To add entries to the menu, you can either create a /boot/grub/custom.cfg file or modify the /etc/grub.d/40_custom file. For more complex configurations, you can modify other files in /etc/grub.d, or add to them; these scripts should return configuration snippets, possibly by making use of external programs. These scripts are the ones that will update the list of kernels to boot: 10_linux takes into consideration the installed Linux kernels; 20_linux_xen takes into account Xen virtual systems, and 30_os-prober adds other existing operating systems (Windows, OSÂ X, Hurd), kernel images, and BIOS/EFI access options to the menu.
			

CAUTION error: symbol `grub_*' not found

				One of the most often reported issues when using GRUB is that users get an error like error symbol `grub_calloc' not found and they are unable to boot the system anymore. Most of the time this error is caused by installing an updated version of GRUB that causes new modules in /boot/grub to be incompatible with old core images in the boot sector that your firmware jumps to when booting your machine. This happens on systems that are configured to run grub-install to a target device that is not actually the one that the firmware uses to boot your system (e.g. after replacing disks or moving them around - one can see the issue when running dpkg-reconfigure grub-pc showing the wrong target device). With the release of Debian 11 Bullseye (the change was also populated to Buster) the update process now checks for this issue and exits with an error in case it finds such a constellation. âÂ [https://bugs.debian.org/bug=966575#95](https://bugs.debian.org/bug=966575#95)

```bash
Setting up grub-pc (2.02+dfsg1-20+deb10u4) ...
/dev/disk/by-id/[..] does not exist, so cannot grub-install to it!
You must correct your GRUB install devices before proceeding:

  DEBIAN_FRONTEND=dialog dpkg --configure grub-pc
  dpkg --configure -a
dpkg: error processing package grub-pc (--configure):
 installed grub-pc package post-installation script subprocess returned error exit status 1
```

				To fix the issue and continue the upgrade procedure, just follow the advice and run the commands given by the error message as root.
			

### 8.8.3.Â Using GRUB with EFI and Secure Boot

				Using GRUB to boot either a traditional BIOS system (legacy or UEFI-CSM) or a UEFI system is quite different. Fortunately the user doesn't need to know the differences because Debian provides different packages for each purpose and the installer automatically cares about which one(s) to choose. The grub-pc package is chosen for legacy systems, where GRUB is installed into the MBR, while UEFI systems require grub-efi-arch, where GRUB is installed into the EFI System Partition (ESP). The latter requires a GTP partition table as well as an EFI partition. âÂ [https://wiki.debian.org/Grub2#UEFI_vs_BIOS_boot](https://wiki.debian.org/Grub2#UEFI_vs_BIOS_boot)

				To switch an existing system (supporting UEFI) from legacy to UEFI boot mode not only requires to switch the GRUB packages on the system, but also to adjust the partition table and the to create an EFI partition (probably including resizing existing partitions to create the necessary free space). It is therefore quite an elaborate process and we cannot cover it here. Fortunately, there are some manuals by bloggers describing the necessary procedures.
			
				If you are using a system with âSecure Bootâ enabled and have installed shim-signed (see sidebar [CULTURE Secure Boot and the shim bootloader](sect.installation-steps.en.md#sidebar.secure-boot)), you must also install grub-efi-arch-signed. This package is not pulled in automatically, only if the installation of recommended package has been enabled.
			[Prev8.7.Â Printer Configuration](sect.config-printing.en.md)[Up](#)[Home](index.en.md)[Next8.9.Â Other Configurations: Time Synchronization, ...](sect.config-misc.en.md)