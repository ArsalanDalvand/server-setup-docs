---
title: "8.11.2. Installing withdpkg"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.kernel-installation.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.kernel-installation.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.11.2. Installing withdpkg"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.kernel-compilation.en.md)The Debian Administrator's Handbook[Next](unix-services.en.md)

## 8.11.Â Installing a Kernel

### 8.11.1.Â Features of a Debian Kernel Package

				A Debian kernel package installs the kernel image (vmlinuz-version), its configuration (config-version) and its symbols table (System.map-version) in /boot/. The modules are installed in the /lib/modules/version/ directory.
			

CULTURE The symbols table

				The symbols table helps developers understand the meaning of a kernel error message; without it, kernel âoopsesâ (an âoopsâ is the kernel equivalent of a segmentation fault for user-space programs, in other words messages generated following an invalid pointer dereference) only contain numeric memory addresses, which is useless information without the table mapping these addresses to symbols and function names.
			
				The package's configuration scripts automatically generate an initramfs image (the successor of the old initial ramdisk initrd image), which is a compressed mini-system designed to be loaded in memory (hence the name, which stands for âinitial ram filesystemâ) by the bootloader, and used by the Linux kernel solely for loading the modules needed to access the devices containing the complete Debian system (for example, the driver for SATA disks). Finally, the post-installation scripts update the symbolic links /vmlinuz, /vmlinuz.old, /initrd.img, and /initrd.img.old so that they point to the latest two kernels installed, respectively, as well as the corresponding initramfs boot images.
			
				Most of those tasks are offloaded to hook scripts in the /etc/kernel/*.d/ directories. For instance, the integration with grub relies on /etc/kernel/postinst.d/zz-update-grub and /etc/kernel/postrm.d/zz-update-grub to call update-grub when kernels are installed or removed.
			

### 8.11.2.Â Installing with dpkg

				Using apt is so convenient that it makes it easy to forget about the lower-level tools, but the easiest way of installing a compiled kernel is to use a command such as dpkg -i package.deb, where package.deb is the name of a linux-image package such as linux-image-5.10.46-falcot_5.10.46-1_amd64.deb.
			
				The configuration steps described in this chapter are basic and can lead both to a server system or a workstation, and it can be massively duplicated in semi-automated ways. However, it is not enough by itself to provide a fully configured system. A few pieces are still in need of configuration, starting with low-level programs known as the âUnix servicesâ.
			[Prev8.10.Â Compiling a Kernel](sect.kernel-compilation.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 9.Â Unix Services](unix-services.en.md)