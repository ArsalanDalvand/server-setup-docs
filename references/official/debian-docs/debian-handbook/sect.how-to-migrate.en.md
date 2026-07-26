---
title: "3.2.5. Installing and Configuring the Selected Services"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.how-to-migrate.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.how-to-migrate.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "3.2.5. Installing and Configuring the Selected Services"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](existing-setup.en.md)The Debian Administrator's Handbook[Next](installation.en.md)

## 3.2.Â How To Migrate

			In order to guarantee continuity of the services, each computer migration must be planned and executed according to the plan. This principle applies regardless of which operating system is used.
		

### 3.2.1.Â Survey and Identify Services

				As simple as it seems, this step is essential. A serious administrator truly knows the principal roles of each server, but such roles can change, and sometimes experienced users may have installed âwildâ services. Knowing that they exist will at least allow you to decide what to do with them, rather than delete them haphazardly.
			
				For this purpose, it is wise to inform your users of the project before migrating the server. To involve them in the project, it may be useful to install the most common free software programs on their desktops prior to migration, which they will come across again after the migration to Debian; LibreOffice and the Mozilla suite are the best examples here.
			

#### 3.2.1.1.Â Network and Processes

					The nmap tool (in the package with the same name) will quickly identify Internet services hosted by a network connected machine without even requiring to log in to it. Simply call the following command on another machine connected to the same network:
				

```bash
$ nmap mirwiz
Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-29 14:41 CEST
Nmap scan report for mirwiz (192.168.1.104)
Host is up (0.00062s latency).
Not shown: 992 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
25/tcp   open  smtp
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
5666/tcp open  nrpe
9999/tcp open  abyss

Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds
```

ALTERNATIVE Use ss to find the list of available services

					On a Linux machine, the ss -anptu command will show the list of active or pending TCP sessions, as well as UDP ports on which running programs are listening. This facilitates identification of services offered on the network.
				

GOING FURTHER IPv6

					Some network commands may work either with IPv4 (the default usually) or with IPv6. These include the nmap and ss commands, but also others, such as route or ip. The convention is that this behavior is enabled by the -6 command-line option.
				
					If the server is a Unix machine offering shell accounts to users, it is interesting to determine if processes are executed in the background in the absence of their owner. The command ps auxw displays a list of all processes with their user identity. By checking this information against the output of the who or w commands, which give a list of logged in users, it is possible to identify rogue or undeclared servers or programs running in the background. Looking at crontabs (tables listing automatic actions scheduled by users) will often provide interesting information on functions fulfilled by the server (a complete explanation of cron is available in [SectionÂ 9.7, âScheduling Tasks with cron and atdâ](sect.task-scheduling-cron-atd.en.md)).
				
					In any case, it is essential to backup your servers: this allows recovery of information after the fact, when users will report specific problems due to the migration.
				

### 3.2.2.Â Backing up the Configuration

				It is wise to retain the configuration of every identified service in order to be able to install the equivalent on the updated server. The bare minimum is to make a backup copy of the configuration files.
			
				For Unix machines, the configuration files are usually found in /etc/, but they may be located in a sub-directory of /usr/local/. This is the case if a program has been installed from sources, rather than with a package. In some cases, one may also find them under /opt/.
			
				For data managing services (such as databases), it is strongly recommended to export the data to a standard format that will be easily imported by the new software. Such a format is usually in text mode and documented; it may be, for example, an SQL dump for a database, or an LDIF file for an LDAP server.
			

FigureÂ 3.2.Â Database backups

				Each server software is different, and it is impossible to describe all existing cases in detail. Compare the documentation for the existing and the new software to identify the exportable (thus, re-importable) portions and those which will require manual handling. Reading this book will clarify the configuration of the main Linux server programs.
			

