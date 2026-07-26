---
title: "A.3. Document format"
source: "https://www.debian.org/doc/manuals/debian-reference/apa.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-reference/apa.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-reference
aliases:
  - "A.3. Document format"
---
# AppendixÂ A.Â Appendix

Table of Contents

[A.1. The Debian maze](apa.en.md#_the_debian_maze)

[A.2. Copyright history](apa.en.md#_copyright_history)

[A.3. Document format](apa.en.md#_document_format)

Here are backgrounds of this document.

## A.1.Â The Debian maze

The Linux system is a very powerful computing platform for a networked computer.  However, learning how to use all its capabilities is not easy. Setting up the LPR printer queue with a non-PostScript printer was a good example of stumble points. (There are no issues anymore since newer installations use the new CUPS system.)

There is a complete, detailed map called the "SOURCE CODE".  This is very accurate but very hard to understand.  There are also references called HOWTO and mini-HOWTO.  They are easier to understand but tend to give too much detail and lose the big picture.  I sometimes have a problem finding the right section in a long HOWTO when I need a few commands to invoke.

I hope this "Debian Reference (version 2.143)" (2026-07-10 14:59:15 UTC) provides a good starting direction for people in the Debian maze.

## A.2.Â Copyright history

The Debian Reference was initiated by me, Osamu Aoki <osamu at debian dot org>, as a personal system administration memo. Many contents came from the knowledge I gained from [the debian-user mailing list](https://lists.debian.org/debian-user/) and other Debian resources.

Following a suggestion from Josip Rodin, who was very active with the [Debian Documentation Project (DDP)](https://www.debian.org/doc/ddp), "Debian Reference (version 1, 2001-2007)" was created as a part of DDP documents.

After 6 years, I realized that the original "Debian Reference (version 1)" was outdated and started to rewrite many contents.  New "Debian Reference (version 2)" is released in 2008.

I have updated "Debian Reference (version 2)" to address new topics (Systemd, Wayland, IMAP, PipeWire, Linux kernel 5.10) and removed outdated topics (SysV init, CVS, Subversion, SSH protocol 1, Linux kernels before 2.5).  References to Jessie 8 (2015-2020) release situation or older are mostly removed.

This "Debian Reference (version 2.143)" (2026-07-10 14:59:15 UTC) covers mostly Trixie (=stable) and Forky (=testing) Debian releases.

The tutorial contents can trace its origin and its inspiration in followings.

"[Linux User's Guide](https://tldp.org/pub/Linux/docs/ldp-archived/users-guide/)" by Larry Greenfield (December 1996)

obsoleted by "Debian Tutorial"

"Debian Tutorial" by Havoc Pennington. (11 December, 1998)

partially written by Oliver Elphick, Ole Tetlie, James Treacy, Craig Sawyer, and Ivan E. Moore II

obsoleted by "Debian GNU/Linux: Guide to Installation and Usage"

"[Debian GNU/Linux: Guide to Installation and Usage](https://www.gutenberg.org/files/6527/6527-h/6527-h.htm)" by John Goerzen and Ossama Othman (1999)

obsoleted by "Debian Reference (version 1)"

The package and archive description can trace some of their origin and their inspiration in following.

"[Debian FAQ](https://www.debian.org/doc/manuals/debian-faq/)" (March 2002 version, when this was maintained by Josip Rodin)

The other contents can trace some of their origin and their inspiration in following.

"Debian Reference (version 1)" by Osamu Aoki (2001â2007)

obsoleted by the newer "Debian Reference (version 2)" in 2008.

The previous "Debian Reference (version 1)" was created with many contributors.

the major contents contribution on network configuration topics by Thomas Hood

significant contents contribution on X and VCS related topics by Brian Nelson

the help on the build scripts and many content corrections by Jens Seidel

extensive proofreading by David Sewell

many contributions by the translators, contributors, and bug reporters

Many manual pages and info pages on the Debian system as well as upstream web pages and [Wikipedia](https://www.wikipedia.org/) documents were used as the primary references to write this document.  To the extent Osamu Aoki considered within the [fair use](https://en.wikipedia.org/wiki/Fair_use), many parts of them, especially command definitions, were used as phrase pieces after careful editorial efforts to fit them into the style and the objective of this document.

The gdb debugger description was expanded using [Debian wiki contents on backtrace](https://wiki.debian.org/HowToGetABacktrace) with consent by Ari Pollak, LoÃ¯c Minier, and Dafydd Harries.

Contents of the current "Debian Reference (version 2.143)" (2026-07-10 14:59:15 UTC) are mostly my own work except as mentioned above.  These has been updated by the contributors too.

The author, Osamu Aoki, thanks all those who helped make this document possible.

## A.3.Â Document format

The source of the English original document is currently written in [DocBook](https://en.wikipedia.org/wiki/DocBook) XML files.  This Docbook XML source are converted to HTML, plain text, PostScript, and PDF. (Some formats may be skipped for distribution.)