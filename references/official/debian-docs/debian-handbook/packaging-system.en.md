---
title: "5.1. Structure of a Binary Package"
source: "https://www.debian.org/doc/manuals/debian-handbook/packaging-system.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/packaging-system.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "5.1. Structure of a Binary Package"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.after-first-boot.en.md)The Debian Administrator's Handbook[Next](sect.package-meta-information.en.md)

# ChapterÂ 5.Â Packaging System: Tools and Fundamental Principles

[5.1. Structure of a Binary Package](packaging-system.en.md#sect.binary-package-structure)[5.2. Package Meta-Information](sect.package-meta-information.en.md)[5.2.1. Description: the control File](sect.package-meta-information.en.md#sect.control)[5.2.2. Configuration Scripts](sect.package-meta-information.en.md#sect.configuration-scripts)[5.2.3. Checksums, List of Configuration Files, et al.](sect.package-meta-information.en.md#sect.conffiles)[5.3. Structure of a Source Package](sect.source-package-structure.en.md)[5.3.1. Format](sect.source-package-structure.en.md#id-1.8.7.4)[5.3.2. Usage within Debian](sect.source-package-structure.en.md#id-1.8.7.5)[5.4. Manipulating Packages with dpkg](sect.manipulating-packages-with-dpkg.en.md)[5.4.1. Installing Packages](sect.manipulating-packages-with-dpkg.en.md#id-1.8.8.5)[5.4.2. Package Removal](sect.manipulating-packages-with-dpkg.en.md#id-1.8.8.6)[5.4.3. Querying dpkg's Database and Inspecting .deb Files](sect.manipulating-packages-with-dpkg.en.md#sect.dpkg-list)[5.4.4. dpkg's Log File](sect.manipulating-packages-with-dpkg.en.md#sect.dpkg-log)[5.4.5. Multi-Arch Support](sect.manipulating-packages-with-dpkg.en.md#sect.multi-arch)[5.5. Coexistence with Other Packaging Systems](sect.coexistence-with-other-packaging-systems.en.md)
		As a Debian system administrator, you will routinely handle .deb packages, since they contain consistent functional units (applications, documentation, etc.), whose installation and maintenance they facilitate. It is therefore a good idea to know what they are and how to use them.
	
		This chapter describes the structure and contents of âbinaryâ and âsourceâ packages. The former are files directly usable by dpkg, while the latter contain the source code, as well as instructions for building binary packages.
	

## 5.1.Â Structure of a Binary Package

			The Debian package format is designed so that its content may be extracted on any Unix system that has the classic commands ar, tar, and xz or sometimes gzip or bzip2. This seemingly trivial property is important for portability and disaster recovery.
		
			Imagine, for example, that you mistakenly deleted the dpkg program, and that you could thus no longer install Debian packages. dpkg being a Debian package itself, it would seem your system would be done for... Fortunately, you know the format of a package and can therefore [download](https://www.debian.org/distrib/packages#search_packages) the .deb file of the dpkg package and install it manually (see sidebar [TOOLS dpkg, APT and ar](packaging-system.en.md#sidebar.dpkg-apt-ar)). If by some misfortune one or more of the programs ar, tar or gzip/xz/bzip2 have disappeared, you will only need to copy the missing program from another system (since each of these operates in a completely autonomous manner, without dependencies, a simple copy will suffice). If your system suffered some even more outrageous fortune, and even these don't work (maybe the deepest system libraries are missing?), you should try the static version of busybox (provided in the busybox-static package), which is even more self-contained, and provides subcommands such as busybox ar, busybox tar and busybox xz.
		
In case of a misfortune you better also have a backup of your system (see [SectionÂ 9.10, âBackupâ](sect.backup.en.md)).

TOOLS dpkg, APT and ar

dpkg is the program that handles .deb files (binary packages), notably extracting, analyzing, and unpacking them.
		
APT (the abbreviation of "Advanced Packaging Tool") is a group of programs that allows the execution of higher-level modifications to the system: installing or removing a package (while keeping dependencies satisfied), updating and upgrading the system, listing the available packages, etc.
		
			As for the ar program, provided by the binutils package, it allows handling files of the same name:
		
ar t archive displays the list of files contained in such an archive,
				
ar x archive extracts the files from the archive into the current working directory,
				
ar d archive file deletes a file from the archive, etc.
				
			Its man page ar(1) documents all its other features. ar is a very rudimentary tool that a Unix administrator would only use on rare occasions, but admins routinely use tar, a more evolved archive and file management program. This is why it is easy to restore dpkg in the event of an erroneous deletion. You would only have to download the Debian package and extract the content from the data.tar.xz archive in the system's root (/):
		

```bash
# ar x dpkg_1.20.9_amd64.deb
# tar -C / -p -xJf data.tar.xz
```

BACK TO BASICS Man page notation

			It can be confusing for beginners to find references to âar(1)â in the literature. This is generally a convenient means of referring to the man page entitled ar in section 1.
		
			Sometimes this notation is also used to remove ambiguities, for example to distinguish between the printf command that can also be indicated by printf(1) and the printf function in the C programming language, which can also be referred to as printf(3).
		
[ChapterÂ 7, Solving Problems and Finding Relevant Information](solving-problems.en.md) discusses manual pages in further detail (see [SectionÂ 7.1.1, âManual Pagesâ](solving-problems.en.md#sect.manual-pages)).
		
			Have a look at the content of a .deb file:
		

```bash
$ ar t dpkg_1.20.9_amd64.deb
debian-binary
control.tar.gz
data.tar.xz
$ ar x dpkg_1.20.9_amd64.deb
$ ls
control.tar.gz  data.tar.xz  debian-binary  dpkg_1.20.9_amd64.deb
$ tar tJf data.tar.xz | head -n 16
./
./etc/
./etc/alternatives/
./etc/alternatives/README
./etc/cron.daily/
./etc/cron.daily/dpkg
./etc/dpkg/
./etc/dpkg/dpkg.cfg
./etc/dpkg/dpkg.cfg.d/
./etc/logrotate.d/
./etc/logrotate.d/alternatives
./etc/logrotate.d/dpkg
./sbin/
./sbin/start-stop-daemon
./usr/
./usr/bin/
$ tar tJf control.tar.xz
./
./conffiles
./control
./md5sums
./postrm
$ cat debian-binary
2.0
```

			As you can see, the ar archive of a Debian package is comprised of three files:
		debian-binary
						This is a text file which simply indicates the version of the .deb file package format version. In Debian Bullseye it is still versionÂ 2.0.
					control.tar.xz
						This archive file contains all of the available meta-information, like the name and version of the package as well as some scripts to run before, during or after (un-)installation of it. Some of the meta-information allows package management tools to determine if it is possible to install or uninstall it, for example according to the list of packages already on the machine, and if files shipped have been modified locally.
					data.tar.xz, data.tar.bz2, data.tar.gz
						This archive contains all of the files to be extracted from the package; this is where the executable files, libraries, documentation, etc., are all stored. Packages may use different compression formats, in which case the file will be named differently for xz, bzip2 or gzip.
					[Prev4.3.Â After the First Boot](sect.after-first-boot.en.md)[Up](#)[Home](index.en.md)[Next5.2.Â Package Meta-Information](sect.package-meta-information.en.md)