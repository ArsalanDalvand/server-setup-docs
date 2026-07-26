---
title: "B.3.4. The User Space"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.computer-layers.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.computer-layers.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "B.3.4. The User Space"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.filesystem-hierarchy.en.md)The Debian Administrator's Handbook[Next](sect.kernel-role-and-tasks.en.md)

## B.3.Â Inner Workings of a Computer: the Different Layers Involved

			A computer is often considered as something rather abstract, and the externally visible interface is much simpler than its internal complexity. Such complexity comes in part from the number of pieces involved. However, these pieces can be viewed in layers, where a layer only interacts with those immediately above or below.
		
			An end-user can get by without knowing these detailsâ¦ as long as everything works. When confronting a problem such as, âThe internet doesn't work!â, the first thing to do is to identify in which layer the problem originates. Is the network card (hardware) working? Is it recognized by the computer? Does the Linux kernel see it? Are the network parameters properly configured? All these questions isolate an appropriate layer and focus on a potential source of the problem.
		

### B.3.1.Â The Deepest Layer: the Hardware

				Let us start with a basic reminder that a computer is, first and foremost, a set of hardware elements. There is generally a main board (known as the motherboard), with one (or more) processor(s), some RAM, device controllers, and extension slots for option boards (for other device controllers). Most noteworthy among these controllers are IDE (Parallel ATA), SCSI and Serial ATA, for connecting to storage devices such as hard disks. Other controllers include USB, which is able to host a great variety of devices (ranging from webcams to thermometers, from keyboards to home automation systems) and IEEEÂ 1394 (Firewire). These controllers often allow connecting several devices so the complete subsystem handled by a controller is therefore usually known as a âbusâ. Option boards include graphics cards (into which monitor screens will be plugged), sound cards, network interface cards, and so on. Some main boards are pre-built with these features, and don't need option boards.
			

IN PRACTICE Checking that the hardware works

				Checking that a piece of hardware works can be tricky. On the other hand, proving that it doesn't work is sometimes quite simple.
			
				A hard disk drive is made of spinning platters and moving magnetic heads. When a hard disk is powered up, the platter motor makes a characteristic whir. It also dissipates energy as heat. Consequently, a hard disk drive that stays cold and silent when powered up is broken.
			
				Network cards often include LEDs displaying the state of the link. If a cable is plugged in and leads to a working network hub or switch, at least one LED will be on. If no LED lights up, either the card itself, the network device, or the cable between them, is faulty. The next step is therefore testing each component individually.
			
				Some option boards â especially 3D video cards â include cooling devices, such as heat sinks and/or fans. If the fan does not spin even though the card is powered up, a plausible explanation is the card overheated. This also applies to the main processor(s) located on the main board.
			

### B.3.2.Â The Starter: the BIOS or UEFI

				Hardware, on its own, is unable to perform useful tasks without a corresponding piece of software driving it. Controlling and interacting with the hardware is the purpose of the operating system and applications. These, in turn, require functional hardware to run.
			
				This symbiosis between hardware and software does not happen on its own. When the computer is first powered up, some initial setup is required. This role is assumed by the BIOS or UEFI, a piece of software embedded into the main board that runs automatically upon power-up. Its primary task is searching for software it can hand over control to. Usually, as you may have learned in [SectionÂ 9.1, âSystem Bootâ](unix-services.en.md#sect.system-boot), in the BIOS case, this involves looking for the first hard disk with a boot sector (also known as the master boot record or MBR), loading that boot sector, and running it. From then on, the BIOS is usually not involved (until the next boot). In the case of UEFI, the process involves scanning disks to find a dedicated EFI partition containing further EFI applications to execute.
			

TOOL Setup, the BIOS/UEFI configuration tool

				The BIOS/UEFI also contains a piece of software called Setup, designed to allow configuring aspects of the computer. In particular, it allows choosing which boot device is preferred (for instance, you can select a USB key or a CD-ROM drive instead of the default harddisk), setting the system clock, and so on. Starting Setup usually involves pressing a key very soon after the computer is powered on. This key is often Del or Esc, sometimes F2, F5, F8, or F10. Most of the time, the choice is flashed on screen while booting. If not, you best check the manual of your mainboard or your computer.
			
				The boot sector (or the EFI partition), in turn, contains another piece of software, called the bootloader, whose purpose is to find and run an operating system. Since this bootloader is not embedded in the main board but loaded from disk, it can be smarter than the BIOS, which explains why the BIOS does not load the operating system by itself. For instance, the bootloader (often GRUB on Linux systems) can list the available operating systems and ask the user to choose one. Usually, a time-out and default choice is provided. Sometimes the user can also choose to add parameters to pass to the kernel, and so on. Eventually, a kernel is found, loaded into memory, and executed.
			

NOTE UEFI, a modern replacement to the BIOS

				BIOS (which stands for Basic Input/Output System) is a software that is included in the motherboard - the electronic board connecting all peripherals - and executed when the computer is booted, in order to load an operating system (via an adapted bootloader). It stays in the background to provide an interface between the hardware and the software (in our case, the Linux kernel).
			
				In 2005/2006 its successor, the UEFI (Unified Extensible Firmware Interface) specification, has been published. This specification defines an interface that has the same purpose as the BIOS, but tries to provide more usability, extensibility, and flexibility, a graphical user interface, and true 64bit support. And it got rid of some of the limitations of BIOS booting: with the usage of a dedicated partition, the bootloaders no longer need special tricks to fit in a tiny master boot record and then discover the kernel to boot. Even better, with a suitably built Linux kernel, UEFI can directly boot the kernel without any intermediary bootloader. UEFI is also the basic foundation used to deliver Secure Boot, a technology ensuring that you run only software validated by your operating system vendor.
			
				Nowadays, most new computers will boot in UEFI mode by default, but usually they also support BIOS booting alongside for backwards compatibility with operating systems that are not ready to exploit UEFI. This is called the Compatibility Support Mode/Module (CSM). But, several manufacturers have either announced to cease support for the legacy BIOS mode in their UEFI implementation, or they already did. This transition, however, has not been without discussion nor without criticism.
			
				The BIOS/UEFI is also in charge of detecting and initializing a number of devices. Obviously, this includes the IDE/SATA devices (usually hard disk(s) and CD/DVD-ROM drives), but also PCI devices. Detected devices are often listed on screen during the boot process. If this list goes by too fast, use the Pause key to freeze it for long enough to read. Installed PCI devices that don't appear are a bad omen. At worst, the device is faulty. At best, it is merely incompatible with the current version of the BIOS or main board. PCI specifications evolve, and old main boards are not guaranteed to handle newer PCI devices.
			

### B.3.3.Â The Kernel

				Both the BIOS/UEFI and the bootloader only run for a few seconds each; now we are getting to the first piece of software that runs for a longer time, the operating system kernel. This kernel assumes the role of a conductor in an orchestra, and ensures coordination between hardware and software. This role involves several tasks including: driving hardware, managing processes, users and permissions, the filesystem, and so on. The kernel provides a common base to all other programs on the system.
			

### B.3.4.Â The User Space

				Although everything that happens outside of the kernel can be lumped together under âuser spaceâ, we can still separate it into software layers. However, their interactions are more complex than before, and the classifications may not be as simple. An application commonly uses libraries, which in turn involve the kernel, but the communications can also involve other programs, or even many libraries calling each other.
			[PrevB.2.Â Organization of the Filesystem Hierarchy](sect.filesystem-hierarchy.en.md)[Up](#)[Home](index.en.md)[NextB.4.Â Some Tasks Handled by the Kernel](sect.kernel-role-and-tasks.en.md)