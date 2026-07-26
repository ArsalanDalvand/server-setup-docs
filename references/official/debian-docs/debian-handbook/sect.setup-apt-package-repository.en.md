---
title: "15.3. Creating a Package Repository for APT"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.setup-apt-package-repository.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.setup-apt-package-repository.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "15.3. Creating a Package Repository for APT"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.building-first-package.en.md)The Debian Administrator's Handbook[Next](sect.becoming-package-maintainer.en.md)

## 15.3.Â Creating a Package Repository for APT

			Falcot Corp gradually started maintaining a number of Debian packages either locally modified from existing packages or created from scratch to distribute internal data and programs.
		
			To make deployment easier, they want to integrate these packages in a package archive that can be directly used by APT. For obvious maintenance reasons, they wish to separate internal packages from locally-rebuilt packages. The goal is for the matching entries in a /etc/apt/sources.list.d/falcot.list file to be as follows:
		

```bash
deb http://packages.falcot.com/ updates/
deb http://packages.falcot.com/ internal/
```

			The administrators therefore configure a virtual host on their internal HTTP server, with /srv/vhosts/packages/ as the root of the associated web space. The management of the archive itself is delegated to the mini-dinstall command (in the similarly-named package). This tool keeps an eye on an incoming/ directory (in our case, /srv/vhosts/packages/mini-dinstall/incoming/) and waits for new packages there; when a package is uploaded, it is installed into a Debian archive at /srv/vhosts/packages/. The mini-dinstall command reads the *.changes file created when the Debian package is generated. These files contain a list of all other files associated with the version of the package (*.deb, *.dsc, *.diff.gz/*.debian.tar.gz, *.orig.tar.gz, or their equivalents with other compression tools), and these allow mini-dinstall to know which files to install. *.changes files also contain the name of the target distribution (often unstable) mentioned in the latest debian/changelog entry, and mini-dinstall uses this information to decide where the package should be installed. This is why administrators must always change this field before building a package, and set it to internal or updates, depending on the target location. mini-dinstall then generates the files required by APT, such as Packages.gz.
		

ALTERNATIVE apt-ftparchive, aptly, and reprepro

			If mini-dinstall seems too complex for your Debian archive needs, you can also use the apt-ftparchive command. This tool scans the contents of a directory and displays (on its standard output) a matching Packages file. In the Falcot Corp case, administrators could upload the packages directly into /srv/vhosts/packages/updates/ or /srv/vhosts/packages/internal/, then run the following commands to create the Packages.gz files:
		

```bash
$ cd /srv/vhosts/packages
$ apt-ftparchive packages updates >updates/Packages
$ gzip updates/Packages
$ apt-ftparchive packages internal >internal/Packages
$ gzip internal/Packages
```

			The apt-ftparchive sources command allows creating Sources.gz files in a similar fashion.
		
reprepro and aptly are more advanced tools for the same purpose. They can produce, manage and synchronize a number of package repositories, mirror other repositories, even cherry-pick packages from official repositories and provide them locally, create signatures of the generated package indices and thus provide signed repositories. reprepro stores packages and checksums in a Berkeley DB database file, so no database server is needed. aptly provides the same functionality, but it can be easier in handling. Unfortunately, both projects, although probably the most feature-rich solutions, are not actively developed at the moment.
		
reprepro comes with a lot of documentation and examples in /usr/share/doc/reprepro/, while aptly has a vast online documentation. âÂ [https://www.aptly.info/](https://www.aptly.info/)

			Configuring mini-dinstall requires setting up a ~/.mini-dinstall.conf file; in the Falcot Corp case, the contents are as follows:
		

```bash
[DEFAULT]
archive_style = flat
archivedir = /srv/vhosts/packages

verify_sigs = 0
mail_to = admin@falcot.com

generate_release = 1
release_origin = Falcot Corp
release_codename = stable

[updates]
release_label = Recompiled Debian Packages

[internal]
release_label = Internal Packages
```

			One decision worth noting is the generation of Release files for each archive. This can help manage package installation priorities using the /etc/apt/preferences configuration file (see [SectionÂ 6.2.5, âManaging Package Prioritiesâ](sect.apt-get.en.md#sect.apt.priorities) for details).
		

SECURITY mini-dinstall and permissions

			Since mini-dinstall has been designed to run as a regular user, there is no need to run it as root. The easiest way is to configure everything within the user account belonging to the administrator in charge of creating the Debian packages. Since only this administrator has the required permissions to put files in the incoming/ directory, we can deduce that the administrator authenticated the origin of each package prior to deployment and mini-dinstall does not need to do it again. This explains the verify_sigs = 0 parameter (which means that signatures need not be verified). However, if the contents of packages are sensitive, we can reverse the setting and elect to authenticate with a keyring containing the public keys of persons allowed to create packages (configured with the extra_keyrings parameter); mini-dinstall will then check the origin of each incoming package by analyzing the signature integrated to the *.changes file.
		
			Invoking mini-dinstall actually starts a daemon in the background. As long as this daemon runs, it will check for new packages in the incoming/ directory every half-hour; when a new package arrives, it will be moved to the archive and the appropriate Packages.gz and Sources.gz files will be regenerated. If running a daemon is a problem, mini-dinstall can also be manually invoked in batch mode (with the -b option) every time a package is uploaded into the incoming/ directory. Other possibilities provided by mini-dinstall are documented in its mini-dinstall(1) manual page.
		

EXTRA Generating a signed archive

			The APT suite checks a chain of cryptographic signatures on the packages it handles before installing them, in order to ensure their authenticity (see [SectionÂ 6.6, âChecking Package Authenticityâ](sect.package-authentication.en.md)). Private APT archives can then be a problem, since the machines using them will keep displaying warnings about unsigned packages. A diligent administrator will therefore integrate private archives with the secure APT mechanism.
		
			To help with this process, mini-dinstall includes a release_signscript configuration option that allows specifying a script to use for generating the signature. A good starting point is the sign-release.sh script provided by the mini-dinstall package in /usr/share/doc/mini-dinstall/examples/; local changes may be relevant.
		[Prev15.2.Â Building your First Package](sect.building-first-package.en.md)[Up](#)[Home](index.en.md)[Next15.4.Â Becoming a Package Maintainer](sect.becoming-package-maintainer.en.md)