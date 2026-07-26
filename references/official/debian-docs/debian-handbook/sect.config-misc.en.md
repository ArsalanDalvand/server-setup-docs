---
title: "8.9.6.locateandupdatedb"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.config-misc.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.config-misc.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.9.6.locateandupdatedb"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.config-bootloader.en.md)The Debian Administrator's Handbook[Next](sect.kernel-compilation.en.md)

## 8.9.Â Other Configurations: Time Synchronization, Logs, Sharing Accessâ¦

			The many elements listed in this section are good to know for anyone who wants to master all aspects of configuration of the GNU/Linux system. They are, however, treated briefly and frequently refer to the documentation.
		

### 8.9.1.Â Timezone

BACK TO BASICS Symbolic links

				A symbolic link is a pointer to another file. When you access it, the file to which it points is opened. Removal of the link will not cause deletion of the file to which it points. Likewise, it does not have its own set of permissions, but rather retains the permissions of its target. Finally, it can point to any type of file: directories, special files (sockets, named pipes, device files, etc.), even other symbolic links.
			
				The ln -s target link-name command creates a symbolic link, named link-name, pointing to target.
			
				If the target does not exist, then the link is âbrokenâ and accessing it will result in an error indicating that the target file does not exist. If the link points to another link, you will have a âchainâ of links that turns into a âcycleâ if one of the targets points to one of its predecessors. In this case, accessing one of the links in the cycle will result in a specific error (âtoo many levels of symbolic linksâ); this means the kernel gave up after several rounds of the cycle.
			
				The timezone, configured during initial installation, is a configuration item for the tzdata package. To modify it, use the dpkg-reconfigure tzdata command, which allows you to choose the timezone to be used in an interactive manner. Its configuration is stored in the /etc/timezone file. Additionally, /etc/localtime becomes a symbolic link to the corresponding file in the /usr/share/zoneinfo; the file that contains the rules governing the dates where daylight saving time (DST) is active, for countries that use it.
			
				When you need to temporarily change the timezone, use the TZ environment variable, which takes priority over the configured system default:
			

```bash
$ date
Thu Sep  2 22:29:48 CEST 2021
$ TZ="Pacific/Honolulu" date
Thu 02 Sep 2021 10:31:01 AM HST
```

NOTE System clock, hardware clock

				There are two time sources in a computer. A computer's motherboard has a hardware clock, called the âCMOS clockâ. This clock is not very precise, and provides rather slow access times. The operating system kernel has its own, the software clock, which it keeps up to date with its own means (possibly with the help of time servers, see [SectionÂ 8.9.2, âTime Synchronizationâ](sect.config-misc.en.md#sect.time-synchronization)). This system clock is generally more accurate, especially since it doesn't need access to hardware variables. However, since it only exists in live memory, it is zeroed out every time the machine is booted, contrary to the CMOS clock, which has a battery and therefore âsurvivesâ rebooting or halting of the machine. The system clock is, thus, set from the CMOS clock during boot, and the CMOS clock is updated on shutdown (to take into account possible changes or corrections if it has been improperly adjusted).
			
				In practice, there is a problem, since the CMOS clock is nothing more than a counter and contains no information regarding the time zone. There is a choice to make regarding its interpretation: either the system considers it runs in universal time (UTC, formerly GMT), or in local time. This choice could be a simple shift, but things are actually more complicated: as a result of daylight saving time, this offset is not constant. The result is that the system has no way to determine whether the offset is correct, especially around periods of time change. Since it is always possible to reconstruct local time from universal time and the timezone information, we strongly recommend using the CMOS clock in universal time.
			
				Unfortunately, some Windows systems in their default configuration ignore this recommendation; they keep the CMOS clock on local time, applying time changes when booting the computer by trying to guess during time changes if the change has already been applied or not. This works relatively well, as long as the system has only Windows running on it. But when a computer has several systems (whether it be a âdual-bootâ configuration or running other systems via virtual machine), chaos ensues, with no means to determine if the time is correct. If you absolutely must retain Windows on a computer, you should either configure it to keep the CMOS clock as UTC (setting the registry key HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\RealTimeIsUniversal to â1â as a DWORD), or use hwclock --localtime --set on the Debian system to set the hardware clock and mark it as tracking the local time (and make sure to manually check your clock in spring and autumn).
			

