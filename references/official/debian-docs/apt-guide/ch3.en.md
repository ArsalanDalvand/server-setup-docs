---
title: "3. DSelect"
source: "https://www.debian.org/doc/manuals/apt-guide/ch3.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "apt-guide/ch3.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - apt-guide
aliases:
  - "3. DSelect"
---
# ChapterÂ 3.Â DSelect

The APT dselect method provides the complete
APT system with the dselect package selection
GUI. dselect is used to select the packages to be
installed or removed and APT actually installs them.

To enable the APT method you need to select [A]ccess in
dselect and then choose the APT method. You will be
prompted for a set of Sources which are places to fetch
archives from. These can be remote Internet sites, local Debian mirrors or
CD-ROMs. Each source can provide a fragment of the total Debian archive, APT
will automatically combine them to form a complete set of packages. If you
have a CD-ROM then it is a good idea to specify it first and then specify a
mirror so that you have access to the latest bug fixes. APT will automatically
use packages on your CD-ROM before downloading from the Internet.

```bash
Set up a list of distribution source locations

 Please give the base URL of the debian distribution.
 The access schemes I know about are: http file

 For example:
      file:/mnt/debian,
      https://deb.debian.org/debian,
      http://deb.debian.org/debian,

 URL [http://llug.sep.bnl.gov/debian]:
```

The Sources setup starts by asking for the base of the
Debian archive, defaulting to an HTTP mirror. Next it asks for the distribution
to get.

```bash
Please give the distribution tag to get or a path to the
 package file ending in a /. The distribution
 tags are typically something like: stable unstable testing

 Distribution [stable]:
```

The distribution refers to the Debian version in the archive,
stable refers to the latest released version
and unstable refers to the developmental
version.

```bash
Please give the components to get
 The components are typically something like: main contrib non-free non-free-firmware

 Components [main contrib non-free non-free-firmware]:
```

The components list refers to the list of sub distributions to fetch. The
distribution is split up based on software licenses, main being DFSG free
packages while contrib and non-free contain things that have various
restrictions placed on their use and distribution.

Any number of sources can be added, the setup script will continue to prompt
until you have specified all that you want.

Before starting to use dselect it is necessary to update
the available list by selecting [U]pdate from the menu. This is a superset of
apt-get update that makes the fetched information available
to dselect. [U]pdate must be performed even if
apt-get update has been run before.

You can then go on and make your selections using [S]elect and then perform
the installation using [I]nstall. When using the APT method the [C]onfig and
[R]emove commands have no meaning, the [I]nstall command performs both of
them together.

By default APT will automatically remove the package (.deb) files once they
have been successfully installed. To change this behavior place
Dselect::clean "prompt"; in /etc/apt/apt.conf.