---
title: "15.1.3. Starting the Rebuild"
source: "https://www.debian.org/doc/manuals/debian-handbook/debian-packaging.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/debian-packaging.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "15.1.3. Starting the Rebuild"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.dealing-with-compromised-machine.en.md)The Debian Administrator's Handbook[Next](sect.building-first-package.en.md)

# ChapterÂ 15.Â Creating a Debian Package

[15.1. Rebuilding a Package from its Sources](debian-packaging.en.md#sect.rebuilding-package)[15.1.1. Getting the Sources](debian-packaging.en.md#id-1.18.4.5)[15.1.2. Making Changes](debian-packaging.en.md#id-1.18.4.6)[15.1.3. Starting the Rebuild](debian-packaging.en.md#id-1.18.4.7)[15.2. Building your First Package](sect.building-first-package.en.md)[15.2.1. Meta-Packages or Fake Packages](sect.building-first-package.en.md#id-1.18.5.2)[15.2.2. Simple File Archive](sect.building-first-package.en.md#id-1.18.5.3)[15.3. Creating a Package Repository for APT](sect.setup-apt-package-repository.en.md)[15.4. Becoming a Package Maintainer](sect.becoming-package-maintainer.en.md)[15.4.1. Learning to Make Packages](sect.becoming-package-maintainer.en.md#id-1.18.7.5)[15.4.2. Acceptance Process](sect.becoming-package-maintainer.en.md#id-1.18.7.6)
		It is quite common, for an administrator who has been handling Debian packages in a regular fashion, to eventually feel the need to create their own packages, or to modify an existing package. This chapter aims to answer the most common questions in this field, and provide the required elements to take advantage of the Debian infrastructure in the best way. With any luck, after trying your hand for local packages, you may even feel the need to go further than that and join the Debian project itself!
	

## 15.1.Â Rebuilding a Package from its Sources

			Rebuilding a binary package is required under several sets of circumstances. In some cases, the administrator needs a software feature that requires the software to be compiled from sources, with a particular compilation option; in others, the software as packaged in the installed version of Debian is not recent enough. In the latter case, the administrator will usually build a more recent package taken from a newer version of Debian â such as Testing or even Unstable â so that this new package works in their Stable distribution; this operation is called âbackportingâ. As usual, care should be taken, before undertaking such a task, to check whether it has been done already â a quick look on the Debian Package Tracker for that package will reveal that information. âÂ [https://tracker.debian.org/](https://tracker.debian.org/) 

### 15.1.1.Â Getting the Sources

				Rebuilding a Debian package starts with getting its source code. The easiest way is to use the apt-get source package-name command. This command requires a deb-src line in the /etc/apt/sources.list file, and up-to-date index files (i.e. apt-get update). These conditions should already be met if you followed the instructions from the chapter dealing with APT configuration (see [SectionÂ 6.1, âFilling in the sources.list Fileâ](apt.en.md#sect.apt-sources.list)). Note, however, that you will be downloading the source packages from the Debian version mentioned in the deb-src line.
			
				If you need another version, you may need to download it manually from a Debian mirror or from the web site. This involves fetching two or three files (with extensions *.dsc â for Debian Source Control â *.tar.comp, and sometimes *.diff.gz or *.debian.tar.comp â comp taking one value among gz, bz2 or xz depending on the compression tool in use), then run the dpkg-source -x file.dsc command. If the *.dsc file is directly accessible at a given URL, there is an even simpler way to fetch it all, with the dget URL command. This command (which can be found in the devscripts package) fetches the *.dsc file at the given address, then analyzes its contents, and automatically fetches the file or files referenced within. Once everything has been downloaded, it verifies the integrity of the downloaded source packages using dscverify, and it extracts the source package (unless the -d or --download-only option is used). The Debian keyring is needed, unless the option -u is supplied.
			

### 15.1.2.Â Making Changes

				Let us use the samba package as an example.
			

```bash
$ apt source samba
Reading package lists... Done
NOTICE: 'samba' packaging is maintained in the 'Git' version control system at:
https://salsa.debian.org/samba-team/samba.git
Please use:
git clone https://salsa.debian.org/samba-team/samba.git
to retrieve the latest (possibly unreleased) updates to the package.
Need to get 12.3 MB of source archives.
Get:1 http://security.debian.org/debian-security bullseye-security/main samba 2:4.13.13+dfsg-1~deb11u3 (dsc) [4,514 B]
Get:2 http://security.debian.org/debian-security bullseye-security/main samba 2:4.13.13+dfsg-1~deb11u3 (tar) [11.8 MB]
Get:3 http://security.debian.org/debian-security bullseye-security/main samba 2:4.13.13+dfsg-1~deb11u3 (diff) [468 kB]
Fetched 12.3 MB in 3s (4,582 kB/s)
dpkg-source: info: extracting samba in samba-4.13.13+dfsg
dpkg-source: info: unpacking samba_4.13.13+dfsg.orig.tar.xz
dpkg-source: info: unpacking samba_4.13.13+dfsg-1~deb11u3.debian.tar.xz
dpkg-source: info: using patch list from debian/patches/series
dpkg-source: info: applying 07_private_lib
dpkg-source: info: applying bug_221618_precise-64bit-prototype.patch
dpkg-source: info: applying [...]
```

				The source of the package is now available in a directory named after the source package and its version (samba-4.13.13+dfsg); this is where we'll work on our local changes.
			
				The first thing to do is to change the package version number, so that the rebuilt packages can be distinguished from the original packages provided by Debian. Assuming the current version is 2:4.13.13+dfsg-1~deb11u3, we can create version 2:4.13.13+dfsg-1~deb11u3+falcot1, which clearly indicates the origin of the package. This makes the package version number higher than the one provided by Debian, so that the package will easily install as an update to the original package. Such a change is best effected with the dch command (Debian CHangelog) from the devscripts package.   

```bash
$ cd 4.13.13+dfsg-1~deb11u3
$ dch --local +falcot
```

				 The last command invokes a text editor (sensible-editor â this should be your favorite editor if it is mentioned in the VISUAL or EDITOR environment variables, and the default editor otherwise) to allow documenting the differences brought by this rebuild. This editor shows us that dch really did change the debian/changelog file.
			
				When a change in build options is required, the changes need to be made in debian/rules, which drives the steps in the package build process. In the simplest cases, the lines concerning the initial configuration (./configure â¦) or the actual build ($(MAKE) â¦ or make â¦ or cmake â¦ or â¦) are easy to spot. If these commands are not explicitly called, they are probably a side effect of another explicit command, in which case please refer to their documentation to learn more about how to change the default behavior. With packages using dh, you might need to add an override for the dh_auto_configure or dh_auto_build commands (see their respective manual pages and debhelper(7) for explanations on how to achieve this).
			
				Depending on the local changes to the packages, an update may also be required in the debian/control file, which contains a description of the generated packages. In particular, this file contains Build-Depends lines controlling the list of dependencies that must be fulfilled at package build time. These often refer to versions of packages contained in the distribution the source package comes from, but which may not be available in the distribution used for the rebuild. There is no automated way to determine if a dependency is real or only specified to guarantee that the build should only be attempted with the latest version of a library â this is the only available way to force an autobuilder to use a given package version during build, which is why Debian maintainers frequently use strictly versioned build-dependencies.
			
				If you know for sure that these build-dependencies are too strict, you should feel free to relax them locally. Reading the files which document the standard way of building the software â these files are often called INSTALL â will help you figure out the appropriate dependencies. Ideally, all dependencies should be satisfiable from the distribution used for the rebuild; if they are not, a recursive process starts, whereby the packages mentioned in the Build-Depends field must be backported before the target package can be. Some packages may not need backporting, and can be installed as-is during the build process (a notable example is debhelper). Note that the backporting process can quickly become complex if you are not careful. Therefore, backports should be kept to a strict minimum when possible.
			

TIP Installing Build-Depends

apt-get allows installing all packages mentioned in the Build-Depends fields of a source package available in a distribution mentioned in a deb-src line of the /etc/apt/sources.list file. This is a simple matter of running the apt-get build-dep package command.
			

### 15.1.3.Â Starting the Rebuild

				When all the needed changes have been applied to the sources, we can start generating the actual binary package (.deb file). The whole process is managed by the dpkg-buildpackage command.
			

ExampleÂ 15.1.Â Rebuilding a package

```bash
$ dpkg-buildpackage -us -uc
[...]
```

				The previous command can fail if the Build-Depends field has not been updated, or if the related packages are not installed. In such a case, it is possible to overrule this check by passing the -d option to dpkg-buildpackage. However, explicitly ignoring these dependencies runs the risk of the build process failing at a later stage. Worse, the package may seem to build correctly but fail to run properly: some programs automatically disable some of their features when a required library is not available at build time. The switch can still be very useful if you only want to create a source package, which is supposed to be passed to clean build environments as described in [QUICK LOOK Building packages in chrooted and virtual environments](debian-packaging.en.md#sidebar.schroot).
			
				The other options used in the above example make sure that neither the source package's .dsc (-us) nor the produced .changes file (-uc) get signed with the package builder's cryptographic key after the asuccessful build.
			

TOOL fakeroot

				In essence, the package creation process is a simple matter of gathering in an archive a set of existing (or built) files; most of the files will end up being owned by root in the archive. However, building the whole package under this user would imply increased risks; fortunately, this can be avoided with the fakeroot command. This tool can be used to run a program and give it the impression that it runs as root and creates files with arbitrary ownership and permissions. When the program creates the archive that will become the Debian package, it is tricked into creating an archive containing files marked as belonging to arbitrary owners, including root. This setup is so convenient that dpkg-buildpackage uses fakeroot by default when building packages.
			
				Note that the program is only tricked into âbelievingâ that it operates as a privileged account, and the process actually runs as the user running fakeroot program (and the files are actually created with that user's permissions). At no time does it actually get root privileges that it could abuse.
			
				More often than not, Debian developers use a higher-level program such as debuild; this runs dpkg-buildpackage as usual, but it also adds an invocation of a program that runs many checks to validate the generated package against the Debian policy. This script also cleans up the environment so that local environment variables do not âpolluteâ the package build. The debuild command is one of the tools in the devscripts suite, which share some consistency and configuration to make the maintainers' task easier.
			

QUICK LOOK Building packages in chrooted and virtual environments

				Building a package on an existing system, like a workstation or server, can pose several problems. Maybe the system has packages installed, that conflict with the build requirements. Or maybe it has packages (libraries, headers, etc.) installed, that the project to build is not supposed to use - e.g. from Testing, Unstable, or even from third parties. Also it might have packages installed, which are actually required for the build, but are not listed in debian/control. It is therefore a good practice to build a packages in âcleanâ environments.
			
				The sbuild, pbuilder, or cowbuilder programs (in the similarly named packages) allow building a Debian package in a chrooted environment. Each of them first creates a temporary directory containing the minimal system required for building the package (including the packages mentioned in the Build-Depends field). This directory is then used as the root directory (/), using the chroot command, during the build process.
			
				These tools allow the build process to happen in an environment that is not altered by users' manipulations. This also allows for quick detection of the missing build-dependencies (since the build will fail unless the appropriate dependencies are documented). Finally, it allows building a package for a Debian version that is not the one used by the system as a whole: the machine can be using Stable for its normal workload, and the chrooted environments can be running on the same machine using Unstable for package builds. Using these tools, it is also possible to build for a different distribution, like Ubuntu.
			
schroot, sbuild-shell, or pbuilder allow running a command or a login shell in a chrooted environment.
			
				Sometimes it is not enough to build a package in a chrooted environment. This can be due to building for a different architecture where cross-compilation does not work, or due to the tools not being able to create a chrooted environment for the desired target system. Then one can create virtual machines to build the package there. These setups are usually more complex then the tools mentioned above, and rarely needed, so we will only mention this possibility without going into much detail. Check out [SectionÂ 12.2, âVirtualizationâ](sect.virtualization.en.md) for an introduction into this topic.
			[Prev14.7.Â Dealing with a Compromised Machine](sect.dealing-with-compromised-machine.en.md)[Up](#)[Home](index.en.md)[Next15.2.Â Building your First Package](sect.building-first-package.en.md)