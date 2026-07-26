---
title: "2.3. Why a GNU/Linux Distribution?"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.why-gnu-linux.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.why-gnu-linux.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "2.3. Why a GNU/Linux Distribution?"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.master-plan.en.md)The Debian Administrator's Handbook[Next](sect.why-debian.en.md)

## 2.3.Â Why a GNU/Linux Distribution?

BACK TO BASICS Linux or GNU/Linux?

			Linux, as you already know, is only a kernel. The expressions, âLinux distributionâ and âLinux systemâ are, thus, incorrect: they are, in reality, distributions or systems based on Linux. These expressions fail to mention the software that always completes this kernel, among which are the programs developed by the GNU Project. Dr. Richard Stallman, founder of this project, insists that the expression âGNU/Linuxâ be systematically used, in order to better recognize the important contributions made by the GNU Project and the principles of freedom upon which they are founded.
		
			Debian has chosen to follow this recommendation, and, thus, name its distributions accordingly. Thus, the latest stable release is Debian GNU/Linux 11.
		
			Several factors have dictated this choice. The system administrator, who was familiar with this distribution, ensured it was listed among the candidates for the computer system overhaul. Difficult economic conditions and ferocious competition have limited the budget for this operation, despite its critical importance for the future of the company. This is why Open Source solutions were swiftly chosen: several recent studies indicate they are less expensive than proprietary solutions while providing equal or better quality of service so long as qualified personnel are available to run them.
		

IN PRACTICE Total Cost of Ownership

			The Total Cost of Ownership (TCO) is the total of all money expended for the possession or acquisition of an item, in this case referring to the operating system. This price includes any possible license fee, costs for training personnel to work with the new software, replacement of machines that are too slow, additional repairs, etc. Everything arising directly from the initial choice is taken into account.
		
			This TCO, which varies according to the criteria chosen in the assessment thereof, is rarely significant when taken in isolation. However, it is very interesting to compare TCOs for different options if they are calculated according to the same rules. This assessment table is, thus, of paramount importance, and it is easy to manipulate it in order to draw a predefined conclusion. Thus, the TCO for a single machine doesn't make sense, since the cost of an administrator is also reflected in the total number of machines they manage, a number which obviously depends on the operating system and tools proposed.
		
			Among free operating systems, the IT department looked at the free BSD systems (OpenBSD, FreeBSD, and NetBSD), GNU Hurd, and Linux distributions. GNU Hurd, which has not yet released a stable version, was immediately rejected. The choice is simpler between BSD and Linux. The former have many merits, especially on servers. Pragmatism, however, led to choosing a Linux system, since its installed base and popularity are both very significant and have many positive consequences. One of these consequences is that it is easier to find qualified personnel to administer Linux machines than technicians experienced with BSD. Furthermore, Linux adapts to newer hardware faster than BSD (although they are often neck and neck in this race). Finally, Linux distributions are often more adapted to user-friendly graphical user interfaces, indispensable for beginners during migration of all office machines to a new system.
		

ALTERNATIVE Debian GNU/kFreeBSD

			Since Debian 6 Squeeze, it is possible to use Debian with a FreeBSD kernel on 32-bit and 64-bit computers; this is what the kfreebsd-i386 and kfreebsd-amd64 architectures mean. While these architectures are not âofficialâ (they are hosted on a separate mirror â ports.debian.org), they provide over 60% of the software packaged by Debian.
		
			These architectures may be an appropriate choice for Falcot Corp administrators, especially for a firewall (the kernel supports three different firewalls: IPF, IPFW, PF) or for a NAS (network attached storage system, for which the ZFS filesystem has been tested and approved).
		[Prev2.2.Â Master Plan](sect.master-plan.en.md)[Up](#)[Home](index.en.md)[Next2.4.Â Why the Debian Distribution?](sect.why-debian.en.md)