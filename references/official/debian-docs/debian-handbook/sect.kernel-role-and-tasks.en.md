---
title: "B.4.5. Rights Management"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.kernel-role-and-tasks.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.kernel-role-and-tasks.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "B.4.5. Rights Management"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.computer-layers.en.md)The Debian Administrator's Handbook[Next](sect.user-space.en.md)

## B.4.Â Some Tasks Handled by the Kernel

### B.4.1.Â Driving the Hardware

				The kernel is, first and foremost, tasked with controlling the hardware parts, detecting them, switching them on when the computer is powered on, and so on. It also makes them available to higher-level software with a simplified programming interface, so applications can take advantage of devices without having to worry about details such as which extension slot the option board is plugged into. The programming interface also provides an abstraction layer; this allows video-conferencing software, for example, to use a webcam independently of its make and model. The software can just use the Video for Linux (V4L) interface, and the kernel translates the function calls of this interface into the actual hardware commands needed by the specific webcam in use.
			
				The kernel exports many details about detected hardware through the /proc/ and /sys/ virtual filesystems. Several tools summarize those details. Among them, lspci (in the pciutils package) lists PCI devices, lsusb (in the usbutils package) lists USB devices, and lspcmcia (in the pcmciautils package) lists PCMCIA cards. These tools are very useful for identifying the exact model of a device. This identification also allows more precise searches on the web, which in turn, lead to more relevant documents.
			

ExampleÂ B.1.Â Example of information provided by lspci and lsusb

