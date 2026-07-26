---
title: "13.8. Emulating Windows: Wine"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.windows-emulation.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.windows-emulation.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.8. Emulating Windows: Wine"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.office-suites.en.md)The Debian Administrator's Handbook[Next](sect.rtc-clients.en.md)

## 13.8.Â Emulating Windows: Wine

			In spite of all the previously mentioned efforts, there are still a number of tools without a Linux equivalent, or for which the original version is absolutely required. This is where Windows emulation systems come in handy. The most well-known among them is Wine. âÂ [https://www.winehq.org/](https://www.winehq.org/)

COMPLEMENTS CrossOver Linux

CrossOver, produced by CodeWeavers, is a set of enhancements to Wine that broadens the available set of emulated features to a point at which Microsoft Office becomes fully usable. Some of the enhancements are periodically merged into Wine. âÂ [https://www.codeweavers.com/products/](https://www.codeweavers.com/products/)

			However, one should keep in mind that it is only a solution among others, and the problem can also be tackled with a virtual machine or VNC; both of these solutions are detailed in the sidebars [ALTERNATIVE Virtual machines](sect.windows-emulation.en.md#sidebar.virtual-machines) and [ALTERNATIVE Windows Terminal Server or VNC](sect.windows-emulation.en.md#sidebar.wts-or-vnc).
		
			Let us start with a reminder: emulation allows executing a program (developed for a target system) on a different host system. The emulation software uses the host system, where the application runs, to imitate the required features of the target system.
		
			Now let's install the required packages (ttf-mscorefonts-installer is in the contrib section):
		

```bash
# apt install wine ttf-mscorefonts-installer
```

			On a 64 bit (amd64) system, if your Windows applications are 32 bit applications, then you will have to enable multi-arch to be able to install wine32 from the i386 architecture (see [SectionÂ 5.4.5, âMulti-Arch Supportâ](sect.manipulating-packages-with-dpkg.en.md#sect.multi-arch)).
		
			The user then needs to run winecfg and configure which (Debian) locations are mapped to which (Windows) drives. winecfg has some sane defaults and can auto-detect some more drives; note that even if you have a dual-boot system, you should not point the C: drive at where the Windows partition is mounted in Debian, as Wine is likely to overwrite some of the data on that partition, making Windows unusable. Other settings can be kept to their default values. To run Windows programs, you will first need to install them by running their (Windows) installer under Wine, with a command such as wine .../setup.exe; once the program is installed, you can run it with wine .../program.exe. The exact location of the program.exe file depends on where the C: drive is mapped; in many cases, however, simply running wine program will work, since the program is usually installed in a location where Wine will look for it by itself.
		

TIP Working around a winecfg failure

			In some cases, winecfg (which is just a wrapper) might fail. As a workaround, it is possible to try to run the underlying command manually: wine64 /usr/lib/x86_64-linux-gnu/wine/wine/winecfg.exe.so or wine32 /usr/lib/i386-linux-gnu/wine/wine/winecfg.exe.so.
		
			Note that you should not rely on Wine (or similar solutions) without actually testing the particular software: only a real-use test will determine conclusively whether emulation is fully functional.
		

ALTERNATIVE Virtual machines

			An alternative to emulating Microsoft's operating system is to actually run it in a virtual machine that emulates a full hardware machine. This allows running any operating system. [ChapterÂ 12, Advanced Administration](advanced-administration.en.md) describes several virtualization systems, most notably Xen and KVM (but also QEMU, VMWare and VirtualBox).
		

ALTERNATIVE Windows Terminal Server or VNC

			Yet another possibility is to remotely run the legacy Windows applications on a central server with Windows Terminal Server and access the application from Linux machines using rdesktop. This is a Linux client for the RDP protocol (Remote Desktop Protocol) that Windows NT/2000 Terminal Server uses to display desktops on remote machines.
		
			The VNC software provides similar features, with the added benefit of also working with many operating systems. Linux VNC clients and servers are described in [SectionÂ 9.2, âRemote Loginâ](sect.remote-login.en.md).
		[Prev13.7.Â Office Suites](sect.office-suites.en.md)[Up](#)[Home](index.en.md)[Next13.9.Â Real-Time Communications software](sect.rtc-clients.en.md)