---
title: "6.2.8. APT Patterns"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.apt-get.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.apt-get.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.2.8. APT Patterns"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](apt.en.md)The Debian Administrator's Handbook[Next](sect.apt-cache.en.md)

## 6.2.Â aptitude, apt-get, and apt Commands

APT is a vast project, whose original plans included a graphical interface. It is based on a library which contains the core application, and apt-get is the first front end â command-line based â which was developed within the project. apt is a second command-line based front end provided by APT which overcomes some design mistakes of apt-get.
		
			Both tools are built on top of the same library and are thus very close, but the default behavior of apt has been improved for interactive use and to actually do what most users expect. The APT developers reserve the right to change the public interface of this tool to further improve it. Conversely, the public interface of apt-get is well defined and will not change in any backwards incompatible way. It is thus the tool that you want to use when you need to script package installation requests.
		
			Numerous other graphical interfaces then appeared as external projects: synaptic, aptitude (which includes both a text mode interface and a graphical one â even if not complete yet), wajig, etc. The most recommended interface, apt, is the one that we will use in the examples given in this section. Note, however, that apt-get and aptitude have a very similar command line syntax. When there are major differences between these three commands, these will be detailed.
		

### 6.2.1.Â Initialization

				For any work with APT, the list of available packages needs to be updated; this can be done simply through apt update. Depending on the speed of your connection and configuration, the operation can take a while, since it involves downloading a certain number of (usually compressed) files (Packages, Sources, Translation-language-code), which have gradually become bigger and bigger as Debian has developed (at least 10-16Â MB of data for the main section). Of course, installing from a CD-ROM/DVD set does not require any downloading â in this case, the operation is very fast.
			

TIP Incremental updates

				The aim of the apt update command is to download for each package source the corresponding Packages (or Sources) file. However, even after a xz compression, these files can remain rather large (the Packages.xz for the main section of Unstable takes more than 8Â MB). If you wish to update regularly, these downloads can take up a lot of time.
			
				To speed up the process, APT can download âdiffâ files containing the changes since the previous update, as opposed to the entire file. To achieve this, official Debian mirrors distribute different files which list the differences between one version of the Packages file and the following version. They are generated at each update of the archives and a history of one week is kept. Each of these âdiffâ files only takes a few dozen kilobytes for Unstable, so that the amount of data downloaded by a weekly apt update is often divided by 10. For Stable and Testing, which change less, the gain is even more noticeable.
			
				However, it can sometimes be of interest to force the download of the entire Packages file, especially when the last upgrade is very old and when the mechanism of incremental differences would not contribute much. This can also be interesting when network access is very fast but when the processor of the machine to upgrade is rather slow, since the time saved on the download is more than lost when the computer calculates the new versions of these files (starting with the older versions and applying the downloaded differences). To do that, you can use the APT configuration parameter Acquire::PDiffs and set it to false.
			

```bash
$ sudo apt -o "Acquire::PDiffs=false" update
```

				The Acquire::* options also control other aspects of the download, and even the download methods. Acquire::Languages can limit or disable the download of Translation-language-code files and save even more time. For a complete reference see apt.conf(5).
			

### 6.2.2.Â Installing and Removing

				With APT, packages can be added or removed from the system, respectively with apt install package and apt remove package. In both cases, APT will automatically install the necessary dependencies or delete the packages which depend on the package that is being removed. The apt purge package command involves a complete uninstallation by deleting the configuration files as well.
			

TIP Installing the same selection of packages several times

				It can be useful to systematically install the same list of packages on several computers. This can be done quite easily.
			
				First, retrieve the list of packages installed on the computer which will serve as the âmodelâ to copy.
			

```bash
$ dpkg --get-selections >pkg-list
```

				The pkg-list file then contains the list of installed packages. Next, transfer the pkg-list file onto the computers you want to update and use the following commands:
			