### 3.2.3.Â Taking Over an Existing Debian Server

				To effectively take over its maintenance, one may analyze a machine already running with Debian.
			
				The first file to check is /etc/debian_version, which usually contains the version number for the installed Debian system (it is part of the base-files package). If it indicates codename/sid, it means that the system was updated with packages coming from one of the development distributions (either testing or unstable).
			
				The apt-show-versions program (from the Debian package of the same name) checks the list of installed packages and identifies the available versions. aptitude can also be used for these tasks, albeit in a less systematic manner.
			
				A glance at the /etc/apt/sources.list file (and /etc/apt/sources.list.d/ directory) will show where the installed Debian packages likely came from. If many unknown sources appear, the administrator may choose to completely reinstall the computer's system to ensure optimal compatibility with the software provided by Debian.
			
				The sources.list file is often a good indicator: the majority of administrators keep, at least in comments, the list of APT sources that were previously used. But you should not forget that sources used in the past might have been deleted, and that some random packages grabbed on the Internet might have been manually installed (with the help of the dpkg command). In this case, the machine is misleading in its appearance of being a âstandardâ Debian system. This is why you should pay attention to any indication that will give away the presence of external packages (appearance of deb files in unusual directories, package version numbers with a special suffix indicating that it originated from outside the Debian project, such as ubuntu or lmde, etc.).
			
				Likewise, it is interesting to analyze the contents of the /usr/local/ directory, whose purpose is to contain programs compiled and installed manually. Listing software installed in this manner is instructive, since this raises questions on the reasons for not using the corresponding Debian package, if such a package exists.
			

QUICK LOOK cruft/cruft-ng, debsums, and apt-show-versions

				The cruft and cruft-ng packages propose to list the available files that are not owned by any package. They have some filters (more or less effective, and more or less up to date) to avoid reporting some legitimate files (files generated by Debian packages, or generated configuration files not managed by dpkg, etc.).
			
				Be careful to not blindly delete everything that cruft and cruft-ng might list!
			
				The debsums package allows to check the MD5 hashsum of each file installed by a package against a reference hashsum and can help to determine, which files might have been altered (see [TIP Finding changed files](sect.dist-upgrade.en.md#sidebar.debsums)). Be aware that created files (files generated by Debian packages, or generated configuration files not managed by dpkg, etc.) are not subject to this check.
			
				The apt-show-versions package provides a tool to check for installed packages without a package source and can help to determine third party packages (see [SectionÂ 6.7.3.1, âPackages removed from the Debian Archiveâ](sect.dist-upgrade.en.md#sect.apt-show-versions)).
			

### 3.2.4.Â Installing Debian

				Once all the required information on the current server is known, we can shut it down and begin to install Debian on it.
			
				To choose the appropriate version, we must know the computer's architecture. If it is a reasonably recent PC, it is most likely to be amd64 (older PCs were usually i386). In other cases, we can narrow down the possibilities according to the previously used system.
			
[TableÂ 3.1](sect.how-to-migrate.en.md#tab-corresp) is not intended to be exhaustive, but may be helpful. Note that it lists Debian architectures which are no longer supported in the current stable release. In any case, the original documentation for the computer is the most reliable source to find this information.
			

TableÂ 3.1.Â Matching operating system and architecture

Operating SystemArchitecture(s)DEC Unix (OSF/1)alpha, mipselHP Unixia64, hppaIBM AIXpowerpcIrixmipsOSÂ Xamd64, powerpc, i386z/OS, MVSs390x, s390Solaris, SunOSsparc, i386, m68kUltrixmipsVMSalphaWindows 95/98/MEi386Windows NT/2000i386, alpha, ia64, mipselWindows XP / Windows Server 2008i386, amd64, ia64Windows RTarmel, armhf, arm64Windows Vista / Windows 7-8-10i386, amd64

HARDWARE 64 bit PC vs 32 bit PC

				Most recent computers have 64 bit Intel or AMD processors, compatible with older 32 bit processors; the software compiled for âi386â architecture thus works. On the other hand, this compatibility mode does not fully exploit the capabilities of these new processors. This is why Debian provides the âamd64â architecture, which works for recent AMD chips as well as Intel âem64tâ processors (including most of the Core series), which are very similar to AMD64.
			

### 3.2.5.Â Installing and Configuring the Selected Services

				Once Debian is installed, we need to individually install and configure each of the services that this computer must host. The new configuration must take into consideration the prior one in order to ensure a smooth transition. All the information collected in the first two steps will be useful to successfully complete this part.
			

FigureÂ 3.3.Â Install the selected services

				Prior to jumping into this exercise with both feet, it is strongly recommended that you read the remainder of this book. After that you will have a more precise understanding of how to configure the expected services.
			
[PrevChapterÂ 3.Â Analyzing the Existing Setup and Migra...](existing-setup.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 4.Â Installation](installation.en.md)