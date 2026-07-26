---
title: "13.5. Web Browsers"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.web-browsers.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.web-browsers.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.5. Web Browsers"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.main-desktop-tools.en.md)The Debian Administrator's Handbook[Next](sect.development.en.md)

## 13.5.Â Web Browsers

			Epiphany, the web browser in the GNOME suite, uses the WebKit display engine developed by Apple for its Safari browser. The relevant package is epiphany-browser.
		
			Konqueror, available in the konqueror package, is KDE's web browser (but can also assume the role of a file manager). It uses the KDE-specific KHTML rendering engine; KHTML is an excellent engine, as witnessed by the fact that Apple's WebKit is based on KHTML. KDE also has a newer browser called Falkon, available in the falkon package. It uses the QtWebEngine.
		
			Users not satisfied by any of the above can use Firefox. This browser, available in the firefox-esr package, uses the Mozilla project's Gecko renderer, with a thin and extensible interface on top.
		

FigureÂ 13.11.Â The Firefox web browser

VOCABULARY Firefox ESR

			Mozilla has a very fast-paced release cycle for Firefox. New releases are published every six to eight weeks and only the latest version is supported for security issues. This doesn't suit all kind of users so, every 10 cycles, they are promoting one of their release to an Extended Support Release (ESR) which will get security updates (and no functional changes) during the next 10 cycles (which covers a bit more than a year).
		
			Debian has both versions packaged. The ESR one, in the package firefox-esr, is used by default since it is the only version suitable for Debian Stable with its long support period (and even there Debian has to upgrade from one ESR release to the next multiple times during a Debian Stable lifecycle). The regular Firefox is available in the firefox package but it is only available to users of Debian Unstable.
		

CULTURE Iceweasel, Firefox and others

			Before Debian Stretch, Firefox and Thunderbird were missing. The iceweasel package contained Iceweasel, which was basically Firefox under another name.
		
			The rationale behind this renaming was a result of the usage rules imposed by the Mozilla Foundation on the Firefoxâ¢ registered trademark: any software named Firefox had to use the official Firefox logo and icons. However, since these elements are not released under a free license, Debian could not distribute them in its main section. Rather than moving the whole browser to non-free, the package maintainer choose to use a different name.
		
			For similar reasons, the Thunderbirdâ¢ email client was renamed to Icedove in a similar fashion.
		
			Nowadays, the logo and icons are distributed under a free software license and Mozilla recognized that the changes made by the Debian project are respecting their trademark license so Debian is again able to ship Mozilla's applications under their official name.
		

CULTURE Mozilla

			Netscape Navigator was the standard browser when the web started reaching the masses, but lost ground when Microsoft bundled Internet Explorer with Windows and signed contracts with computer manufacturers which forbade them from pre-installing Netscape Navigator. Faced with this failure, Netscape (the company) decided to âfreeâ its source code, by releasing it under a free license, to give it a second life. This was the beginning of the Mozilla project. After many years of development, the results are more than satisfying: the Mozilla project brought forth an HTML rendering engine (called Gecko) that is among the most standard-compliant. This rendering engine is in particular used by the Mozilla Firefox browser, which is one of the major browsers.
		
			Last but not least, Debian also contains the Chromium web browser (available in the chromium package). This browser is developed by Google and has become the most popular browser in just a few years. Its clear purpose is to make web services more attractive, both by optimizing the browser for performance and by increasing the user's security. The free code that powers Chromium is also used by its proprietary version called Google Chromeâ¢.
		[Prev13.4.Â Email](sect.main-desktop-tools.en.md)[Up](#)[Home](index.en.md)[Next13.6.Â Development](sect.development.en.md)