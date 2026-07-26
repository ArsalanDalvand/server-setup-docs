---
title: "8.7. Printer Configuration"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.config-printing.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.config-printing.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.7. Printer Configuration"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.shell-environment.en.md)The Debian Administrator's Handbook[Next](sect.config-bootloader.en.md)

## 8.7.Â Printer Configuration

			Printer configuration used to cause a great many headaches for administrators and users alike. These headaches are now mostly a thing of the past, thanks to CUPS, the free print server using IPP, the Internet Printing Protocol.
		
			Debian distributes CUPS divided between several packages. The heart of the system is the scheduler, cupsd, which is in the cups-daemon package. cups-client contains utility programs to interact with the server, cupsd. lpadmin is probably the most important utility, as it is crucial for setting up a printer, but there are also facilities to disable or enable a printer queue, view or delete print jobs and display or set printer options. The CUPS framework is based on the System V printing system, but there is a compatibility package, cups-bsd, allowing use of commands such as lpr, lpq and lprm from the traditional BSD printing system.
		

COMMUNITY CUPS

			CUPS is a project and a trademark owned and managed by Apple, Inc. Prior to its acquisition by Apple it was known as the Common Unix Printing System. âÂ [https://www.cups.org/](https://www.cups.org/)

			The scheduler manages print jobs and these jobs traverse a filtering system to produce a file that the printer will understand and print. The filtering system is provided by the cups-filters package in conjunction with printer-driver-* packages. CUPS in combination with these packages is the basis for the Debian printing system.
		
			Modern printers manufactured and sold within the last ten years are nearly always AirPrint-capable, and CUPS and cups-filters on Debian Bullseye have everything which is needed to take advantage of this facility on the network. In essence, these printers are IPP printers and an excellent fit for a driverless printing system, reducing the system to CUPS plus cups-filters. A printer-driver package can be dispensed with, and non-free printing software from vendors like Canon and Brother is no longer required. A USB-connected printer can take advantage of a modern printer with the ippusbxd package.
		
			The command apt install cups will install CUPS and the filters. It will also install the recommended printer-driver-gutenprint to provide a driver for a wide range of printers, but, unless the printer is being operated driverlessly, an alternative printer-driver might be needed for the particular device.
		
			As a package recommended by cups-daemon, cups-browsed will be on the system and networked print queues, and modern printers can be automatically discovered and set up from their DNS-SD broadcasts (Bonjour). USB printers will have to be set up manually as described in the next paragraph.
		
			The printing system is administered easily through a web interface accessible at the local address http://localhost:631/. Members of the lpadmin group can add and remove USB and network printers and administer most aspects of their behavior. Similar administration tasks can also be carried out via the graphical interface provided by a desktop environment or the system-config-printer graphical interface (from the homonym Debian package).
		[Prev8.6.Â Shell Environment](sect.shell-environment.en.md)[Up](#)[Home](index.en.md)[Next8.8.Â Configuring the Bootloader](sect.config-bootloader.en.md)