---
title: "15.4.2. Acceptance Process"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.becoming-package-maintainer.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.becoming-package-maintainer.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "15.4.2. Acceptance Process"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.setup-apt-package-repository.en.md)The Debian Administrator's Handbook[Next](conclusion.en.md)

## 15.4.Â Becoming a Package Maintainer

### 15.4.1.Â Learning to Make Packages

				Creating a quality Debian package is not always a simple task, and becoming a package maintainer takes some learning, both with theory and practice in technical and legal matters. It is not a simple matter of building and installing software; rather, the bulk of the complexity comes from understanding the problems and conflicts, and more generally the interactions, with the myriad of other packages available.
			

#### 15.4.1.1.Â Rules

					A Debian package must comply with the precise rules compiled in the Debian policy, and each package maintainer must know them. There is no requirement to know them by heart, but rather to know they exist and to refer to them whenever a choice presents a non-trivial alternative. Every Debian maintainer has made mistakes by not knowing about a rule, but this is not a huge problem as long as the error gets fixed when a user reports it as a bug report (which tends to happen fairly soon thanks to advanced users). The Standards-Version field in debian/control specifies the version of the Debian policy with which a package complies. Maintainers should comply to the latest version of the Debian policy. âÂ [https://www.debian.org/doc/debian-policy/](https://www.debian.org/doc/debian-policy/)

#### 15.4.1.2.Â Procedures

					Debian is not a simple collection of individual packages. Everyone's packaging work is part of a collective project; being a Debian developer involves knowing how the Debian project operates as a whole. Every developer will, sooner or later, interact with others. The Debian Developer's Reference (in the developers-reference package) summarizes what every developer must know in order to interact as smoothly as possible with the various teams within the project, and to take the best possible advantages of the available resources. This document also enumerates a number of duties a developer is expected to fulfill. âÂ [https://www.debian.org/doc/manuals/developers-reference/](https://www.debian.org/doc/manuals/developers-reference/)

#### 15.4.1.3.Â Tools

					Many tools help package maintainers in their work. This section describes them quickly, but does not give the full details, since they all have comprehensive documentation of their own.
				

##### 15.4.1.3.1.Â devscripts

						The devscripts package contains many programs helping with a wide array of a Debian developer's job:
					
debuild allows generating a package (with dpkg-buildpackage) and running lintian to check its compliance with the Debian policy afterwards.
							
debclean cleans a source package after a binary package has been generated.
							
dch allows quick and easy editing of a debian/changelog file in a source package.
							
uscan checks whether a new version of a software has been released by the upstream author; this requires a debian/watch file with a description of the location of such releases.
							
debi allows installing (with dpkg -i) the Debian package that was just generated without the need to type its full name and path.
							
								In a similar fashion, debc allows scanning the contents of the recently-generated package (with dpkg -c), without needing to type its full name and path.
							
bts controls the bug tracking system from the command line; this program automatically generates the appropriate emails. âÂ [https://www.debian.org/Bugs/server-refcard](https://www.debian.org/Bugs/server-refcard)

debrelease uploads a recently-generated package to a remote server, without needing to type the full name and path of the related .changes file.
							
debsign signs the *.dsc and *.changes files.
							
uupdate automates the creation of a new revision of a package when a new upstream version has been released.
							
						All of the mentioned commands are documented in their respective manual pages. They can further be configured per user in one file: ~/.devscripts.
					

##### 15.4.1.3.2.Â debhelper and dh-make

						Debhelper is a set of scripts easing the creation of policy-compliant packages; these scripts are invoked from debian/rules. Debhelper has been widely adopted within Debian, as evidenced by the fact that it is used by the majority of official Debian packages. All the commands it contains have a dh_ prefix. Each of them is documented in a manual page. The different compatibility levels and common options are described in debhelper(7).
					
						The dh_make script (in the dh-make package) creates files required for generating a Debian package in a directory initially containing the sources for a piece of software. As can be guessed from the name of the program, the generated files use debhelper by default.
					

##### 15.4.1.3.3.Â lintian

						This tool is one of the most important: it is the Debian package checker. It is based on a large array of tests created from the Debian policy, and detects quickly and automatically many errors that can then be fixed before packages are released.
					
						This tool is only a helper, and it sometimes gets it wrong (for instance, since the Debian policy changes over time, lintian is sometimes outdated). It is also not exhaustive: not getting any Lintian error should not be interpreted as a proof that the package is perfect; at most, it avoids the most common errors.
					

##### 15.4.1.3.4.Â piuparts

						This is another important tool: it automates the installation, upgrade, removal and purge of a package (in an isolated environment), and checks that none of these operations leads to an error. It can help in detecting missing dependencies, and it also detects when files are incorrectly left over after the package got purged.
					

##### 15.4.1.3.5.Â autopkgtest

autopkgtest runs tests on binary packages, using the tests supplied in the source package in debian/tests/. Several commands allow the easy creation of chrooted or virtual test environments.
					

##### 15.4.1.3.6.Â reprotest

reprotest builds the same source code twice in different environments, and then checks the binaries produced by each build for differences. If any are found, then diffoscope (if unavailable, diff) is used to display them in detail for later analysis.
					

##### 15.4.1.3.7.Â dupload and dput

						The dupload and dput commands allow uploading a Debian package to a (possibly remote) server. This allows developers to publish their package on the main Debian server (ftp-master.debian.org) so that it can be integrated to the archive and distributed by mirrors. These commands take a .changes file as a parameter, and deduce the other relevant files from its contents.
					

##### 15.4.1.3.8.Â git-buildpackage and dgit

						The project has been using various version control systems over the years to store packaging efforts or package source code, or allow collaborative package maintenance. In an effort to unify the systems and efforts, it was ultimately decided in 2017 to move (almost) all package sources into Git ([CULTURE Git](sect.debian-internals.en.md#sidebar.git)) onto a Gitlab instance called salsa.debian.org. âÂ [https://salsa.debian.org/](https://salsa.debian.org/)

						To make packaging using Git easier for Debian developers, tools have been developed. These allow not only to store the packaging files in Git, but also to use the Git repositories (and their history) of software projects, put patches applied to package sources into Git history, maintain software versions per distribution, etc.
					
						One of the most famous packages is git-buildpackage. An alternative is dgit. Of course it is still possible to use neither of those.
					
						Below is an example for a ~/.gbp.conf configuration file
					

```bash
[DEFAULT]
builder = sbuild -d bullseye --build-dep-resolver=aptitude -s --source-only-changes --build-failed-commands "%SBUILD_SHELL"
pristine-tar = true

[buildpackage]
sign-tags = true
keyid = XXXX
postbuild  = autopkgtest --user debci --apt-upgrade -s "$GBP_CHANGES_FILE" -- lxc --sudo autopkgtest-bullseye-amd64 
export-dir = /tmp/build-area/
notify = off

[import-orig]
filter-pristine-tar = true
sign-tags = true

[pq]
drop = true
```

						Building the package is then as easy as running gbp buildpackage in the Git tree. It will start a package build in a Debian Bullseye chroot using sbuild. When the build succeeds, the created files are checked running the autopkgtest-testsuite (if defined). All the various options are explained in gbp.conf(5) and /etc/git-buildpackage/gbp.conf.
					
						All the tools mentioned so far have been included in the continuous integration (CI) process in the salsa.debian.org instance as well: âÂ [https://salsa.debian.org/salsa-ci-team/pipeline](https://salsa.debian.org/salsa-ci-team/pipeline)

### 15.4.2.Â Acceptance Process

				Becoming a âDebian developerâ is not a simple administrative matter. The process comprises several steps, and is as much an initiation as it is a selection process. In any case, it is formalized and well-documented, so anyone can track their progression on the website dedicated to the new member process. âÂ [https://nm.debian.org/](https://nm.debian.org/)

EXTRA Lightweight process for âDebian Maintainersâ

				âDebian Maintainerâ is another status that gives less privileges than âDebian developerâ but whose associated process is quicker. With this status, the contributors can maintain their own packages only. A Debian developer only needs to perform a check on an initial upload, and issue a statement to the effect that they trust the prospective maintainer with the ability to maintain the package on their own.
			

#### 15.4.2.1.Â Prerequisites

					All candidates are expected to have at least a working knowledge of the English language. This is required at all levels: for the initial communications with the examiner, of course, but also later, since English is the preferred language for most of the documentation; also, package users will be communicating in English when reporting bugs, and they will expect replies in English.
				
					The other prerequisite deals with motivation. Becoming a Debian developer is a process that only makes sense if the candidate knows that their interest in Debian will last for many months. The acceptance process itself may last for several months, and Debian needs developers for the long haul; each package needs permanent maintenance, and not just an initial upload.
				

#### 15.4.2.2.Â Registration

					The first (real) step consists in finding a sponsor or advocate; this means an official developer willing to state that they believe that accepting X would be a good thing for Debian. This usually implies that the candidate has already been active within the community, and that their work has been appreciated. If the candidate is shy and their work is not publicly touted, they can try to convince a Debian developer to advocate them by showing their work in a private way.
				
					At the same time, the candidate must generate a public/private RSA key pair with GnuPG, which should be signed by at least two official Debian developers. The signature authenticates the name on the key. Effectively, during a key signing party, each participant must show an official identification (usually an ID card or passport) together with their key identifiers. This step confirms the link between the human and the keys. This signature thus requires meeting in real life. If you have not yet met any Debian developers in a public free software conference, you can explicitly seek developers living nearby using the list on the following webpage as a starting point. âÂ [https://wiki.debian.org/Keysigning](https://wiki.debian.org/Keysigning)

					Once the registration on nm.debian.org has been validated by the advocate, an Application Manager is assigned to the candidate. The application manager will then drive the process through multiple pre-defined steps and checks.
				
					The first verification is an identity check. If you already have a key signed by two Debian developers, this step is easy; otherwise, the application manager will try and guide you in your search for Debian developers close by to organize a meet-up and a key signing.
				

#### 15.4.2.3.Â Accepting the Principles

					These administrative formalities are followed by philosophical considerations. The point is to make sure that the candidate understands and accepts the social contract and the principles behind Free Software. Joining Debian is only possible if one shares the values that unite the current developers, as expressed in the founding texts (and summarized in [ChapterÂ 1, The Debian Project](the-debian-project.en.md)).
				
					In addition, each candidate wishing to join the Debian ranks is expected to know the workings of the project, and how to interact appropriately to solve the problems they will doubtless encounter as time passes. All of this information is generally documented in manuals targeting the new maintainers, and in the Debian developer's reference. An attentive reading of this document should be enough to answer the examiner's questions. If the answers are not satisfactory, the candidate will be informed. They will then have to read (again) the relevant documentation before trying again. In the cases where the existing documentation does not contain the appropriate answer for the question, the candidate can usually reach an answer with some practical experience within Debian, or potentially by discussing with other Debian developers. This mechanism ensures that candidates get involved somewhat in Debian before becoming a full part of it. It is a deliberate policy, by which candidates who eventually join the project are integrated as another piece of an infinitely extensible jigsaw puzzle.
				
					This step is usually known as the Philosophy & Procedures (P&P for short) in the lingo of the developers involved in the new member process.
				

#### 15.4.2.4.Â Checking Skills

					Each application to become an official Debian developer must be justified. Becoming a project member requires showing that this status is legitimate, and that it facilitates the candidate's job in helping Debian. The most common justification is that being granted Debian developer status eases maintenance of a Debian package, but it is not the only one. Some developers join the project to contribute to porting to a specific architecture, others want to improve documentation, and so on.
				
					This step represents the opportunity for the candidate to state what they intend to do within the Debian project and to show what they have already done towards that end. Debian is a pragmatic project and saying something is not enough, if the actions do not match what is announced. Generally, when the intended role within the project is related to package maintenance, a first version of the prospective package will have to be validated technically and uploaded to the Debian servers by a sponsor among the existing Debian developers.
				

COMMUNITY Sponsoring

					Debian developers can âsponsorâ packages prepared by someone else, meaning that they publish them in the official Debian repositories after having performed a careful review. This mechanism enables external persons, who have not yet gone through the new member process, to contribute occasionally to the project. At the same time, it ensures that all packages included in Debian have always been checked by an official member.
				
					Finally, the examiner checks the candidate's technical (packaging) skills with a detailed questionnaire. Bad answers are not permitted, but the answer time is not limited. All the documentation is available and several attempts are allowed if the first answers are not satisfactory. This step does not intend to discriminate, but to ensure at least a modicum of knowledge common to new contributors.
				
					This step is known as the Tasks & Skills step (T&S for short) in the examiners' jargon.
				

#### 15.4.2.5.Â Final Approval

					At the very last step, the whole process is reviewed by a DAM (Debian Account Manager). The DAM will review all the information about the candidate that the examiner collected, and makes the decision on whether or not to create an account on the Debian servers. In cases where extra information is required, the account creation may be delayed. Refusals are rather rare if the examiner does a good job of following the process, but they sometimes happen. They are never permanent, and the candidate is free to try again at a later time.
				
					The DAM's decision is authoritative and (almost) without appeal, which explains why the people in that seat have often been criticized in the past.
				[Prev15.3.Â Creating a Package Repository for APT](sect.setup-apt-package-repository.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 16.Â Conclusion: Debian's Future](conclusion.en.md)