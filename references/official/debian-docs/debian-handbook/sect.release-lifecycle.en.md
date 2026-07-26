---
title: "1.6.5. TheOldstableandOldoldstableStatus"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.release-lifecycle.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.release-lifecycle.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "1.6.5. TheOldstableandOldoldstableStatus"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.role-of-distributions.en.md)The Debian Administrator's Handbook[Next](case-study.en.md)

## 1.6.Â Lifecycle of a Release

			The project will simultaneously have three to six different versions of each program, named Experimental, Unstable, Testing, Stable, Oldstable, and even Oldoldstable. Each one corresponds to a different phase in development. For a good understanding, let us take a look at a program's journey, from its initial packaging to inclusion in a stable version of Debian.
		

VOCABULARY Release

			The term âreleaseâ, in the Debian project, indicates a particular version of a distribution (e.g., âunstable releaseâ means âthe unstable versionâ). It also indicates the public announcement of the launch of any new version (stable).
		

### 1.6.1.Â The Experimental Status

				First let us take a look at the particular case of the Experimental distribution: this is a group of Debian packages corresponding to the software currently in development, and not necessarily completed, explaining its name. Not everything passes through this step; some developers add packages here in order to get feedback from more experienced (or braver) users.
			
				Otherwise, this distribution frequently houses important modifications to base packages, whose integration into Unstable with serious bugs would have critical repercussions. It is, thus, a completely isolated distribution, its packages never migrate to another version (except by direct, express intervention of the maintainer or the ftpmasters). It is also not self-contained: only a subset of the existing packages are present in Experimental, and it generally does not include the base system. This distribution is therefore mostly useful in combination with another, self-contained, distribution such as Unstable.
			

### 1.6.2.Â The Unstable Status

				Let us turn back to the case of a typical package. The maintainer creates an initial package, which they compile for the Unstable version and place on the ftp-master.debian.org server. This first event involves inspection and validation from the ftpmasters. The software is then available in the Unstable distribution, which is the âcutting edgeâ distribution chosen by users who are more concerned with having up-to-date packages than worried about serious bugs. They discover the program and then test it.
			
				If they encounter bugs, they report them to the package's maintainer. The maintainer then regularly prepares corrected versions, which they upload to the server.
			
				Every newly updated package is updated on all Debian mirrors around the world within six hours. The users then test the corrections and search for other problems resulting from the modifications. Several updates may then occur rapidly. During these times, autobuilder robots come into action. The maintainer uploads the package sources (without any precompiled package). The autobuilders take over and automatically compile versions for all supported architectures. Some compilations may fail; the maintainer will then receive a bug report indicating the problem, which is then to be corrected in the next versions. When the bug is discovered by a specialist for the architecture in question, the bug report may come with a patch ready to use.
			

FigureÂ 1.2.Â Compilation of a package by the autobuilders

QUICK LOOK buildd, the Debian package recompiler

buildd is the abbreviation of âbuild daemonâ. This program automatically recompiles new versions of Debian packages on the architectures on which it is hosted (cross-compilation is avoided as much as possible).
			
				Thus, to produce binaries for the arm64 architecture, the project has arm64 machines available. The buildd program runs on them continuously and creates binary packages for arm64 from source packages sent by Debian developers.
			
				This software is used on all the computers serving as autobuilders for Debian. By extension, the term buildd frequently is used to refer to these machines, which are generally reserved solely for this purpose.
			

