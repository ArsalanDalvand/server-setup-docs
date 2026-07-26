---
title: "9.11.4. A concrete example"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.hotplug.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.hotplug.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.11.4. A concrete example"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.backup.en.md)The Debian Administrator's Handbook[Next](sect.power-management.en.md)

## 9.11.Â Hot Plugging: hotplug

### 9.11.1.Â Introduction

				The hotplug kernel subsystem dynamically handles the addition and removal of devices, by loading the appropriate drivers and by creating the corresponding device files (with the help of udevd). With modern hardware and virtualization, almost everything can be âhotpluggedâ: from the usual USB/PCMCIA/IEEE 1394 peripherals to SATA hard drives, but also the CPU and the memory.
			
				The kernel has a database that associates each device ID with the required driver. This database is used during boot to load all the drivers for the peripheral devices detected on the different buses, but also when an additional hotplug device is connected. Once the device is ready for use, a message is sent to udevd so it will be able to create the corresponding entry in /dev/.
			

### 9.11.2.Â The Naming Problem

				Before the appearance of hotplug connections, it was easy to assign a fixed name to a device. It was based simply on the position of the devices on their respective bus. But this is not possible when such devices can come and go on the bus. The typical case is the use of a digital camera and a USB key, both of which appear to the computer as disk drives. The first one connected may be /dev/sdb and the second /dev/sdc (with /dev/sda representing the computer's own hard drive). The device name is not fixed; it depends on the order in which devices are connected.
			
				Additionally, more and more drivers use dynamic values for devices' major/minor numbers, which makes it impossible to have static entries for the given devices, since these essential characteristics may vary after a reboot.
			
udev was created precisely to solve this problem.
			

### 9.11.3.Â How udev Works

				When udev is notified by the kernel of the appearance of a new device, it collects various information on the given device by consulting the corresponding entries in /sys/, especially those that uniquely identify it (MAC address for a network card, serial number for some USB devices, etc.).
			
				Armed with all of this information, udev then consults all of the rules contained in /etc/udev/rules.d/ and /lib/udev/rules.d/. In this process it decides how to name the device, what symbolic links to create (to give it alternative names), and what commands to execute. All of these files are consulted, and the rules are all evaluated sequentially (except when a file uses âGOTOâ directives). Thus, there may be several rules that correspond to a given event.
			
				The syntax of rules files is quite simple: each row contains selection criteria and variable assignments. The former are used to select events for which there is a need to react, and the latter defines the action to take. They are all simply separated with commas, and the operator implicitly differentiates between a selection criterion (with comparison operators, such as == or !=) or an assignment directive (with operators such as =, += or :=).
			
				Comparison operators are used on the following variables:
			
KERNEL: the name that the kernel assigns to the device;
					
ACTION: the action corresponding to the event (âaddâ when a device has been added, âremoveâ when it has been removed);
					
DEVPATH: the path of the device's /sys/ entry;
					
SUBSYSTEM: the kernel subsystem which generated the request (there are many, but a few examples are âusbâ, âideâ, ânetâ, âfirmwareâ, etc.);
					
ATTR{attribute}: file contents of the attribute file in the /sys/$devpath/ directory of the device. This is where you find the MAC address and other bus specific identifiers;
					
KERNELS, SUBSYSTEMS and ATTRS{attributes} are variations that will try to match the different options on one of the parent devices of the current device;
					
PROGRAM: delegates the test to the indicated program (true if it returns 0, false if not). The content of the program's standard output is stored so that it can be reused by the RESULT test;
					
RESULT: execute tests on the standard output stored during the last call to PROGRAM.
					
				The right operands can use pattern expressions to match several values at the same time. For instance, * matches any string (even an empty one); ? matches any character, and [] matches the set of characters listed between the square brackets (or the opposite thereof if the first character is an exclamation point, and contiguous ranges of characters are indicated like a-z).
			
				Regarding the assignment operators, = assigns a value (and replaces the current value); in the case of a list, it is emptied and contains only the value assigned. := does the same, but prevents later changes to the same variable. As for +=, it adds an item to a list. The following variables can be changed:
			
NAME: the device filename to be created in /dev/. Only the first assignment counts; the others are ignored;
					
SYMLINK: the list of symbolic links that will point to the same device;
					
OWNER, GROUP and MODE define the user and group that owns the device, as well as the associated permission;
					
RUN: the list of programs to execute in response to this event.
					
				The values assigned to these variables may use a number of substitutions:
			
$kernel or %k: equivalent to KERNEL;
					
$number or %n: the order number of the device, for example, for sda3, it would be â3â;
					
$devpath or %p: equivalent to DEVPATH;
					
$attr{attribute} or %s{attribute}: equivalent to ATTRS{attribute};
					
$major or %M: the kernel major number of the device;
					
$minor or %m: the kernel minor number of the device;
					
$result or %c: the string output by the last program invoked by PROGRAM;
					
						and, finally, %% and $$ for the percent and dollar sign, respectively.
					
				The above lists are not complete (they include only the most important parameters), but the udev(7) manual page should be exhaustive.
			

### 9.11.4.Â A concrete example

				Let us consider the case of a simple USB key and try to assign it a fixed name. First, you must find the elements that will identify it in a unique manner. For this, plug it in and run udevadm info -a -n /dev/sdc (replacing /dev/sdc with the actual name assigned to the key).
			

```bash
# udevadm info -a -n /dev/sdc
[...]
  looking at device '/devices/pci0000:00/0000:00:10.0/usb2/2-1/2-1:1.0/host4/target4:0:0/4:0:0:0/block/sdc':
    KERNEL=="sdc"
    SUBSYSTEM=="block"
    DRIVER==""
    ATTR{hidden}=="0"
    ATTR{events}=="media_change"
    ATTR{ro}=="0"
    ATTR{discard_alignment}=="0"
    ATTR{removable}=="1"
    ATTR{events_async}==""
    ATTR{alignment_offset}=="0"
    ATTR{capability}=="51"
    ATTR{events_poll_msecs}=="-1"
    ATTR{stat}=="130  0  6328  435  0  0  0  0  0  252  252  0  0  0  0"
    ATTR{size}=="15100224"
    ATTR{range}=="16"
    ATTR{ext_range}=="256"
    ATTR{inflight}=="0  0"
[...]

  looking at parent device '/devices/pci0000:00/0000:00:10.0/usb2/2-1/2-1:1.0/host4/target4:0:0/4:0:0:0':
[...]
    ATTRS{max_sectors}=="240"
[...]
  looking at parent device '/devices/pci0000:00/0000:00:10.0/usb2/2-1':
    KERNELS=="2-1"
    SUBSYSTEMS=="usb"
    DRIVERS=="usb"
    ATTRS{bDeviceProtocol}=="00"
    ATTRS{bNumInterfaces}==" 1"
    ATTRS{busnum}=="2"
    ATTRS{quirks}=="0x0"
    ATTRS{authorized}=="1"
    ATTRS{ltm_capable}=="no"
    ATTRS{speed}=="480"
    ATTRS{product}=="TF10"
    ATTRS{manufacturer}=="TDK LoR"
[...]
    ATTRS{serial}=="07032998B60AB777"
[...]
```

				To create a new rule, you can use tests on the device's variables, as well as those of one of the parent devices. The above case allows us to create two rules like these:
			

```bash
KERNEL=="sd?", SUBSYSTEM=="block", ATTRS{serial}=="07032998B60AB777", SYMLINK+="usb_key/disk"
KERNEL=="sd?[0-9]", SUBSYSTEM=="block", ATTRS{serial}=="07032998B60AB777", SYMLINK+="usb_key/part%n"
```

				Once these rules are set in a file, named for example /etc/udev/rules.d/010_local.rules, you can simply remove and reconnect the USB key. You can then see that /dev/usb_key/disk represents the disk associated with the USB key, and /dev/usb_key/part1 is its first partition.
			

GOING FURTHER Debugging udev's configuration

				Like many daemons, udevd stores logs in /var/log/daemon.log. But it is not very verbose by default, and it is usually not enough to understand what is happening. The udevadm control --log-priority=info command increases the verbosity level and solves this problem. udevadm control --log-priority=err returns to the default verbosity level.
			[Prev9.10.Â Backup](sect.backup.en.md)[Up](#)[Home](index.en.md)[Next9.12.Â Power Management: Advanced Configuration an...](sect.power-management.en.md)