```bash
$ lspci
[...]
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers (rev 05)
00:01.0 PCI bridge: Intel Corporation 6th-9th Gen Core Processor PCIe Controller (x16) (rev 05)
00:02.0 VGA compatible controller: Intel Corporation HD Graphics 630 (rev 04)
00:14.0 USB controller: Intel Corporation 100 Series/C230 Series Chipset Family USB 3.0 xHCI Controller (rev 31)
00:14.2 Signal processing controller: Intel Corporation 100 Series/C230 Series Chipset Family Thermal Subsystem (rev 31)
[...]
02:00.0 Network controller: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter (rev 32)
03:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTL8411B PCI Express Card Reader (rev 01)
03:00.1 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 12)
04:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983
$ lsusb
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 003: ID 0bda:5621 Realtek Semiconductor Corp. HD WebCam
Bus 001 Device 002: ID 04ca:3016 Lite-On Technology Corp. 
Bus 001 Device 018: ID 145f:01bc Trust GXT 155 Gaming Mouse
Bus 001 Device 004: ID 04f3:0c03 Elan Microelectronics Corp. ELAN:Fingerprint
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

				These programs have a -v option that lists much more detailed (but usually not necessary) information. Finally, the lsdev command (in the procinfo package) lists communication resources used by devices.
			
				Applications often access devices by way of special files created within /dev/ (see sidebar [BACK TO BASICS Device access permissions](sect.creating-accounts.en.md#sidebar.special-files)). These are special files that represent disk drives (for instance, /dev/hda and /dev/sdc), partitions (/dev/hda1 or /dev/sdc3), mice (/dev/input/mouse0), keyboards (/dev/input/event0), soundcards (/dev/snd/*), serial ports (/dev/ttyS*), and so on.
			

### B.4.2.Â Filesystems

				Filesystems are one of the most prominent aspects of the kernel. Unix systems merge all the file stores into a single hierarchy, which allows users (and applications) to access data simply by knowing its location within that hierarchy.
			
				The starting point of this hierarchical tree is called the root, /. This directory can contain named subdirectories. For instance, the home subdirectory of / is called /home/. This subdirectory can, in turn, contain other subdirectories, and so on. Each directory can also contain files, where the actual data will be stored. Thus, the /home/rmas/Desktop/hello.txt name refers to a file named hello.txt stored in the Desktop subdirectory of the rmas subdirectory of the home directory present in the root. The kernel translates between this naming system and the actual, physical storage on a disk.
			
				Unlike other systems, there is only one such hierarchy, and it can integrate data from several disks. One of these disks is used as the root, and the others are âmountedâ on directories in the hierarchy (the Unix command is called mount); these other disks are then available under these âmount pointsâ. This allows storing users' home directories (traditionally stored within /home/) on a second hard disk, which will contain the rhertzog and rmas directories. Once the disk is mounted on /home/, these directories become accessible at their usual locations, and paths such as /home/rmas/Desktop/hello.txt keep working.
			
				There are many filesystem formats, corresponding to many ways of physically storing data on disks. The most widely known are ext3 and ext4, but others exist. For instance, vfat is the system that was historically used by DOS and Windows operating systems, which allows using hard disks under Debian as well as under Windows. In any case, a filesystem must be prepared on a disk before it can be mounted and this operation is known as âformattingâ. Commands such as mkfs.ext3 (where mkfs stands for MaKe FileSystem) handle formatting. These commands require, as a parameter, a device file representing the partition to be formatted (for instance, /dev/sda1). This operation is destructive and should only be run once, except if one deliberately wishes to wipe a filesystem and start afresh.
			
				There are also network filesystems, such as NFS, where data is not stored on a local disk. Instead, data is transmitted through the network to a server that stores and retrieves them on demand. The filesystem abstraction shields users from having to care: files remain accessible in their usual hierarchical way.
			

### B.4.3.Â Shared Functions

				Since a number of the same functions are used by all software, it makes sense to centralize them in the kernel. For instance, shared filesystem handling allows any application to simply open a file by name, without needing to worry where the file is stored physically. The file can be stored in several different slices on a hard disk, or split across several hard disks, or even stored on a remote file server. Shared communication functions are used by applications to exchange data independently of the way the data is transported. For instance, transport could be over any combination of local or wireless networks, or over a telephone landline.
			

### B.4.4.Â Managing Processes

				A process is a running instance of a program. This requires memory to store both the program itself and its operating data. The kernel is in charge of creating and tracking them. When a program runs, the kernel first sets aside some memory, then loads the executable code from the filesystem into it, and then starts the code running. It keeps information about this process, the most visible of which is an identification number known as pid (process identifier).
			
				Unix-like kernels (including Linux), like most other modern operating systems, are capable of âmulti-taskingâ. In other words, they allow running many processes âat the same timeâ. There is actually only one running process at any one time, but the kernel cuts time into small slices and runs each process in turn. Since these time slices are very short (in the millisecond range), they create the illusion of processes running in parallel, although they are actually only active during some time intervals and idle the rest of the time. The kernel's job is to adjust its scheduling mechanisms to keep that illusion, while maximizing the global system performance. If the time slices are too long, the application may not appear as responsive as desired. Too short, and the system loses time switching tasks too frequently. These decisions can be tweaked with process priorities. High-priority processes will run for longer and with more frequent time slices than low-priority processes.
			

NOTE Multi-processor systems (and variants)

				The limitation described above of only one process being able to run at a time, doesn't always apply. The actual restriction is that there can only be one running process per processor core at a time. Multi-processor, multi-core or âhyper-threadedâ systems allow several processes to run in parallel. The same time-slicing system is still used, though, so as to handle cases where there are more active processes than available processor cores. This is far from unusual: a basic system, even a mostly idle one, almost always has tens of running processes.
			
				Of course, the kernel allows running several independent instances of the same program. But each can only access its own time slices and memory. Their data thus remain independent.
			

### B.4.5.Â Rights Management

				Unix-like systems are also multi-user. They provide a rights management system that supports separate users and groups; it also allows control over actions based on permissions. The kernel manages data for each process, allowing it to control permissions. Most of the time, a process is identified by the user who started it. That process is only permitted to take those actions available to its owner. For instance, trying to open a file requires the kernel to check the process identity against access permissions (for more details on this particular example, see [SectionÂ 9.3, âManaging Rightsâ](sect.rights-management.en.md)).
			[PrevB.3.Â Inner Workings of a Computer: the Different ...](sect.computer-layers.en.md)[Up](#)[Home](index.en.md)[NextB.5.Â The User Space](sect.user-space.en.md)