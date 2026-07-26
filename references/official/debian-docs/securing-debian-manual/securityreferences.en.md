---
title: "2.2. Be aware of general security problems"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/securityreferences.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/securityreferences.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "2.2. Be aware of general security problems"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch02.en.md)Securing Debian Manual[Next](ch02s03.en.md)

## 2.2.Â Be aware of general security problems

			The following manual does not (usually) go into the details on why some issues are considered security risks. However, you might want to have a better background regarding general UNIX and (specific) Linux security. Take some time to read over security related documents in order to make informed decisions when you are encountered with different choices. Debian GNU/Linux is based on the Linux kernel, so much of the information regarding Linux, as well as from other distributions and general UNIX security also apply to it (even if the tools used, or the programs available, differ).
		
			Some useful documents include:
		
					The [http://www.tldp.org/HOWTO/Security-HOWTO/](http://www.tldp.org/HOWTO/Security-HOWTO/) is one of the best references regarding general Linux security.
				
					The [http://www.tldp.org/HOWTO/Security-Quickstart-HOWTO/](http://www.tldp.org/HOWTO/Security-Quickstart-HOWTO/) is also a very good starting point for novice users (both to Linux and security).
				
					The [http://seifried.org/lasg/](http://seifried.org/lasg/) is a complete guide that touches all the issues related to security in Linux, from kernel security to VPNs. Note that it has not been updated since 2001, but some information is still relevant. [[1]](#ftn.idm314)

					Kurt Seifried's [http://seifried.org/security/os/linux/20020324-securing-linux-step-by-step.html](http://seifried.org/security/os/linux/20020324-securing-linux-step-by-step.html).
				
					In [http://www.tldp.org/links/p_books.html#securing_linux](http://www.tldp.org/links/p_books.html#securing_linux) you can find a similar document to this manual but related to Red Hat, some of the issues are not distribution-specific and also apply to Debian.
				
					Another Red Hat related document is [https://web.archive.org/web/20050520170309/https://ltp.sourceforge.net/docs/RHEL-EAL3-Configuration-Guide.pdf](https://web.archive.org/web/20050520170309/https://ltp.sourceforge.net/docs/RHEL-EAL3-Configuration-Guide.pdf).
				
					IntersectAlliance has published some documents that can be used as reference cards on how to harden Linux servers (and their services), the documents are available at [https://web.archive.org/web/20030210231943/http://www.intersectalliance.com/projects/index.html](https://web.archive.org/web/20030210231943/http://www.intersectalliance.com/projects/index.html).
				
					For network administrators, a good reference for building a secure network is the [https://web.archive.org/web/20030418093551/http://www.linuxsecurity.com/docs/LDP/Securing-Domain-HOWTO/](https://web.archive.org/web/20030418093551/http://www.linuxsecurity.com/docs/LDP/Securing-Domain-HOWTO/).
				
					If you want to evaluate the programs you are going to use (or want to build up some new ones) you should read the [http://www.tldp.org/HOWTO/Secure-Programs-HOWTO/](http://www.tldp.org/HOWTO/Secure-Programs-HOWTO/) (master copy is available at [http://www.dwheeler.com/secure-programs/](http://www.dwheeler.com/secure-programs/), it includes slides and talks from the author, David Wheeler)
				
					If you are considering installing firewall capabilities, you should read the [http://www.tldp.org/HOWTO/Firewall-HOWTO.html](http://www.tldp.org/HOWTO/Firewall-HOWTO.html) and the [http://www.tldp.org/HOWTO/IPCHAINS-HOWTO.html](http://www.tldp.org/HOWTO/IPCHAINS-HOWTO.html) (for kernels previous to 2.4).
				
					Finally, a good card to keep handy is the [https://web.archive.org/web/20030308013020/http://www.linuxsecurity.com/docs/QuickRefCard.pdf](https://web.archive.org/web/20030308013020/http://www.linuxsecurity.com/docs/QuickRefCard.pdf).
				
			In any case, there is more information regarding the services explained here (NFS, NIS, SMB...) in many of the HOWTOs of the [http://www.tldp.org/](http://www.tldp.org/). Some of these documents speak on the security side of a given service, so be sure to take a look there too.
		
			The HOWTO documents from the Linux Documentation Project are available in Debian GNU/Linux through the installation of the doc-linux-text (text version) or doc-linux-html (HTML version). After installation these documents will be available at the /usr/share/doc/HOWTO/en-txt and /usr/share/doc/HOWTO/en-html directories, respectively.
		
			Other recommended Linux books:
		
					Maximum Linux Security : A Hacker's Guide to Protecting Your Linux Server and Network. Anonymous. Paperback - 829 pages. Sams Publishing. ISBN: 0672313413. July 1999.
				
					Linux Security By John S. Flowers. New Riders; ISBN: 0735700354. March 1999.
				
[https://web.archive.org/web/20030202131658/https://www.linux.org/books/ISBN_0072127732.html](https://web.archive.org/web/20030202131658/https://www.linux.org/books/ISBN_0072127732.html) By Brian Hatch. McGraw-Hill Higher Education. ISBN 0072127732. April, 2001
				
			Other books (which might be related to general issues regarding UNIX and security and not Linux specific):
		
[https://web.archive.org/web/20030206231652/http://www.oreilly.com/catalog/puis/](https://web.archive.org/web/20030206231652/http://www.oreilly.com/catalog/puis/) Garfinkel, Simpson, and Spafford, Gene; O'Reilly Associates; ISBN 0-56592-148-8; 1004pp; 1996.
				
					Firewalls and Internet Security Cheswick, William R. and Bellovin, Steven M.; Addison-Wesley; 1994; ISBN 0-201-63357-4; 320pp.
				
			Some useful web sites to keep up to date regarding security:
		
[http://csrc.nist.gov/](http://csrc.nist.gov/).
				
[https://cve.mitre.org/data/refs/refmap/source-BUGTRAQ.html](https://cve.mitre.org/data/refs/refmap/source-BUGTRAQ.html) CVE Reference Map for Source BUGTRAQ
				
[http://www.linuxsecurity.com/](http://www.linuxsecurity.com/). General information regarding Linux security (tools, news...). Most useful is the [https://linuxsecurity.com/howtos](https://linuxsecurity.com/howtos) page.
				[[1]](#idm314)
						At a given time it was superseded by the "Linux Security Knowledge Base". This documentation is also provided in Debian through the lskb package. Now it's back as the Lasg again.
					[PrevChapterÂ 2.Â Before you begin](ch02.en.md)[Up](#)[Home](index.en.md)[Next2.3.Â How does Debian handle security?](ch02s03.en.md)