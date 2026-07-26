---
title: "13.7. Office Suites"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.office-suites.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.office-suites.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.7. Office Suites"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.development.en.md)The Debian Administrator's Handbook[Next](sect.windows-emulation.en.md)

## 13.7.Â Office Suites

			Office software has long been seen as lacking in the free software world. Users require replacements for Microsoft tools such as Word and Excel, but these are so complex that replacements were hard to develop. The situation changed when Sun released the StarOffice code under a free license as OpenOffice, a project which later gave birth to LibreOffice, which is available on Debian. The KDE project also has its own office suite, called Calligra Suite (previously KOffice), and GNOME, while never offering a comprehensive office suite, provides AbiWord as a word processor and Gnumeric as a spreadsheet. The various projects each have their strengths. For instance, the Gnumeric spreadsheet is better than OpenOffice.org/LibreOffice in some domains, notably the precision of its calculations. On the word processing front, the LibreOffice suite still leads the way.
		
			Another important feature for users is the ability to import Microsoft Office documents. Even though all office suites have this feature, only the ones in OpenOffice.org and LibreOffice are functional enough for daily use.
		

THE BROADER VIEW LibreOffice replaces OpenOffice.org

			OpenOffice.org contributors set up a foundation (The Document Foundation) to foster the project's development. The idea had been discussed for some time, but the actual trigger was Oracle's acquisition of Sun. The new ownership made the future of OpenOffice under Oracle uncertain. Since Oracle declined to join the foundation, the developers had to give up on the OpenOffice.org name. This office suite is now known as LibreOffice, and is available in Debian.
		
			After a period of relative stagnation on OpenOffice.org, Oracle donated the code and associated rights to the Apache Software Foundation, and OpenOffice is now an Apache project. This project is not currently available in Debian and is rather moribund when compared to LibreOffice.
		
			LibreOffice and Calligra Suite are available in the libreoffice and calligra Debian packages, respectively.
		
			Language-specific packs for LibreOffice are distributed in separate packages, most notably libreoffice-l10n-* and libreoffice-help-*. Some features such as spelling dictionaries, hyphenation patterns and thesauri are in separate packages, such as myspell-*, hunspell-*, hyphen-* and mythes-*.
		[Prev13.6.Â Development](sect.development.en.md)[Up](#)[Home](index.en.md)[Next13.8.Â Emulating Windows: Wine](sect.windows-emulation.en.md)