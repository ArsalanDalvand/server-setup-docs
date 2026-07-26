---
title: "1.5.2. The Software Library"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.role-of-distributions.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.role-of-distributions.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "1.5.2. The Software Library"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.follow-debian-news.en.md)The Debian Administrator's Handbook[Next](sect.release-lifecycle.en.md)

## 1.5.Â The Role of Distributions

			A GNU/Linux distribution has two main objectives: install a free operating system on a computer (either with or without an existing system or systems), and provide a range of software covering all of the users' needs.
		

### 1.5.1.Â The Installer: debian-installer

				The debian-installer, designed to be extremely modular in order to be as generic as possible, targets the first objective. It covers a broad range of installation situations and in general, greatly facilitates the creation of a derivative installer corresponding to a particular case.
			
				This modularity, which also makes it very complex, may be daunting for the developers discovering this tool; but whether used in graphical or text mode, the user's experience is still similar. Great efforts have been made to reduce the number of questions asked at installation time, in particular thanks to the inclusion of automatic hardware detection software.
			
				It is interesting to note that distributions derived from Debian differ greatly on this aspect, and provide a more limited installer (often confined to the i386 or amd64 architectures), but more user-friendly for the uninitiated. On the other hand, they usually refrain from straying too far from package contents in order to benefit as much as possible from the vast range of software offered without causing compatibility problems.
			

### 1.5.2.Â The Software Library

				Quantitatively, Debian is undeniably the leader in this respect, with over 31,000 source packages. Qualitatively, Debianâs policy and long testing period prior to releasing a new stable version justify its reputation for stability and consistency. As far as availability, everything is available on-line through many mirrors worldwide, with updates pushed out every six hours.
			
				Most new free software programs quickly find their way into the development version which allows them to be installed. If this requires too many updates due to their dependencies, the program can also be recompiled for the stable version of Debian (see [ChapterÂ 15, Creating a Debian Package](debian-packaging.en.md) for more information on this topic).
			[Prev1.4.Â Follow Debian News](sect.follow-debian-news.en.md)[Up](#)[Home](index.en.md)[Next1.6.Â Lifecycle of a Release](sect.release-lifecycle.en.md)