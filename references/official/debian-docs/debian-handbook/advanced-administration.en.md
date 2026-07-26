---
title: "12.1.3. RAID or LVM?"
source: "https://www.debian.org/doc/manuals/debian-handbook/advanced-administration.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/advanced-administration.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "12.1.3. RAID or LVM?"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.rtc-services.en.md)The Debian Administrator's Handbook[Next](sect.virtualization.en.md)

# ChapterÂ 12.Â Advanced Administration

[12.1. RAID and LVM](advanced-administration.en.md#sect.raid-and-lvm)[12.1.1. Software RAID](advanced-administration.en.md#sect.raid-soft)[12.1.2. LVM](advanced-administration.en.md#sect.lvm)[12.1.3. RAID or LVM?](advanced-administration.en.md#sect.raid-or-lvm)[12.2. Virtualization](sect.virtualization.en.md)[12.2.1. Xen](sect.virtualization.en.md#sect.xen)[12.2.2. LXC](sect.virtualization.en.md#sect.lxc)[12.2.3. Virtualization with KVM](sect.virtualization.en.md#id-1.15.5.16)[12.3. Automated Installation](sect.automated-installation.en.md)[12.3.1. Fully Automatic Installer (FAI)](sect.automated-installation.en.md#sect.fai)[12.3.2. Preseeding Debian-Installer](sect.automated-installation.en.md#sect.d-i-preseeding)[12.3.3. Simple-CDD: The All-In-One Solution](sect.automated-installation.en.md#sect.simple-cdd)[12.4. Monitoring](sect.monitoring.en.md)[12.4.1. Setting Up Munin](sect.monitoring.en.md#sect.munin)[12.4.2. Setting Up Nagios](sect.monitoring.en.md#sect.nagios)
		This chapter revisits some aspects we already described, with a different perspective: instead of installing one single computer, we will study mass-deployment systems; instead of creating RAID or LVM volumes at install time, we'll learn to do it by hand so we can later revise our initial choices. Finally, we will discuss monitoring tools and virtualization techniques. As a consequence, this chapter is more particularly targeting professional administrators, and focuses somewhat less on individuals responsible for their home network.
	

## 12.1.Â RAID and LVM

[ChapterÂ 4, Installation](installation.en.md) presented these technologies from the point of view of the installer, and how it integrated them to make their deployment easy from the start. After the initial installation, an administrator must be able to handle evolving storage space needs without having to resort to an expensive re-installation. They must therefore understand the required tools for manipulating RAID and LVM volumes.
		
			RAID and LVM are both techniques to abstract the mounted volumes from their physical counterparts (actual hard-disk drives or partitions thereof); the former ensures the security and availability of the data in case of hardware failure by introducing redundancy, the latter makes volume management more flexible and independent of the actual size of the underlying disks. In both cases, the system ends up with new block devices, which can be used to create filesystems or swap space, without necessarily having them mapped to one physical disk. RAID and LVM come from quite different backgrounds, but their functionality can overlap somewhat, which is why they are often mentioned together.
		

PERSPECTIVE Btrfs combines LVM and RAID

			While LVM and RAID are two distinct kernel subsystems that come between the disk block devices and their filesystems, btrfs is a filesystem, initially developed at Oracle, that purports to combine the feature sets of LVM and RAID and much more. âÂ [https://btrfs.wiki.kernel.org/](https://btrfs.wiki.kernel.org/)

			Among the noteworthy features are the ability to take a snapshot of a filesystem tree at any point in time. This snapshot copy doesn't initially use any disk space, the data only being duplicated when one of the copies is modified. The filesystem also handles transparent compression of files, and checksums ensure the integrity of all stored data.
		
			In both the RAID and LVM cases, the kernel provides a block device file, similar to the ones corresponding to a hard disk drive or a partition. When an application, or another part of the kernel, requires access to a block of such a device, the appropriate subsystem routes the block to the relevant physical layer. Depending on the configuration, this block can be stored on one or several physical disks, and its physical location may not be directly correlated to the location of the block in the logical device.
		

### 12.1.1.Â Software RAID

				RAID means Redundant Array of Independent Disks. The goal of this system is to prevent data loss and ensure availability in case of hard disk failure. The general principle is quite simple: data are stored on several physical disks instead of only one, with a configurable level of redundancy. Depending on this amount of redundancy, and even in the event of an unexpected disk failure, data can be losslessly reconstructed from the remaining disks.
			

CULTURE Independent or inexpensive?

				The I in RAID initially stood for inexpensive, because RAID allowed a drastic increase in data safety without requiring investing in expensive high-end disks. Probably due to image concerns, however, it is now more customarily considered to stand for independent, which doesn't have the unsavory flavor of cheapness.
			
				RAID can be implemented either by dedicated hardware (RAID modules integrated into SCSI or SATA controller cards) or by software abstraction (the kernel). Whether hardware or software, a RAID system with enough redundancy can transparently stay operational when a disk fails; the upper layers of the stack (applications) can even keep accessing the data in spite of the failure. Of course, this âdegraded modeâ can have an impact on performance, and redundancy is reduced, so a further disk failure can lead to data loss. In practice, therefore, one will strive to only stay in this degraded mode for as long as it takes to replace the failed disk. Once the new disk is in place, the RAID system can reconstruct the required data so as to return to a safe mode. The applications won't notice anything, apart from potentially reduced access speed, while the array is in degraded mode or during the reconstruction phase.
			
				When RAID is implemented by hardware, its configuration generally happens within the BIOS setup tool, and the kernel will consider a RAID array as a single disk, which will work as a standard physical disk, although the device name may be different (depending on the driver).
			
				We only focus on software RAID in this book.
			

#### 12.1.1.1.Â Different RAID Levels

					RAID is actually not a single system, but a range of systems identified by their levels; the levels differ by their layout and the amount of redundancy they provide. The more redundant, the more failure-proof, since the system will be able to keep working with more failed disks. The counterpart is that the usable space shrinks for a given set of disks; seen the other way, more disks will be needed to store a given amount of data.
				Linear RAID
								Even though the kernel's RAID subsystem allows creating âlinear RAIDâ, this is not proper RAID, since this setup doesn't involve any redundancy. The kernel merely aggregates several disks end-to-end and provides the resulting aggregated volume as one virtual disk (one block device). That is about its only function. This setup is rarely used by itself (see later for the exceptions), especially since the lack of redundancy means that one disk failing makes the whole aggregate, and therefore all the data, unavailable.
							RAID-0
								This level doesn't provide any redundancy either, but disks aren't simply stuck on end one after another: they are divided in stripes, and the blocks on the virtual device are stored on stripes on alternating physical disks. In a two-disk RAID-0 setup, for instance, even-numbered blocks of the virtual device will be stored on the first physical disk, while odd-numbered blocks will end up on the second physical disk.
							
								This system doesn't aim at increasing reliability, since (as in the linear case) the availability of all the data is jeopardized as soon as one disk fails, but at increasing performance: during sequential access to large amounts of contiguous data, the kernel will be able to read from both disks (or write to them) in parallel, which increases the data transfer rate. The disks are utilized entirely by the RAID device, so they should have the same size not to lose performance.
							
								RAID-0 use is shrinking, its niche being filled by LVM (see later).
							RAID-1
								This level, also known as âRAID mirroringâ, is both the simplest and the most widely used setup. In its standard form, it uses two physical disks of the same size, and provides a logical volume of the same size again. Data are stored identically on both disks, hence the âmirrorâ nickname. When one disk fails, the data is still available on the other. For really critical data, RAID-1 can of course be set up on more than two disks, with a direct impact on the ratio of hardware cost versus available payload space.
							

NOTE Disks and cluster sizes

								If two disks of different sizes are set up in a mirror, the bigger one will not be fully used, since it will contain the same data as the smallest one and nothing more. The useful available space provided by a RAID-1 volume therefore matches the size of the smallest disk in the array. This still holds for RAID volumes with a higher RAID level, even though redundancy is stored differently.
							
								It is therefore important, when setting up RAID arrays (except for RAID-0 and âlinear RAIDâ), to only assemble disks of identical, or very close, sizes, to avoid wasting resources.
							

NOTE Spare disks

								RAID levels that include redundancy allow assigning more disks than required to an array. The extra disks are used as spares when one of the main disks fails. For instance, in a mirror of two disks plus one spare, if one of the first two disks fails, the kernel will automatically (and immediately) reconstruct the mirror using the spare disk, so that redundancy stays assured after the reconstruction time. This can be used as another kind of safeguard for critical data.
							
								One would be forgiven for wondering how this is better than simply mirroring on three disks to start with. The advantage of the âspare diskâ configuration is that the spare disk can be shared across several RAID volumes. For instance, one can have three mirrored volumes, with redundancy assured even in the event of one disk failure, with only seven disks (three pairs, plus one shared spare), instead of the nine disks that would be required by three triplets.
							
								This RAID level, although expensive (since only half of the physical storage space, at best, is useful), is widely used in practice. It is simple to understand, and it allows very simple backups: since both disks have identical contents, one of them can be temporarily extracted with no impact on the working system. Read performance is often increased since the kernel can read half of the data on each disk in parallel, while write performance isn't too severely degraded. In case of a RAID-1 array of N disks, the data stays available even with N-1 disk failures.
							

CAUTION RAID is not Backup

								RAID systems are not backup mechanisms. While RAID increases the redundancy - and therefore the availability of a system - and protects against disk failures, backups are done to protect data from being altered, deleted, getting corrupted, etc., and to be able to restore them if necessary. To demonstrate this: If you remove one or all files by accident, a RAID will mirror this change, but it will not provide the means to restore the file(s). So while there is clearly an overlap, they are not the same and should be used in conjunction with each other.
							RAID-4
								This RAID level, not widely used, uses N disks to store useful data, and an extra disk to store redundancy information. If that disk fails, the system can reconstruct its contents from the other N. If one of the N data disks fails, the remaining N-1 combined with the âparityâ disk contain enough information to reconstruct the required data.
							
								RAID-4 isn't too expensive since it only involves a one-in-N increase in costs and has no noticeable impact on read performance, but writes are slowed down. Furthermore, since a write to any of the N disks also involves a write to the parity disk, the latter sees many more writes than the former, and its lifespan can shorten dramatically as a consequence. Data on a RAID-4 array is safe only up to one failed disk (of the N+1).
							RAID-5
								RAID-5 addresses the asymmetry issue of RAID-4: parity blocks are spread over all of the N+1 disks, with no single disk having a particular role.
							
								Read and write performance are identical to RAID-4. Here again, the system stays functional with up to one failed disk (of the N+1), but no more.
							RAID-6
								RAID-6 can be considered an extension of RAID-5, where each series of N blocks involves two redundancy blocks, and each such series of N+2 blocks is spread over N+2 disks.
							
								This RAID level is slightly more expensive than the previous two, but it brings some extra safety since up to two drives (of the N+2) can fail without compromising data availability. The counterpart is that write operations now involve writing one data block and two redundancy blocks, which makes them even slower.
							RAID-1+0
								This isn't strictly speaking, a RAID level, but a stacking of two RAID groupings. Starting from 2ÃN disks, one first sets them up by pairs into N RAID-1 volumes; these N volumes are then aggregated into one, either by âlinear RAIDâ or (increasingly) by LVM. This last case goes farther than pure RAID, but there is no problem with that.
							
								RAID-1+0 can survive multiple disk failures: up to N in the 2ÃN array described above, provided that at least one disk keeps working in each of the RAID-1 pairs.
							

GOING FURTHER RAID-10

								RAID-10 is generally considered a synonym of RAID-1+0, but a Linux specificity makes it actually a generalization. This setup allows a system where each block is stored on two different disks, even with an odd number of disks, the copies being spread out along a configurable model.
							
								Performances will vary depending on the chosen repartition model and redundancy level, and of the workload of the logical volume.
							
					Obviously, the RAID level will be chosen according to the constraints and requirements of each application. Note that a single computer can have several distinct RAID arrays with different configurations.
				

#### 12.1.1.2.Â Setting up RAID

					Setting up RAID volumes requires the mdadm package; it provides the mdadm command, which allows creating and manipulating RAID arrays, as well as scripts and tools integrating it to the rest of the system, including the monitoring system.
				
					Our example will be a server with a number of disks, some of which are already used, the rest being available to setup RAID. We initially have the following disks and partitions:
				
							the sdb disk, 4Â GB, is entirely available;
						
							the sdc disk, 4Â GB, is also entirely available;
						
							on the sdd disk, only partition sdd2 (about 4Â GB) is available;
						
							finally, a sde disk, still 4Â GB, entirely available.
						

NOTE Identifying existing RAID volumes

					The /proc/mdstat file lists existing volumes and their states. When creating a new RAID volume, care should be taken not to name it the same as an existing volume.
				
					We're going to use these physical elements to build two volumes, one RAID-0 and one mirror (RAID-1). Let's start with the RAID-0 volume:
				

```bash
# mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
# mdadm --query /dev/md0
/dev/md0: 7.99GiB raid0 2 devices, 0 spares. Use mdadm --detail for more detail.
# mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Mon Feb 28 01:54:24 2022
        Raid Level : raid0
        Array Size : 8378368 (7.99 GiB 8.58 GB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Mon Feb 28 01:54:24 2022
             State : clean 
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 0

            Layout : -unknown-
        Chunk Size : 512K

Consistency Policy : none

              Name : debian:0  (local to host debian)
              UUID : a75ac628:b384c441:157137ac:c04cd98c
            Events : 0

    Number   Major   Minor   RaidDevice State
       0       8        0        0      active sync   /dev/sdb
       1       8       16        1      active sync   /dev/sdc
# mkfs.ext4 /dev/md0
mke2fs 1.46.2 (28-Feb-2021)
Discarding device blocks: done                            
Creating filesystem with 2094592 4k blocks and 524288 inodes
Filesystem UUID: ef077204-c477-4430-bf01-52288237bea0
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done 

# mkdir /srv/raid-0
# mount /dev/md0 /srv/raid-0
# df -h /srv/raid-0
Filesystem      Size  Used Avail Use% Mounted on
/dev/md0        7.8G   24K  7.4G   1% /srv/raid-0
```

					The mdadm --create command requires several parameters: the name of the volume to create (/dev/md*, with MD standing for Multiple Device), the RAID level, the number of disks (which is compulsory despite being mostly meaningful only with RAID-1 and above), and the physical drives to use. Once the device is created, we can use it like we'd use a normal partition, create a filesystem on it, mount that filesystem, and so on. Note that our creation of a RAID-0 volume on md0 is nothing but coincidence, and the numbering of the array doesn't need to be correlated to the chosen amount of redundancy. It is also possible to create named RAID arrays, by giving mdadm parameters such as /dev/md/linear instead of /dev/md0.
				
					Creation of a RAID-1 follows a similar fashion, the differences only being noticeable after the creation:
				

```bash
# mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/sdd2 /dev/sde
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: largest drive (/dev/sdc2) exceeds size (4189184K) by more than 1%
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
# mdadm --query /dev/md1
/dev/md1: 4.00GiB raid1 2 devices, 0 spares. Use mdadm --detail for more detail.
# mdadm --detail /dev/md1
/dev/md1:
           Version : 1.2
     Creation Time : Mon Feb 28 02:07:48 2022
        Raid Level : raid1
        Array Size : 4189184 (4.00 GiB 4.29 GB)
     Used Dev Size : 4189184 (4.00 GiB 4.29 GB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Mon Feb 28 02:08:09 2022
             State : clean, resync
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 0

Consistency Policy : resync

    Rebuild Status : 13% complete

              Name : debian:1  (local to host debian)
              UUID : 2dfb7fd5:e09e0527:0b5a905a:8334adb8
            Events : 17

    Number   Major   Minor   RaidDevice State
       0       8       34        0      active sync   /dev/sdd2
       1       8       48        1      active sync   /dev/sde
# mdadm --detail /dev/md1
/dev/md1:
[...]
          State : clean
[...]
```

TIP RAID, disks and partitions

					As illustrated by our example, RAID devices can be constructed out of disk partitions as well, and do not require full disks.
				
					A few remarks are in order. First, mdadm notices that the physical elements have different sizes; since this implies that some space will be lost on the bigger element, a confirmation is required.
				
					More importantly, note the state of the mirror. The normal state of a RAID mirror is that both disks have exactly the same contents. However, nothing guarantees this is the case when the volume is first created. The RAID subsystem will therefore provide that guarantee itself, and there will be a synchronization phase as soon as the RAID device is created. After some time (the exact amount will depend on the actual size of the disksâ¦), the RAID array switches to the âactiveâ or âcleanâ state. Note that during this reconstruction phase, the mirror is in a degraded mode, and redundancy isn't assured. A disk failing during that risk window could lead to losing all the data. Large amounts of critical data, however, are rarely stored on a freshly created RAID array before its initial synchronization. Note that even in degraded mode, the /dev/md1 is usable, and a filesystem can be created on it, as well as some data copied on it.
				

TIP Starting a mirror in degraded mode

					Sometimes two disks are not immediately available when one wants to start a RAID-1 mirror, for instance because one of the disks one plans to include is already used to store the data one wants to move to the array. In such circumstances, it is possible to deliberately create a degraded RAID-1 array by passing missing instead of a device file as one of the arguments to mdadm. Once the data have been copied to the âmirrorâ, the old disk can be added to the array. A synchronization will then take place, giving us the redundancy that was wanted in the first place.
				

TIP Setting up a mirror without synchronization

					RAID-1 volumes are often created to be used as a new disk, often considered blank. The actual initial contents of the disk is therefore not very relevant, since one only needs to know that the data written after the creation of the volume, in particular the filesystem, can be accessed later.
				
					One might therefore wonder about the point of synchronizing both disks at creation time. Why care whether the contents are identical on zones of the volume that we know will only be read after we have written to them?
				
					Fortunately, this synchronization phase can be avoided by passing the --assume-clean option to mdadm. However, this option can lead to surprises in cases where the initial data will be read (for instance if a filesystem is already present on the physical disks), which is why it isn't enabled by default.
				
					Now let's see what happens when one of the elements of the RAID-1 array fails. mdadm, in particular its --fail option, allows simulating such a disk failure:
				

```bash
# mdadm /dev/md1 --fail /dev/sde
mdadm: set /dev/sde faulty in /dev/md1
# mdadm --detail /dev/md1
/dev/md1:
           Version : 1.2
     Creation Time : Mon Feb 28 02:07:48 2022
        Raid Level : raid1
        Array Size : 4189184 (4.00 GiB 4.29 GB)
     Used Dev Size : 4189184 (4.00 GiB 4.29 GB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Mon Feb 28 02:15:34 2022
             State : clean, degraded 
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 1
     Spare Devices : 0

Consistency Policy : resync

              Name : debian:1  (local to host debian)
              UUID : 2dfb7fd5:e09e0527:0b5a905a:8334adb8
            Events : 19

    Number   Major   Minor   RaidDevice State
       0       8       34        0      active sync   /dev/sdd2
       -       0        0        1      removed

       1       8       48        -      faulty   /dev/sde
```

					The contents of the volume are still accessible (and, if it is mounted, the applications don't notice a thing), but the data safety isn't assured anymore: should the sdd disk fail in turn, the data would be lost. We want to avoid that risk, so we'll replace the failed disk with a new one, sdf:
				

```bash
# mdadm /dev/md1 --add /dev/sdf
mdadm: added /dev/sdf
# mdadm --detail /dev/md1
/dev/md1:
           Version : 1.2
     Creation Time : Mon Feb 28 02:07:48 2022
        Raid Level : raid1
        Array Size : 4189184 (4.00 GiB 4.29 GB)
     Used Dev Size : 4189184 (4.00 GiB 4.29 GB)
      Raid Devices : 2
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Mon Feb 28 02:25:34 2022
             State : clean, degraded, recovering 
    Active Devices : 1
   Working Devices : 2
    Failed Devices : 1
     Spare Devices : 1

Consistency Policy : resync

    Rebuild Status : 47% complete

              Name : debian:1  (local to host debian)
              UUID : 2dfb7fd5:e09e0527:0b5a905a:8334adb8
            Events : 39

    Number   Major   Minor   RaidDevice State
       0       8       34        0      active sync   /dev/sdd2
       2       8       64        1      spare rebuilding   /dev/sdf

       1       8       48        -      faulty   /dev/sde
# [...]
[...]
# mdadm --detail /dev/md1
/dev/md1:
           Version : 1.2
     Creation Time : Mon Feb 28 02:07:48 2022
        Raid Level : raid1
        Array Size : 4189184 (4.00 GiB 4.29 GB)
     Used Dev Size : 4189184 (4.00 GiB 4.29 GB)
      Raid Devices : 2
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Mon Feb 28 02:25:34 2022
             State : clean
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 1
     Spare Devices : 0

Consistency Policy : resync

              Name : debian:1  (local to host debian)
              UUID : 2dfb7fd5:e09e0527:0b5a905a:8334adb8
            Events : 41

    Number   Major   Minor   RaidDevice State
       0       8       34        0      active sync   /dev/sdd2
       2       8       64        1      active sync   /dev/sdf

       1       8       48        -      faulty   /dev/sde
```

					Here again, the kernel automatically triggers a reconstruction phase during which the volume, although still accessible, is in a degraded mode. Once the reconstruction is over, the RAID array is back to a normal state. One can then tell the system that the sde disk is about to be removed from the array, so as to end up with a classical RAID mirror on two disks:
				

```bash
# mdadm /dev/md1 --remove /dev/sde
mdadm: hot removed /dev/sde from /dev/md1
# mdadm --detail /dev/md1
/dev/md1:
[...]
    Number   Major   Minor   RaidDevice State
       0       8       34        0      active sync   /dev/sdd2
       2       8       64        1      active sync   /dev/sdf
```

					From then on, the drive can be physically removed when the server is next switched off, or even hot-removed when the hardware configuration allows hot-swap. Such configurations include some SCSI controllers, most SATA disks, and external drives operating on USB or Firewire.
				

#### 12.1.1.3.Â Backing up the Configuration

					Most of the meta-data concerning RAID volumes are saved directly on the disks that make up these arrays, so that the kernel can detect the arrays and their components and assemble them automatically when the system starts up. However, backing up this configuration is encouraged, because this detection isn't fail-proof, and it is only expected that it will fail precisely in sensitive circumstances. In our example, if the sde disk failure had been real (instead of simulated) and the system had been restarted without removing this sde disk, this disk could start working again due to having been probed during the reboot. The kernel would then have three physical elements, each claiming to contain half of the same RAID volume. In reality this leads to the RAID starting from the individual disks alternately - distributing the data also alternately, depending on which disk started the RAID in degraded mode Another source of confusion can come when RAID volumes from two servers are consolidated onto one server only. If these arrays were running normally before the disks were moved, the kernel would be able to detect and reassemble the pairs properly; but if the moved disks had been aggregated into an md1 on the old server, and the new server already has an md1, one of the mirrors would be renamed.
				
					Backing up the configuration is therefore important, if only for reference. The standard way to do it is by editing the /etc/mdadm/mdadm.conf file, an example of which is listed here:
				

ExampleÂ 12.1.Â mdadm configuration file

```bash
# mdadm.conf
#
# !NB! Run update-initramfs -u after updating this file.
# !NB! This will ensure that initramfs has an uptodate copy.
#
# Please refer to mdadm.conf(5) for information about this file.
#

# by default (built-in), scan all partitions (/proc/partitions) and all
# containers for MD superblocks. alternatively, specify devices to scan, using
# wildcards if desired.
DEVICE /dev/sd*

# automatically tag new arrays as belonging to the local system
HOMEHOST <system>

# instruct the monitoring daemon where to send mail alerts
MAILADDR root

# definitions of existing MD arrays
ARRAY /dev/md/0  metadata=1.2 UUID=a75ac628:b384c441:157137ac:c04cd98c name=debian:0
ARRAY /dev/md/1  metadata=1.2 UUID=2dfb7fd5:e09e0527:0b5a905a:8334adb8 name=debian:1
# This configuration was auto-generated on Mon, 28 Feb 2022 01:53:48 +0100 by mkconf
```

					One of the most useful details is the DEVICE option, which lists the devices where the system will automatically look for components of RAID volumes at start-up time. In our example, we replaced the default value, partitions containers, with an explicit list of device files, since we chose to use entire disks and not only partitions, for some volumes.
				
					The last two lines in our example are those allowing the kernel to safely pick which volume number to assign to which array. The metadata stored on the disks themselves are enough to re-assemble the volumes, but not to determine the volume number (and the matching /dev/md* device name).
				
					Fortunately, these lines can be generated automatically:
				

```bash
# mdadm --misc --detail --brief /dev/md?
ARRAY /dev/md/0  metadata=1.2 UUID=a75ac628:b384c441:157137ac:c04cd98c name=debian:0
ARRAY /dev/md/1  metadata=1.2 UUID=2dfb7fd5:e09e0527:0b5a905a:8334adb8 name=debian:1
```

					The contents of these last two lines doesn't depend on the list of disks included in the volume. It is therefore not necessary to regenerate these lines when replacing a failed disk with a new one. On the other hand, care must be taken to update the file when creating or deleting a RAID array.
				

### 12.1.2.Â LVM

				LVM, the Logical Volume Manager, is another approach to abstracting logical volumes from their physical supports, which focuses on increasing flexibility rather than increasing reliability. LVM allows changing a logical volume transparently as far as the applications are concerned; for instance, it is possible to add new disks, migrate the data to them, and remove the old disks, without unmounting the volume.
			

#### 12.1.2.1.Â LVM Concepts

					This flexibility is attained by a level of abstraction involving three concepts.
				
					First, the PV (Physical Volume) is the entity closest to the hardware: it can be partitions on a disk, or a full disk, or even any other block device (including, for instance, a RAID array). Note that when a physical element is set up to be a PV for LVM, it should only be accessed via LVM, otherwise the system will get confused.
				
					A number of PVs can be clustered in a VG (Volume Group), which can be compared to disks both virtual and extensible. VGs are abstract, and don't appear in a device file in the /dev hierarchy, so there is no risk of using them directly.
				
					The third kind of object is the LV (Logical Volume), which is a chunk of a VG; if we keep the VG-as-disk analogy, the LV compares to a partition. The LV appears as a block device with an entry in /dev, and it can be used as any other physical partition can be (most commonly, to host a filesystem or swap space).
				
					The important thing is that the splitting of a VG into LVs is entirely independent of its physical components (the PVs). A VG with only a single physical component (a disk for instance) can be split into a dozen logical volumes; similarly, a VG can use several physical disks and appear as a single large logical volume. The only constraint, obviously, is that the total size allocated to LVs can't be bigger than the total capacity of the PVs in the volume group.
				
					It often makes sense, however, to have some kind of homogeneity among the physical components of a VG, and to split the VG into logical volumes that will have similar usage patterns. For instance, if the available hardware includes fast disks and slower disks, the fast ones could be clustered into one VG and the slower ones into another; chunks of the first one can then be assigned to applications requiring fast data access, while the second one will be kept for less demanding tasks.
				
					In any case, keep in mind that an LV isn't particularly attached to any one PV. It is possible to influence where the data from an LV are physically stored, but this possibility isn't required for day-to-day use. On the contrary: when the set of physical components of a VG evolves, the physical storage locations corresponding to a particular LV can be migrated across disks (while staying within the PVs assigned to the VG, of course).
				

#### 12.1.2.2.Â Setting up LVM

					Let us now follow, step by step, the process of setting up LVM for a typical use case: we want to simplify a complex storage situation. Such a situation usually happens after some long and convoluted history of accumulated temporary measures. For the purposes of illustration, we'll consider a server where the storage needs have changed over time, ending up in a maze of available partitions split over several partially used disks. In more concrete terms, the following partitions are available:
				
							on the sdb disk, a sdb2 partition, 4Â GB;
						
							on the sdc disk, a sdc3 partition, 3Â GB;
						
							the sdd disk, 4Â GB, is fully available;
						
							on the sdf disk, a sdf1 partition, 4Â GB; and a sdf2 partition, 5Â GB.
						
					In addition, let's assume that disks sdb and sdf are faster than the other two.
				
					Our goal is to set up three logical volumes for three different applications: a file server requiring 5Â GB of storage space, a database (1Â GB) and some space for back-ups (12Â GB). The first two need good performance, but back-ups are less critical in terms of access speed. All these constraints prevent the use of partitions on their own; using LVM can abstract the physical size of the devices, so the only limit is the total available space.
				
					The required tools are in the lvm2 package and its dependencies. When they're installed, setting up LVM takes three steps, matching the three levels of concepts.
				
					First, we prepare the physical volumes using pvcreate:
				

```bash
# pvcreate /dev/sdb2
  Physical volume "/dev/sdb2" successfully created.
# pvdisplay
  "/dev/sdb2" is a new physical volume of "4.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb2
  VG Name               
  PV Size               4.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               yK0K6K-clbc-wt6e-qk9o-aUh9-oQqC-k1T71B

# for i in sdc3 sdd sdf1 sdf2 ; do pvcreate /dev/$i ; done
  Physical volume "/dev/sdc3" successfully created.
  Physical volume "/dev/sdd" successfully created.
  Physical volume "/dev/sdf1" successfully created.
  Physical volume "/dev/sdf2" successfully created.
# pvdisplay -C
  PV         VG Fmt  Attr PSize PFree
  /dev/sdb2     lvm2 ---  4.00g 4.00g
  /dev/sdc3     lvm2 ---  3.00g 3.00g
  /dev/sdd      lvm2 ---  4.00g 4.00g
  /dev/sdf1     lvm2 ---  4.00g 4.00g
  /dev/sdf2     lvm2 ---  5.00g 5.00g
```

					So far, so good; note that a PV can be set up on a full disk as well as on individual partitions of it. As shown above, the pvdisplay command lists the existing PVs, with two possible output formats.
				
					Now let's assemble these physical elements into VGs using vgcreate. We'll gather only PVs from the fast disks into a vg_critical VG; the other VG, vg_normal, will also include slower elements.
				

```bash
# vgcreate vg_critical /dev/sdb2 /dev/sdf1
  Volume group "vg_critical" successfully created
# vgdisplay
  --- Volume group ---
  VG Name               vg_critical
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               7.99 GiB
  PE Size               4.00 MiB
  Total PE              2046
  Alloc PE / Size       0 / 0   
  Free  PE / Size       2046 / 7.99 GiB
  VG UUID               JgFWU3-emKg-9QA1-stPj-FkGX-mGFb-4kzy1G

# vgcreate vg_normal /dev/sdc3 /dev/sdd /dev/sdf2
  Volume group "vg_normal" successfully created
# vgdisplay -C
  VG          #PV #LV #SN Attr   VSize   VFree  
  vg_critical   2   0   0 wz--n-   7.99g   7.99g
  vg_normal     3   0   0 wz--n- <11.99g <11.99g
```

					Here again, commands are rather straightforward (and vgdisplay proposes two output formats). Note that it is quite possible to use two partitions of the same physical disk into two different VGs. Note also that we used a vg_ prefix to name our VGs, but it is nothing more than a convention.
				
					We now have two âvirtual disksâ, sized about 8Â GB and 12Â GB respectively. Let's now carve them up into âvirtual partitionsâ (LVs). This involves the lvcreate command, and a slightly more complex syntax:
				

```bash
# lvdisplay
# lvcreate -n lv_files -L 5G vg_critical
  Logical volume "lv_files" created.
# lvdisplay
  --- Logical volume ---
  LV Path                /dev/vg_critical/lv_files
  LV Name                lv_files
  VG Name                vg_critical
  LV UUID                Nr62xe-Zu7d-0u3z-Yyyp-7Cj1-Ej2t-gw04Xd
  LV Write Access        read/write
  LV Creation host, time debian, 2022-03-01 00:17:46 +0100
  LV Status              available
  # open                 0
  LV Size                5.00 GiB
  Current LE             1280
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

# lvcreate -n lv_base -L 1G vg_critical
  Logical volume "lv_base" created.
# lvcreate -n lv_backups -L 11.98G vg_normal
  Rounding up size to full physical extent 11.98 GiB
  Rounding up size to full physical extent 11.98 GiB
  Logical volume "lv_backups" created.
# lvdisplay -C
  LV         VG          Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_base    vg_critical -wi-a-----  1.00g                                                    
  lv_files   vg_critical -wi-a-----  5.00g                                                    
  lv_backups vg_normal   -wi-a----- 11.98g
```

					Two parameters are required when creating logical volumes; they must be passed to the lvcreate as options. The name of the LV to be created is specified with the -n option, and its size is generally given using the -L option. We also need to tell the command what VG to operate on, of course, hence the last parameter on the command line.
				

GOING FURTHER lvcreate options

					The lvcreate command has several options to allow tweaking how the LV is created.
				
					Let's first describe the -l option, with which the LV's size can be given as a number of blocks (as opposed to the âhumanâ units we used above). These blocks (called PEs, physical extents, in LVM terms) are contiguous units of storage space in PVs, and they can't be split across LVs. When one wants to define storage space for an LV with some precision, for instance to use the full available space, the -l option will probably be preferred over -L.
				
					It is also possible to hint at the physical location of an LV, so that its extents are stored on a particular PV (while staying within the ones assigned to the VG, of course). Since we know that sdb is faster than sdf, we may want to store the lv_base there if we want to give an advantage to the database server compared to the file server. The command line becomes: lvcreate -n lv_base -L 1G vg_critical /dev/sdb2. Note that this command can fail if the PV doesn't have enough free extents. In our example, we would probably have to create lv_base before lv_files to avoid this situation â or free up some space on sdb2 with the pvmove command.
				
					Logical volumes, once created, end up as block device files in /dev/mapper/:
				

```bash
# ls -l /dev/mapper
total 0
crw------- 1 root root 10, 236 Mar  1 00:17 control
lrwxrwxrwx 1 root root       7 Mar  1 00:19 vg_critical-lv_base -> ../dm-1
lrwxrwxrwx 1 root root       7 Mar  1 00:17 vg_critical-lv_files -> ../dm-0
lrwxrwxrwx 1 root root       7 Mar  1 00:19 vg_normal-lv_backups -> ../dm-2 
# ls -l /dev/dm-*
brw-rw---- 1 root disk 253, 0 Mar  1 00:17 /dev/dm-0
brw-rw---- 1 root disk 253, 1 Mar  1 00:19 /dev/dm-1
brw-rw---- 1 root disk 253, 2 Mar  1 00:19 /dev/dm-2
```

NOTE Auto-detecting LVM volumes

					When the computer boots, the lvm2-activation systemd service unit executes vgchange -aay to âactivateâ the volume groups: it scans the available devices; those that have been initialized as physical volumes for LVM are registered into the LVM subsystem, those that belong to volume groups are assembled, and the relevant logical volumes are started and made available. There is therefore no need to edit configuration files when creating or modifying LVM volumes.
				
					Note, however, that the layout of the LVM elements (physical and logical volumes, and volume groups) is backed up in /etc/lvm/backup, which can be useful in case of a problem (or just to sneak a peek under the hood).
				
					To make things easier, convenience symbolic links are also created in directories matching the VGs:
				

```bash
# ls -l /dev/vg_critical
total 0
lrwxrwxrwx 1 root root 7 Mar  1 00:19 lv_base -> ../dm-1
lrwxrwxrwx 1 root root 7 Mar  1 00:17 lv_files -> ../dm-0 
# ls -l /dev/vg_normal
total 0
lrwxrwxrwx 1 root root 7 Mar  1 00:19 lv_backups -> ../dm-2
```

					The LVs can then be used exactly like standard partitions:
				

```bash
# mkfs.ext4 /dev/vg_normal/lv_backups
mke2fs 1.46.2 (28-Feb-2021)
Discarding device blocks: done                            
Creating filesystem with 3140608 4k blocks and 786432 inodes
Filesystem UUID: 7eaf0340-b740-421e-96b2-942cdbf29cb3
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done 

# mkdir /srv/backups
# mount /dev/vg_normal/lv_backups /srv/backups
# df -h /srv/backups
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/vg_normal-lv_backups   12G   24K   12G   1% /srv/backups
# [...]
[...]
# cat /etc/fstab
[...]
/dev/vg_critical/lv_base    /srv/base       ext4 defaults 0 2
/dev/vg_critical/lv_files   /srv/files      ext4 defaults 0 2
/dev/vg_normal/lv_backups   /srv/backups    ext4 defaults 0 2
```

					From the applications' point of view, the myriad small partitions have now been abstracted into one large 12Â GB volume, with a friendlier name.
				

#### 12.1.2.3.Â LVM Over Time

					Even though the ability to aggregate partitions or physical disks is convenient, this is not the main advantage brought by LVM. The flexibility it brings is especially noticed as time passes, when needs evolve. In our example, let's assume that new large files must be stored, and that the LV dedicated to the file server is too small to contain them. Since we haven't used the whole space available in vg_critical, we can grow lv_files. For that purpose, we'll use the lvresize command, then resize2fs to adapt the filesystem accordingly:
				

```bash
# df -h /srv/files/
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/vg_critical-lv_files  4.9G  4.2G  485M  90% /srv/files
# lvdisplay -C vg_critical/lv_files
  LV       VG          Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_files vg_critical -wi-ao---- 5.00g                                                    
# vgdisplay -C vg_critical
  VG          #PV #LV #SN Attr   VSize VFree
  vg_critical   2   2   0 wz--n- 7.99g 1.99g
# lvresize -L 6G vg_critical/lv_files
  Size of logical volume vg_critical/lv_files changed from 5.00 GiB (1280 extents) to 6.00 GiB (1536 extents).
  Logical volume vg_critical/lv_files successfully resized.
# lvdisplay -C vg_critical/lv_files
  LV       VG          Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_files vg_critical -wi-ao---- 6.00g                                                    
# resize2fs /dev/vg_critical/lv_files
resize2fs 1.46.2 (28-Feb-2021)
Filesystem at /dev/vg_critical/lv_files is mounted on /srv/files; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/vg_critical/lv_files is now 1572864 (4k) blocks long.

# df -h /srv/files/
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/vg_critical-lv_files  5.9G  4.2G  1.5G  75% /srv/files
```

CAUTION Resizing filesystems

					Not all filesystems can be resized online; resizing a volume can therefore require unmounting the filesystem first and remounting it afterwards. Of course, if one wants to shrink the space allocated to an LV, the filesystem must be shrunk first; the order is reversed when the resizing goes in the other direction: the logical volume must be grown before the filesystem on it. It is rather straightforward, since at no time must the filesystem size be larger than the block device where it resides (whether that device is a physical partition or a logical volume).
				
					The ext3, ext4 and xfs filesystems can be grown online, without unmounting; shrinking requires an unmount. The reiserfs filesystem allows online resizing in both directions. The venerable ext2 allows neither, and always requires unmounting.
				
					We could proceed in a similar fashion to extend the volume hosting the database, only we've reached the VG's available space limit:
				

```bash
# df -h /srv/base/
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/vg_critical-lv_base  974M  883M   25M  98% /srv/base
# vgdisplay -C vg_critical
  VG          #PV #LV #SN Attr   VSize VFree   
  vg_critical   2   2   0 wz--n- 7.99g 1016.00m
```

					No matter, since LVM allows adding physical volumes to existing volume groups. For instance, maybe we've noticed that the sdb3 partition, which was so far used outside of LVM, only contained archives that could be moved to lv_backups. We can now recycle it and integrate it to the volume group, and thereby reclaim some available space. This is the purpose of the vgextend command. Of course, the partition must be prepared as a physical volume beforehand. Once the VG has been extended, we can use similar commands as previously to grow the logical volume then the filesystem:
				

```bash
# pvcreate /dev/sdb3
  Physical volume "/dev/sdb3" successfully created.
# vgextend vg_critical /dev/sdb3
  Volume group "vg_critical" successfully extended
# vgdisplay -C vg_critical
  VG          #PV #LV #SN Attr   VSize   VFree 
  vg_critical   3   2   0 wz--n- <12.99g <5.99g 
# lvresize -L 2G vg_critical/lv_base
[...]
# resize2fs /dev/vg_critical/lv_base
[...]
# df -h /srv/base/
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/vg_critical-lv_base  2.0G  886M  991M  48% /srv/base
```

GOING FURTHER Advanced LVM

					LVM also caters for more advanced uses, where many details can be specified by hand. For instance, an administrator can tweak the size of the blocks that make up physical and logical volumes, as well as their physical layout. It is also possible to move blocks across PVs, for instance, to fine-tune performance or, in a more mundane way, to free a PV when one needs to extract the corresponding physical disk from the VG (whether to affect it to another VG or to remove it from LVM altogether). The manual pages describing the commands are generally clear and detailed. A good entry point is the lvm(8) manual page.
				

### 12.1.3.Â RAID or LVM?

				RAID and LVM both bring indisputable advantages as soon as one leaves the simple case of a desktop computer with a single hard disk where the usage pattern doesn't change over time. However, RAID and LVM go in two different directions, with diverging goals, and it is legitimate to wonder which one should be adopted. The most appropriate answer will of course depend on current and foreseeable requirements.
			
				There are a few simple cases where the question doesn't really arise. If the requirement is to safeguard data against hardware failures, then obviously RAID will be set up on a redundant array of disks, since LVM doesn't really address this problem. If, on the other hand, the need is for a flexible storage scheme where the volumes are made independent of the physical layout of the disks, RAID doesn't help much and LVM will be the natural choice.
			

NOTE If performance mattersâ¦

				If input/output speed is of the essence, especially in terms of access times, using LVM and/or RAID in one of the many combinations may have some impact on performances, and this may influence decisions as to which to pick. However, these differences in performance are really minor, and will only be measurable in a few use cases. If performance matters, the best gain to be obtained would be to use non-rotating storage media (solid-state drives or SSDs); their cost per megabyte is higher than that of standard hard disk drives, and their capacity is usually smaller, but they provide excellent performance for random accesses. If the usage pattern includes many input/output operations scattered all around the filesystem, for instance for databases where complex queries are routinely being run, then the advantage of running them on an SSD far outweigh whatever could be gained by picking LVM over RAID or the reverse. In these situations, the choice should be determined by other considerations than pure speed, since the performance aspect is most easily handled by using SSDs.
			
				The third notable use case is when one just wants to aggregate two disks into one volume, either for performance reasons or to have a single filesystem that is larger than any of the available disks. This case can be addressed both by a RAID-0 (or even linear-RAID) and by an LVM volume. When in this situation, and barring extra constraints (for instance, keeping in line with the rest of the computers if they only use RAID), the configuration of choice will often be LVM. The initial set up is barely more complex, and that slight increase in complexity more than makes up for the extra flexibility that LVM brings if the requirements change or if new disks need to be added.
			
				Then of course, there is the really interesting use case, where the storage system needs to be made both resistant to hardware failure and flexible when it comes to volume allocation. Neither RAID nor LVM can address both requirements on their own; no matter, this is where we use both at the same time â or rather, one on top of the other. The scheme that has all but become a standard since RAID and LVM have reached maturity is to ensure data redundancy first by grouping disks in a small number of large RAID arrays, and to use these RAID arrays as LVM physical volumes; logical partitions will then be carved from these LVs for filesystems. The selling point of this setup is that when a disk fails, only a small number of RAID arrays will need to be reconstructed, thereby limiting the time spent by the administrator for recovery.
			
				Let's take a concrete example: the public relations department at Falcot Corp needs a workstation for video editing, but the department's budget doesn't allow investing in high-end hardware from the bottom up. A decision is made to favor the hardware that is specific to the graphic nature of the work (monitor and video card), and to stay with generic hardware for storage. However, as is widely known, digital video does have some particular requirements for its storage: the amount of data to store is large, and the throughput rate for reading and writing this data is important for the overall system performance (more than typical access time, for instance). These constraints need to be fulfilled with generic hardware, in this case two 300 GB SATA hard disk drives; the system data must also be made resistant to hardware failure, as well as some of the user data. Edited video clips must indeed be safe, but video rushes pending editing are less critical, since they're still on the videotapes.
			
				RAID-1 and LVM are combined to satisfy these constraints. The disks are attached to two different SATA controllers to optimize parallel access and reduce the risk of a simultaneous failure, and they therefore appear as sda and sdc. They are partitioned identically along the following scheme:
			

```bash
# sfdisk -l /dev/sda
Disk /dev/sda: 894.25 GiB, 960197124096 bytes, 1875385008 sectors
Disk model: SAMSUNG MZ7LM960
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: BB14C130-9E9A-9A44-9462-6226349CA012

Device         Start        End   Sectors   Size Type
/dev/sda1        2048       4095      2048     1M BIOS boot
/dev/sda2        4096  100667391 100663296    48G Linux RAID
/dev/sda3   100667392  134221823  33554432    16G Linux RAID
/dev/sda4   134221824  763367423 629145600   300G Linux RAID
/dev/sda5   763367424 1392513023 629145600   300G Linux RAID
/dev/sda6  1392513024 1875384974 482871951 230.3G Linux LVM
```

						The first partitions of both disks are BIOS boot partitions.
					
						The next two partitions sda2 and sdc2 (about 48Â GB) are assembled into a RAID-1 volume, md0. This mirror is directly used to store the root filesystem.
					
						The sda3 and sdc3 partitions are assembled into a RAID-0 volume, md1, and used as swap partition, providing a total 32Â GB of swap space. Modern systems can provide plenty of RAM and our system won't need hibernation. So with this amount added, our system will unlikely run out of memory.
					
						The sda4 and sdc4 partitions, as well as sda5 and sdc5, are assembled into two new RAID-1 volumes of about 300Â GB each, md2 and md3. Both these mirrors are initialized as physical volumes for LVM, and assigned to the vg_raid volume group. This VG thus contains about 600Â GB of safe space.
					
						The remaining partitions, sda6 and sdc6, are directly used as physical volumes, and assigned to another VG called vg_bulk, which therefore ends up with roughly 460Â GB of space.
					
				Once the VGs are created, they can be partitioned in a very flexible way. One must keep in mind that LVs created in vg_raid will be preserved even if one of the disks fails, which will not be the case for LVs created in vg_bulk; on the other hand, the latter will be allocated in parallel on both disks, which allows higher read or write speeds for large files.
			
				We will therefore create the lv_var and lv_home LVs on vg_raid, to host the matching filesystems; another large LV, lv_movies, will be used to host the definitive versions of movies after editing. The other VG will be split into a large lv_rushes, for data straight out of the digital video cameras, and a lv_tmp for temporary files. The location of the work area is a less straightforward choice to make: while good performance is needed for that volume, is it worth risking losing work if a disk fails during an editing session? Depending on the answer to that question, the relevant LV will be created on one VG or the other.
			
				We now have both some redundancy for important data and much flexibility in how the available space is split across the applications.
			

NOTE Why three RAID-1 volumes?

				We could have set up one RAID-1 volume only, to serve as a physical volume for vg_raid. Why create three of them, then?
			
				The rationale for the first split (md0 vs. the others) is about data safety: data written to both elements of a RAID-1 mirror are exactly the same, and it is therefore possible to bypass the RAID layer and mount one of the disks directly. In case of a kernel bug, for instance, or if the LVM metadata become corrupted, it is still possible to boot a minimal system to access critical data such as the layout of disks in the RAID and LVM volumes; the metadata can then be reconstructed and the files can be accessed again, so that the system can be brought back to its nominal state.
			
				The rationale for the second split (md2 vs. md3) is less clear-cut, and more related to acknowledging that the future is uncertain. When the workstation is first assembled, the exact storage requirements are not necessarily known with perfect precision; they can also evolve over time. In our case, we can't know in advance the actual storage space requirements for video rushes and complete video clips. If one particular clip needs a very large amount of rushes, and the VG dedicated to redundant data is less than halfway full, we can re-use some of its unneeded space. We can remove one of the physical volumes, say md3, from vg_raid and either assign it to vg_bulk directly (if the expected duration of the operation is short enough that we can live with the temporary drop in performance), or undo the RAID setup on md3 and integrate its components sda5 and sdc5 into the bulk VG (which grows by 600Â GB instead of 300Â GB); the lv_rushes logical volume can then be grown according to requirements.
			[Prev11.8.Â Real-Time Communication Services](sect.rtc-services.en.md)[Up](#)[Home](index.en.md)[Next12.2.Â Virtualization](sect.virtualization.en.md)