---
title: "6.8. Keeping a System Up to Date"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.regular-upgrades.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.regular-upgrades.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.8. Keeping a System Up to Date"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.dist-upgrade.en.md)The Debian Administrator's Handbook[Next](sect.automatic-upgrades.en.md)

## 6.8.Â Keeping a System Up to Date

			The Debian distribution is dynamic and changes continually. Most of the changes are in the Testing and Unstable versions, but even Stable is updated from time to time, mostly for security-related fixes. Whatever version of Debian a system runs, it is generally a good idea to keep it up to date, so that you can get the benefit of recent evolution and bug fixes.
		
			While it is of course possible to periodically run a tool to check for available updates and run the upgrades, such a repetitive task is tedious, especially when it needs to be performed on several machines. Fortunately, like many repetitive tasks, it can be partly automated, and a set of tools have already been developed to that effect.
		
			The first of these tools is apticron, in the package of the same name. Its main effect is to run a script daily (via cron). The script updates the list of available packages, and, if some installed packages are not in the latest available version, it sends an email with a list of these packages along with the changes that have been made in the new versions. Obviously, this package mostly targets users of Debian Stable, since the daily emails would be very long for the faster paced versions of Debian. When updates are available, apticron automatically downloads them. It does not install them â the administrator will still do it â but having the packages already downloaded and available locally (in APT's cache) makes the job faster.
		
			Administrators in charge of several computers will no doubt appreciate being informed of pending upgrades, but the upgrades themselves are still as tedious as they used to be. Periodic upgrades can be enabled: it uses a systemd timer unit or cron. If systemd is not installed, the /etc/cron.daily/apt-compat script (in the apt package) comes in handy. This script is run daily (and non-interactively) by cron. To control the behavior, use APT configuration variables (which are therefore stored in a file /etc/apt/apt.conf.d/10periodic). The main variables are:
		  APT::Periodic::Update-Package-Lists 
						This option allows you to specify the frequency (in days) at which the package lists are refreshed. apticron users can do without this variable, since apticron already does this task.
					  APT::Periodic::Download-Upgradeable-Packages 
						Again, this option indicates a frequency (in days), this time for the downloading of the actual packages. Again, apticron users won't need it.
					  APT::Periodic::AutocleanInterval 
						This option covers a feature that apticron doesn't have. It controls how often obsolete packages (those not referenced by any distribution anymore) are removed from the APT cache. This keeps the APT cache at a reasonable size and means that you don't need to worry about that task.
					  APT::Periodic::Unattended-Upgrade 
						When this option is enabled, the daily script will execute unattended-upgrade (from the unattended-upgrades package) which â as its name suggest â can automatize the upgrade process for some packages (by default it only takes care of security updates, but this can be customized in /etc/apt/apt.conf.d/50unattended-upgrades). Note that this option can be set with the help of debconf by running dpkg-reconfigure -plow unattended-upgrades. If apt-listbugs is installed, it will prevent an automatic upgrade of packages which are affected by an already reported serious or grave bug.
					
			Other options can allow you to control the cache cleaning behavior with more precision. They are not listed here, but they are described in the /usr/lib/apt/apt.systemd.daily script.
		
			These tools work very well for servers, but desktop users generally prefer a more interactive system. The package gnome-software provides an icon in the notification area of desktop environments when updates are available; clicking on this icon then runs an interface to perform updates. You can browse through available updates, read the short description of the relevant packages and the corresponding changelog entries, and select whether to apply the update or not on a case-by-case basis.
		

FigureÂ 6.3.Â Upgrading with gpk-update-viewer

			This tool is no longer installed in the default GNOME desktop. The new philosophy is that security updates should be automatically installed, either in the background or, preferably, when you shutdown your computer so as to not confuse any running application.
		[Prev6.7.Â Upgrading from One Stable Distribution to th...](sect.dist-upgrade.en.md)[Up](#)[Home](index.en.md)[Next6.9.Â Automatic Upgrades](sect.automatic-upgrades.en.md)