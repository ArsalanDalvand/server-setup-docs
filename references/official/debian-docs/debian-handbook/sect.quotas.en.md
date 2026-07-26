---
title: "9.9. Quotas"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.quotas.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.quotas.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.9. Quotas"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.asynchronous-task-scheduling-anacron.en.md)The Debian Administrator's Handbook[Next](sect.backup.en.md)

## 9.9.Â Quotas

			The quota system allows limiting disk space allocated to a user or group of users. To set it up, you must have a kernel that supports it (compiled with the CONFIG_QUOTA option) â as is the case with Debian kernels. The quota management software is found in the quota Debian package.
		
			To activate quota in a filesystem, you have to indicate the usrquota and grpquota options in /etc/fstab for the user and group quotas, respectively. Rebooting the computer will then update the quotas in the absence of disk activity (a necessary condition for proper accounting of already used disk space).
		
			The edquota user (or edquota -g group) command allows you to change the limits while examining current disk space usage.
		

GOING FURTHER Defining quotas with a script

			The setquota program can be used in a script to automatically change many quotas. Its setquota(8) manual page details the syntax to use.
		
			The quota system allows you to set four limits:
		
					two limits (called âsoftâ and âhardâ) refer to the number of blocks consumed. If the filesystem was created with a block-size of 1Â kibibyte, the 1024Â available bytes of each block can only be assigned to one file. Unsaturated blocks thus induce losses of disk space. A quota of 100 blocks, which theoretically allows storage of 102,400 bytes, will, however, be saturated with just 100Â files of 500Â bytes each, only representing 50,000 bytes in total.
				
					two limits (âsoftâ and âhardâ) refer to the number of inodes used. Each file occupies at least one inode to store information about it (permissions, owner, timestamp of last access, etc.). It is thus a limit on the number of user files.
				
			A âsoftâ limit can be temporarily exceeded; the user will simply be warned that they are exceeding the quota by the warnquota command, which is usually invoked by cron. A âhardâ limit can never be exceeded: the system will refuse any operation that will cause a hard quota to be exceeded.
		

VOCABULARY Blocks and inodes

			The filesystem divides the hard drive into blocks â small contiguous areas. The size of these blocks is defined during creation of the filesystem, and generally varies between 1 and 8 kibibytes.
		
			A block can be used either to store the real data of a file, or for meta-data used by the filesystem. Among this meta-data, you will especially find the inodes. An inode uses a block on the hard drive (but this block is not taken into consideration in the block quota, only in the inode quota), and contains both the information on the file to which it corresponds (name, owner, permissions, etc.) and the pointers to the data blocks that are actually used. For very large files that occupy more blocks than it is possible to reference in a single inode, there is an indirect block system; the inode references a list of blocks that do not directly contain data, but another list of blocks.
		
			With the edquota -t command, you can define a maximum authorized âgrace periodâ within which a soft limit may be exceeded. After this period, the soft limit will be treated like a hard limit, and the user will have to reduce their disk space usage to within this limit in order to be able to write anything to the hard drive.
		

GOING FURTHER Setting up a default quota for new users

			To automatically setup a quota for new users, you have to configure a template user (with edquota or setquota) and indicate their user name in the QUOTAUSER variable in the /etc/adduser.conf file. This quota configuration will then be automatically applied to each new user created with the adduser command.
		[Prev9.8.Â Scheduling Asynchronous Tasks: anacron](sect.asynchronous-task-scheduling-anacron.en.md)[Up](#)[Home](index.en.md)[Next9.10.Â Backup](sect.backup.en.md)