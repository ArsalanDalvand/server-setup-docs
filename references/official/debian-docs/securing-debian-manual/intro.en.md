---
title: "NextChapterÂ 1.Â Introduction"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/intro.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/intro.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "NextChapterÂ 1.Â Introduction"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](index.en.md)Securing Debian Manual[Next](guideformats.en.md)

# ChapterÂ 1.Â Introduction

[1.1. Authors](intro.en.md#authors)[1.2. Where to get the manual (and available formats)](guideformats.en.md)[1.3. Organizational notes/feedback](ch01s03.en.md)[1.4. Prior knowledge](references.en.md)[1.5. Things that need to be written (FIXME/TODO)](ch01s05.en.md)[1.6. Credits and thanks!](credits.en.md)
		One of the hardest things about writing security documents is that every case is unique. Two things you have to pay attention to are the threat environment and the security needs of the individual site, host, or network. For instance, the security needs of a home user are completely different from a network in a bank. While the primary threat a home user needs to face is the script kiddie type of cracker, a bank network has to worry about directed attacks. Additionally, the bank has to protect their customer's data with arithmetic precision. In short, every user has to consider the trade-off between usability and security/paranoia.
	
		Note that this manual only covers issues relating to software. The best software in the world can't protect you if someone can physically access the machine. You can place it under your desk, or you can place it in a hardened bunker with an army in front of it. Nevertheless the desktop computer can be much more secure (from a software point of view) than a physically protected one if the desktop is configured properly and the software on the protected machine is full of security holes. Obviously, you must consider both issues.
	
		This document just gives an overview of what you can do to increase the security of your Debian GNU/Linux system. If you have read other documents regarding Linux security, you will find that there are common issues which might overlap with this document. However, this document does not try to be the ultimate source of information you will be using, it only tries to adapt this same information so that it is meaningful to a Debian GNU/Linux system. Different distributions do some things in different ways (startup of daemons is one example); here, you will find material which is appropriate for Debian's procedures and tools.
	

## 1.1.Â Authors

			The current maintainer of this document is Javier FernÃ¡ndez-Sanguino PeÃ±a. Please forward him any comments, additions or suggestions, and they will be considered for inclusion in future releases of this manual.
		
			This manual was started as a HOWTO by Alexander Reelsen. After it was published on the Internet, Javier FernÃ¡ndez-Sanguino PeÃ±a incorporated it into the [Debian Documentation Project](http://www.debian.org/doc). A number of people have contributed to this manual (all contributions are listed in the changelog) but the following deserve special mention since they have provided significant contributions (full sections, chapters or appendices):
		
					Stefano Canepa
				
					Era Eriksson
				
					Carlo Perassi
				
					Alexandre Ratti
				
					Jaime Robles
				
					Yotam Rubin
				
					Frederic Schutz
				
					Pedro Zorzenon Neto
				
					Oohara Yuuma
				
					Davor Ocelic
				[PrevSecuring Debian Manual](index.en.md)[Up](#)[Home](index.en.md)[Next1.2.Â Where to get the manual (and available forma...](guideformats.en.md)