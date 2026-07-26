---
title: "13.3.4. Other Desktop Environments"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.graphical-desktops.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.graphical-desktops.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.3.4. Other Desktop Environments"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.customizing-graphical-interface.en.md)The Debian Administrator's Handbook[Next](sect.main-desktop-tools.en.md)

## 13.3.Â Graphical Desktops

			The free graphical desktop field is dominated by two large software collections: GNOME and Plasma by KDE. Both of them are very popular.
		
			This diversity is rooted in history. Plasma (initially only KDE, which is now the name of the community) was the first graphical desktop project, but it chose the Qt graphical toolkit and that choice wasn't acceptable for a large number of developers. Qt was not free software at the time, and GNOME was started based on the GTK+ toolkit. Qt has since become free software, but the projects still evolved in parallel.
		
			The GNOME and KDE communities still work together: under the FreeDesktop.org umbrella, the projects collaborated in defining standards for interoperability across applications.
		
			Choosing âthe bestâ graphical desktop is a sensitive topic which we prefer to steer clear of. We will merely describe the many possibilities and give a few pointers for further thoughts. The best choice will be the one you make after some experimentation.
		

### 13.3.1.Â GNOME

				Debian Bullseye includes GNOME versionÂ 3.38, which can be installed by a simple apt install gnome (it can also be installed by selecting the âDebian desktop environmentâ task - task-desktop and task-gnome-desktop).
			
				GNOME is noteworthy for its efforts in usability and accessibility. Design professionals have been involved in writing its standards and recommendations, which has helped developers to create satisfying graphical user interfaces. The project also gets encouragement from the big players of computing, such as Intel, IBM, Oracle, Novell, and of course, various Linux distributions. Finally, many programming languages can be used in developing applications interfacing to GNOME.
			

FigureÂ 13.1.Â The GNOME desktop

				For administrators, GNOME seems to be better prepared for massive deployments. Application configuration is handled through the GSettings interface and stores its data in the DConf database. The configuration settings can thus be queried and edited with the gsettings, and dconf command-line tools, or by the dconf-editor graphical user interfaces. The administrator can therefore change users' configuration with a simple script. The GNOME website provides information to guide administrators who manage GNOME workstations: âÂ [https://help.gnome.org/admin/](https://help.gnome.org/admin/)

### 13.3.2.Â KDE and Plasma

				Debian Bullseye includes versionÂ 5.20 of KDE Plasma, which can be installed with apt install kde-standard (task-kde-desktop).
			
				Plasma has had a rapid evolution based on a very hands-on approach. Its authors quickly got very good results, which allowed them to grow a large user-base. These factors contributed to the overall project quality. Plasma is a mature desktop environment with a wide range of applications.
			

FigureÂ 13.2.Â The Plasma desktop

				Since the QtÂ 4.0 release, the last remaining license problem with KDE software has been solved. This version was released under the GPL both for Linux and Windows (the Windows version was previously released under a non-free license). KDE applications are primarily developed using the C++ language.
			

### 13.3.3.Â Xfce and Others

				Xfce is a simple and lightweight graphical desktop, which is a perfect match for computers with limited resources. It can be installed with apt install xfce4 (task-xfce-desktop). Like GNOME, Xfce is based on the GTK+ toolkit, and several components are common across both desktops.
			
				Unlike GNOME and Plasma, Xfce does not aim to become a vast project. Beyond the basic components of a modern desktop (file manager, window manager, session manager, a panel for application launchers and so on), it only provides a few specific applications: a terminal, a calendar (orage), an image viewer, a CD/DVD burning tool, a media player (parole), sound volume control and a text editor (mousepad). âÂ [https://xfce.org/](https://xfce.org/)

FigureÂ 13.3.Â The Xfce desktop

### 13.3.4.Â Other Desktop Environments

LXDE and LXQt are two desktop environments focusing on the âlightweightâ aspect. The former is GTK+ based while the latter is Qt based. They can be installed with the lxde (task-lxde-desktop), and lxqt (task-lxqt-desktop) metapackages. âÂ [https://lxde.org/](https://lxde.org/) âÂ [https://lxqt.org/](https://lxqt.org/)

FigureÂ 13.4.Â The LXDE desktop

FigureÂ 13.5.Â The LXQT desktop

Cinnamon and MATE both started when GNOME 3 moved away from the traditional desktop paradigm, dropping the usual panel and its menu in favor of the new search-based shell. The former reintroduced a panel by forking GNOME Shell and the latter is a continuation of GNOME 2. They can be installed with the cinnamon-desktop-environment (task-cinnamon-desktop) and mate-desktop-environment (task-mate-desktop) meta-packages âÂ [https://developer.linuxmint.com/projects/cinnamon-projects.html](https://developer.linuxmint.com/projects/cinnamon-projects.html) âÂ [https://mate-desktop.org/](https://mate-desktop.org/)

FigureÂ 13.6.Â The Cinnamon desktop

FigureÂ 13.7.Â The MATE desktop

[Prev13.2.Â Customizing the Graphical Interface](sect.customizing-graphical-interface.en.md)[Up](#)[Home](index.en.md)[Next13.4.Â Email](sect.main-desktop-tools.en.md)