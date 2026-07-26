---
title: "4.1.4. Other Installation Methods"
source: "https://www.debian.org/doc/manuals/debian-handbook/installation.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/installation.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "4.1.4. Other Installation Methods"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.how-to-migrate.en.md)The Debian Administrator's Handbook[Next](sect.installation-steps.en.md)

# ChapterÂ 4.Â Installation

[4.1. Installation Methods](installation.en.md#sect.installation-methods)[4.1.1. Installing from a CD-ROM/DVD-ROM](installation.en.md#sect.install-cd)[4.1.2. Booting from a USB Key](installation.en.md#sect.install-usb)[4.1.3. Installing through Network Booting](installation.en.md#sect.install-net)[4.1.4. Other Installation Methods](installation.en.md#sect.install-auto)[4.2. Installing, Step by Step](sect.installation-steps.en.md)[4.2.1. Booting and Starting the Installer](sect.installation-steps.en.md#sect.install-boot)[4.2.2. Selecting the language](sect.installation-steps.en.md#sect.install-lang)[4.2.3. Selecting the country](sect.installation-steps.en.md#sect.install-country)[4.2.4. Selecting the keyboard layout](sect.installation-steps.en.md#sect.install-keyboard)[4.2.5. Detecting Hardware](sect.installation-steps.en.md#sect.install-detect-hardware)[4.2.6. Loading Components](sect.installation-steps.en.md#sect.install-load-components)[4.2.7. Detecting Network Hardware](sect.installation-steps.en.md#sect.install-detect-network)[4.2.8. Configuring the Network](sect.installation-steps.en.md#sect.install-network-config)[4.2.9. Administrator Password](sect.installation-steps.en.md#id-1.7.12.10)[4.2.10. Creating the First User](sect.installation-steps.en.md#id-1.7.12.11)[4.2.11. Configuring the Clock](sect.installation-steps.en.md#sect.install-clock-config)[4.2.12. Detecting Disks and Other Devices](sect.installation-steps.en.md#sect.install-detect-disks)[4.2.13. Starting the Partitioning Tool](sect.installation-steps.en.md#sect.install-partman)[4.2.14. Installing the Base System](sect.installation-steps.en.md#id-1.7.12.15)[4.2.15. Configuring the Package Manager (apt)](sect.installation-steps.en.md#id-1.7.12.16)[4.2.16. Debian Package Popularity Contest](sect.installation-steps.en.md#id-1.7.12.17)[4.2.17. Selecting Packages for Installation](sect.installation-steps.en.md#id-1.7.12.18)[4.2.18. Installing the GRUB Bootloader](sect.installation-steps.en.md#sect.installing-grub)[4.2.19. Finishing the Installation and Rebooting](sect.installation-steps.en.md#id-1.7.12.20)[4.3. After the First Boot](sect.after-first-boot.en.md)[4.3.1. Installing Additional Software](sect.after-first-boot.en.md#id-1.7.13.5)[4.3.2. Upgrading the System](sect.after-first-boot.en.md#id-1.7.13.6)
		To use Debian, you need to install it on a computer; this task is taken care of by the debian-installer program. A proper installation involves many operations. This chapter reviews them in their chronological order.
	

BACK TO BASICS A catch-up course in the appendices

		Installing a computer is always simpler when you are familiar with the way it works. If you are not, make a quick detour to [AppendixÂ B, Short Remedial Course](short-remedial-course.en.md) before reading this chapter.
	
		The installer for Bullseye is based on debian-installer. Its modular design enables it to work in various scenarios and allows it to evolve and adapt to changes. Despite the limitations implied by the need to support a large number of architectures, this installer is very accessible to beginners, since it assists users at each stage of the process. Automatic hardware detection, guided partitioning, and graphical user interfaces have solved most of the problems that novices used to face in the early years of Debian.
	
		Installation requires 256Â MB of RAM (Random Access Memory) and at least 2Â GB of hard drive space. All Falcot computers meet these criteria. Note, however, that these figures apply to the installation of a very limited system without a graphical desktop. A minimum of 2Â GB of RAM and 10Â GB of hard drive space are really recommended for a basic office desktop workstation.
	

BEWARE Upgrading from Buster

		If you already have Debian Bullseye installed on your computer, this chapter is not for you! Unlike other distributions, Debian allows updating a system from one version to the next without having to reinstall the system. Reinstalling, in addition to being unnecessary, could even be dangerous, since it could remove already installed programs.
	
		The upgrade process will be described in [SectionÂ 6.7, âUpgrading from One Stable Distribution to the Nextâ](sect.dist-upgrade.en.md).
	
		Upgrading from even older Debian systems directly is not supported. In this case you need to update gradually to the stable release which followed your Debian version, and so on. Previous versions of this book will assist you in that matter.
	

## 4.1.Â Installation Methods

			A Debian system can be installed from several types of media, as long as the BIOS/UEFI (see [NOTE UEFI, a modern replacement to the BIOS](sect.computer-layers.en.md#sidebar.bios-uefi)) of the machine allows it. You can for instance boot with a CD-ROM, a USB key, or even through a network.
		

### 4.1.1.Â Installing from a CD-ROM/DVD-ROM

				The most widely used installation method is from a CD-ROM (or DVD-ROM, which behaves exactly the same way): the computer is booted from this media, and the installation program takes over.
			
				Various CD-ROM families have different purposes: netinst (network installation) contains the installer and the base Debian system; all other programs are then downloaded. Its âimageâ, that is the ISO-9660 filesystem that contains the exact contents of the disk, only takes up about 150 to 280Â MB (depending on the architecture). On the other hand, the complete set offers all packages and allows for installation on a computer that has no Internet access; it requires around 19 DVD-ROMs (or 4 Blu-ray disks). There is no more official CD-ROMs set as they were really huge, rarely used and now most of the computers use DVD-ROMs as well as CD-ROMs. But the programs are divided among the disks according to their popularity and importance; the first disk will be sufficient for most installations, since it contains the most used software.
			
				There is a last type of image, known as mini.iso, which is only available as a by-product of the installer. The image only contains the minimum required to configure the network and everything else is downloaded (including parts of the installer itself, which is why those images tend to break when a new version of the installer is released). Those images can be found on the normal Debian mirrors under the dists/release/main/installer-arch/current/images/netboot/ directory.
			

TIP Multi-architecture disks

				Most installation CD- and DVD-ROMs work only with a specific hardware architecture. If you wish to download the complete images, you must take care to choose those which work on the hardware of the computer on which you wish to install them.
			
				Some CD/DVD-ROM images can work on several architectures. We thus have a CD-ROM image combining the netinst images of the i386 and amd64 architectures.
			
				To acquire Debian CD-ROM images, you may, of course, download them and burn them to disk. You may also purchase them, and, thus, provide the project with a little financial support. Check the website to see the list of DVD-ROM image vendors and download sites. âÂ [https://www.debian.org/CD/](https://www.debian.org/CD/)

### 4.1.2.Â Booting from a USB Key

				Since most computers are able to boot from USB devices, you can also install Debian from a USB key (this is nothing more than a small flash-memory disk).
			
				The installation manual explains how to create a USB key that contains the debian-installer. The procedure is very simple because ISO images for i386 and amd64 are hybrid images that can boot from a CD-ROM as well as from a USB key.
			
				You must first identify the device name of the USB key (ex: /dev/sdb); the simplest means to do this is to check the messages issued by the kernel using the dmesg command. Then you must copy the previously downloaded ISO image (for example, debian-11.0.0-amd64-netinst.iso) with the command cat debian-11.0.0-amd64-netinst.iso >/dev/sdb; sync. This command requires administrator rights, since it accesses the USB key directly and blindly erases its content.
			
				A more detailed explanation is available in the installation manual. Among other things, it describes an alternative method of preparing a USB key that is more complex, but that allows you to customize the installer's default options (those set in the kernel command line). âÂ [https://www.debian.org/releases/stable/amd64/ch04s03](https://www.debian.org/releases/stable/amd64/ch04s03)

### 4.1.3.Â Installing through Network Booting

				Many BIOSes allow booting directly from the network by downloading a kernel and a minimal filesystem image. This method (which has several names, such as PXE or TFTP boot) can be a life-saver if the computer does not have a CD-ROM reader, or if the BIOS can't boot from such media.
			
				This installation method works in two steps. First, while booting the computer, the BIOS (or the network card) issues a BOOTP/DHCP request to automatically acquire an IP address. When a BOOTP or DHCP server returns a response, it includes a filename, as well as network settings. After having configured the network, the client computer then issues a TFTP (Trivial File Transfer Protocol) request for a file whose name was previously indicated. Once this file is acquired, it is executed as though it were a bootloader. This then launches the Debian installation program, which is executed as though it were running from the hard drive, a CD-ROM, or a USB key.
			
				All the details of this method are available in the installation guide (âPreparing files for TFTP Net Bootingâ section). âÂ [https://www.debian.org/releases/stable/amd64/ch05s01#boot-tftp-x86](https://www.debian.org/releases/stable/amd64/ch05s01#boot-tftp-x86) âÂ [https://www.debian.org/releases/stable/amd64/ch04s05](https://www.debian.org/releases/stable/amd64/ch04s05)

### 4.1.4.Â Other Installation Methods

				When we have to deploy customized installations for a large number of computers, we generally choose an automated rather than a manual installation method. Depending on the situation and the complexity of the installations to be made, we can use FAI (Fully Automatic Installer, described in [SectionÂ 12.3.1, âFully Automatic Installer (FAI)â](sect.automated-installation.en.md#sect.fai)), or even a customized installation DVD with preseeding (see [SectionÂ 12.3.2, âPreseeding Debian-Installerâ](sect.automated-installation.en.md#sect.d-i-preseeding)).
			
				It should also be noted that the installer can load and run an SSH server and thus provides the ability to install Debian remotely via an SSH session. The release notes also describe how to run the installer from an existing system using grub to replace it completely.
			[Prev3.2.Â How To Migrate](sect.how-to-migrate.en.md)[Up](#)[Home](index.en.md)[Next4.2.Â Installing, Step by Step](sect.installation-steps.en.md)