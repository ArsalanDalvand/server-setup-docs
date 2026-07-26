---
title: "6.1.6. Caching Proxy for Debian Packages"
source: "https://www.debian.org/doc/manuals/debian-handbook/apt.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/apt.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.1.6. Caching Proxy for Debian Packages"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.coexistence-with-other-packaging-systems.en.md)The Debian Administrator's Handbook[Next](sect.apt-get.en.md)

# ChapterÂ 6.Â Maintenance and Updates: The APT Tools

[6.1. Filling in the sources.list File](apt.en.md#sect.apt-sources.list)[6.1.1. Syntax](apt.en.md#id-1.9.16.6)[6.1.2. Repositories for Stable Users](apt.en.md#sect.apt-sources.list.stable)[6.1.3. Repositories for Testing/Unstable Users](apt.en.md#sect.apt-sources.list.testing)[6.1.4. Using Alternate Mirrors](apt.en.md#sect.apt-sources.list.cdn)[6.1.5. Non-Official Resources: mentors.debian.net](apt.en.md#sect.apt-sources.list.mentors)[6.1.6. Caching Proxy for Debian Packages](apt.en.md#sect.apt-sources.list.cache-proxy)[6.2. aptitude, apt-get, and apt Commands](sect.apt-get.en.md)[6.2.1. Initialization](sect.apt-get.en.md#sect.apt-update)[6.2.2. Installing and Removing](sect.apt-get.en.md#sect.apt.install)[6.2.3. System Upgrade](sect.apt-get.en.md#sect.apt-upgrade)[6.2.4. Configuration Options](sect.apt-get.en.md#sect.apt-config)[6.2.5. Managing Package Priorities](sect.apt-get.en.md#sect.apt.priorities)[6.2.6. Working with Several Distributions](sect.apt-get.en.md#sect.apt-mix-distros)[6.2.7. Tracking Automatically Installed Packages](sect.apt-get.en.md#sect.automatic-tracking)[6.2.8. APT Patterns](sect.apt-get.en.md#sect.apt-patterns)[6.3. The apt-cache Command](sect.apt-cache.en.md)[6.3.1. The apt-cache policy Command](sect.apt-cache.en.md#sect.apt-cache-policy)[6.4. The apt-file Command](sect.apt-file.en.md)[6.5. Frontends: aptitude, synaptic](sect.apt-frontends.en.md)[6.5.1. aptitude](sect.apt-frontends.en.md#sect.aptitude)[6.5.2. synaptic](sect.apt-frontends.en.md#id-1.9.20.7)[6.6. Checking Package Authenticity](sect.package-authentication.en.md)[6.7. Upgrading from One Stable Distribution to the Next](sect.dist-upgrade.en.md)[6.7.1. Recommended Procedure](sect.dist-upgrade.en.md#id-1.9.22.6)[6.7.2. Handling Problems after an Upgrade](sect.dist-upgrade.en.md#sect.dist-upgrade-problems)[6.7.3. Cleaning Up after an Upgrade](sect.dist-upgrade.en.md#id-1.9.22.8)[6.8. Keeping a System Up to Date](sect.regular-upgrades.en.md)[6.9. Automatic Upgrades](sect.automatic-upgrades.en.md)[6.9.1. Configuring dpkg](sect.automatic-upgrades.en.md#id-1.9.24.8)[6.9.2. Configuring APT](sect.automatic-upgrades.en.md#id-1.9.24.9)[6.9.3. Configuring debconf](sect.automatic-upgrades.en.md#id-1.9.24.10)[6.9.4. Handling Command Line Interactions](sect.automatic-upgrades.en.md#id-1.9.24.11)[6.9.5. The Miracle Combination](sect.automatic-upgrades.en.md#id-1.9.24.12)[6.10. Searching for Packages](sect.searching-packages.en.md)
		What makes Debian so popular with administrators is how easily software can be installed and how easily the whole system can be updated. This unique advantage is largely due to the APT program, which Falcot Corp administrators studied with enthusiasm.
	
APT is the abbreviation for Advanced Packaging Tool. What makes this program âadvancedâ is its approach to packages. It doesn't simply evaluate them individually, but it considers them as a whole and produces the best possible combination of packages depending on what is available and compatible according to dependencies.
	

VOCABULARY Package source and source package

		The word source can be ambiguous. A "source package" â a package containing the source code of a program â should not be confused with a "package source" â a repository (website, FTP server, CD-ROM, local directory, etc.) which contains packages.
	
		APT needs to be given a âlist of package sources (repositories)â: the file /etc/apt/sources.list will list the different repositories that publish Debian packages. APT will then import the list of packages published by each of these sources. This operation is achieved by downloading Packages.xz files or a variant such as Packages.gz or .bz2 (using a different compression method) in case of a source of binary packages and by analyzing their contents. In case of a source of source packages, APT downloads Sources.xz files or a variant using a different compression method. When an old copy of these files is already present, APT can update it by only downloading the differences (see sidebar [TIP Incremental updates](sect.apt-get.en.md#sidebar.apt-pdiff)).
	

BACK TO BASICS gzip, bzip2, LZMA and XZ Compression

		A .gz extension refers to a file compressed with the gzip utility. gzip is the fast and efficient traditional Unix utility to compress files. Newer tools achieve better rates of compression but require more resources (computation time and memory) to compress and uncompress a file. Among them, and by order of appearance, there are bzip2 (generating files with a .bz2 extension), lzma (generating .lzma files) and xz (generating .xz files).
	

## 6.1.Â Filling in the sources.list File

### 6.1.1.Â Syntax

				Each active line in the /etc/apt/sources.list file represents a package source (repository) and is made of at least three parts separated by spaces. For a complete description of the file format and the accepted entry compositions see sources.list(5).
			

ExampleÂ 6.1.Â Example entry format in /etc/apt/sources.list

```bash
deb url distribution component1 component2 component3 [..] componentX
deb-src url distribution component1 component2 component3 [..] componentX
```

				The first field indicates the source type:
			deb
							package source (repository) of binary packages
						deb-src
							package source (repository) of source packages
						
				The second field gives the base URL of the source. Combined with the filenames listed in the Packages.xz files, it must give a full and valid URL. This can consist in a Debian mirror or in any other package archive set up by a third party. The URL can start with file:// to indicate a local source installed in the system's file hierarchy, with http:// or https:// to indicate a source accessible from a web server, or with ftp:// or ftps:// for a source available on an FTP server. The URL can also start with cdrom: for CD-ROM/DVD/Blu-ray disc based installations, although this is less frequent, since network-based installation methods are eventually more common. More methods like ssh:// or tor+http(s):// are supported and are either described in sources.list(5) or their respective apt-transport-method package documentation.
			
				The syntax of the last field depends on the structure of the repository. In the simplest case, you can simply indicate a subdirectory (with a required trailing slash) of the desired source. This is often a simple â./â which refers to the absence of a subdirectory. The packages are then directly at the specified URL. But in the most common case, the repositories will be structured like a Debian mirror, with multiple distributions, each having multiple components. In those cases, name the chosen distribution by its âcodenameâ â see the list in sidebar [COMMUNITY Bruce Perens, a controversial leader](sect.foundation-documents.en.md#sidebar.bruce-perens) â or by the corresponding âsuiteâ (oldoldstable, oldstable, stable, testing, unstable) and then the components to enable. A typical Debian mirror provides the components main, contrib, and non-free.
			

VOCABULARY The main, contrib and non-free archives

				Debian uses three components to differentiate packages according to the licenses chosen by the authors of each work. Main gathers all packages which fully comply with the [Debian Free Software Guidelines](sect.foundation-documents.en.md#sect.dfsg).
			
				The non-free component is different because it contains software which does not (entirely) conform to these principles but which can, nevertheless, be distributed without restrictions. This archive, which is not officially part of Debian, is a service for users who could need some of those programs and, nowadays, also require the firmware for their hardware. However, Debian always recommends giving priority to free software. The existence of this component represents a considerable problem for Richard M. Stallman and keeps the Free Software Foundation from recommending Debian to users.
			
Contrib (contributions) is a set of open source software which cannot function without some non-free elements â these elements can be software from the non-free section, or non-free files such as game ROMs, BIOS of consoles, etc. â or some elements, not available from the Debian main archive at all. The contrib component also includes free software whose compilation requires proprietary elements. This was initially the case for the OpenOffice.org office suite, which used to require a proprietary Java environment.
			

TIP Files in /etc/apt/sources.list.d/

				If many package sources are referenced, it can be useful to split them in multiple files. Each part is then stored in /etc/apt/sources.list.d/filename.list (see sidebar [BACK TO BASICS Directories ending in .d](sect.apt-get.en.md#sidebar.directory.d)).
			
				The cdrom entries describe the CD/DVD-ROMs you have. Contrary to other entries, a CD-ROM is not always available since it has to be inserted into the drive and since only one disc can be read at a time. For those reasons, these sources are managed in a slightly different way, and need to be added with the apt-cdrom program, usually executed with the add parameter. The latter will then request the disc to be inserted in the drive and will browse its contents looking for Packages files. It will use these files to update its database of available packages (this operation is usually done by the apt update command). From then on, APT can require the disc to be inserted if it needs one of its packages.
			

### 6.1.2.Â Repositories for Stable Users

				Here is a standard sources.list for a system running the Stable version of Debian:
			

ExampleÂ 6.2.Â /etc/apt/sources.list file for users of Debian Stable

```bash
# Security updates
deb http://security.debian.org/ bullseye-security main contrib non-free
deb-src http://security.debian.org/ bullseye-security main contrib non-free

## Debian mirror

# Base repository
deb https://deb.debian.org/debian bullseye main contrib non-free
deb-src https://deb.debian.org/debian bullseye main contrib non-free

# Stable updates
deb https://deb.debian.org/debian bullseye-updates main contrib non-free
deb-src https://deb.debian.org/debian bullseye-updates main contrib non-free

# Stable backports
deb https://deb.debian.org/debian bullseye-backports main contrib non-free
deb-src https://deb.debian.org/debian bullseye-backports main contrib non-free
```

				This file lists all sources of packages associated with the Bullseye version of Debian (the current Stable suite as of this writing). In the example above, we opted to name âbullseyeâ explicitly instead of using the corresponding âstableâ aliases (stable, stable-updates, stable-backports) because we don't want to have the underlying distribution changed outside of our control when the next stable release comes out.
			
				Most packages will come from the âbase repositoryâ, which contains all packages but is seldom updated (about once every 2 months for a âpoint releaseâ). The other repositories are partial (they do not contain all packages) and can host updates (packages with newer version) that APT might install. The following sections will explain the purpose and the rules governing each of those repositories.
			
				Note that when the desired version of a package is available on several repositories, the first one listed in the sources.list file will be used. For this reason, non-official sources are usually added at the end of the file.
			
				As a side note, most of what this section says about Stable applies equally well to Oldstable since the latter is just an older Stable that is maintained in parallel.
			

#### 6.1.2.1.Â Security Updates

					Debian takes security seriously. Known software vulnerabilities in Debian are tracked in the [Security Bug Tracker](https://security-tracker.debian.org) and usually get fixed in a reasonable timeframe. The security updates are not hosted on the usual network of Debian mirrors but on security.debian.org, a small set of machines maintained by the [Debian System Administrators](sect.debian-internals.en.md#dsa-team). This archive contains security updates prepared by the Debian Security Team and/or by package maintainers for the Stable and Oldstable distribution.
				
					The server can also host security updates for Testing but this doesn't happen very often since those updates tend to reach that suite via the regular flow of updates coming from Unstable.
				
					For serious issues, the security team issues a Debian Security Advisory (DSA) and announces it together with the security update on the [debian-security-announce@lists.debian.org](mailto:debian-security-announce@lists.debian.org) mailing list ([archive](https://lists.debian.org/debian-security-announce/)).
				

#### 6.1.2.2.Â Stable Updates

					Stable updates are not security sensitive but are deemed important enough to be pushed to users before the next stable point release.
				
					This repository will typically contain fixes for critical and serious bugs which could not be fixed before release or which have been introduced by subsequent updates. Depending on the urgency, it can also contain updates for packages that have to evolve over time, like spamassassin's spam detection rules, clamav's virus database, the daylight-saving time rules of all timezones (tzdata), the ESR version of Firefox (firefox-esr) or cryptographic keyrings like debian-archive-keyring.
				
					In practice, this repository is a subset of the proposed-updates repository, carefully selected by the [Stable Release Managers](sect.release-lifecycle.en.md#srm-team). All updates are announced on the [debian-stable-announce@lists.debian.org](mailto:debian-stable-announce@lists.debian.org) mailing list ([archive](https://lists.debian.org/debian-stable-announce/)) and will be included in the next Stable point release anyway.
				

#### 6.1.2.3.Â Proposed Updates

					Once published, the Stable distribution is only updated about once every 2 months. The proposed-updates repository is where the expected updates are prepared (under the supervision of the Stable Release Managers).
				
					The security and stable updates documented in the former sections are always included in this repository, but there is more too, because package maintainers also have the opportunity to fix important bugs that do not deserve an immediate release.
				
					Anyone can use this repository to test those updates before their official publication. The extract below uses the bullseye-proposed-updates alias which is both more explicit and more consistent since buster-proposed-updates also exists (for the Oldstable updates):
				

```bash
deb https://deb.debian.org/debian bullseye-proposed-updates main contrib non-free
```

#### 6.1.2.4.Â Stable Backports

					The stable-backports repository hosts âpackage backportsâ. The term refers to a package of some recent software which has been recompiled for an older distribution, generally for Stable.
				
					When the distribution becomes a little dated, numerous software projects have released new versions that are not integrated into the current Stable suite, which is only modified to address the most critical problems, such as security issues. Since the Testing and Unstable suites can be more risky, package maintainers sometimes voluntarily offer recompilations of recent software applications for Stable, which has the advantage to users and system administrators to limit potential instability to a small number of chosen packages. The page [https://backports.debian.org](https://backports.debian.org) provides more information.
				
					Backports from stable-backports are only created from packages available in Testing. This ensures that all installed backports will be upgradable to the corresponding stable version once the next stable release of Debian is available.
				
					Even though this repository provides newer versions of packages, APT will not install them unless you give explicit instructions to do so (or unless you have already done so with a former version of the given backport):
				

```bash
$ sudo apt-get install package/bullseye-backports
$ sudo apt-get install -t bullseye-backports package
```

### 6.1.3.Â Repositories for Testing/Unstable Users

				Here is a standard sources.list for a system running the Testing or Unstable version of Debian:
			

ExampleÂ 6.3.Â /etc/apt/sources.list file for users of Debian Testing/Unstable

```bash
# Unstable
deb https://deb.debian.org/debian unstable main contrib non-free
deb-src https://deb.debian.org/debian unstable main contrib non-free

# Testing
deb https://deb.debian.org/debian testing main contrib non-free
deb-src https://deb.debian.org/debian testing main contrib non-free

# Testing security updates
deb http://security.debian.org/ testing-security main contrib non-free
deb-src http://security.debian.org/ testing-security main contrib non-free

# Stable
deb https://deb.debian.org/debian stable main contrib non-free
deb-src https://deb.debian.org/debian stable main contrib non-free

# Stable security updates
deb http://security.debian.org/ stable-security main contrib non-free
deb-src http://security.debian.org/ stable-security main contrib non-free
```

NOTE Layout of security repositories

				Starting with Debian 11 Bullseye, the codename of the repository providing security updates has been renamed from codename/updates into codename-security to avoid the confusion with codename-updates (see [SectionÂ 6.1.2.2, âStable Updatesâ](apt.en.md#sect.apt-sources.list.stable.updates)). âÂ [https://www.debian.org/releases/bullseye/amd64/release-notes/ch-information.en.html#security-archive](https://www.debian.org/releases/bullseye/amd64/release-notes/ch-information.en.html#security-archive)

				With this sources.list file APT will install packages from the Unstable suite. If that is not desired, use the APT::Default-Release setting (see [SectionÂ 6.2.3, âSystem Upgradeâ](sect.apt-get.en.md#sect.apt-upgrade)) to instruct APT to pick packages from another suite (most likely Testing in this case).
			
				There are good reasons to include all those repositories, even though a single one should be enough. Testing users will appreciate the possibility to cherry-pick a fixed package from Unstable when the version in Testing is affected by an annoying bug. On the other hand, Unstable users bitten by unexpected regressions have the possibility to downgrade packages to their (supposedly working) Testing version.
			
				The inclusion of Stable is more debatable but it often gives access to some packages, which have been removed from the development versions. It also ensures that you get the latest updates for packages, which have not been modified since the last stable release.
			

#### 6.1.3.1.Â The Experimental Repository

					The archive of Experimental packages is present on all Debian mirrors, and contains packages which are not in the Unstable version yet because of their substandard quality â they are often software development versions or pre-versions (alpha, beta, release candidateâ¦). A package can also be sent there after undergoing subsequent changes which can generate problems. The maintainer then tries to uncover them with help from advanced users who can handle important issues. After this first stage, the package is moved into Unstable, where it reaches a much larger audience and where it will be tested in much more detail.
				
Experimental is generally used by users who do not mind breaking their system and then repairing it. This distribution gives the possibility to import a package which a user wants to try or use as the need arises. That is exactly how Debian approaches it, since adding it in APT's sources.list file does not lead to the systematic use of its packages. The line to be added is:
				

```bash
deb https://deb.debian.org/debian experimental main contrib non-free
```

### 6.1.4.Â Using Alternate Mirrors

				The sources.list examples in this chapter refer to package repositories hosted on deb.debian.org. Those URLs will redirect you to servers which are close to you and which are managed by Content Delivery Networks (CDN) whose main role is to store multiple copies of the files across the world, and to deliver them as fast as possible to users. The CDN companies that Debian is working with are Debian partners who are offering their services freely to Debian. While none of those servers are under direct control of Debian, the fact that the whole archive is sealed by GPG signatures makes it a non-issue. âÂ [https://deb.debian.org/](https://deb.debian.org/)

				Picky users who are not satisfied with the performance of deb.debian.org can try to find a better mirror in the official mirror list: âÂ [https://www.debian.org/mirror/list](https://www.debian.org/mirror/list)

				But when you don't know which mirror is best for you, this list is of not much use. Fortunately for you, Debian maintains DNS entries of the form ftp.country-code.debian.org (e.g. ftp.us.debian.org for the USA, ftp.fr.debian.org for France, etc.) which are covering many countries and which are pointing to one (or more) of the best mirrors available within that country.
			
				As an alternative to deb.debian.org, there used to be httpredir.debian.org. This service would identify a mirror close to you (among the list of official mirrors, using GeoIP mainly) and would redirect APT's requests to that mirror. This service has been deprecated due to reliability concerns and now httpredir.debian.org provides the same CDN-based service as deb.debian.org.
			

### 6.1.5.Â Non-Official Resources: mentors.debian.net

				There are numerous non-official sources of Debian packages set up by advanced users who have recompiled some software â Ubuntu made this popular with their Personal Package Archive (PPA) service â by programmers who make their creation available to all, and even by Debian developers who offer pre-versions of their package online.
			
				The [mentors.debian.net](https://mentors.debian.net) site is interesting (although it only provides source packages), since it gathers packages created by candidates to the status of official Debian developer or by volunteers who wish to create Debian packages without going through that process of integration. These packages are made available without any guarantee regarding their quality; make sure that you check their origin and integrity and then test them before you consider using them in production.
			

COMMUNITY The debian.net sites

				The debian.net domain is not an official resource of the Debian project. Each Debian developer may use that domain name for their own use. These websites can contain unofficial services (sometimes personal sites) hosted on a machine which does not belong to the project and set up by Debian developers, or even prototypes about to be moved on to debian.org. Two reasons can explain why some of these prototypes remain on debian.net: either no one has made the necessary effort to transform it into an official service (hosted on the debian.org domain, and with a certain guarantee of maintenance), or the service is too controversial to be officialized.
			
				Installing a package means giving root rights to its creator, because they decide on the contents of the initialization scripts which are run under that identity. Official Debian packages are created by volunteers who have been co-opted and reviewed and who can seal their packages so that their origin and integrity can be checked.
			
				In general, be wary of a package whose origin you don't know and which isn't hosted on one of the official Debian servers: evaluate the degree to which you can trust the creator, and check the integrity of the package.
			

GOING FURTHER Old package versions: snapshot.debian.org and archive.debian.org

				The Debian project frequently releases new stable versions of their distribution, rendering old releases eventually outdated and unsupported (see [SectionÂ 1.6, âLifecycle of a Releaseâ](sect.release-lifecycle.en.md)). The repositories belonging to these releases are then removed from the official mirrors, but can still be accessed using the service at [archive.debian.org](https://www.debian.org/distrib/archive.en). Note, that this service allows access to any of the old Debian releases, but does not provide the source package for every binary package offered.
			
				The [snapshot.debian.org](https://snapshot.debian.org) service, introduced in April 2010, can be used to âgo backwards in timeâ and to find an old binary or source version of a package not longer contained in the Debian archives. It can be used, for example, to identify which version of a package introduced a regression, and more concretely, to come back to the former version while waiting for the regression fix. This service does not only offer source and binary packages from older releases, but also versions superseeded by regular maintainer uploads.
			

### 6.1.6.Â Caching Proxy for Debian Packages

				When an entire network of machines is configured to use the same remote server to download the same updated packages, any administrator knows that it would be beneficial to have an intermediate proxy acting as a network-local cache (see sidebar [VOCABULARY Cache](sect.apt-cache.en.md#sidebar.cache)).
			
				You can configure APT to use a "standard" proxy (see [SectionÂ 6.2.4, âConfiguration Optionsâ](sect.apt-get.en.md#sect.apt-config) for the APT side, and [SectionÂ 11.6, âHTTP/FTP Proxyâ](sect.http-ftp-proxy.en.md) for the proxy side), but the Debian ecosystem offers better options to solve this problem. The dedicated software presented in this section are smarter than a plain proxy cache because they can rely on the specific structure of APT repositories (for instance they know when individual files are obsolete or not, and thus adjust the time during which they are kept).
			
apt-cacher and apt-cacher-ng work like usual proxy cache servers. APT's sources.list is left unchanged, but APT is configured to use them as proxy for outgoing requests.
			
approx, on the other hand, acts like an HTTP server that âmirrorsâ any number of remote repositories in its top-level URLs. The mapping between those top-level directories and the remote URLs of the repositories is stored in /etc/approx/approx.conf:
			

```bash
# <name>   <repository-base-url>
debian     https://deb.debian.org/debian
security   http://security.debian.org/debian-security
```

approx runs by default on port 9999 via a systemd socket and requires the users to adjust their sources.list file to point to the approx server:
			

```bash
# Sample sources.list pointing to a local approx server
deb http://localhost:9999/security bullseye-security main contrib non-free
deb http://localhost:9999/debian   bullseye main contrib non-free
```

[Prev5.5.Â Coexistence with Other Packaging Systems](sect.coexistence-with-other-packaging-systems.en.md)[Up](#)[Home](index.en.md)[Next6.2.Â aptitude, apt-get, and apt Commands](sect.apt-get.en.md)