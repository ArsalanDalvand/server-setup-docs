---
title: "13.1. Configuring the X11 Server"
source: "https://www.debian.org/doc/manuals/debian-handbook/workstation.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/workstation.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.1. Configuring the X11 Server"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.monitoring.en.md)The Debian Administrator's Handbook[Next](sect.customizing-graphical-interface.en.md)

# ChapterÂ 13.Â Workstation

[13.1. Configuring the X11 Server](workstation.en.md#sect.x11-server-configuration)[13.2. Customizing the Graphical Interface](sect.customizing-graphical-interface.en.md)[13.2.1. Choosing a Display Manager](sect.customizing-graphical-interface.en.md#id-1.16.5.2)[13.2.2. Choosing a Window Manager](sect.customizing-graphical-interface.en.md#id-1.16.5.3)[13.2.3. Menu Management](sect.customizing-graphical-interface.en.md#id-1.16.5.4)[13.3. Graphical Desktops](sect.graphical-desktops.en.md)[13.3.1. GNOME](sect.graphical-desktops.en.md#sect.gnome-desktop)[13.3.2. KDE and Plasma](sect.graphical-desktops.en.md#id-1.16.6.13)[13.3.3. Xfce and Others](sect.graphical-desktops.en.md#id-1.16.6.14)[13.3.4. Other Desktop Environments](sect.graphical-desktops.en.md#id-1.16.6.15)[13.4. Email](sect.main-desktop-tools.en.md)[13.4.1. Evolution](sect.main-desktop-tools.en.md#id-1.16.7.3)[13.4.2. KMail](sect.main-desktop-tools.en.md#id-1.16.7.4)[13.4.3. Thunderbird](sect.main-desktop-tools.en.md#id-1.16.7.5)[13.5. Web Browsers](sect.web-browsers.en.md)[13.6. Development](sect.development.en.md)[13.6.1. Tools for GTK+ on GNOME](sect.development.en.md#id-1.16.9.2)[13.6.2. Tools for Qt](sect.development.en.md#id-1.16.9.3)[13.7. Office Suites](sect.office-suites.en.md)[13.8. Emulating Windows: Wine](sect.windows-emulation.en.md)[13.9. Real-Time Communications software](sect.rtc-clients.en.md)
		Now that server deployments are done, the administrators can focus on installing the individual workstations and creating a typical configuration.
	

## 13.1.Â Configuring the X11 Server

			A brief reminder: X.org is the software component that allows graphical applications to display windows on screen. It includes a driver that makes efficient use of the video card. The features offered to the graphical applications are exported through a standard interface, X11 (Bullseye contains version X11R7.7).
		

PERSPECTIVE X11, XFree86 and X.org

			X11 is the graphical system most widely used on Unix-like systems (also available for Windows and Mac OS). Strictly speaking, the term âX11â only refers to a protocol specification, but it is also used to refer to the implementation in practice.
		
			X11 had a rough start, but the 1990s saw XFree86 emerge as the reference implementation because it was free software, portable, and maintained by a collaborative community. However, the rate of evolution slowed down near the end when the software only gained new drivers. That situation, along with a very controversial license change, led to the X.org fork in 2004. This is now the reference implementation, and Debian Bullseye uses X.org version 7.7.
		
			Current versions of X.org are able to auto-detect the available hardware: this applies to the video card and the monitor, as well as keyboards and mice; in fact, it is so convenient that the package no longer even creates a /etc/X11/xorg.conf configuration file.
		
			The keyboard configuration is currently set up in /etc/default/keyboard. This file is used both to configure the text console and the graphical interface, and it is handled by the keyboard-configuration package. Details on configuring the keyboard layout are available in [SectionÂ 8.1.2, âConfiguring the Keyboardâ](basic-configuration.en.md#sect.keyboard-config).
		
			The xserver-xorg-core package provides a generic X server, as used by the 7.x versions of X.org. This server is modular and uses a set of independent drivers to handle the many different kinds of video cards. Installing xserver-xorg ensures that both the server and at least one video driver are installed.
		
			Note that if the detected video card is not handled by any of the available drivers, X.org tries using the vesa and fbdev drivers. VESA is a generic driver that should work everywhere, but with limited capabilities (fewer available resolutions, no hardware acceleration for games and visual effects for the desktop, and so on) while fbdev works on top of the kernel's framebuffer device. Nowadays the X server can run without any administrative privileges (this used to be required to be able to configure the screen) and its log file is then stored in the user's home directory in ~/.local/share/xorg/Xorg.0.log, whereas it is /var/log/Xorg.0.log for X servers started with root privileges and for versions older than Debian 9 Stretch. That log file is where one would look to know what driver is currently in use. For example, the following snippet matches what the intel driver outputs when it is loaded:
		

```bash
(==) Matched nouveau as autoconfigured driver 0
(==) Matched modesetting as autoconfigured driver 1
(==) Matched fbdev as autoconfigured driver 2
(==) Matched vesa as autoconfigured driver 3
(==) Assigned the driver to the xf86ConfigLayout
(II) LoadModule: "intel"
(II) Loading /usr/lib/xorg/modules/drivers/intel_drv.so
(II) Module intel: vendor="X.Org Foundation"
```

EXTRA Proprietary drivers

			Some video card makers (most notably NVIDIA) refuse to publish the hardware specifications that would be required to implement good free drivers. They do, however, provide proprietary drivers that allow using their hardware. This policy is nefarious, because even when the provided driver exists, it is usually not as polished as it should be; more importantly, it does not necessarily follow the X.org updates, which may prevent the latest available driver from loading correctly (or at all). We cannot condone this behavior, and we recommend you avoid these makers and favor more cooperative manufacturers.
		
			If you still end up with such a card, you will find the required packages in the non-free section: nvidia-driver for NVIDIA cards. It requires a matching kernel module. Building the module can be automated by installing the package nvidia-kernel-dkms (for NVIDIA).
		
			The nouveau project aims to develop a free software driver for NVIDIA cards and is the default driver that you get for those cards in Debian. In general, its feature set and performance do not match the proprietary driver. In the developers' defense, we should mention that the required information can only be gathered by reverse engineering, which makes things difficult. The free drivers for ATI video cards, called radeon and amdgpu, are much better in that regard although it often requires non-free firmware from the firmware-amd-graphics package.
		[Prev12.4.Â Monitoring](sect.monitoring.en.md)[Up](#)[Home](index.en.md)[Next13.2.Â Customizing the Graphical Interface](sect.customizing-graphical-interface.en.md)