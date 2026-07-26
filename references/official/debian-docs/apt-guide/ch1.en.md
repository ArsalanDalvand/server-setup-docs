---
title: "Next"
source: "https://www.debian.org/doc/manuals/apt-guide/ch1.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "apt-guide/ch1.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - apt-guide
aliases:
  - "Next"
---
# ChapterÂ 1.Â General

Table of Contents

[1.1. Anatomy of the Package System](ch1.en.md#s1.1)

The APT package currently contains two sections, the APT
dselect method and the apt-get command
line user interface. Both provide a way to install and remove packages as well
as download new packages from the Internet.

## 1.1.Â Anatomy of the Package System

The Debian packaging system has a large amount of information associated with
each package to help assure that it integrates cleanly and easily into the
system. The most prominent of its features is the dependency system.

The dependency system allows individual programs to make use of shared elements
in the system such as libraries. It simplifies placing infrequently used
portions of a program in separate packages to reduce the number of things the
average user is required to install. Also, it allows for choices in mail
transport agents, X servers and so on.

The first step to understanding the dependency system is to grasp the concept
of a simple dependency. The meaning of a simple dependency is that a package
requires another package to be installed at the same time to work properly.

For instance, mailcrypt is an emacs extension that aids in encrypting email
with GPG. Without GPGP installed mailcrypt is useless, so mailcrypt has a
simple dependency on GPG. Also, because it is an emacs extension it has a
simple dependency on emacs, without emacs it is completely useless.

The other important dependency to understand is a conflicting dependency. It
means that a package, when installed with another package, will not work and
may possibly be extremely harmful to the system. As an example consider a mail
transport agent such as sendmail, exim or qmail. It is not possible to have
two mail transport agents installed because both need to listen to the network
to receive mail. Attempting to install two will seriously damage the system so
all mail transport agents have a conflicting dependency with all other mail
transport agents.

As an added complication there is the possibility for a package to pretend to
be another package. Consider that exim and sendmail for many intents are
identical, they both deliver mail and understand a common interface. Hence,
the package system has a way for them to declare that they are both
mail-transport-agents. So, exim and sendmail both declare that they provide a
mail-transport-agent and other packages that need a mail transport agent depend
on mail-transport-agent. This can add a great deal of confusion when trying to
manually fix packages.

At any given time a single dependency may be met by packages that are already
installed or it may not be. APT attempts to help resolve dependency issues by
providing a number of automatic algorithms that help in selecting packages for
installation.