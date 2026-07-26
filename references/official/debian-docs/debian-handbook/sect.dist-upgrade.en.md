---
title: "6.7.3. Cleaning Up after an Upgrade"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.dist-upgrade.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.dist-upgrade.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.7.3. Cleaning Up after an Upgrade"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.package-authentication.en.md)The Debian Administrator's Handbook[Next](sect.regular-upgrades.en.md)

## 6.7.Â Upgrading from One Stable Distribution to the Next

			One of the best-known features of Debian is its ability to upgrade an installed system from one stable release to the next: dist-upgrade â a well-known phrase â has largely contributed to the project's reputation. With a few precautions, upgrading a computer can take as little as a few minutes, or a few dozen minutes, depending on the download speed from the package repositories.
		

### 6.7.1.Â Recommended Procedure

				Since Debian has quite some time to evolve in-between stable releases, you should read the release notes before upgrading.
			

BACK TO BASICS Release notes

				The release notes for an operating system (and, more generally, for any software) are a document giving an overview of the software, with some details concerning the particularities of one version. These documents are generally short compared to the complete documentation, and they usually list the features which have been introduced since the previous version. They also give details on upgrading procedures, warnings for users of previous versions, and sometimes errata.
			
				Release notes for Debian are available online: the release notes for the current stable release have a dedicated URL, while older release notes can be found with their codenames: âÂ [https://www.debian.org/releases/stable/releasenotes](https://www.debian.org/releases/stable/releasenotes) âÂ [https://www.debian.org/releases/buster/releasenotes](https://www.debian.org/releases/buster/releasenotes)

				In this section, we will focus on upgrading a Buster system to Bullseye. This is a major operation on a system; as such, it is never 100% risk-free, and should not be attempted before all important data has been backed up.
			
				Another good habit which makes the upgrade easier (and shorter) is to tidy your installed packages and keep only the ones that are really needed. Helpful tools to do that include aptitude, deborphan, debfoster, and apt-show-versions (see [SectionÂ 6.2.7, âTracking Automatically Installed Packagesâ](sect.apt-get.en.md#sect.automatic-tracking)). For example, you can use the following command, and then use aptitude's interactive mode to double check and fine-tune the scheduled removals:
			

```bash
# deborphan | xargs aptitude --schedule-only remove
```

TIP Finding changed files

				The debsums command can check if files on the local system, which are part of an installed package, have been altered. It uses a simple hashsum algorithm and the information in /var/lib/dpkg/info/package.md5sums (see [SectionÂ 5.2.3, âChecksums, List of Configuration Files, et al.â](sect.package-meta-information.en.md#sect.conffiles)). To find all altered configuration files use debsums -ec. To check the whole system, use debsums -c.
			
				Now for the upgrading itself. First, you need to change the /etc/apt/sources.list file to tell APT to get its packages from Bullseye instead of Buster. If the file only contains references to Stable rather than explicit codenames, the change isn't even required, since Stable always refers to the latest released version of Debian. In both cases, the database of available packages must be refreshed with the apt update command or the refresh button in synaptic ([SectionÂ 6.2.1, âInitializationâ](sect.apt-get.en.md#sect.apt-update)).
			

NOTE Repository information changes

				When a new stable version of Debian is released, some fields in the Release and InRelease files of a repository change, like the Suite field. When this happens, downloading data from the repository is declined until the change is confirmed to ensure the user is prepared for it. To confirm the change use the --allow-releaseinfo-change or --allow-releaseinfo-change-field options for apt-get or the Acquire::AllowReleaseInfoChange configuration option.
			
				Once these new package sources are registered, you should first do a minimal upgrade with apt upgrade et al. as described in [SectionÂ 6.2.3, âSystem Upgradeâ](sect.apt-get.en.md#sect.apt-upgrade). By doing the upgrade in two steps, we ease the job of the package management tools and often ensure that we have the latest versions of those, which might have accumulated bugfixes and improvements required to complete the full distribution upgrade.
			
				Once this first upgrade is done, it is time to handle the upgrade itself, either with apt full-upgrade, aptitude, or synaptic ([SectionÂ 6.7, âUpgrading from One Stable Distribution to the Nextâ](sect.dist-upgrade.en.md)). You should carefully check the suggested actions before applying them: you might want to add suggested packages or deselect packages which are only recommended and known not to be useful. In any case, the frontend should come up with a scenario ending in a coherent and up-to-date Bullseye system. Then, all you need is to do is wait while the required packages are downloaded, answer the debconf questions and possibly those about locally modified configuration files, and sit back while APT does its magic.
			

### 6.7.2.Â Handling Problems after an Upgrade

				In spite of the Debian maintainers' best efforts, a major system upgrade isn't always as smooth as you could wish. New software versions may be incompatible with previous ones (for instance, their default behavior or their data format may have changed). Also, some bugs may slip through the cracks despite the testing phase which always precedes a Debian release.
			
				To anticipate some of these problems, you can install the apt-listchanges package, which displays information about possible problems at the beginning of a package upgrade. This information is compiled by the package maintainers and put in /usr/share/doc/package/NEWS.Debian files for the benefit of users. Reading these files (possibly through apt-listchanges) should help you avoid bad surprises.
			
				You might sometimes find that the new version of a software doesn't work at all. This generally happens if the application isn't particularly popular and hasn't been tested enough; a last-minute update can also introduce regressions which are only found after the stable release. In both cases, the first thing to do is to have a look at the bug tracking system at [https://bugs.debian.org/package](https://bugs.debian.org), and check whether the problem has already been reported. If this is case, it will be also listed before the upgrade begins if you have apt-listbugs installed. If it hasn't, you should report it yourself with reportbug. If it is already known, the bug report and the associated messages are usually an excellent source of information related to the bug:
			
						sometimes a patch already exists, and it is available on the bug report; you can then recompile a fixed version of the broken package locally (see [SectionÂ 15.1, âRebuilding a Package from its Sourcesâ](debian-packaging.en.md#sect.rebuilding-package));
					
						in other cases, users may have found a workaround for the problem and shared their insights about it in their replies to the report;
					
						in yet other cases, a fixed package may have already been prepared and made public by the maintainer.
					
				Depending on the severity of the bug, a new version of the package may be prepared specifically for a new revision of the stable release. When this happens, the fixed package is made available in the proposed-updates section of the Debian mirrors (see [SectionÂ 6.1.2.3, âProposed Updatesâ](apt.en.md#sect.apt-sources.list.stable.proposed-updates)). The corresponding entry can then be temporarily added to the sources.list file, and updated packages can be installed with apt or aptitude.
			
				Sometimes the fixed package isn't available in this section yet because it is pending a validation by the Stable Release Managers. You can verify if that is the case on their web page. Packages listed there aren't available yet, but at least you know that the publication process is ongoing. âÂ [https://release.debian.org/proposed-updates/stable.html](https://release.debian.org/proposed-updates/stable.html)

### 6.7.3.Â Cleaning Up after an Upgrade

				APT usually ensures a clean upgrade, pulling in new and updated dependencies, or removing conflicting packages. But even being such a great tool, it cannot cover all tasks users and administrators will face after an upgrade, because they require a human decision.
			

#### 6.7.3.1.Â Packages removed from the Debian Archive

					Sometimes the Debian ftpmasters remove packages from the Debian archive, because they contain release critical bugs, were abandoned by their upstream author or their package maintainer, or simply reached their end of life. In this case a newer Debian release does not ship the package anymore. To find all packages, which do not have a package source, use the apt-show-versions command:
				

```bash
$ apt-show-versions | grep "No available version"
```

					A similar result can be achieved by aptitude search ~o. If the packages found are not required anymore, they should be purged from the system, because they will not face any updates for critical or security related bugs anymore.
				

#### 6.7.3.2.Â Dummy and Transitional Packages

					Sometimes, it might be necessary for a package to get a new name. In this case often the old package is kept as an (almost) empty package, depending on the new one and installing only the mandatory files in /usr/share/doc/package/. Such packages are called "dummy" or "transitional" packages. If the package maintainer in charge also changed the section of this package to oldlibs, then tools like aptitude, deboprhan, or debfoster (see sidebar [ALTERNATIVE deborphan and debfoster](sect.apt-get.en.md#sidebar.deborphan-debfoster)) can pickup these packages to suggest their removal.
				
					Unfortunately there is currently no foolproof way of making sure that these packages are automatically removed or picked by the tools mentioned above. One way to check if the system still has some of these packages installed, is to look through the package descriptions of installed packages and then check the results. Be careful not to schedule the results for automatic removal, because this method can lead to false positives:
				

```bash
$ dpkg -l | grep ^ii | grep -i -E "(transition|dummy)"
```

					Because the new package is pulled in as a dependency of the transitional package, it is usually marked as automatically installed and might be scheduled for removal if you try to purge the transitional package from your system. In this case you can use either of the approaches described in sidebar [TIP Removing and installing at the same time](sect.apt-get.en.md#sidebar.apt-install-and-remove) and [SectionÂ 6.2.7, âTracking Automatically Installed Packagesâ](sect.apt-get.en.md#sect.automatic-tracking) to selectively remove the transitional package.
				

#### 6.7.3.3.Â Old or Unused Configuration Files

					If the upgrade was successful there might be some configuration file cruft, either from dpkg (see [SectionÂ 5.2.3, âChecksums, List of Configuration Files, et al.â](sect.package-meta-information.en.md#sect.conffiles)), ucf or from removed packages. The latter can be [purged](sect.package-meta-information.en.md#sidebar.purge) by using apt autoremove --purge. The configuration files that were handled by dpkg or ucf during the upgrade process have left some counterparts with a dedicated suffix, e.g. .dpkg-dist, .dpkg-old, .ucf-old. Using the find or locate command can help to track them down. If they are no longer of any use, they can be deleted.
				

#### 6.7.3.4.Â Files not owned by any Package

					The Debian policy enforces that packages don't leave files behind when they are purged. Violating this principle is a serious bug and you will rarely encounter it. If you do, report it; and if you are curious though, you can use the cruft or cruft-ng package to check your system for files not owned by any package.
				[Prev6.6.Â Checking Package Authenticity](sect.package-authentication.en.md)[Up](#)[Home](index.en.md)[Next6.8.Â Keeping a System Up to Date](sect.regular-upgrades.en.md)