```bash
## Update dpkg's database of known packages
# avail=`mktemp`
# apt-cache dumpavail > "$avail"
# dpkg --merge-avail "$avail"
# rm -f "$avail"
## Update dpkg's selections
# dpkg --set-selections < pkg-list
## Ask apt-get to install the selected packages
# apt-get dselect-upgrade
```

				The first commands record the list of available packages in the dpkg database. Then dpkg --set-selections restores the selection of packages that you wish to install, and the apt-get invocation executes the required operations! aptitude does not have this command.
			

TIP Removing and installing at the same time

				It is possible to ask apt (or apt-get, or aptitude) to install certain packages and remove others on the same command line by adding a suffix. With an apt install command, add â-â to the names of the packages you wish to remove. With an apt remove command, add â+â to the names of the packages you wish to install.
			
				The next example shows two different ways to install package1 and to remove package2.
			

```bash
# apt install package1 package2-
```

```bash
# apt remove package1+ package2
```

				This can also be used to exclude packages which would otherwise be installed, for example, due to an automatic installation of Recommends. In general, the dependency solver will use that information as a hint to look for alternative solutions.
			

TIP apt --reinstall and aptitude reinstall

				The system can sometimes be damaged after the removal or modification of files in a package. The easiest way to retrieve these files is to reinstall the affected package. Unfortunately, the packaging system finds that the latter is already installed and politely refuses to reinstall it; to avoid this, use the --reinstall option of the apt and apt-get commands. The following command reinstalls postfix even if it is already present:
			

