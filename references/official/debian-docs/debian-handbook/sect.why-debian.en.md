---
title: "2.4.1. Commercial and Community Driven Distributions"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.why-debian.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.why-debian.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "2.4.1. Commercial and Community Driven Distributions"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.why-gnu-linux.en.md)The Debian Administrator's Handbook[Next](sect.why-debian-stable.en.md)

## 2.4.Â Why the Debian Distribution?

			Once the Linux family has been selected, a more specific option must be chosen. Again, there are plenty of criteria to consider. The chosen distribution must be able to operate for several years, since the migration from one to another would entail additional costs (although less than if the migration were between two totally different operating systems, such as Windows or OSÂ X).
		
			Sustainability is, thus, essential, and it must guarantee regular updates and security patches over several years. The timing of updates is also significant, since, with so many machines to manage, Falcot Corp cannot handle this complex operation too frequently. The IT department, therefore, insists on running the latest stable version of the distribution, benefiting from the best technical assistance, and guaranteed security patches. In effect, security updates are generally only guaranteed for a limited duration on older versions of a distribution.
		
			Finally, for reasons of homogeneity and ease of administration, the same distribution must run on all the servers and office computers.
		

### 2.4.1.Â Commercial and Community Driven Distributions

				There are two main categories of Linux distributions: commercial and community-driven. The former, developed by companies, are sold with commercial support services. The latter are developed according to the same open development model as the free software of which they are comprised.
			
				A commercial distribution will have, thus, a tendency to release new versions more frequently, in order to better market updates and associated services. Their future is directly connected to the commercial success of their company, and many have already disappeared (Caldera Linux, StormLinux, Mandriva Linux, etc.).
			
				A community distribution doesn't follow any schedule but its own. Like the Linux kernel, new versions are released when they are stable, never before. Its survival is guaranteed, as long as it has enough individual developers or third party companies to support it.
			
				A comparison of various Linux distributions led to the choice of Debian for various reasons:
			
						It is a community distribution, with development ensured independently from any commercial constraints; its objectives are, thus, essentially of a technical nature, which seem to favor the overall quality of the product.
					
						Of all community distributions, it is the most significant from many perspectives: in number of contributors, number of software packages available, and years of continuous existence. The size of its community is an incontestable witness to its continuity.
					
						Statistically, new versions are released every 18 to 24 months, and they are supported for 5 years, a schedule which is agreeable to administrators.
					
						A survey of several French service companies specialized in free software has shown that all of them provide technical assistance for Debian; it is also, for many of them, their chosen distribution, internally. This diversity of potential providers is a major asset for Falcot Corp's independence.
					
						Finally, Debian is available on a multitude of architectures, including ppc64el for OpenPOWER processors; it will, thus, be possible to install it on Falcot Corp's latest IBM servers.
					

IN PRACTICE Debian Long Term Support

				The Debian Long Term Support (LTS) project started in 2014 and aims to provide 5 years of security support to all stable Debian releases. As LTS is of primary importance to organizations with large deployments, the project tries to pool resources from Debian-using companies. âÂ [https://wiki.debian.org/LTS](https://wiki.debian.org/LTS)

				Falcot Corp is not big enough to let one member of its IT staff contribute to the LTS project, so the company opted to subscribe to Freexian's Debian LTS contract and provides financial support. Thanks to this, the Falcot administrators know that the packages they use will be handled in priority and they have a direct contact with the LTS team in case of problems. âÂ [https://wiki.debian.org/LTS/Funding](https://wiki.debian.org/LTS/Funding) âÂ [https://www.freexian.com/services/debian-lts.html](https://www.freexian.com/services/debian-lts.html)

				Once Debian has been chosen, the matter of which version to use must be decided. Let us see why the administrators have picked Debian Bullseye.
			[Prev2.3.Â Why a GNU/Linux Distribution?](sect.why-gnu-linux.en.md)[Up](#)[Home](index.en.md)[Next2.5.Â Why Debian Bullseye?](sect.why-debian-stable.en.md)