### 8.9.2.Â Time Synchronization

				Time synchronization, which may seem superfluous on a computer, is very important on a network. Since users do not have permissions allowing them to modify the date and time, it is important for this information to be precise to prevent confusion. Furthermore, having all of the computers on a network synchronized allows better cross-referencing of information from logs on different machines. Thus, in the event of an attack, it is easier to reconstruct the chronological sequence of actions on the various machines involved in the compromise. Data collected on several machines for statistical purposes won't make a great deal of sense if they are not synchronized.
			

BACK TO BASICS NTP

				NTP (Network Time Protocol) allows a machine to synchronize with others fairly accurately, taking into consideration the delays induced by the transfer of information over the network and other possible offsets.
			
				While there are numerous NTP servers on the Internet, the more popular ones may be overloaded. This is why we recommend using the pool.ntp.org NTP server, which is, in reality, a group of machines that have agreed to serve as public NTP servers. You could even limit use to a sub-group specific to a country, with, for example, us.pool.ntp.org for the United States, or ca.pool.ntp.org for Canada, etc.
			
				However, if you manage a large network, it is recommended that you install your own NTP server, which will synchronize with the public servers. In this case, all the other machines on your network can use your internal NTP server instead of increasing the load on the public servers. You will also increase homogeneity with your clocks, since all the machines will be synchronized on the same source, and this source is very close in terms of network transfer times.
			

#### 8.9.2.1.Â For Workstations

					Since work stations are regularly rebooted (even if only to save energy), synchronizing them by NTP at boot is enough. To do so, simply install the ntpdate package. You can change the NTP server used if needed by modifying the /etc/default/ntpdate file.
				

#### 8.9.2.2.Â For Servers

					Servers are only rarely rebooted, and it is very important for their system time to be correct. To permanently maintain correct time, you would install a local NTP server, a service offered in the ntp package. In its default configuration, the server will synchronize with pool.ntp.org and provide time in response to requests coming from the local network. You can configure it by editing the /etc/ntp.conf file, the most significant alteration being the NTP server to which it refers. If the network has a lot of servers, it may be interesting to have one local time server which synchronizes with the public servers and is used as a time source by the other servers of the network.
				

GOING FURTHER GPS modules and other time sources

					If time synchronization is particularly crucial to your network, it is possible to equip a server with a GPS module (which will use the time from GPS satellites) or a DCF-77 module (which will sync time with the atomic clock near Frankfurt, Germany). In this case, the configuration of the NTP server is a little more complicated, and prior consultation of the documentation is an absolute necessity.
				

### 8.9.3.Â Rotating Log Files

				Log files can grow, fast, and it is necessary to archive them. The most common scheme is a rotating archive: the log file is regularly archived, and only the latest X archives are retained. logrotate, the program responsible for these rotations, follows directives given in the /etc/logrotate.conf file and all of the files in the /etc/logrotate.d/ directory. The administrator may modify these files, if they wish to adapt the log rotation policy defined by Debian. The logrotate(1) man page describes all of the options available in these configuration files. You may want to increase the number of files retained in log rotation, or move the log files to a specific directory dedicated to archiving them rather than delete them. You could also send them by e-mail to archive them elsewhere.
			
				The logrotate program is executed daily by the cron scheduling program (described in [SectionÂ 9.7, âScheduling Tasks with cron and atdâ](sect.task-scheduling-cron-atd.en.md)).
			

