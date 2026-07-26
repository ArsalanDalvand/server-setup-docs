---
title: "15.2.2. Simple File Archive"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.building-first-package.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.building-first-package.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "15.2.2. Simple File Archive"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](debian-packaging.en.md)The Debian Administrator's Handbook[Next](sect.setup-apt-package-repository.en.md)

## 15.2.Â Building your First Package

### 15.2.1.Â Meta-Packages or Fake Packages

				Fake packages and meta-packages are similar, in that they are empty shells that only exist for the effects their meta-data have on the package handling stack.
			
				The purpose of a fake package is to trick dpkg and apt into believing that some package is installed even though it is only an empty shell. This allows satisfying dependencies on a package when the corresponding software was installed outside the scope of the packaging system. Such a method works, but it should still be avoided whenever possible, since there is no guarantee that the manually installed software behaves exactly like the corresponding package would and other packages depending on it would not work properly.
			
				On the other hand, a meta-package exists mostly as a collection of dependencies, so that installing the meta-package will actually bring in a set of other packages in a single step.
			
				Both these kinds of packages can be created by the equivs-control and equivs-build commands (in the equivs package). The equivs-control file command creates a Debian package header file that should be edited to contain the name of the expected package, its version number, the name of the maintainer, its dependencies, and its description. Other fields without a default value are optional and can be deleted. The Copyright, Changelog, Readme and Extra-Files fields are not standard fields in Debian packages; they only make sense within the scope of equivs-build, and they will not be kept in the headers of the generated package.
			

ExampleÂ 15.2.Â Header file of the libxml-libxml-perl fake package

```bash
Section: perl
Priority: optional
Standards-Version: 4.5.1

Package: libxml-libxml-perl
Version: 2.0207-1
Maintainer: Raphael Hertzog <hertzog@debian.org>
Depends: libxml2 (>= 2.9.10)
Architecture: all
Description: Fake package - module manually installed in site_perl
 This is a fake package to let the packaging system
 believe that this Debian package is installed.
 .
 In fact, the package is not installed since a newer version
 of the module has been manually compiled &amp; installed in the
 site_perl directory.
```

				The next step is to generate the Debian package with the equivs-build file command. VoilÃ : the package is created in the current directory and it can be handled like any other Debian package would.
			

```bash
$ equivs-build file
equivs-build control 
dpkg-buildpackage: info: source package libxml-libxml-perl
dpkg-buildpackage: info: source version 2.0207-1
dpkg-buildpackage: info: source distribution unstable
dpkg-buildpackage: info: source changed by Raphael Hertzog <hertzog@debian.org>
dpkg-buildpackage: info: host architecture amd64
 dpkg-source --before-build .
 debian/rules clean
dh clean
   dh_clean
 debian/rules binary
dh binary
   dh_update_autotools_config
   dh_autoreconf
   create-stamp debian/debhelper-build-stamp
   dh_prep
   dh_install
   dh_installdocs
   dh_installchangelogs
   dh_perl
   dh_link
   dh_strip_nondeterminism
   dh_compress
   dh_fixperms
   dh_missing
   dh_installdeb
   dh_gencontrol
   dh_md5sums
   dh_builddeb
dpkg-deb: building package 'libxml-libxml-perl' in '../libxml-libxml-perl_2.0207-1_all.deb'.
 dpkg-genbuildinfo --build=binary
 dpkg-genchanges --build=binary >../libxml-libxml-perl_2.0207-1_amd64.changes
dpkg-genchanges: info: binary-only upload (no source code included)
 dpkg-source --after-build .
dpkg-buildpackage: info: binary-only upload (no source included)

The package has been created.
Attention, the package has been created in the current directory,
not in ".." as indicated by the messaige above!
```

### 15.2.2.Â Simple File Archive

				The Falcot Corp administrators need to create a Debian package in order to ease deployment of a set of documents on a large number of machines. The administrator in charge of this task first reads the âNew Maintainer's Guideâ, then starts working on their first package. âÂ [https://www.debian.org/doc/manuals/maint-guide/](https://www.debian.org/doc/manuals/maint-guide/)

				The first step is creating a falcot-data-1.0 directory to contain the target source package. The package will logically, be named falcot-data and bear the 1.0 version number. The administrator then places the document files in a data subdirectory. Then they invoke the dh_make command (from the dh-make package) to add files required by the package generation process, which will all be stored in a debian subdirectory:
			

```bash
$ cd falcot-data-1.0
$ dh_make --native

Type of package: (single, indep, library, python)
[s/i/l/p]? i

Maintainer Name     : Raphael Hertzog
Email-Address       : hertzog@debian.org
Date                : Sat, 26 Feb 2021 13:02:06 +0100
Package Name        : falcot-data
Version             : 1.0
License             : gpl3
Package Type        : indep
Are the details correct? [Y/n/q]
Currently there is not top level Makefile. This may require additional tuning
Done. Please edit the files in the debian/ subdirectory now.

$
```

				The selected type of package (indep) indicates that this source package will generate a single binary package that can be shared across all architectures (Architecture: all in debian/control). single acts as a counterpart, and leads to a single binary package that is dependent on the target architecture (Architecture: any). In this case, the former choice is more relevant since the package only contains documents and no binary programs, so it can be used similarly on computers of all architectures.
			
				The library type corresponds to a source package leading to several binary packages. It is useful for shared libraries, since they need to follow strict packaging rules.
			

