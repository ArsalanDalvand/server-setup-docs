---
title: "13.4.3. Thunderbird"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.main-desktop-tools.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.main-desktop-tools.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.4.3. Thunderbird"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.graphical-desktops.en.md)The Debian Administrator's Handbook[Next](sect.web-browsers.en.md)

## 13.4.Â Email

### 13.4.1.Â Evolution

COMMUNITY Popular packages

				Installing the popularity-contest package enables participation in an automated survey that informs the Debian project about the most popular packages. A script is run weekly by cron which sends an anonymized list of the installed packages (by HTTP or email) and the latest access date for the files they contain. This allows the Debian maintainers to know which packages are most frequently installed, and of these, how frequently they are actually used.
			
				This information is a great help to the Debian project. It is used to determine which packages should go on the first installation disks. The installation data is also an important factor used to decide whether to remove a package with very few users from the distribution. We heartily recommend installing the popularity-contest package, and participating in the survey.
			
				The collected data are made public every day. âÂ [https://popcon.debian.org/](https://popcon.debian.org/)

				These statistics can also help users to choose between two packages that seem otherwise equivalent. Choosing the more popular package is probably a safer choice.
			
				Evolution is the GNOME email client and can be installed with apt install evolution. It is more than a simple email client: it also provides a calendar, an address book, a task list, and a memo (free-form note) application. Its email component includes a powerful message indexing system, and allows for the creation of virtual folders based on search queries on all archived messages. In other words, all messages are stored the same way but displayed in a folder-based organization, each folder containing messages that match a set of filtering criteria.
			

FigureÂ 13.8.Â The Evolution email software

				An extension to Evolution allows integration with a Microsoft Exchange email system; the required package is evolution-ews.
			

### 13.4.2.Â KMail

				The KDE email software can be installed with apt install kmail. KMail only handles email, but it belongs to a software suite called KDE-PIM (for Personal Information Manager) that includes features such as address books, a calendar component, and so on. KMail has all the features one would expect from an excellent email client.
			

FigureÂ 13.9.Â The KMail email software

### 13.4.3.Â Thunderbird

				The thunderbird package provides the email client from the Mozilla software suite. Various localization sets are available in thunderbird-l10n-* packages; the enigmail extension handles message encrypting and signing, but it is not available in all languages.
			

FigureÂ 13.10.Â The Thunderbird email software

[Prev13.3.Â Graphical Desktops](sect.graphical-desktops.en.md)[Up](#)[Home](index.en.md)[Next13.5.Â Web Browsers](sect.web-browsers.en.md)