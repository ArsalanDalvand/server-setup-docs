---
title: "6.3.1. Theapt-cache policyCommand"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.apt-cache.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.apt-cache.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.3.1. Theapt-cache policyCommand"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.apt-get.en.md)The Debian Administrator's Handbook[Next](sect.apt-file.en.md)

## 6.3.Â The apt-cache Command

			The apt-cache command can display much of the information stored in APT's internal database. This information is a sort of cache since it is gathered from the different sources listed in the sources.list file. This happens during the apt update operation.
		

VOCABULARY Cache

			A cache is a temporary storage system used to speed up frequent data access when the usual access method is expensive (performance-wise). This concept can be applied in numerous situations and at different scales, from the core of microprocessors up to high-end storage systems.
		
			In the case of APT, the reference Packages files are those located on Debian mirrors. That said, it would be very ineffective to go through the network for every search that we might want to do in the database of available packages. That is why APT stores a copy of those files (in /var/lib/apt/lists/) and searches are done within those local files. Similarly, /var/cache/apt/archives/ contains a cache of already downloaded packages (see sidebar [GOING FURTHER The cache of .deb files](sect.apt-get.en.md#sidebar.apt-clean)) to avoid downloading them again if you need to reinstall them after a removal.
		
			On the other hand, it is mandatory to run apt update regularly to update the cache. Otherwise your package search results will always miss the latest updates distributed by the Debian mirrors.
		
			The apt-cache command can do keyword-based package searches with apt-cache search keyword. It can also display the headers of the package's available versions with apt-cache show package. This command provides the package's description, its dependencies, the name of its maintainer, etc. Note that apt search, apt show, aptitude search, and aptitude show work in the same way.
		

ALTERNATIVE axi-cache

apt-cache search is a very rudimentary tool, basically implementing grep on package's descriptions. It often returns too many results or none at all when you include too many keywords.
		
axi-cache search term, on the other hand, provides better results, sorted by relevancy. It uses the Xapian search engine and is part of the apt-xapian-index package which indexes all package information (and more, like the .desktop files from all Debian packages). It knows about tags (see sidebar [GOING FURTHER The Tag field](sect.package-meta-information.en.md#sidebar.debtags)) and returns results in a matter of milliseconds.
		

```bash
$ axi-cache search package use::searching
axi-cache search package use::searching
13 results found.
Results 1-13:
100% packagesearch - GUI for searching packages and viewing package information
87% debtags - Debian Package Tags support tools
86% whohas - query multiple distributions' package archives
84% recoll - Personal full text search package
82% apt-forktracer - utility for tracking non-official package versions
[..]
66% apt - commandline package manager
More terms: debian dpkg search debtags whohas forktracer dctrl
More tags: role::program admin::package-management suite::debian works-with::software:package interface::commandline scope::utility protocol::http
```

			Some features are more rarely used. For instance, apt-cache dumpavail displays the headers of all available versions of all packages. apt-cache pkgnames displays the list of all the packages which appear at least once in the cache.
		
			One of the features which might come in handy more often is apt-cache policy, described in the following section.
		

### 6.3.1.Â The apt-cache policy Command

				The apt-cache policy command displays the pinning priorities and distribution properties of each package source as explained in [SectionÂ 6.2.5, âManaging Package Prioritiesâ](sect.apt-get.en.md#sect.apt.priorities). It can also show the pinning priorities for all available versions and sources of a package. For the sources.list example used in [ExampleÂ 6.2, â/etc/apt/sources.list file for users of Debian Stableâ](apt.en.md#example.stable-sources-list) and APT::Default-Release set to "bullseye", the output will look like this:
			

```bash
$ apt-cache policy
Package files:
 100 /var/lib/dpkg/status
     release a=now
 100 https://deb.debian.org/debian bullseye-backports/main amd64 Packages
     release o=Debian Backports,a=bullseye-backports,n=bullseye-backports,l=Debian Backports,c=main,b=amd64
     origin deb.debian.org
 990 https://deb.debian.org/debian bullseye/non-free amd64 Packages
     release v=11.0,o=Debian,a=stable,n=bullseye,l=Debian,c=non-free,b=amd64
     origin deb.debian.org
 990 https://deb.debian.org/debian bullseye/contrib amd64 Packages
     release v=11.0,o=Debian,a=stable,n=bullseye,l=Debian,c=contrib,b=amd64
     origin deb.debian.org
 990 https://deb.debian.org/debian bullseye/main amd64 Packages
     release v=11.0,o=Debian,a=stable,n=bullseye,l=Debian,c=main,b=amd64
     origin deb.debian.org
 500 http://security.debian.org bullseye-security/main amd64 Packages
     release v=11,o=Debian,a=stable-security,n=bullseye-security,l=Debian-Security,c=main,b=amd64
     origin security.debian.org
Pinned packages:
```

apt-cache policy can also show the pinning priorities for all available versions and sources of a given package.
			

```bash
$ apt-cache policy limnoria
limnoria:
  Installed: 2021.06.15-1
  Candidate: 2021.06.15-1
  Version table:
     2021.07.21-1~bpo11+1 100
        100 https://deb.debian.org/debian bullseye-backports/main amd64 Packages
 *** 2021.06.15-1 990
        990 https://deb.debian.org/debian bullseye/main amd64 Packages
        100 /var/lib/dpkg/status
```

				Although there is a newer version of limnoria in the bullseye-backports repository, APT will not install it automatically based on the priority. One would have to use apt install limnoria/bullseye-backports or add a higher pinning priority to /etc/apt/preferences.d/limnoria.pref:
			

```bash
Package: limnoria
Pin: release o=Debian Backports, a=bullseye-backports
Pin-Priority: 1001
```

```bash
$ apt-cache policy limnoria
limnoria:
  Installed: 2021.06.15-1
  Candidate: 2021.07.21-1~bpo11+1
  Version table:
     2021.07.21-1~bpo11+1 1001
        100 https://deb.debian.org/debian bullseye-backports/main amd64 Packages
 *** 2021.06.15-1 990
        990 https://deb.debian.org/debian bullseye/main amd64 Packages
        100 /var/lib/dpkg/status
```

[Prev6.2.Â aptitude, apt-get, and apt Commands](sect.apt-get.en.md)[Up](#)[Home](index.en.md)[Next6.4.Â The apt-file Command](sect.apt-file.en.md)