```bash
# apt --reinstall install postfix
```

				The aptitude command line is slightly different, but achieves the same result with aptitude reinstall postfix.
			
				The problem does not arise with dpkg, but the administrator rarely uses it directly.
			
				Be careful! Using apt --reinstall to restore packages modified during an attack will certainly not recover the system as it was. [SectionÂ 14.7, âDealing with a Compromised Machineâ](sect.dealing-with-compromised-machine.en.md) details the necessary steps to take with a compromised system.
			
				These commands will not restore the configuration files. But as you have learned in [SectionÂ 5.2.3, âChecksums, List of Configuration Files, et al.â](sect.package-meta-information.en.md#sect.conffiles) (see also sidebar [GOING FURTHER Force dpkg to ask configuration file questions](sect.package-meta-information.en.md#sidebar.questions-conffiles-bis)), you can use the following command to be asked to install the unmodified version and even restore any deleted configuration file as well.
			

```bash
# apt --reinstall -o Dpkg::Options::="--force-confask,confmiss" install package
```

				Some packages don't ship the configuration file found in /etc with the package. Instead they create it during installation by either copying a skeleton or writing it by a script. The file /etc/inputrc, for example, is a copy of /usr/share/readline/inputrc. In such cases the commands shown above won't work.
			
				If the file sources.list mentions several distributions, it is possible to give the version of the package to install. A specific version number can be requested with apt install package=version, but indicating its distribution of origin (Stable, Testing or Unstable) â with apt install package/distribution â is usually preferred. With this command, it is possible to go back to an older version of a package (if, for instance, you know that it works well), provided that it is still available in one of the sources referenced by the sources.list file. Otherwise the snapshot.debian.org archive can come to the rescue (see sidebar [GOING FURTHER Old package versions: snapshot.debian.org and archive.debian.org](apt.en.md#sidebar.snapshot.debian.org)).
			

ExampleÂ 6.4.Â Installation of the Unstable version of spamassassin

```bash
# apt install spamassassin/unstable
```

				If the package to install has been made available to you under the form of a simple .deb file without any associated package repository, it is still possible to use APT to install it together with its dependencies (provided that the dependencies are available in the configured repositories) with a simple command: apt install ./path-to-the-package.deb. The leading ./ is important to make it clear that we are referring to a filename and not to the name of a package available in one of the repositories.
			

GOING FURTHER The cache of .deb files

				APT keeps a copy of each downloaded .deb file in the directory /var/cache/apt/archives/. In case of frequent updates, this directory can quickly take a lot of disk space with several versions of each package; you should regularly sort through them. Two commands can be used: apt-get clean entirely empties the directory; apt-get autoclean only removes packages which can no longer be downloaded (because they have disappeared from the Debian mirror) and are therefore clearly useless (the configuration parameter APT::Clean-Installed can prevent the removal of .deb files that are currently installed).
			

### 6.2.3.Â System Upgrade

				Regular upgrades are recommended, because they include the latest security updates. To upgrade, use apt upgrade, apt-get upgrade or aptitude safe-upgrade (of course after apt update). This command looks for installed packages which can be upgraded without removing any packages. In other words, the goal is to ensure the least intrusive upgrade possible. apt-get is slightly more demanding than aptitude or apt because it will refuse to install packages which were not installed beforehand.
			
apt will generally select the most recent version number (except for packages from Experimental and stable-backports, which are ignored by default whatever their version number). If you specified Testing or Unstable in your sources.list, apt upgrade will switch most of your Stable system to Testing or Unstable, which might not be what you intended.
			
				To tell apt to use a specific distribution when searching for upgraded packages, you need to use the -t or --target-release option, followed by the name of the distribution you want (for example, apt -t stable upgrade). To avoid specifying this option every time you use apt, you can add APT::Default-Release "stable"; in the file /etc/apt/apt.conf.d/local.
			
				For more important upgrades, such as the change from one major Debian version to the next, you need to use apt full-upgrade. With this instruction, apt will complete the upgrade even if it has to remove some obsolete packages or install new dependencies. This is also the command used by users who work daily with the Debian Unstable release and follow its evolution day by day. It is so simple that it hardly needs explanation: APT's reputation is based on this great functionality.
			
				Unlike apt and aptitude, apt-get doesn't know the full-upgrade command. Instead, you should use apt-get dist-upgrade (âdistribution upgradeâ), the historical and well-known command that apt and aptitude also accept for the convenience of users who got used to it.
			
				The results of these operations are logged into /var/log/apt/history.log and /var/log/apt/term.log, whereas dpkg keeps its log in a file called /var/log/dpkg.log.
			

### 6.2.4.Â Configuration Options

				Besides the configuration elements already mentioned, it is possible to configure certain aspects of APT by adding directives in a file of the /etc/apt/apt.conf.d/ directory or /etc/apt/apt.conf itself. Remember, for instance, that it is possible for APT to tell dpkg to ignore file conflict errors by specifying DPkg::options { "--force-overwrite"; }.
			
				If the Web can only be accessed through a proxy, add a line like Acquire::http::proxy "http://yourproxy:3128". For an FTP proxy, write Acquire::ftp::proxy "ftp://yourproxy". To discover more configuration options, read the apt.conf(5) manual page (for details on manual pages, see [SectionÂ 7.1.1, âManual Pagesâ](solving-problems.en.md#sect.manual-pages)).
			

BACK TO BASICS Directories ending in .d

				Directories with a .d suffix are used more and more often. Each directory represents a configuration file which is split over multiple files. In this sense, all of the files in /etc/apt/apt.conf.d/ are instructions for the configuration of APT. APT includes them in alphabetical order, so that the last ones can modify a configuration element defined in one of the first ones.
			
				This structure brings some flexibility to the machine administrator and to the package maintainers. Indeed, the administrator can easily modify the configuration of the software by adding a ready-made file in the directory in question without having to change an existing file. Package maintainers use the same approach when they need to adapt the configuration of another software to ensure that it perfectly co-exists with theirs. The Debian policy explicitly forbids modifying configuration files of other packages â only users are allowed to do this. Remember that during a package upgrade, the user gets to choose the version of the configuration file that should be kept when a modification has been detected. Any external modification of the file would trigger that request, which would disturb the administrator, who is sure not to have changed anything.
			
				Without a .d directory, it is impossible for an external package to change the settings of a program by modifying its configuration file directly. Instead it must either invite the user to do it themselves and lists the operations to be done in the file /usr/share/doc/package/README.Debian, or it must create a file diversion for the configuration file using the dpkg-divert command and then install its own configuration file for the software in question. The latter is sometimes used by third party packages which try to handle configuration files of software components they use.
			
				Depending on the application, the .d directory is used directly or managed by an external script which will concatenate all the files to create the configuration file itself. It is important to execute the script after any change in that directory so that the most recent modifications are taken into account. In the same way, it is important not to work directly in the configuration file created automatically, since everything would be lost at the next execution of the script. The chosen method (.d directory used directly or a file generated from that directory) is usually dictated by implementation constraints, but in both cases the gains in terms of configuration flexibility more than make up for the small complications that they entail. The EximÂ 4 mail server is an example of the generated file method: it can be configured through several files (/etc/exim4/conf.d/*) which are concatenated into /var/lib/exim4/config.autogenerated by the update-exim4.conf command.
			

### 6.2.5.Â Managing Package Priorities

				One of the most important aspects in the configuration of APT is the management of the priorities associated with each package source. For instance, you might want to extend one distribution with one or two newer packages from Testing, Unstable or Experimental. It is possible to assign a priority to each available package (the same package can have several priorities depending on its version or the distribution providing it). These priorities will influence APT's behavior: for each package, it will always select the version with the highest priority (except if this version is older than the installed one and if its priority is less thanÂ 1000).
			
				APT defines several default priorities. Each installed package version has a priority ofÂ 100. A non-installed version has a priority ofÂ 500 by default, but it can jump toÂ 990 if it is part of the target release (defined with the -t command-line option or the APT::Default-Release configuration directive).
			
				You can modify the priorities by adding entries in a file in /etc/apt/preferences.d/ or the /etc/apt/preferences file with the names of the affected packages, their version, their origin and their new priority.
			
				APT will never install an older version of a package (that is, a package whose version number is lower than the one of the currently installed package) except if its priority is higher thanÂ 1000 (or it is explicitly requested by the user, see [SectionÂ 6.2.2, âInstalling and Removingâ](sect.apt-get.en.md#sect.apt.install)). APT will always install the highest priority package which follows this constraint. If two packages have the same priority, APT installs the newest one (whose version number is the highest). If two packages of same version have the same priority but differ in their content, APT installs the version that is not installed (this rule has been created to cover the case of a package update without the increment of the revision number, which is usually required).
			
				In more concrete terms, a package whose priority is
			< 0
							will never be installed,
						1..99
							will only be installed if no other version of the package is already installed,
						100..499
							will only be installed if there is no other newer version installed or available in another distribution,
						500....989
							will only be installed if there is no newer version installed or available in the target distribution,
						990..1000
							will be installed except if the installed version is newer,
						> 1000
							will always be installed, even if it forces APT to downgrade to an older version.
						
				When APT checks /etc/apt/preferences and /etc/apt/preferences.d/, it first takes into account the most specific entries (often those specifying the concerned package), then the more generic ones (including, for example, all the packages of a distribution). If several generic entries exist, the first match is used. The available selection criteria include the package's name and the source providing it. Every package source is identified by the information contained in a Release file that APT downloads together with the Packages files. It specifies the origin (usually âDebianâ for the packages of official mirrors, but it can also be a person's or an organization's name for third-party repositories). It also gives the name of the distribution (usually Stable, Testing, Unstable or Experimental for the standard distributions provided by Debian) together with its version (for example, 11 for Debian Bullseye). Let's have a look at its syntax through some realistic case studies of this mechanism.
			

SPECIFIC CASE Priority of experimental

				If you listed Experimental in your sources.list file, the corresponding packages will almost never be installed because their default APT priority isÂ 1. This is of course a specific case, designed to keep users from installing Experimental packages by mistake. The packages can only be installed by typing aptitude install package/experimental â users typing this command can only be aware of the risks that they take. It is still possible (though not recommended) to treat packages of Experimental like those of other distributions by giving them a priority ofÂ 500. This is done with a specific entry in /etc/apt/preferences:
			

```bash
Package: *
Pin: release a=experimental
Pin-Priority: 500
```

				Let's suppose that you only want to use packages from the stable version of Debian. Those provided in other versions should not be installed except if explicitly requested. You could write the following entries in the /etc/apt/preferences file:
			

```bash
Package: *
Pin: release a=stable
Pin-Priority: 900

Package: *
Pin: release o=Debian
Pin-Priority: -10
```

a=stable defines the name of the selected distribution. o=Debian limits the scope to packages whose origin is âDebianâ.
			
				Let's now assume that you have a server with several local programs depending on the versionÂ 5.28 of Perl and that you want to ensure that upgrades will not install another version of it. You could use this entry:
			

```bash
Package: perl
Pin: version 5.28*
Pin-Priority: 1001
```

				To gain a better understanding of the mechanisms of priority and distribution or repository properties to pin do not hesitate to execute apt-cache policy to display the default priority associated with each package source, or apt-cache policy package to display the default priority for each available version and source of a package as explained in [SectionÂ 6.3.1, âThe apt-cache policy Commandâ](sect.apt-cache.en.md#sect.apt-cache-policy).
			
				The reference documentation for the files /etc/apt/preferences and /etc/apt/preferences.d/ is available in the manual page apt_preferences(5), which you can display with man apt_preferences.
			

TIP Comments in /etc/apt/preferences

				There is no official syntax to put comments in the /etc/apt/preferences file, but some textual descriptions can be provided by putting one or more âExplanationâ fields at the start of each entry:
			

```bash
Explanation: The package xserver-xorg-video-intel provided
Explanation: in experimental can be used safely
Package: xserver-xorg-video-intel
Pin: release a=experimental
Pin-Priority: 500
```

### 6.2.6.Â Working with Several Distributions

apt being such a marvelous tool, it is tempting to pick packages coming from other distributions. For example, after having installed a Stable system, you might want to try out a software package available in Testing or Unstable without diverging too much from the system's initial state.
			
				Even if you will occasionally encounter problems while mixing packages from different distributions, apt manages such coexistence very well and limits risks very effectively. The best way to proceed is to list all distributions used in /etc/apt/sources.list (some people always put the three distributions, but remember that Unstable is reserved for experienced users) and to define your reference distribution with the APT::Default-Release parameter (see [SectionÂ 6.2.3, âSystem Upgradeâ](sect.apt-get.en.md#sect.apt-upgrade)).
			
				Let's suppose that Stable is your reference distribution but that Testing and Unstable are also listed in your sources.list file. In this case, you can use apt install package/testing to install a package from Testing. If the installation fails due to some unsatisfiable dependencies, let it solve those dependencies within Testing by adding the -t testing parameter. The same obviously applies to Unstable.
			
				In this situation, upgrades (upgrade and full-upgrade) are done within Stable except for packages already upgraded to another distribution: those will follow updates available in the other distributions. We will explain this behavior with the help of the default priorities set by APT below. Do not hesitate to use apt-cache policy ([SectionÂ 6.3.1, âThe apt-cache policy Commandâ](sect.apt-cache.en.md#sect.apt-cache-policy)) to verify the given priorities.
			
				Everything centers around the fact that APT only considers packages of higher or equal version than the installed one (assuming that /etc/apt/preferences has not been used to force priorities higher thanÂ 1000 for some packages).
			
				Let's assume that you have installed versionÂ 1 of a first package from Stable and that versionÂ 2 andÂ 3 are available respectively in Testing and Unstable. The installed version has a priority ofÂ 100 but the version available in Stable (the very same) has a priority ofÂ 990 (because it is part of the target release). Packages in Testing and Unstable have a priority ofÂ 500 (the default priority of a non-installed version). The winner is thus versionÂ 1 with a priority ofÂ 990. The package âstays in Stableâ.
			
				Let's take the example of another package whose versionÂ 2 has been installed from Testing. VersionÂ 1 is available in Stable and versionÂ 3 in Unstable. VersionÂ 1 (of priorityÂ 990 â thus lower thanÂ 1000) is discarded because it is lower than the installed version. This only leaves versionÂ 2 andÂ 3, both of priorityÂ 500. Faced with this alternative, APT selects the newest version, the one from Unstable. If you don't want a package installed from Testing to migrate to the version in Unstable, you have to assign a priority lower thanÂ 500 (490 for example) to packages coming from Unstable. You can modify /etc/apt/preferences to this effect:
			

```bash
Package: *
Pin: release a=unstable
Pin-Priority: 490
```

### 6.2.7.Â Tracking Automatically Installed Packages

				One of the essential functionalities of apt is the tracking of packages installed only through dependencies. These packages are called âautomaticâ, and often include libraries.
			
				With this information, when packages are removed, the package managers can compute a list of automatic packages that are no longer needed (because there is no âmanually installedâ packages depending on them). apt-get autoremove or apt autoremove will get rid of those packages. aptitude does not have this command because it removes them automatically as soon as they are identified. In all cases, the tools display a clear message listing the affected packages.
			
				It is a good habit to mark as automatic any package that you don't need directly so that they are automatically removed when they aren't necessary anymore. apt-mark auto package will mark the given package as automatic whereas apt-mark manual package does the opposite. aptitude markauto and aptitude unmarkauto work in the same way although they offer more features for marking many packages at once (see [SectionÂ 6.5.1, âaptitudeâ](sect.apt-frontends.en.md#sect.aptitude)). The console-based interactive interface of aptitude also makes it easy to review the âautomatic flagâ on many packages.
			
				People might want to know why an automatically installed package is present on the system. To get this information from the command line, you can use aptitude why package (apt and apt-get have no similar feature):
			

```bash
$ aptitude why python3-debian
i   aptitude         Suggests apt-xapian-index
p   apt-xapian-index Depends  python3-debian (>= 0.1.14)
```

ALTERNATIVE deborphan and debfoster

				In days where apt, apt-get and aptitude were not able to track automatic packages, there were two utilities producing lists of unnecessary packages: deborphan and debfoster. Both can still be useful.
			
deborphan scans the libs and oldlibs sections by default (in the absence of supplementary instructions) looking for the packages that are currently installed and that no other package depends on. The resulting list can then serve as a basis to remove unneeded packages.
			
debfoster has a more elaborate approach, very similar to APT's one: it maintains a list of packages that have been explicitly installed, and remembers what packages are really required between each invocation. If new packages appear on the system and if debfoster doesn't know them as required packages, they will be shown on the screen together with a list of their dependencies. The program then offers a choice: remove the package (possibly together with those that depend on it), mark it as explicitly required, or ignore it temporarily.
			

### 6.2.8.Â APT Patterns

				Patterns allow you to specify complex search queries to select the packages you want to install or show. They were first implemented for aptitude (see [SectionÂ 6.5, âFrontends: aptitude, synapticâ](sect.apt-frontends.en.md)), but now APT can also make use of them.
			
				For example, we can use apt list ?automatic to list all the packages automatically installed. To find packages automatically installed no longer depended upon by manually installed packages the pattern ?garbage can be used.
			
				Logic patterns can be combined with other packages to form more complex expressions. For instance, we could use a pattern like ?and(PATTERN, PATTERN). See apt-patterns(7) and glob(7) for all the patterns you can use and the complex expressions you can create with them.
			[PrevChapterÂ 6.Â Maintenance and Updates: The APT Tools](apt.en.md)[Up](#)[Home](index.en.md)[Next6.3.Â The apt-cache Command](sect.apt-cache.en.md)