### 1.6.3.Â Migration to Testing

				A bit later, the package will have matured; compiled on all the architectures, it will not have undergone recent modifications. It is then a candidate for inclusion in the Testing distribution â a group of Unstable packages chosen according to some quantifiable criteria. Every day a program automatically selects the packages to include in Testing, according to elements guaranteeing a certain level of quality:
			
						successful compilation on all officially supported architectures;
					
						lack of critical bugs, or, at least fewer than the version currently included in Testing;
					
						at least 5 days spent in Unstable, which is usually sufficient time to find and report any serious problems (successfully passing the package's own test suite, if it has one, reduces that time);
					
						dependencies that can be satisfied in Testing, or that can at least be moved there together with the package in question;
					
						automatic quality tests of the package (autopkgtest) â if defined â don't show any regression.
					
				This system is clearly not infallible; critical bugs are regularly found in packages included in Testing. Still, it is generally effective, and Testing poses far fewer problems than Unstable, being for many, a good compromise between stability and novelty.
			

NOTE Limitations of Testing

				While very interesting in principle, Testing does have some practical problems: the tangle of cross-dependencies between packages is such that a package can rarely move there completely on its own. With packages all depending upon each other, it is sometimes necessary to migrate a large number of packages simultaneously, which is impossible when some are uploading updates regularly. On the other hand, the script identifying the families of related packages works hard to create them (this would be an NP-complete problem, for which, fortunately, we know some good heuristics). This is why we can manually interact with and guide this script by suggesting groups of packages, or imposing the inclusion of certain packages in a group, even if this temporarily breaks some dependencies. This functionality is accessible to the Release Managers and their assistants.
			
				Recall that an NP-complete problem is of an exponential algorithmic complexity according to the size of the data, here being the length of the code (the number of figures) and the elements involved. The only way to resolve it is frequently to examine all possible configurations, which could require enormous means. A heuristic is an approximate, but satisfying, solution.
			

COMMUNITY The Release Manager

				Release Manager is an important title, associated with heavy responsibilities. The bearer of this title must, in effect, manage the release of a new, stable version of Debian, and define the process for development of Testing until it meets the quality criteria for Stable. They also define a tentative schedule (not always followed).
			
				We also have Stable Release Managers, often abbreviated SRM, who manage and select updates for the current stable version of Debian. They systematically include security patches and examine all other proposals for inclusion, on a case by case basis, sent by Debian developers eager to update their package in the stable version.
			

### 1.6.4.Â The Promotion from Testing to Stable

				Let us suppose that our package is now included in Testing. As long as it has room for improvement, its maintainer must continue to improve it and restart the process from Unstable (but its later inclusion in Testing is generally faster: unless it changed significantly, all of its dependencies are already available). When it reaches perfection, the maintainer has completed their work. The next step is the inclusion in the Stable distribution, which is, in reality, a simple copy of Testing at a moment chosen by the Release Manager. Ideally, this decision is made when the installer is ready, and when no program in Testing has any known critical bugs.
			
				Since this moment never truly arrives, in practice, Debian must compromise: remove packages whose maintainer has failed to correct bugs on time, or agree to release a distribution with some bugs in the thousands of programs. The Release Manager will have previously announced a freeze period, during which each update to Testing must be approved. The goal here is to prevent any new version (and its new bugs), and to only approve updates fixing bugs.
			

FigureÂ 1.3.Â A package's path through the various Debian versions

VOCABULARY Freeze: the home straight

				During the freeze period, development of the Testing distribution is blocked; no more automatic updates are allowed. Only the Release Managers are then authorized to change packages, according to their own criteria. The purpose is to prevent the appearance of new bugs by introducing new versions; only thoroughly examined updates are authorized when they correct significant bugs.
			
				After the release of a new stable version, the Stable Release Managers manage all further development (called ârevisionsâ, ex: 10.1, 10.2, 10.3 for versionÂ 10). These updates systematically include all security patches. They will also include the most important corrections (the maintainer of a package must prove the gravity of the problem that they wish to correct in order to have their updates included).
			
				At the end of the journey, our hypothetical package is now included in the stable distribution. This journey, not without its difficulties, explains the significant delays separating the Debian Stable releases. This contributes, over all, to its reputation for quality. Furthermore, the majority of users are satisfied using one of the three distributions simultaneously available. The system administrators, concerned above all about the stability of their servers, don't need the latest and greatest version of GNOME; they can choose Debian Stable, and they will be satisfied. End users, more interested in the latest versions of GNOME or KDE Plasma than in rock-solid stability, will find Debian Testing to be a good compromise between a lack of serious problems and relatively up-to-date software. Finally, developers and more experienced users may blaze the trail, testing all the latest developments in Debian Unstable right out of the gate, at the risk of suffering the headaches and bugs inherent in any new version of a program. To each their own Debian!
			

CULTURE GNOME and KDE Plasma, graphical desktop environments

				GNOME (originally an acronym for GNU Network Object Model Environment) and Plasma by KDE (formerly known as K Desktop Environment) are the two most popular/established graphical desktop environments in the free software world, and will be presented in greater detail in [SectionÂ 13.3, âGraphical Desktopsâ](sect.graphical-desktops.en.md).
			
				A desktop environment is a set of programs grouped together to allow easy management of the most common operations through a graphical interface. They generally include a file manager, office suite, web browser, e-mail program, multimedia accessories, etc. The most visible difference resides in the choice of the graphical library used: GNOME has chosen GTK+ (free software licensed under the LGPL), and the KDE community has selected Qt (a company-backed project, available nowadays both under the GPL and a commercial license). âÂ [https://www.gnome.org/](https://www.gnome.org/) âÂ [https://kde.org/](https://kde.org/)

FigureÂ 1.4.Â Chronological path of a program packaged by Debian

### 1.6.5.Â The Oldstable and Oldoldstable Status

				Each Stable release has an expected lifetime of about 5 years and given that releases tend to happen every 2 years, there can be up to 3 supported releases at a given point of time. When a new stable release happens, the former release becomes Oldstable and the one even before becomes Oldoldstable.
			
				This Long Term Support (LTS) of Debian releases is a recent initiative: individual contributors and companies joined forces to create the Debian LTS team. Older releases which are no longer supported by the Debian security team fall under the responsibility of this new team.
			
				The Debian security team handles security support in the current Stable release and also in the Oldstable release (but only for as long as is needed to ensure one year of overlap with the current stable release). This amounts roughly to three years of support for each release. The Debian LTS team handles the last (two) years of security support so that each release benefits from at least 5 years of support and so that users can upgrade from version N to N+2, for example, from Debian 9 Stretch to Debian 11 Bullseye. âÂ [https://wiki.debian.org/LTS](https://wiki.debian.org/LTS)

COMMUNITY Companies sponsoring the LTS effort

				Long Term Support is a difficult commitment to make in Debian because volunteers tend to avoid the work that is not very fun. And providing security support for 5 year old software is â for many contributors â a lot less fun than packaging new upstream versions or developing new features.
			
				To bring this project to life, the project counted on the fact that long term support was particularly relevant for companies and that they would be willing to mutualize the cost of this security support.
			
				The project started in June 2014: some organizations allowed their employees to contribute part-time to Debian LTS while others preferred to sponsor the project with money so that Debian contributors get paid to do the work that they would not do for free. Most Debian contributors willing to be paid to work on LTS got together to create a clear sponsorship offer managed by Freexian (RaphaÃ«l Hertzog's company): âÂ [https://www.freexian.com/services/debian-lts.html](https://www.freexian.com/services/debian-lts.html)

				In the Debian LTS team, the volunteers work on packages they care about while the paid contributors prioritize packages used by their sponsors.
			
				The project is always looking for new sponsors: What about your company? Can you let an employee work part-time on long term support? Can you allocate a small budget for security support? âÂ [https://wiki.debian.org/LTS/Funding](https://wiki.debian.org/LTS/Funding)
[Prev1.5.Â The Role of Distributions](sect.role-of-distributions.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 2.Â Presenting the Case Study](case-study.en.md)