---
title: "4. Book Structure"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.book-structure.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.book-structure.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "4. Book Structure"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.selected-approach.en.md)The Debian Administrator's Handbook[Next](sect.contributing.en.md)

## 4.Â Book Structure

			This book is built around a case study providing both support and illustration for all topics being addressed.
		

NOTE Web site, authors' email

			This book has its own website, which hosts whatever elements that can make it more useful. In particular, it includes an online version of the book with clickable links, and possible errata. Feel free to browse it and to leave us some feedback. We will be happy to read your comments or support messages. Send them by email to [hertzog@debian.org](mailto:hertzog@debian.org) (RaphaÃ«l) and [lolando@debian.org](mailto:lolando@debian.org) (Roland). âÂ [https://debian-handbook.info/](https://debian-handbook.info/)

Chapter 1 focuses on a non-technical presentation of the Debian project and describes its goals and organization. These aspects are important because they define a general framework that other chapters will complete with more concrete information.
		
Chapters 2 and 3 provide a broad outline of the case study. At this point, novice readers can take the time to read appendix B, where they will find a short remedial course explaining a number of basic computing notions, as well as concepts inherent to any Unix system.
		
			To get on with our real subject matter, we will quite naturally start with the installation process (chapter 4); chapters 5 and 6 will unveil basic tools that any Debian administrator will use, such as those of the APT family, which is largely responsible for the distribution's excellent reputation. These chapters are in no way restricted to professionals, since everyone is their own administrator at home.
		
Chapter 7 will be an important parenthesis; it describes workflows to efficiently use documentation and to quickly gain an understanding of problems in order to solve them.
		
			The next chapters will be a more detailed tour of the system, starting with basic infrastructure and services (chapters 8 to 10) and going progressively up the stack to reach the user applications in chapter 13. Chapter 12 deals with more advanced subjects that will most directly concern administrators of large sets of computers (including servers), while chapter 14 is a brief introduction to the wider subject of computer security and gives a few keys to avoid most problems.
		
Chapter 15 is for administrators who want to go further and create their own Debian packages. Finally, Chapter 16 describes the possible future of Debian.
		

VOCABULARY Debian package

			A Debian package is an archive containing all the files required to install a piece of software. It is generally a file with a .deb extension, and it can be handled with the dpkg command. Also called a binary package, it contains files that can be directly used (such as programs or documentation). On the other hand, a source package contains the source code for the software and the instructions required for building the binary package.
		
			The present version is already the tenth edition of the book (we include the first four that were only available in French). This edition covers version 11 of Debian, code-named Bullseye. Among the changes, Debian now supports UEFI Secure Boot, providing some extra safety against attacks on the boot infrastructure, and making it easier to install Debian on new computers where Secure Boot is usually enabled by default. Again at the security level, AppArmor, a Mandatory Access Control system that regulates what various applications are allowed to perform, is now enabled by default. All included packages have obviously been updated, including the GNOME desktop, which is now in its version 3.38.
		
			We have added some notes and remarks in sidebars. They have a variety of roles: they can draw attention to a difficult point, complete a notion of the case study, define some terms, or serve as reminders. Here is a list of the most common of these sidebars:
		
					BACK TO BASICS: a reminder of some information that is supposed to be known;
				
					VOCABULARY: defines a technical term, sometimes Debian specific;
				
					COMMUNITY: highlights important persons or roles within the project;
				
					POLICY: a rule or recommendation from the Debian Policy; This document is essential within the project, and describes how to package software. The parts of the policy highlighted in this book bring direct benefits to users (for example, knowing that the policy standardizes the location of documentation and examples makes it easy to find them even in a new package);
				
					TOOL: presents a relevant tool or service;
				
					IN PRACTICE: theory and practice do not always match; these sidebars contain advice resulting from our experience. They can also give detailed and concrete examples;
				
					other more or less frequent sidebars are rather explicit: CULTURE, TIP, CAUTION, GOING FURTHER, SECURITY, and so on.
				[Prev3.Â General Approach](sect.selected-approach.en.md)[Up](#)[Home](index.en.md)[Next5.Â Contributing](sect.contributing.en.md)