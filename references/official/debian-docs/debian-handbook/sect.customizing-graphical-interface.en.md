---
title: "13.2.3. Menu Management"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.customizing-graphical-interface.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.customizing-graphical-interface.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.2.3. Menu Management"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](workstation.en.md)The Debian Administrator's Handbook[Next](sect.graphical-desktops.en.md)

## 13.2.Â Customizing the Graphical Interface

### 13.2.1.Â Choosing a Display Manager

				The graphical interface only provides display space. Running the X server by itself only leads to an empty screen, which is why most installations use a display manager to display a user authentication screen and start the graphical desktop once the user has authenticated. The three most popular display managers in current use are gdm3 (GNOME Display Manager), sddm (suggested for KDE Plasma) and lightdm (Light Display Manager). More alternatives exist and can be found by searching for the x-display-manager virtual package.
			
				Since the Falcot Corp administrators have opted to use the GNOME desktop environment, they logically picked gdm3 as a display manager too. The /etc/gdm3/daemon.conf configuration file has many options (the list can be found in the /usr/share/gdm/gdm.schemas schema file) to control its behavior while /etc/gdm3/greeter.dconf-defaults contains settings for the greeter âsessionâ (more than just a login window, it is a limited desktop with power management and accessibility related tools). Note that some of the most useful settings for end-users can be tweaked with GNOME's control center.
			

### 13.2.2.Â Choosing a Window Manager

				Since each graphical desktop provides its own window manager, which window manager you choose is usually influenced by which desktop you have selected. GNOME uses the mutter window manager, Plasma uses kwin, and Xfce (which we present later) has xfwm. The Unix philosophy always allows using one's window manager of choice, but following the recommendations allows an administrator to best take advantage of the integration efforts led by each project.
			

BACK TO BASICS Window manager

				The window manager displays the âdecorationsâ around the windows belonging to the currently running applications, which includes frames and the title bar. It also allows reducing, restoring, maximizing, and hiding windows. Most window managers also provide a menu that pops up when the desktop is clicked in a specific way. This menu provides the means to close the window manager session, start new applications, and in some cases, change to another window manager (if installed).
			
				Older computers may, however, have a hard time running heavyweight graphical desktop environments. In these cases, a lighter alternative (search for the x-window-manager virtual package) should be used. âLightâ (or small footprint) window managers include WindowMaker (in the wmaker package), afterstep, icewm, blackbox, fluxbox, or openbox. In these cases, the system should be configured so that the appropriate window manager gets precedence; the standard way is to change the x-window-manager alternative with the command update-alternatives --config x-window-manager.
			

DEBIAN SPECIFICITY Alternatives

				The Debian policy lists a number of standardized commands able to perform a particular action. For example, the x-window-manager command invokes a window manager. But Debian does not assign this command to a fixed window manager. The administrator can choose which manager it should invoke.
			
				For each window manager, the relevant package therefore registers the appropriate command as a possible choice for x-window-manager along with an associated priority. Barring explicit configuration by the administrator, this priority allows picking the best installed window manager when the generic command is run.
			
				Both the registration of commands and the explicit configuration involve the update-alternatives script. Choosing where a symbolic command points at is a simple matter of running update-alternatives --config symbolic-command. This script creates (and maintains) symbolic links in the /etc/alternatives/ directory, which in turn references the location of the executable. As time passes, packages are installed or removed, and/or the administrator makes explicit changes to the configuration. When a package providing an alternative is removed, the alternative automatically goes to the next best choice among the remaining possible commands.
			
				Not all symbolic commands are explicitly listed by the Debian policy; some Debian package maintainers deliberately chose to use this mechanism in less straightforward cases where it still brings interesting flexibility (examples include x-www-browser, www-browser, cc, c++, awk, and so on).
			

### 13.2.3.Â Menu Management

				Modern desktop environments and many window managers provide menus listing the available applications for the user. In order to keep menus up-to-date in relation to the actual set of available applications, each package usually provides a .desktop file in /usr/share/applications. The format of those files has been standardized by FreeDesktop.org: âÂ [https://specifications.freedesktop.org/desktop-entry-spec/latest/](https://specifications.freedesktop.org/desktop-entry-spec/latest/)

				The applications menus can be further customized by administrators through system-wide configuration files as described by the âDesktop Menu Specificationâ. End-users can also customize the menus with graphical tools such as kmenuedit (in Plasma), alacarte (in GNOME) or menulibre. âÂ [https://specifications.freedesktop.org/menu-spec/latest/](https://specifications.freedesktop.org/menu-spec/latest/)

HISTORY The Debian menu system

				Historically â way before the FreeDesktop.org standards emerged â Debian had invented its own menu system where each package provided a generic description of the desired menu entries in /usr/share/menu/. This tool is still available in Debian (in the menu package) but it is only marginally useful since package maintainers are encouraged to rely on .desktop files instead.
			[PrevChapterÂ 13.Â Workstation](workstation.en.md)[Up](#)[Home](index.en.md)[Next13.3.Â Graphical Desktops](sect.graphical-desktops.en.md)