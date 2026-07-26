---
title: "3.1.3. Integration with Other Linux/Unix Machines"
source: "https://www.debian.org/doc/manuals/debian-handbook/existing-setup.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/existing-setup.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "3.1.3. Integration with Other Linux/Unix Machines"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.why-debian-stable.en.md)The Debian Administrator's Handbook[Next](sect.how-to-migrate.en.md)

# ChapterÂ 3.Â Analyzing the Existing Setup and Migrating

[3.1. Coexistence in Heterogeneous Environments](existing-setup.en.md#sect.heterogeneous-environments)[3.1.1. Integration with Windows Machines](existing-setup.en.md#id-1.6.4.4)[3.1.2. Integration with macOS machines](existing-setup.en.md#id-1.6.4.5)[3.1.3. Integration with Other Linux/Unix Machines](existing-setup.en.md#id-1.6.4.6)[3.2. How To Migrate](sect.how-to-migrate.en.md)[3.2.1. Survey and Identify Services](sect.how-to-migrate.en.md#id-1.6.5.4)[3.2.2. Backing up the Configuration](sect.how-to-migrate.en.md#id-1.6.5.5)[3.2.3. Taking Over an Existing Debian Server](sect.how-to-migrate.en.md#id-1.6.5.6)[3.2.4. Installing Debian](sect.how-to-migrate.en.md#id-1.6.5.7)[3.2.5. Installing and Configuring the Selected Services](sect.how-to-migrate.en.md#id-1.6.5.8)
		Any computer system overhaul should take the existing system into account. This allows reuse of available resources as much as possible and guarantees interoperability of the various elements comprising the system. This study will introduce a generic framework to follow in any migration of a computing infrastructure to Linux.
	

## 3.1.Â Coexistence in Heterogeneous Environments

			Debian integrates very well in all types of existing environments and plays well with any other operating system. This near-perfect harmony comes from market pressure which demands that software publishers develop programs that follow standards. Compliance with standards allows administrators to switch out programs: clients or servers, whether free or not.
		

### 3.1.1.Â Integration with Windows Machines

				Samba's SMB/CIFS support ensures excellent communication within a Windows context. It shares files and print queues to Windows clients and includes software that allows a Linux machine to use resources available on Windows servers.
			

TOOL Samba

				The latest version of Samba can replace most of the Windows features: from those of a simple Windows NT server (authentication, files, print queues, downloading printer drivers, DFS, etc.) to the most advanced one (a domain controller compatible with Active Directory).
			

### 3.1.2.Â Integration with macOS machines

				macOS machines provide, and are able to use, network services such as file servers and printer sharing. These services are published on the local network, which allows other machines to discover them and make use of them without any manual configuration, using the Bonjour implementation of the Zeroconf protocol suite. Debian includes another implementation, called Avahi, which provides the same functionality.
			
				In the other direction, the Netatalk daemon can be used to provide file servers to macOS machines on the network. It implements the AFP protocol (Apple Filing Protocol, now AppleShare) as well as the required notifications so that the servers can be automatically discovered by the macOS clients.
			
				Older MacÂ OS networks (before OSÂ X) used a different protocol called AppleTalk. For environments involving machines using this protocol, Netatalk also provides the AppleTalk protocol (in fact, it started as a reimplementation of that protocol). It ensures the operation of the file server and print queues, as well as time server (clock synchronization). Its router function allows interconnection with AppleTalk networks.
			

### 3.1.3.Â Integration with Other Linux/Unix Machines

				Finally, NFS (Network File System) and NIS (Network Information Service), both included, guarantee interaction with Unix systems. NFS ensures file server functionality, while NIS creates user directories. The BSD printing layer, used by most Unix systems, also allows sharing of print queues.
			

FigureÂ 3.1.Â Coexistence of Debian with OSÂ X, Windows and Unix systems

[Prev2.5.Â Why Debian Bullseye?](sect.why-debian-stable.en.md)[Up](#)[Home](index.en.md)[Next3.2.Â How To Migrate](sect.how-to-migrate.en.md)