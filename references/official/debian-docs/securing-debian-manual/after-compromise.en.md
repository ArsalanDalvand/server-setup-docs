---
title: "11.1. General behavior"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/after-compromise.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/after-compromise.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "11.1. General behavior"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch10s05.en.md)Securing Debian Manual[Next](ch11s02.en.md)

# ChapterÂ 11.Â After the compromise (incident response)

[11.1. General behavior](after-compromise.en.md#idm4446)[11.2. Backing up the system](ch11s02.en.md)[11.3. Contact your local CERT](ch11s03.en.md)[11.4. Forensic analysis](ch11s04.en.md)[11.5. Analysis of malware](ch11s05.en.md)

## 11.1.Â General behavior

			If you are physically present when an attack is happening, your first response should be to remove the machine from the network by unplugging the network card (if this will not adversely affect any business transactions). Disabling the network at layer 1 is the only true way to keep the attacker out of the compromised box (Phillip Hofmeister's wise advice).
		
			However, some tools installed by rootkits, trojans and, even, a rogue user connected through a back door, might be capable of detecting this event and react to it. Seeing a rm -rf / executed when you unplug the network from the system is not really much fun. If you are unwilling to take the risk, and you are sure that the system is compromised, you should unplug the power cable (all of them if more than one) and cross your fingers. This may be extreme but, in fact, will avoid any logic-bomb that the intruder might have programmed. In this case, the compromised system should not be re-booted. Either the hard disks should be moved to another system for analysis, or you should use other media (a CD-ROM) to boot the system and analyze it. You should not use Debian's rescue disks to boot the system, but you can use the shell provided by the installation disks (remember, Alt+F2 will take you to it) to analyze [[68]](#ftn.idm4455) the system.
		
			The most recommended method for recovering a compromised system is to use a live-filesystem on CD-ROM with all the tools (and kernel modules) you might need to access the compromised system. You can use the mkinitrd-cd package to build such a CD-ROM[[69]](#ftn.idm4459). You might find the [http://www.caine-live.net/](http://www.caine-live.net/) (Computer Aided Investigative Environment) CD-ROM useful here too, since it's also a live CD-ROM under active development with forensic tools useful in these situations. There is not (yet) a Debian-based tool such as this, nor an easy way to build the CD-ROM using your own selection of Debian packages and mkinitrd-cd (so you'll have to read the documentation provided with it to make your own CD-ROMs).
		
			If you really want to fix the compromise quickly, you should remove the compromised host from your network and re-install the operating system from scratch. Of course, this may not be effective because you will not learn how the intruder got root in the first place. For that case, you must check everything: firewall, file integrity, log host, log files and so on. For more information on what to do following a break-in, see [http://www.cert.org/tech_tips/root_compromise.html](http://www.cert.org/tech_tips/root_compromise.html) or SANS's [https://www.sans.org/white-papers/](https://www.sans.org/white-papers/).
		
			Some common questions on how to handle a compromised Debian GNU/Linux system are also available in.
		[[68]](#idm4455)
				>If you are adventurous, you can login to the system and save information on all running processes (you'll get a lot from /proc/nnn/). It is possible to get the whole executable code from memory, even if the attacker has deleted the executable files from disk. Then pull the power cord.
			[[69]](#idm4459)
				>In fact, this is the tool used to build the CD-ROMs for the [http://www.gibraltar.at/](http://www.gibraltar.at/) project (a firewall on a live CD-ROM based on the Debian distribution).
			[Prev10.5.Â Genius/Paranoia Ideas - what you could do](ch10s05.en.md)[Up](#)[Home](index.en.md)[Next11.2.Â Backing up the system](ch11s02.en.md)