TIP Maintainer's name and email address

				Most of the programs involved in package maintenance will look for your name and email address in the DEBFULLNAME and DEBEMAIL or EMAIL environment variables. Defining them once and for all will avoid you having to type them multiple times. If your usual shell is bash, it is a simple matter of adding the following two lines in your ~/.bashrc file (you will obviously replace the values with more relevant ones!):
			

```bash
export EMAIL="hertzog@debian.org"
export DEBFULLNAME="Raphael Hertzog"
```

				The dh_make command created a debian subdirectory with many files. Some are required, in particular rules, control, changelog and copyright. Files with the .ex extension are example files that can be used by modifying them (and removing the extension) when appropriate. When they are not needed, removing them is recommended. The compat file is not used nor created anymore. Instead of defining the debhelper compatibility level as a number in this file, it is now defined as a build-dependency on the debhelper-compat virtual package in the Build-Depends file in debian/control.
			
				The copyright file must contain information about the authors of the documents included in the package, and the related copyright and license. In our case, these are internal documents and their use is restricted to within the Falcot Corp company. The default format used for this file is defined in the Format field. âÂ [https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/](https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/)

				The default changelog file is generally appropriate; replacing the âInitial releaseâ with a more verbose explanation and changing the distribution from UNRELEASED or unstable to the target release name is enough.
			
				The control file must also be updated: the Section field can be changed to misc and the Homepage, Vcs-Git and Vcs-Browser fields were removed. The Depends fields was completed with firefox-esr | www-browser so as to ensure the availability of a web browser able to display the documents in the package. If the package does not require to run any commands as root (see [TOOL fakeroot](debian-packaging.en.md#sidebar.fakeroot)), the Rules-Requires-Root field can be left as is.
			

ExampleÂ 15.3.Â The control file

```bash
Source: falcot-data
Section: misc
Priority: optional
Maintainer: Raphael Hertzog <hertzog@debian.org>
Build-Depends: debhelper-compat (= 13)
Standards-Version: 4.5.1
Rules-Requires-Root: no

Package: falcot-data
Architecture: all
Depends: firefox-esr | www-browser, ${misc:Depends}
Description: Internal Falcot Corp Documentation
 This package provides several documents describing the internal
 structure at Falcot Corp.  This includes:
  - organization diagram
  - contacts for each department.
 .
 These documents MUST NOT leave the company.
 Their use is INTERNAL ONLY.
```

ExampleÂ 15.4.Â The changelog file

```bash
falcot-data (1.0) bullseye; urgency=low

  * Initial Release.
  * Let's start with few documents:
    - internal company structure;
    - contacts for each department.

 -- Raphael Hertzog <hertzog@debian.org>  Sat, 26 Feb 2022 15:12:06 +0100
```

ExampleÂ 15.5.Â The copyright file

```bash
Format: https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
Upstream-Name: falcot-data

Files: *
Copyright: 2004-2021 Falcot Corp
License:
 All rights reserved.
```

BACK TO BASICS Makefile file

				A Makefile file is a script used by the make program; it describes rules for how to build a set of files from each other in a tree of dependencies (for instance, a program can be built from a set of source files). The Makefile file describes these rules in the following format:
			

```bash
target: source1 source2 ...
        command1
        command2
```

				The basic interpretation of such a rule is as follows: if one of the source* files is more recent than the target file, then the target needs to be generated, using command1 and command2.
			
				Note that the command lines must start with a tab character; also note that when a command line starts with a dash character (-), failure of the command does not interrupt the whole process.
			
				The rules file usually contains a set of rules used to configure, build and install the software in a dedicated subdirectory (named after the generated binary package). The contents of this subdirectory is then archived within the Debian package as if it were the root of the filesystem. In our case, files will be installed in the debian/falcot-data/usr/share/falcot-data/ subdirectory, so that installing the generated package will deploy the files under /usr/share/falcot-data/. The rules file is used as a Makefile, with a few standard targets (including clean and binary, used respectively to clean the source directory and generate the binary package).
			
				Although this file is the heart of the process, it increasingly contains only the bare minimum for running a standard set of commands provided by the debhelper tool. Such is the case for files generated by dh_make. To install our files, we simply configure the behavior of the dh_install command by creating the following debian/falcot-data.install file:
			

```bash
data/* usr/share/falcot-data/
```

				At this point, the package can be created. We will, however, add a lick of paint. Since the administrators want the documents to be easily accessed from the menus of graphical desktop environments, we add a falcot-data.desktop file and get it installed in /usr/share/applications by adding a second line to debian/falcot-data.install.
			

ExampleÂ 15.6.Â The falcot-data.desktop file

```bash
[Desktop Entry]
Name=Internal Falcot Corp Documentation
Comment=Starts a browser to read the documentation
Exec=x-www-browser /usr/share/falcot-data/index.html
Terminal=false
Type=Application
Categories=Documentation;
```

				The updated debian/falcot-data.install looks like this:
			

```bash
data/* usr/share/falcot-data/
falcot-data.desktop usr/share/applications/
```

				Our source package is now ready. All that is left to do is to generate the binary package, with the same method we used previously for rebuilding packages: we run the dpkg-buildpackage -us -uc command from within the falcot-data-1.0 directory.
			[PrevChapterÂ 15.Â Creating a Debian Package](debian-packaging.en.md)[Up](#)[Home](index.en.md)[Next15.3.Â Creating a Package Repository for APT](sect.setup-apt-package-repository.en.md)