---
title: "6.4. Theapt-fileCommand"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.apt-file.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.apt-file.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.4. Theapt-fileCommand"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.apt-cache.en.md)The Debian Administrator's Handbook[Next](sect.apt-frontends.en.md)

## 6.4.Â The apt-file Command

			Sometimes we refer to a file or a command and you might wonder, in which package it will be found. Fortunately the Debian repositories not only contain information about all the binary packages provided, but also all the files shipped with them. This information is stored in files named Contents-arch.gz and Contents-udeb-arch.gz. This information is not automatically downloaded by APT. Instead it needs the apt-file update command (from the similar named package) to retrieve the contents of all package sources mentioned in /etc/apt/sources.list. By default it downloads Contents*.pdiff files as described in sidebar [TIP Incremental updates](sect.apt-get.en.md#sidebar.apt-pdiff) to reduce the amount of data necessary to be downloaded. To update the database on a weekly base, the following entry can be added to /etc/crontab if convenient.
		

```bash
@weekly root test -x /usr/bin/apt-file && /usr/bin/apt-file update >> /dev/null 2>&1
```

			After the database has been updated, the command apt-file search pattern will list all packages, which contain a filename or path containing the pattern.
		

```bash
$ apt-file search bin/axi-cache
apt-xapian-index: /usr/bin/axi-cache
```

			The command apt-file list package will list all files shipped with the package instead.
		

TIP Listing a package contents and finding a file's package

			Similar to apt-file list the command dpkg -L package lists all files, but only for an installed package. To find the package, a local file belongs to, use dpkg -S file (see [SectionÂ 5.4.3, âQuerying dpkg's Database and Inspecting .deb Filesâ](sect.manipulating-packages-with-dpkg.en.md#sect.dpkg-list)). To list all local files not belonging to any installed package, you might want to take a look at the cruft or the cruft-ng package.
		[Prev6.3.Â The apt-cache Command](sect.apt-cache.en.md)[Up](#)[Home](index.en.md)[Next6.5.Â Frontends: aptitude, synaptic](sect.apt-frontends.en.md)