### 8.9.4.Â Sharing Administrator Rights

				Frequently, several administrators work on the same network. Sharing the root passwords is not very elegant, and opens the door for abuse due to the anonymity such sharing creates. The solution to this problem is the sudo program, which allows certain users to execute certain commands with special rights. In the most common use case, sudo allows a trusted user to execute any command as root. To do so, the user simply executes sudo command and authenticates using their personal password.
			
				When installed, the sudo package gives full root rights to members of the sudo Unix group. To delegate other rights, the administrator can use the visudo command, which allows them to modify the /etc/sudoers configuration file (here again, this will invoke the vi editor, or any other editor indicated in the EDITOR environment variable). Alternatively they might put rules in small files in /etc/sudoers.d/ as long as this directory is included by /etc/sudoers via @includedir /etc/sudoers.d, which is the default for Debian. Adding a line with username ALL=(ALL) ALL allows the user in question to execute any command as root.
			
				More sophisticated configurations allow authorization of only specific commands to specific users. All the details of the various possibilities are given in the sudoers(5) manual page.
			

### 8.9.5.Â List of Mount Points

BACK TO BASICS Mounting and unmounting

				In a Unix-like system such as Debian, files are organized in a single tree-like hierarchy of directories. The / directory is called the âroot directoryâ; all additional directories are sub-directories within this root. âMountingâ is the action of including the content of a peripheral device (often a hard drive) into the system's general file tree. As a consequence, if you use a separate hard drive to store users' personal data, this disk will have to be âmountedâ in the /home/ directory (during installation you already made relevant choices, see [SectionÂ 4.2.13.1, âGuided partitioningâ](sect.installation-steps.en.md#sect.install-autopartman-mode)) The root filesystem is always mounted at boot by the kernel; other devices are often mounted later during the startup sequence or manually with the mount command.
			
				Some removable devices are automatically mounted when connected, especially when using the GNOME, Plasma or other graphical desktop environments. Others have to be mounted manually by the user. Likewise, they must be unmounted (removed from the file tree). Normal users do not usually have permission to execute the mount and umount commands. The administrator can, however, authorize these operations (independently for each mount point) by including the user option in the /etc/fstab file.
			
				The mount command can be used without arguments to list all mounted filesystems (as found in /proc/mounts as well); you can execute findmnt --fstab to show only the filesystems from /etc/fstab. The following parameters are required to mount or unmount a device - for the complete list, please refer to the corresponding man pages, mount(8) and umount(8). For simple cases, the syntax is simple too: for example, to mount the /dev/sdc1 partition, which has an ext3 filesystem, into the /mnt/tmp/ directory, you would simply run mount -t ext3 /dev/sdc1 /mnt/tmp/.
			
				The /etc/fstab file gives a list of all possible mounts that happen either automatically on boot or manually for removable storage devices. Each mount point is described by a line with several space-separated fields:
			
						file system: this indicates where the filesystem to be mounted can be found, it can be a local device (hard drive partition, CD-ROM) or a remote filesystem (such as NFS or even SSHFS).
					
						This field is frequently replaced with the unique ID of the filesystem (which you can determine with blkid device) prefixed with UUID=. This guards against a change in the name of the device in the event of addition or removal of disks, or if disks are detected in a different order. [SectionÂ 8.8.1, âIdentifying the Disksâ](sect.config-bootloader.en.md#sect.identify-disks) covers this topic in more detail.
					
						mount point: this is the location on the local filesystem where the device, remote system, or partition will be mounted.
					
						type: this field defines the filesystem used on the mounted device. ext4, ext3, vfat, ntfs, btrfs, xfs are a few examples.
					

BACK TO BASICS NFS, a network filesystem

						NFS is a network filesystem; under Linux, it allows transparent access to remote files by including them in the local filesystem.
					
						A complete list of known filesystems is available in the mount(8) manual page. The swap special value is for swap partitions; the auto special value tells the mount program to automatically detect the filesystem (which is especially useful for disk readers and USB keys, since each one might have a different filesystem);
					
						options: there are many of them, depending on the filesystem, and they are documented in the mount man page. The most common are
					
rw or ro, meaning, respectively, that the device will be mounted with read/write or read-only permissions.
							
noauto deactivates automatic mounting on boot.
							
nofail allows the boot to proceed even when the device is not present. Make sure to put this option for external drives that might be unplugged when you boot, because systemd really ensures that all mount points that must be automatically mounted are actually mounted before letting the boot process continue to its end. Note that you can combine this with x-systemd.device-timeout=5s to tell systemd to not wait more than 5 seconds for the device to appear (see systemd.mount(5)).
							
user authorizes all users to mount this filesystem (an operation which would otherwise be restricted to the root user).
							
defaults means the group of default options: rw, suid, dev, exec, auto, nouser and async, each of which can be individually disabled after defaults by adding nosuid, nodev and so on to block suid, dev and so on. Adding the user option reactivates it, since defaults includes nouser.
							
						dump: this field is almost always set to 0 and kind of a relic. When it is greater then zero, it tells the dump tool that the partition contains data that is to be backed up frequently. The tool supports Ext2/3/4 filesystems only and will use the value here when run via dump -W or dump -w to determine which partitions need to be backed up. Consider the examples in /usr/share/doc/dump/examples/ if you want to use this feature. But there are better alternatives to backup a filesystem, like fsarchiver.
					
						pass: this last field indicates whether the integrity of the filesystem should be checked on boot, and in which order this check should be executed. If it is 0, no check is conducted. The root filesystem should have the value 1, while other permanent filesystems get the value 2.
					

ExampleÂ 8.5.Â Example /etc/fstab file

```bash
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# systemd generates mount units based on this file, see systemd.mount(5).
# Please run 'systemctl daemon-reload' after making changes here.
#
# <file system>                           <mount point>   <type>      <options>         <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=7a250fb8-c16d-4a4e-9808-ec08ae92b6c6 /               ext4        errors=remount-ro 0       1
# swap was on /dev/sda5 during installation
UUID=13f367ae-dbaf-40ed-85c0-4072a2ebe426 none            swap        sw                0       0
/dev/sr0                                  /media/cdrom0   udf,iso9660 user,noauto       0       0
/dev/fd0                                  /media/floppy   auto        rw,user,noauto    0       0
arrakis:/shared                           /shared         nfs         defaults          0       0
```

					The last entry in the example corresponds to a network filesystem (NFS): the /shared/ directory on the arrakis server is mounted at /shared/ on the local machine.
				
				The format of the /etc/fstab file is documented in the fstab(5) manual page.
			

GOING FURTHER Auto-mounting

systemd is able to manage automount points: those are filesystems that are mounted on-demand when a user attempts to access their target mount points. It can also unmount these filesystems when no process is accessing them any longer.
			
				Like most concepts in systemd, automount points are managed with dedicated units, using the .automount suffix. See systemd.automount(5) for their precise syntax.
			
				Other auto-mounting utilities exist, such as automount in the autofs package or amd in the am-utils package.
			
				Note also that GNOME, Plasma, and other graphical desktop environments work together with udisks2, and can automatically mount removable media when they are connected.
			

### 8.9.6.Â locate and updatedb

				The locate command can find the location of a file when you only know part of the name. It sends a result almost instantaneously, since it consults a database that stores the location of all the files on the system; this database is updated daily by the updatedb command. There are multiple implementations of the locate command and Debian picked mlocate for its standard system. If you want to consider an alternative, you can try plocate which provides the same command line options and can be considered a drop-in replacement.
			
locate is smart enough to only return files which are accessible to the user running the command even though it uses a database that knows about all files on the system (since its updatedb implementation runs with root rights). For extra safety, the administrator can use PRUNEDPATHS in /etc/updatedb.conf to exclude some directories from being indexed.
			[Prev8.8.Â Configuring the Bootloader](sect.config-bootloader.en.md)[Up](#)[Home](index.en.md)[Next8.10.Â Compiling a Kernel](sect.kernel-compilation.en.md)