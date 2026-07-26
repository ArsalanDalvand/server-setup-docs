---
title: "5.2.3. Checksums, List of Configuration Files, et al."
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.package-meta-information.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.package-meta-information.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "5.2.3. Checksums, List of Configuration Files, et al."
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](packaging-system.en.md)The Debian Administrator's Handbook[Next](sect.source-package-structure.en.md)

## 5.2.Â Package Meta-Information

			The Debian package is not only an archive of files intended for installation. It is part of a larger whole and describes its relationship with other Debian packages (requisites, dependencies, conflicts, suggestions). It also provides scripts that enable the execution of commands at different stages in the package's lifecycle (installation, upgrade, removal). These data are used by the package management tools, but are not part of the packaged software; they are, within the package, what is called its âmeta-informationâ - information about other information.
		

### 5.2.1.Â Description: the control File

				This file uses a structure similar to email headers (as defined by [RFCÂ 2822](sect.package-meta-information.en.md#sidebar.rfc)) and is fully described in the Debian Policy and the manual pages deb-control(5) and deb822(5). âÂ [https://www.debian.org/doc/debian-policy/ch-controlfields.html](https://www.debian.org/doc/debian-policy/ch-controlfields.html)

				For example, for apt, the control file looks like the following:
			

```bash
$ apt-cache show apt
Package: apt
Version: 2.2.4
Installed-Size: 4337
Maintainer: APT Development Team <deity@lists.debian.org>
Architecture: amd64
Replaces: apt-transport-https (<< 1.5~alpha4~), apt-utils (<< 1.3~exp2~)
Provides: apt-transport-https (= 2.2.4)
Depends: adduser, gpgv | gpgv2 | gpgv1, libapt-pkg6.0 (>= 2.2.4), debian-archive-keyring, libc6 (>= 2.15), libgcc-s1 (>= 3.0), libgnutls30 (>= 3.7.0), libseccomp2 (>= 2.4.2), libstdc++6 (>= 9), libsystemd0
Recommends: ca-certificates
Suggests: apt-doc, aptitude | synaptic | wajig, dpkg-dev (>= 1.17.2), gnupg | gnupg2 | gnupg1, powermgmt-base
Breaks: apt-transport-https (<< 1.5~alpha4~), apt-utils (<< 1.3~exp2~), aptitude (<< 0.8.10)
Description-en: commandline package manager
 This package provides commandline tools for searching and
 managing as well as querying information about packages
 as a low-level access to all features of the libapt-pkg library.
 .
 These include:
  * apt-get for retrieval of packages and information about them
    from authenticated sources and for installation, upgrade and
    removal of packages together with their dependencies
  * apt-cache for querying available information about installed
    as well as installable packages
  * apt-cdrom to use removable media as a source for packages
  * apt-config as an interface to the configuration settings
  * apt-key as an interface to manage authentication keys
Description-md5: 9fb97a88cb7383934ef963352b53b4a7
Tag: admin::package-management, devel::lang:ruby, hardware::storage,
 hardware::storage:cd, implemented-in::c++, implemented-in::perl,
 implemented-in::ruby, interface::commandline, network::client,
 protocol::ftp, protocol::http, protocol::ipv6, role::program,
 scope::application, scope::utility, suite::debian, use::downloading,
 use::organizing, use::playing, use::searching, works-with-format::html,
 works-with::audio, works-with::software:package, works-with::text
Section: admin
Priority: required
Filename: pool/main/a/apt/apt_2.2.4_amd64.deb
Size: 1491328
MD5sum: 24d53e8dd75095640a167f40476c0442
SHA256: 75f07c4965ff0813f26623a1164e162538f5e94defba6961347527ed71bc4f3d
```

				Let us have a closer look at the purpose of some of the fields listed by the previous command.
			

BACK TO BASICS RFC â Internet standards

RFC is the abbreviation of âRequest For Commentsâ. An RFC is generally a technical document that describes what will become an Internet standard. Before becoming standardized and frozen, these standards are submitted for public review (hence their name). The IETF (Internet Engineering Task Force) decides on the evolution of the status of these documents (proposed standard, draft standard, or standard).
			
				RFC 2026 defines the process for standardization of Internet protocols. âÂ [http://www.faqs.org/rfcs/rfc2026.html](http://www.faqs.org/rfcs/rfc2026.html)

#### 5.2.1.1.Â Dependencies: the Depends Field

					The dependencies are defined in the Depends field in the package header. It is a list of conditions to be met for the package to work correctly. This information is used by tools such as apt in order to install the required libraries, tools, drivers, etc., in appropriate versions fulfilling the dependencies of the package to be installed. For each dependency it is possible to restrict the range of versions that meet that condition. In other words, it is possible to express the fact that we need the package libc6 in a version equal to or greater than â2.15â (written âlibc6 (>= 2.15)â). Version comparison operators are as follows:
				
<<: less than;
						
<=: less than or equal to;
						
=: equal to (note that â2.6.1â is not equal to â2.6.1-1â);
						
>=: greater than or equal to;
						
>>: greater than.
						
					In a list of conditions to be met, the comma serves as a separator. It must be interpreted as a logical âandâ. In conditions, the vertical bar (â|â) expresses a logical âorâ (it is an inclusive âorâ, not an exclusive âeither/orâ). Carrying greater priority than âandâ, it can be used as many times as necessary. Thus, the dependency â(A or B) and Câ is written A | B, C. In contrast, the expression âA or (B and C)â should be written as â(A or B) and (A or C)â, since the Depends field does not tolerate parentheses that change the order of priorities between the logical operators âorâ and âandâ. It would thus be written A | B, A | C. âÂ [https://www.debian.org/doc/debian-policy/ch-relationships.html#syntax-of-relationship-fields](https://www.debian.org/doc/debian-policy/ch-relationships.html#syntax-of-relationship-fields)

					The dependencies system is a good mechanism for guaranteeing the operation of a program, but it has another use with âmeta-packagesâ. These are empty packages that only describe dependencies. They facilitate the installation of a consistent group of programs pre-selected by the meta-package maintainer; as such, apt install meta-package will automatically install all of these programs using the meta-package's dependencies. The gnome, kde-full and linux-image-amd64 packages are examples of meta-packages.
				

DEBIAN POLICY Recommends, Suggests, and Enhances fields

					The Recommends and Suggests fields describe dependencies that are not compulsory. The ârecommendedâ dependencies, the most important, considerably improve the functionality offered by the package but are not indispensable to its operation. The âsuggestedâ dependencies, of secondary importance, indicate that certain packages may complement and increase their respective utility, but it is perfectly reasonable to install one without the others.
				
					You should always install the ârecommendedâ packages, unless you know exactly why you do not need them. This is now also the default for APT unless configured otherwise. Conversely, it is not necessary to install âsuggestedâ packages unless you know why you need them (the package description or its documentation may contain information which extra functionality they will activate). The behavior of apt can be controlled by using the APT::Install-Recommends and APT::Install-Suggests configuration options or the corresponding command line options --[no-]install-recommends and --[no-]install-suggests.
				
					The Enhances field also describes a suggestion, but in a different context. It is indeed located in the suggested package, and not in the package that benefits from the suggestion. Its interest lies in that it is possible to add a suggestion without having to modify the package that is concerned. Thus, all add-ons, plug-ins, and other extensions of a program can then appear in the list of suggestions related to the software. Although it has existed for several years, this last field is still largely ignored by programs such as apt or synaptic. Its purpose is for a suggestion made by the Enhances field to appear to the user in addition to the traditional suggestions â found in the Suggests field.
				

DEBIAN POLICY Pre-Depends, a more demanding Depends

					âPre-dependenciesâ, which are listed in the âPre-Dependsâ field in the package headers, complete the normal dependencies; their syntax is identical. A normal dependency indicates that the package in question must be unpacked and configured before configuration of the package declaring the dependency. A pre-dependency stipulates that the package in question must be unpacked and configured before execution of the pre-installation script of the package declaring the pre-dependency, that is before its installation.
				
					A pre-dependency is very demanding for apt, because it adds a strict constraint on the ordering of the packages to install. As such, pre-dependencies are discouraged unless absolutely necessary. It is even recommended to consult other developers on [debian-devel@lists.debian.org](mailto:debian-devel@lists.debian.org) before adding a pre-dependency. It is generally possible to find another solution as a workaround.
				

#### 5.2.1.2.Â Conflicts: the Conflicts field

					The Conflicts field indicates when a package cannot be installed simultaneously with another. The most common reasons for this are that both packages include a file of the same name and path, or provide the same service on the same TCP port, or would hinder each other's operation.
				
dpkg will refuse to install a package if it triggers a conflict with an already installed package, except if the new package specifies that it will âreplaceâ the installed package, in which case dpkg will choose to replace the old package with the new one. apt always follows your instructions: if you choose to install a new package, it will automatically offer to uninstall the package that poses a problem.
				

#### 5.2.1.3.Â Incompatibilities: the Breaks Field

					The Breaks field has an effect similar to that of the Conflicts field, but with a special meaning. It signals that the installation of a package will âbreakâ another package (or particular versions of it). In general, this incompatibility between two packages is transitory, and the Breaks relationship specifically refers to the incompatible versions.
				
dpkg will refuse to install a package that breaks an already installed package, and apt will try to resolve the problem by updating the package that would be broken to a newer version (which is assumed to be fixed and, thus, compatible again).
				
					This type of situation may occur in the case of updates without backwards compatibility: this is the case if the new version no longer functions with the older version, and causes a malfunction in another program without making special provisions. The Breaks field prevents the user from running into these problems.
				

#### 5.2.1.4.Â Provided Items: the Provides Field

					This field introduces the very interesting concept of a âvirtual packageâ. It has many roles, but two are of particular importance. The first role consists in using a virtual package to associate a generic service with it (the package âprovidesâ the service). The second indicates that a package completely replaces another, and that for this purpose it can also satisfy the dependencies that the other would satisfy. It is thus possible to create a substitution package without having to use the same package name.
				

VOCABULARY Meta-package and virtual package

					It is essential to clearly distinguish meta-packages from virtual packages. The former are real packages (including real .deb files), whose only purpose is to express dependencies.
				
					Virtual packages, however, do not exist physically; they are only a means of identifying real packages based on common, logical criteria (service provided, compatibility with a standard program or a pre-existing package, etc.).
				

##### 5.2.1.4.1.Â Providing a âServiceâ

						Let us discuss the first case in greater detail with an example: all mail servers, such as postfix or sendmail are said to âprovideâ the mail-transport-agent virtual package. Thus, any package that needs this service to be functional (e.g. a mailing list manager, such as smartlist or sympa) simply states in its dependencies that it requires a mail-transport-agent instead of specifying a large yet incomplete list of possible solutions (e.g. postfix | sendmail | exim4 | â¦). Furthermore, it is useless to install two mail servers on the same machine, which is why each of these packages declares a conflict with the mail-transport-agent virtual package. A conflict between a package and itself is ignored by the system, but this technique will prohibit the installation of two mail servers side by side.
					

DEBIAN POLICY List of virtual packages

						For virtual packages to be useful, everyone must agree on their name. This is why they are standardized in the Debian Policy. The list includes among others mail-transport-agent for mail servers, c-compiler for C programming language compilers, www-browser for web browsers, httpd for web servers, ftp-server for FTP servers, x-terminal-emulator for terminal emulators in graphical mode (xterm), and x-window-manager for window managers.
					
						The full list can be found on the Web. âÂ [https://www.debian.org/doc/packaging-manuals/virtual-package-names-list.yaml](https://www.debian.org/doc/packaging-manuals/virtual-package-names-list.yaml)

##### 5.2.1.4.2.Â Interchangeability with Another Package

						The Provides field is also interesting when the content of a package is included in a larger package. For example, the libdigest-md5-perl Perl module was an optional module in Perl 5.6, and has been integrated as standard in Perl 5.8 (and later versions, such as 5.32.1 present in Bullseye). As such, the package perl has since version 5.8 declared Provides: libdigest-md5-perl so that the dependencies on this package are met if the user has Perl 5.8 (or newer). The libdigest-md5-perl package itself has eventually been deleted, since it no longer had any purpose when old Perl versions were removed.
					

FigureÂ 5.1.Â Use of a Provides field in order to not break dependencies

						This feature is very useful, since it is never possible to anticipate the vagaries of development, and it is necessary to be able to adjust to renaming, and other automatic replacement, of obsolete software.
					

BACK TO BASICS Perl, a programming language

						Perl (Practical Extraction and Report Language) is a very popular programming language. It has many ready-to-use modules that cover a vast spectrum of applications, and that are distributed by the CPAN (Comprehensive Perl Archive Network) servers, an exhaustive network of Perl packages. âÂ [https://www.perl.org/](https://www.perl.org/) âÂ [https://www.cpan.org/](https://www.cpan.org/)

						Since it is an interpreted language, a program written in Perl does not require compilation prior to execution. This is why they are called âPerl scriptsâ.
					

##### 5.2.1.4.3.Â Past Limitations

						Virtual packages used to suffer from some limitations, the most significant of which was the absence of a version number. To return to the previous example, a dependency such as Depends: libdigest-md5-perl (>= 1.6), despite the presence of PerlÂ 5.10, would have never been considered as satisfied by the packaging system â while in fact it most likely was satisfied. Unaware of this, the package system chose the least risky option, assuming that the versions do not match.
					
						This limitation has been lifted in dpkgÂ 1.17.11, and is no longer relevant. Packages, like perl 5.32.1, can assign a version to the virtual packages they provide, such as Provides: libdigest-md5-perl (= 2.55.01), and thus allow other packages to use versioned dependencies.
					

#### 5.2.1.5.Â Replacing Files: The Replaces Field

					The Replaces field indicates that the package contains files that are also present in another package, but that the package is legitimately entitled to replace them. Without this specification, dpkg fails to install the package, stating that it cannot overwrite the files of another package (technically, it is possible to force it to do so with the --force-overwrite option, but that is not considered standard operation). This allows identification of potential problems and requires the maintainer to study the matter prior to choosing whether to add such a field.
				
					The use of this field is justified when package names change or when a package is included in another. This also happens when the maintainer decides to distribute files differently among various binary packages produced from the same source package: a replaced file no longer belongs to the old package, but only to the new one.
				
					If all of the files in an installed package have been replaced, the package is considered to be removed. Finally, this field also encourages dpkg to remove the replaced package where there is a conflict.
				

GOING FURTHER The Tag field

					In the apt example at the beginning, we can see the presence of a field that we have not yet described, the Tag field. This field does not describe a relationship between packages, but is simply a way of categorizing a package in a thematic taxonomy. This classification of packages according to several criteria (type of interface, programming language, domain of application, etc.) has been available for a long time. Despite this, not all packages have accurate tags and it is not yet integrated in all Debian tools; aptitude displays these tags, and allows them to be used as search criteria. For those who dislike aptitude's search criteria, the following website allows navigation of the tag database: âÂ [https://wiki.debian.org/Debtags](https://wiki.debian.org/Debtags)

### 5.2.2.Â Configuration Scripts

				In addition to the control file, the control.tar.gz archive for each Debian package may contain a number of scripts, called by dpkg at different stages in the processing of a package. The Debian Policy describes the possible cases in detail, specifying the scripts called and the arguments that they receive. These sequences may be complicated, since if one of the scripts fails, dpkg will try to return to a satisfactory state by canceling the installation or removal in progress (insofar as it is possible). âÂ [https://www.debian.org/doc/debian-policy/ch-maintainerscripts.html](https://www.debian.org/doc/debian-policy/ch-maintainerscripts.html)

GOING FURTHER dpkg's database

				All of the configuration scripts for installed packages are stored in the /var/lib/dpkg/info/ directory, in the form of a file prefixed with the package's name. This directory also includes a file with the .list extension for each package, containing the list of files that belong to that package.
			
				The /var/lib/dpkg/status file contains a series of data blocks (in the format of the famous mail headers, RFC 2822) describing the status of each package. The information from the control file of the installed packages is also replicated there.
			
				In general, the preinst script is executed prior to installation of the package, while postinst follows it. Likewise, prerm is invoked before removal of a package and postrm afterwards. An update of a package is equivalent to removal of the previous version and installation of the new one. It is not possible to describe in detail all the possible scenarios here, but we will discuss the most common two: an installation/update and a removal.
			

CAUTION Symbolic names of the scripts

				The sequences described in this section call configuration scripts by specific names, such as old-prerm or new-postinst. They are, respectively, the prerm script contained in the old version of the package (installed before the update) and the postinst script contained in the new version (installed by the update).
			

TIP State diagrams

				Manoj Srivastava and Margarita Manterola made the following diagrams explaining how the configuration scripts are called by dpkg. âÂ [https://people.debian.org/~srivasta/MaintainerScripts.html](https://people.debian.org/~srivasta/MaintainerScripts.html) âÂ [https://www.debian.org/doc/debian-policy/ap-flowcharts.html](https://www.debian.org/doc/debian-policy/ap-flowcharts.html)

#### 5.2.2.1.Â Installation and Upgrade

					During the initial installation and for each upgrade of a package, dpkg calls the so called maintainer scripts such as the prerm or preinst scripts. These scripts can perform additional actions during the different stages of a package's life-cycle. Script names preceded by new- are the scripts from the new version of a package being installed or upgraded to. Script names preceded by old- are the scripts from the old version of a package that is being upgraded from.
				
					During each invocation dpkg will pass certain arguments to each script such as upgrade new-version. The invoked script can then either handle the arguments and perform a particular action, or ignore the arguments and return with an exit code of 0, if nothing needs to be done during that step. In practice many packages will not need to perform an action during every step in the life cycle. Thus a typical configuration script will check for a particular argument and ignore all other ones, implicitly returning with exit code 0.
				
					Here is what happens during an installation (or an update). The old-version, new-version and last-version-configured arguments are placeholders for the actual (old and new) version numbers of the package:
				
							For an update, dpkg calls the old-prerm script and passes upgrade new-version as arguments.
						
							Still for an update, dpkg then executes the new-preinst script with the arguments upgrade old-version; for the initial installation, it executes the new-preinst script and passes install as argument. It may add the old version in the last parameter, if the package has already been installed and removed since (but not purged, and thus configuration files have been retained).
						
							The new package files are then unpacked. If a file already exists, it is replaced, but a backup copy is temporarily made.
						
							For an update, dpkg executes the old-postrm script and passes upgrade new-version as arguments.
						
dpkg updates all of the internal data (file list, configuration scripts, etc.) and removes the backups of the replaced files. This is the point of no return: dpkg no longer has access to all of the elements necessary to return to the previous state.
						
dpkg will update the configuration files, asking the user to decide if it is unable to automatically manage this task. The details of this procedure are discussed in [SectionÂ 5.2.3, âChecksums, List of Configuration Files, et al.â](sect.package-meta-information.en.md#sect.conffiles).
						
							Finally, dpkg configures the package by executing the new-postinst script with the arguments configure last-version-configured.
						

#### 5.2.2.2.Â Package Removal

					The steps to remove a package are analogous to the installation steps. The main difference is that the removal scripts of the package are called:
				
dpkg calls the prerm script and passes the remove argument.
						
dpkg removes all of the package's files, with the exception of the configuration files and maintainer scripts.
						
dpkg executes the postrm script and passes remove as argument. Afterwards, all of the maintainer scripts, except the postrm script, are removed. If the user has not used the âpurgeâ option, the process stops here.
						
							For a complete purge of the package (command issued with dpkg --purge or dpkg -P), the configuration files are also deleted, as well as a certain number of copies (*.dpkg-tmp, *.dpkg-old, *.dpkg-new) and temporary files; dpkg then executes the postrm script and passes purge as argument.
						

VOCABULARY Purge, a complete removal

					When a Debian package is removed, the configuration files are retained in order to facilitate possible re-installation. Likewise, the data generated by a daemon (such as the content of an LDAP server directory, or the content of a database for an SQL server) are usually retained.
				
					To remove all data associated with a package, it is necessary to âpurgeâ the package with the command, dpkg -P package, apt-get remove --purge package or aptitude purge package.
				
					Given the definitive nature of such data removals, a purge should not be taken lightly.
				
					The four scripts detailed above are complemented by a config script, provided by packages using debconf to acquire information from the user for configuration. During installation, this script defines in detail the questions asked by debconf. The responses are recorded in the debconf database for future reference. The script is generally executed by apt prior to installing packages one by one in order to group all the questions and ask them all to the user at the beginning of the process. The pre- and post-installation scripts can then use this information to operate according to the user's wishes.
				

TOOL debconf

debconf was created to resolve a recurring problem in Debian. All Debian packages unable to function without a minimum of configuration used to ask questions with calls to the echo and read commands in postinst shell scripts (and other similar scripts). But this also means that during a large installation or update the user must stay with their computer to respond to various questions that may arise at any time. These manual interactions have now been almost entirely dispensed with, thanks to the debconf tool.
				
debconf has many interesting features: it requires the developer to specify user interaction; it allows localization of all the strings displayed to users (all translations are stored in the templates file describing the interactions); it has different frontends to display the questions to the user (text mode, graphical mode, non-interactive); and it allows creation of a central database of responses to share the same configuration with several computers... but the most important is that it is now possible to present all of the questions in a row to the user, prior to starting a long installation or update process. The user can go about their business while the system handles the installation on its own, without having to stay there staring at the screen waiting for questions.
				

### 5.2.3.Â Checksums, List of Configuration Files, et al.

				In addition to the maintainer scripts and control data already mentioned in the previous sections, the control.tar.gz archive of a Debian package may contain other interesting files.
			
				The first, md5sums, contains the MD5 checksums for all of the package's files. Its main advantage is that it allows dpkg --verify (which we will study in [SectionÂ 14.3.4.1, âAuditing Packages with dpkg --verifyâ](sect.supervision.en.md#sect.dpkg-verify)) and debsums (from the package of the same name; see [SectionÂ 14.3.4.2, âAuditing Packages: debsums and its Limitsâ](sect.supervision.en.md#sect.debsums)) to check if these files have been modified since their installation. Note that when this file doesn't exist, which might be the case for some older packages, dpkg will generate it dynamically at installation time (and store it in the dpkg database just like other control files).
			
				The file conffiles lists package files that must be handled as configuration files (see also deb-conffiles(5)). Configuration files can be modified by the administrator, and dpkg will try to preserve those changes during a package update.
			
				In effect, in this situation, dpkg behaves as intelligently as possible: if the standard configuration file has not changed between the two versions, it does nothing. If, however, the file has changed, it will try to update this file. Two cases are possible: either the administrator has not touched this configuration file, in which case dpkg automatically installs the new version; or the file has been modified, in which case dpkg asks the administrator which version they wish to use (the old one with modifications, or the new one provided with the package). To assist in making this decision, dpkg offers to display a âdiffâ that shows the difference between the two versions. If the user chooses to retain the old version, the new one will be stored in the same location in a file with the .dpkg-dist suffix. If the user chooses the new version, the old one is retained in a file with the .dpkg-old suffix. Another available action consists of momentarily interrupting dpkg to edit the file and attempt to re-instate the relevant modifications (previously identified with diff).
			

GOING FURTHER Force dpkg to ask configuration file questions

				The --force-confask option requires dpkg to display the questions about the configuration files, even in cases where they would not normally be necessary. Thus, when reinstalling a package with this option, dpkg will ask the questions again for all of the configuration files modified or deleted by the administrator. This is very convenient, especially for reinstalling the original configuration file if it has been deleted and no other copy is available: a normal re-installation won't work, because dpkg considers removal as a form of legitimate modification, and, thus, doesn't install the desired configuration file.
			
				See the sidebar [GOING FURTHER Avoiding the configuration file questions](sect.package-meta-information.en.md#sidebar.questions-conffiles) for how to use these options with APT.
			

GOING FURTHER Avoiding the configuration file questions

dpkg handles configuration file updates, but, while doing so, regularly interrupts its work to ask for input from the administrator. This makes it less than enjoyable for those who wish to run updates in a non-interactive manner. This is why this program offers options that allow the system to respond automatically according to the same logic: --force-confold retains the old version of the file; --force-confnew will use the new version of the file (these choices are respected, even if the file has not been changed by the administrator, which only rarely has the desired effect). Adding the --force-confdef option tells dpkg to decide by itself when possible (in other words, when the original configuration file has not been touched), and only uses --force-confnew or --force-confold for other cases.
			
				These options apply to dpkg and are explained in detail in dpkg(1) or dpkg --force-help, but most of the time the administrator will work directly with the aptitude or apt programs. It is, thus, necessary to know the syntax used to indicate the options to pass to the dpkg command (their command line interfaces are very similar).
			

```bash
# apt -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" full-upgrade
```

				These options can also be stored directly in apt's configuration. To do so, simply write the following lines in the /etc/apt/apt.conf.d/local file:
			

```bash
Dpkg::Options {
  "--force-confdef";
  "--force-confold";
}
```

				Including this option in the configuration file means that it will also be used in a graphical interface such as aptitude.
			
				The control archive frequently contains other files as well, like triggers, shlibs, or symbols. These files are well described in deb-triggers(5), deb-shlibs(5), and deb-symbols(5).
			
				Triggers were introduced to reduce the amount of duplicated events during package installation, such as file registration or catalog/database update tasks. Packages can define their own or activate defined triggers. A more comprehensive documentation can be found in [/usr/share/doc/dpkg/triggers.txt.gz](https://git.dpkg.org/cgit/dpkg/dpkg.git/tree/doc/triggers.txt).
			
				The shlibs system is an older and simpler alternative to the symbols system for declaring dependencies for shared libraries. It defines the package name and version in which to find a specific SONAME-version of a shared library. The newer symbols system allows to define the dependency by tracking the symbols and when they have been introduced or changed in the library instead.
			[PrevChapterÂ 5.Â Packaging System: Tools and Fundamenta...](packaging-system.en.md)[Up](#)[Home](index.en.md)[Next5.3.Â Structure of a Source Package](sect.source-package-structure.en.md)