---
title: "1.1.3. The Legal Framework: A Non-Profit Organization"
source: "https://www.debian.org/doc/manuals/debian-handbook/the-debian-project.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/the-debian-project.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "1.1.3. The Legal Framework: A Non-Profit Organization"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.acknowledgments.en.md)The Debian Administrator's Handbook[Next](sect.foundation-documents.en.md)

# ChapterÂ 1.Â The Debian Project

[1.1. What Is Debian?](the-debian-project.en.md#sect.what-is-debian)[1.1.1. A Multi-Platform Operating System](the-debian-project.en.md#id-1.4.4.9)[1.1.2. The Quality of Free Software](the-debian-project.en.md#id-1.4.4.10)[1.1.3. The Legal Framework: A Non-Profit Organization](the-debian-project.en.md#id-1.4.4.11)[1.2. The Foundation Documents](sect.foundation-documents.en.md)[1.2.1. The Commitment towards Users](sect.foundation-documents.en.md#sect.social-contract)[1.2.2. The Debian Free Software Guidelines](sect.foundation-documents.en.md#sect.dfsg)[1.3. The Inner Workings of the Debian Project](sect.debian-internals.en.md)[1.3.1. The Debian Developers](sect.debian-internals.en.md#id-1.4.6.6)[1.3.2. The Active Role of Users](sect.debian-internals.en.md#id-1.4.6.7)[1.3.3. Teams, Blends, and Sub-Projects](sect.debian-internals.en.md#id-1.4.6.8)[1.4. Follow Debian News](sect.follow-debian-news.en.md)[1.5. The Role of Distributions](sect.role-of-distributions.en.md)[1.5.1. The Installer: debian-installer](sect.role-of-distributions.en.md#id-1.4.8.4)[1.5.2. The Software Library](sect.role-of-distributions.en.md#id-1.4.8.5)[1.6. Lifecycle of a Release](sect.release-lifecycle.en.md)[1.6.1. The Experimental Status](sect.release-lifecycle.en.md#id-1.4.9.12)[1.6.2. The Unstable Status](sect.release-lifecycle.en.md#id-1.4.9.13)[1.6.3. Migration to Testing](sect.release-lifecycle.en.md#id-1.4.9.14)[1.6.4. The Promotion from Testing to Stable](sect.release-lifecycle.en.md#id-1.4.9.15)[1.6.5. The Oldstable and Oldoldstable Status](sect.release-lifecycle.en.md#id-1.4.9.16)
		Before diving right into the technology, let us have a look at what the Debian Project is, its objectives, its means, and its operations.
	

## 1.1.Â What Is Debian?

CULTURE Origin of the Debian name

			Look no further: Debian is not an acronym. This name is, in reality, a contraction of two first names: that of Ian Murdock, and his girlfriend at the time, Debra. Debra + Ian = Debian.
		
			Debian is a GNU/Linux distribution. We will discuss what a distribution is in further detail in [SectionÂ 1.5, âThe Role of Distributionsâ](sect.role-of-distributions.en.md), but for now, we will simply state that it is a complete operating system, including software and systems for installation and management, all based on the Linux kernel and free software (especially those from the GNU project).
		
			When he created Debian, in 1993, under the leadership of the Free Software Foundation (FSF), Ian Murdock had clear objectives, which he expressed in the Debian Manifesto. The free operating system that he sought would have to have two principal features. First, quality: Debian would be developed with the greatest care, to be worthy of the Linux kernel. It would also be a non-commercial distribution, sufficiently credible to compete with major commercial distributions. This double ambition would, in his eyes, only be achieved by opening the Debian development process just like that of Linux and the GNU project. Thus, peer review would continuously improve the product.
		

CULTURE GNU, the project of the FSF

			The GNU project is a range of free software developed, or sponsored, by the Free Software Foundation FSF, originated by its iconic leader, Dr. Richard M. Stallman. GNU is a recursive acronym, standing for âGNU is Not Unixâ.
		

CULTURE Richard Stallman

FSF's founder and author of the GNU General Public License (GPL), Richard M. Stallman (often referred to by his initials, RMS), is a charismatic leader of the Free Software movement. Due to his uncompromising positions, he is not unanimously admired, but his non-technical contributions to Free Software (in particular, the legal and philosophical) are respected by all.
		

### 1.1.1.Â A Multi-Platform Operating System

COMMUNITY Ian Murdock's journey

				Ian Murdock, founder of the Debian project, was its first leader, from 1993 to 1996. After passing the baton to Bruce Perens, Ian took a less public role. He returned to working behind the scenes of the free software community, creating the Progeny company, with the intention of marketing a distribution derived from Debian. This venture was, sadly, a commercial failure, and development was abandoned. The company, after several years of scraping by, simply as a service provider, eventually filed for bankruptcy in April of 2007. Of the various projects initiated by Progeny, only discover still remains. It is an automatic hardware detection tool.
			
				Ian Murdock died on 28 December 2015 in San Francisco after a series of worrying tweets where he reported having been assaulted by police. In July 2016 it was announced that his death had been ruled a suicide.
			
				Debian, remaining true to its initial principles, has had so much success that, today, it has reached a tremendous size. Currently, it officially supports nineÂ hardware release architectures and several variations of each architecture known as âflavorsâ, and also other kernels like FreeBSD, although the FreeBSD-based ports are not part of the set of officially supported architectures either. Furthermore, with more than 31,000Â source packages, the available software can meet almost any need, whether at home or in the enterprise.
			
				The sheer size of the distribution can be inconvenient: it is really unreasonable to distribute 18Â DVD-ROMs (only containing the packages qualifying as "Free Software") to install a complete version on a standard PCâ¦ This is why Debian is increasingly considered as a âmeta-distributionâ, from which one extracts more specific distributions intended for a particular public: Debian Science for scientific use, Debian Edu for education and pedagogical use in an academic environment, Debian Med for medical applications, Debian Jr. for young children, etc. A more complete list of the sub-projects can be found in [SectionÂ 1.3.3.1, âExisting Debian Sub-Projects and Blendsâ](sect.debian-internals.en.md#sect.sub-projects), dedicated to that purpose.
			
				These partial views of Debian are organized in a well-defined framework, thus guaranteeing hassle-free compatibility between the various âsub-distributionsâ. All of them follow the general planning for release of new versions. And since they build on the same foundations, they can be easily extended, completed, and personalized with applications available in the Debian repositories.
			
				All the Debian tools operate in this direction: debian-cd has for a long time now allowed the creation of a set of CD-ROMs containing only a pre-selected set of packages; debian-installer is also a modular installer, easily adapted to special needs. APT will install packages from various origins, while guaranteeing the overall consistency of the system.
			

TOOL Creating a Debian CD-ROM/DVD/USB Image

debian-cd creates ISO images of installation media (CD, DVD, Blu-Ray, USB, etc.) ready for use and customizable. Any matter regarding this software is discussed (in English) on the [debian-cd@lists.debian.org](mailto:debian-cd@lists.debian.org) mailing list. The team is led by Steve McIntyre who is handling official Debian ISO builds.
			
				As debian-cd has become a quite complex tool, the simple-cdd package contains a wrapper around it to easily create customized installation media.
			

BACK TO BASICS To each computer, its architecture

				The term âarchitectureâ indicates a type of computer (the most well-known include Mac or PC). Each architecture is differentiated primarily according to its processor, usually incompatible with other processors. These differences in hardware involve varying means of operation, thus requiring that software be compiled specifically for each architecture.
			
				Most software available in Debian is written in portable programming languages: the same source code can be compiled for various architectures. In effect, an executable binary, always compiled for a specific architecture, will not usually function on any of the other architectures.
			
				Remember that each program is created by writing source code; this source code is a text file composed of instructions in a given programming language. Before you can use the software, it is necessary to compile the source code, which means transforming the code into a binary (a series of machine instructions executable by the processor). Each programming language has a specific compiler to execute this operation (for example, gcc for the C programming language).
			

TOOL Installer

debian-installer is the name of the Debian installation program. Its modular design allows it to be used in a broad range of installation scenarios. The development work is coordinated on the [debian-boot@lists.debian.org](mailto:debian-boot@lists.debian.org) mailing list under the direction of Cyril Brulebois.
			

### 1.1.2.Â The Quality of Free Software

				Debian follows all of the principles of Free Software, and its new versions are not released until they are ready. Developers do not work upon a set schedule and don't have to rush to meet an arbitrary deadline. People frequently complain of the long time between Debian's stable releases, but this caution ensures that Debian's legendary reliability is met: long months of testing are indeed necessary for the full distribution to receive the âstableâ label.
			
				Debian will not compromise on quality: all known critical bugs on key packages are resolved in any new version, even if this requires the initially forecast release date to be pushed back. Optional packages whose critical bugs are not fixed, and thus do not meet the quality requirements, are simply dropped from the stable release.
			

### 1.1.3.Â The Legal Framework: A Non-Profit Organization

				Legally speaking, Debian is a project managed by an American not-for-profit, volunteer association. The project has around a thousand Debian developers, but brings together a far greater number of contributors (translators, bug reporters, artists, casual developers, etc.).
			
				To carry its mission to fruition, Debian has a large infrastructure, with many servers connected across the Internet, offered and hosted by many sponsors.
			

COMMUNITY Behind Debian, the SPI association, and local branches

				Debian doesn't own any server in its own name, since it is only a project within the Software in the Public Interest (SPI) association, which manages the hardware and financial aspects (donations, purchase of hardware, etc.). Although it was initially created specifically for the Debian project, this association now hosts other free software projects, for example the PostgreSQL database, the Freedesktop.org project for standardization of various parts of modern graphical desktop environments (such as GNOME and KDE Plasma), and the LibreOffice office suite. âÂ [https://www.spi-inc.org/](https://www.spi-inc.org/)

				In addition to SPI, various local associations collaborate closely with Debian in order to generate funds for Debian, without centralizing everything in the USA: they are known as âTrusted Organizationsâ in the Debian jargon. This setup avoids prohibitive international transfer costs, and fits well with the decentralized nature of the project.
			
				Do not hesitate to join your local association and support the project! âÂ [https://wiki.debian.org/Teams/Treasurer/Organizations](https://wiki.debian.org/Teams/Treasurer/Organizations) âÂ [https://france.debian.net/](https://france.debian.net/) âÂ [https://debian.ch/](https://debian.ch/)
[Prev6.Â Acknowledgments](sect.acknowledgments.en.md)[Up](#)[Home](index.en.md)[Next1.2.Â The Foundation Documents](sect.foundation-documents.en.md)