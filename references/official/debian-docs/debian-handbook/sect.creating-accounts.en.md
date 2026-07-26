---
title: "8.5. Creating Accounts"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.creating-accounts.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.creating-accounts.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.5. Creating Accounts"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.user-group-databases.en.md)The Debian Administrator's Handbook[Next](sect.shell-environment.en.md)

## 8.5.Â Creating Accounts

			One of the first actions an administrator needs to do when setting up a new machine is to create user accounts. This is typically done using the adduser command which takes a user-name for the new user to be created, as an argument.
		
			The adduser command asks a few questions before creating the account, but its usage is fairly straightforward. Its configuration file, /etc/adduser.conf, includes all the interesting settings: it can be used to automatically set a quota for each new user by creating a user template, or to change the location of user accounts; the latter is rarely useful, but it comes in handy when you have a large number of users and want to divide their accounts over several disks, for instance. You can also choose a different default shell.
		

BACK TO BASICS Quota

			The term âquotaâ refers to a limit on machine resources that a user is allowed to use. This frequently refers to disk space.
		
			The creation of an account populates the user's home directory with the contents of the /etc/skel/ template. This provides the user with a set of standard directories and configuration files.
		
			In some cases, it will be useful to add a user to a group (other than their default âmainâ group) in order to grant them additional permissions. For example, a user who is included in the audio group can access audio devices (see sidebar [BACK TO BASICS Device access permissions](sect.creating-accounts.en.md#sidebar.special-files)). This can be achieved with a command such as adduser user group.
		

BACK TO BASICS Device access permissions

			Each hardware peripheral device is represented under Unix with a special file, usually stored in the file tree under /dev/ (DEVices). Two types of special files exist according to the nature of the device: âcharacter modeâ and âblock modeâ files, each mode allowing for only a limited number of operations. While character mode limits interaction with read/write operations, block mode also allows seeking within the available data. Finally, each special file is associated with two numbers (âmajorâ and âminorâ) that identify the device to the kernel in a unique manner. Such a file, created by the mknod command, simply contains a symbolic (and more human-friendly) name.
		
			The permissions of a special file map to the permissions necessary to access the device itself. Thus, a file such as /dev/mixer, representing the audio mixer, only has read/write permissions for the root user and members of the audio group. Only these users can operate the audio mixer.
		
			It should be noted that the combination of udev and policykit can add additional permissions to allow users physically connected to the console (and not through the network) to access to certain devices.
		[Prev8.4.Â User and Group Databases](sect.user-group-databases.en.md)[Up](#)[Home](index.en.md)[Next8.6.Â Shell Environment](sect.shell-environment.en.md)