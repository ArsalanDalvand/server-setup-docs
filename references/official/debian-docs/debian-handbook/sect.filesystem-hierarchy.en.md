---
title: "B.2.2. The User's Home Directory"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.filesystem-hierarchy.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.filesystem-hierarchy.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "B.2.2. The User's Home Directory"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](short-remedial-course.en.md)The Debian Administrator's Handbook[Next](sect.computer-layers.en.md)

## B.2.Â Organization of the Filesystem Hierarchy

### B.2.1.Â The Root Directory

				A Debian system is organized along the Filesystem Hierarchy Standard (FHS). This standard defines the purpose of each directory. For instance, the top-level directories are described as follows:
			
/bin/: basic programs;
					
/boot/: Linux kernel and other files required for its early boot process;
					
/dev/: device files;
					
/etc/: configuration files;
					
/home/: user's personal files;
					
/lib/: basic libraries;
					
/media/*: mount points for removable devices (CD-ROM, USB keys and so on);
					
/mnt/: temporary mount point;
					
/opt/: extra applications provided by third parties;
					
/root/: administrator's (root's) personal files;
					
/run/: volatile runtime data that does not persist across reboots;
					
/sbin/: system programs;
					
/srv/: data used by servers hosted on this system;
					
/tmp/: temporary files; this directory is often emptied at boot;
					
/usr/: applications; this directory is further subdivided into bin, sbin, lib (according to the same logic as in the root directory). Furthermore, /usr/share/ contains architecture-independent data. /usr/local/ is meant to be used by the administrator for installing applications manually without overwriting files handled by the packaging system (dpkg).
					
/var/: variable data handled by daemons. This includes log files, queues, spools, caches and so on.
					
/proc/ and /sys/ are specific to the Linux kernel (and not part of the FHS). They are used by the kernel for exporting data to user space (see [SectionÂ B.3.4, âThe User Spaceâ](sect.computer-layers.en.md#sect.userspace-presentation) and [SectionÂ B.5, âThe User Spaceâ](sect.user-space.en.md) for explanations about this concept).
					
				Note that many modern distributions, Debian included, are shipping /bin, /sbin and /lib as symlinks to the corresponding directories below /usr so that all programs and libraries are available in a single tree. It makes it easier to protect the integrity of the system files, and to share those system files among multiple containers, etc.
			

### B.2.2.Â The User's Home Directory

				The contents of a user's home directory is not standardized, but there are still a few noteworthy conventions. One is that a user's home directory is often referred to by a tilde (â~â). That is useful to know because command interpreters automatically replace a tilde with the correct directory (usually /home/user/).
			
				Traditionally, application configuration files are often stored directly under the user's home directory, but their names usually start with a dot (for instance, the mutt email client stores its configuration in ~/.muttrc). Note that filenames that start with a dot are hidden by default; and ls only lists them when the -a option is used, and graphical file managers need to be told to display hidden files.
			
				Some programs also use multiple configuration files organized in one directory (for instance, ~/.ssh/). Some applications also use their directory to store a cache of downloaded data. This means that those directories can end up using a lot of disk space.
			
				These configuration files stored directly in a user's home directory, often collectively referred to as dotfiles, have long proliferated to the point that these directories can be quite cluttered with them. Fortunately, an effort led collectively under the FreeDesktop.org umbrella has resulted in the âXDG Base Directory Specificationâ, a convention that aims at cleaning up these files and directories. This specification states that configuration files should be stored under ~/.config/, cache files under ~/.cache/, and application data files under ~/.local/ (or subdirectories thereof). This convention is slowly gaining traction, and several applications (especially graphical ones) have started following it.
			
				Graphical desktops usually display the contents of the ~/Desktop/ directory (or whatever the appropriate translation is for systems not configured in English) on the desktop (i.e. what is visible on screen once all applications are closed or iconized).
			
				Finally, the email system sometimes stores incoming emails into a ~/Mail/ directory.
			[PrevAppendixÂ B.Â Short Remedial Course](short-remedial-course.en.md)[Up](#)[Home](index.en.md)[NextB.3.Â Inner Workings of a Computer: the Different ...](sect.computer-layers.en.md)