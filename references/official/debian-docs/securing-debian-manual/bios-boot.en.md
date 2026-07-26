---
title: "4.3. Change the BIOS (again)"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/bios-boot.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/bios-boot.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.3. Change the BIOS (again)"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](security-update.en.md)Securing Debian Manual[Next](lilo-passwd.en.md)

## 4.3.Â Change the BIOS (again)

			Remember [SectionÂ 3.1, âChoose a BIOS passwordâ](ch03.en.md#bios-passwd)? Well, then you should now, once you do not need to boot from removable media, to change the default BIOS setup so that it only boots from the hard drive. Make sure you will not lose the BIOS password, otherwise, in the event of a hard disk failure you will not be able to return to the BIOS and change the setup so you can recover it using, for example, a CD-ROM.
		
			Another less secure but more convenient way is to change the setup to have the system boot up from the hard disk and, if it fails, try removable media. By the way, this is often done because most people don't use the BIOS password that often; it's easily forgotten.
		[Prev4.2.Â Execute a security update](security-update.en.md)[Up](#)[Home](index.en.md)[Next4.4.Â Set a LILO or GRUB password](lilo-passwd.en.md)