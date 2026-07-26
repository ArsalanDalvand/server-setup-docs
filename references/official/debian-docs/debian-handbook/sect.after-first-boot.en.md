---
title: "4.3.2. Upgrading the System"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.after-first-boot.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.after-first-boot.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "4.3.2. Upgrading the System"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.installation-steps.en.md)The Debian Administrator's Handbook[Next](packaging-system.en.md)

## 4.3.Â After the First Boot

			If you activated the task âDebian desktop environmentâ without any explicit desktop choice (or with the âGNOMEâ choice), the computer will display the gdm3 login manager.
		

FigureÂ 4.16.Â First boot

			The user that has already been created can then log in and begin working immediately.
		

### 4.3.1.Â Installing Additional Software

				The installed packages correspond to the profiles selected during installation, but not necessarily to the use that will actually be made of the machine. As such, you might want to use a package management tool to refine the selection of installed packages. The two most frequently used tools (which are installed if the âDebian desktop environmentâ profile was chosen) are apt (accessible from the command line) and synaptic (âSynaptic Package Managerâ in the menus).
			
				To facilitate the installation of coherent groups of programs, Debian creates âtasksâ that are dedicated to specific uses (mail server, file server, etc.). You already had the opportunity to select them during installation, and you can access them again thanks to package management tools such as aptitude (the tasks are listed in a distinct section) and synaptic (through the menu Edit â Mark Packages by Taskâ¦).
			
				Aptitude is an interface to APT in full-screen text mode. It allows the user to browse the list of available packages according to various categories (installed or not-installed packages, by task, by section, etc.), and to view all of the information available on each of them (dependencies, conflicts, description, etc.). Each package can be marked âinstallâ (to be installed, + key) or âremoveâ (to be removed, - key). All of these operations will be conducted simultaneously once you've confirmed them by pressing the g key (âgâ for âgo!â). If you have forgotten some programs, no worries; you will be able to run aptitude again once the initial installation has been completed.
			

TIP Debian thinks of speakers of non-English languages

				Several tasks are dedicated to the localization of the system in other languages beyond English. They include translated documentation, dictionaries, and various other packages useful for speakers of different languages. The appropriate task is automatically selected if a non-English language was chosen during installation.
			
				Of course, it is possible not to select any task to be installed. In this case, you can manually install the desired software with the apt or aptitude command (which are both accessible from the command line). The packages can be found using the apt-cache search task keyword command.
			

VOCABULARY Package dependencies, conflicts

				In the Debian packaging lingo, a âdependencyâ is another package necessary for the proper functioning of the package in question. Conversely, a âconflictâ is a package that can not be installed side-by-side with another.
			
				These concepts are discussed in greater detail in [ChapterÂ 5, Packaging System: Tools and Fundamental Principles](packaging-system.en.md).
			

### 4.3.2.Â Upgrading the System

				A first apt upgrade (a command used to automatically update installed programs) is generally required, especially for possible security updates issued since the release of the latest Debian stable version. These updates may involve some additional questions through debconf, the standard Debian configuration tool. For further information on these updates conducted by apt, please refer to [SectionÂ 6.2.3, âSystem Upgradeâ](sect.apt-get.en.md#sect.apt-upgrade).
			[Prev4.2.Â Installing, Step by Step](sect.installation-steps.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 5.Â Packaging System: Tools and Fundamenta...](packaging-system.en.md)