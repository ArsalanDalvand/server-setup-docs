---
title: "2. apt-get"
source: "https://www.debian.org/doc/manuals/apt-guide/ch2.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "apt-guide/ch2.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - apt-guide
aliases:
  - "2. apt-get"
---
# ChapterÂ 2.Â apt-get

apt-get provides a simple way to install packages from the
command line. Unlike dpkg, apt-get does
not understand .deb files, it works with the package's proper name and can only
install .deb archives from a Source.

The first [[1]](#ftn.id1341) thing that should be done before using apt-get
is to fetch the package lists from the Sources so that it
knows what packages are available. This is done with apt-get
update. For instance,

```bash
# apt-get update
Get http://ftp.de.debian.org/debian/ stable/binary-i386/ Packages
Get http://llug.sep.bnl.gov/debian/ testing/contrib Packages
Reading Package Lists... Done
Building Dependency Tree... Done
```

Once updated there are several commands that can be used:

upgrade

Upgrade will attempt to gently upgrade the whole system. Upgrade will never
install a new package or remove an existing package, nor will it ever upgrade a
package that might cause some other package to break. This can be used daily
to relatively safely upgrade the system. Upgrade will list all of the packages
that it could not upgrade, this usually means that they depend on new packages
or conflict with some other package. dselect or
apt-get install can be used to force these packages to
install.

install

Install is used to install packages by name. The package is automatically
fetched and installed. This can be useful if you already know the name of the
package to install and do not want to go into a GUI to select it. Any number
of packages may be passed to install, they will all be fetched. Install
automatically attempts to resolve dependency problems with the listed packages
and will print a summary and ask for confirmation if anything other than its
arguments are changed.

dist-upgrade

Dist-upgrade is a complete upgrader designed to simplify upgrading between
releases of Debian. It uses a sophisticated algorithm to determine the best
set of packages to install, upgrade and remove to get as much of the system to
the newest release. In some situations it may be desired to use dist-upgrade
rather than spend the time manually resolving dependencies in
dselect. Once dist-upgrade has completed then
dselect can be used to install any packages that may have
been left out.

It is important to closely look at what dist-upgrade is going to do, its
decisions may sometimes be quite surprising.

apt-get has several command line options that are detailed
in its man page,
apt-get(8). The
most useful option is -d which does not install the
fetched files. If the system has to download a large number of package it
would be undesired to start installing them in case something goes wrong. When
-d is used the downloaded archives can be installed by
simply running the command that caused them to be downloaded again without
-d.

[[1]](#id1341) If you are using an http proxy server you must set
the http_proxy environment variable first, see sources.list(5)