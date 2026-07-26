---
title: "1.3.3. Teams, Blends, and Sub-Projects"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.debian-internals.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.debian-internals.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "1.3.3. Teams, Blends, and Sub-Projects"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.foundation-documents.en.md)The Debian Administrator's Handbook[Next](sect.follow-debian-news.en.md)

## 1.3.Â The Inner Workings of the Debian Project

			The abundant end results produced by the Debian project derive simultaneously from the work on the infrastructure performed by experienced Debian developers, from the individual or collective work of developers on Debian packages, and from user feedback.
		

### 1.3.1.Â The Debian Developers

				Debian developers have various responsibilities, and as official project members, they have great influence on the direction the project takes. A Debian developer is generally responsible for at least one package, but according to their available time and desire, they are free to become involved in numerous teams and projects, thus acquiring more responsibilities within the project. âÂ [https://www.debian.org/devel/people](https://www.debian.org/devel/people) âÂ [https://www.debian.org/intro/organization](https://www.debian.org/intro/organization) âÂ [https://wiki.debian.org/Teams](https://wiki.debian.org/Teams)

TOOL Developer's database

				Debian has a database including all developers registered with the project, and their relevant information (address, telephone, geographical coordinates such as longitude and latitude, etc.). Some of the information (first and last name, country, username within the project, IRC username, GnuPG key, etc.) is public and available on the Web. âÂ [https://db.debian.org/](https://db.debian.org/)

				The geographical coordinates allow the creation of a map locating all of the developers around the globe. Debian is truly an international project: its developers can be found on all continents, although the majority are in âWestern countriesâ.
			

FigureÂ 1.1.Â World-wide distribution of Debian developers

				Package maintenance is a relatively regimented activity, very documented or even regulated. It must, in effect, comply with all the standards established by the Debian Policy. Fortunately, there are many tools that facilitate the maintainer's work. The developer can, thus, focus on the specifics of their package and on more complex tasks, such as squashing bugs. âÂ [https://www.debian.org/doc/debian-policy/](https://www.debian.org/doc/debian-policy/)

BACK TO BASICS Package maintenance, the developer's work

				Maintaining a package entails, first, âpackagingâ a software. Specifically, this means to define the means of installation so that, once installed, this software will operate and comply with the rules that the Debian project sets for itself. The result of this operation is saved in a .deb file. Effective installation of the program will then require nothing more than extraction of this compressed archive and execution of some pre-installation or post-installation scripts contained therein.
			
				After this initial phase, the maintenance cycle truly begins: preparing updates to follow the latest version of the Debian Policy, fixing bugs reported by users, and including new âupstreamâ versions of the program which naturally continues to develop simultaneously. For instance, at the time of the initial packaging, a program was at version 1.2.3. After some months of development, the original authors release a new stable version, numbered 1.4.0. At this point, the Debian package maintainer should update the package, so that users can benefit from its latest stable version.
			
				The Policy, an essential element of the Debian Project, establishes the norms ensuring both the quality of the packages and perfect interoperability of the distribution. Thanks to this Policy, Debian remains consistent despite its gigantic size. This Policy is not fixed in stone, but continuously evolves thanks to proposals formulated on the [debian-policy@lists.debian.org](mailto:debian-policy@lists.debian.org) mailing list. Amendments that are agreed upon by all interested parties are accepted and applied to the text by a small group of maintainers who have no editorial responsibility (they only include the modifications agreed upon by the Debian developers that are members of the above-mentioned list). You can read current amendment proposals on the bug tracking system: âÂ [https://bugs.debian.org/debian-policy](https://bugs.debian.org/debian-policy)

COMMUNITY Policy editorial process

				Anyone can propose an amendment to the Debian Policy by submitting a bug report with a severity level of âwishlistâ against the debian-policy package. The process that then starts is documented in [https://www.debian.org/doc/debian-policy/ap-process.html](https://www.debian.org/doc/debian-policy/ap-process.html): if it is acknowledged that the problem must be resolved by creating a new rule in the Debian Policy, a discussion begins on the [debian-policy@lists.debian.org](mailto:debian-policy@lists.debian.org) mailing list until consensus is reached and a proposal issued. Someone then drafts a desired amendment and submits it for approval (in the form of a patch to review). As soon as two other developers approve the fact that the proposed amendment reflects the consensus reached in the previous discussion (they âsecondâ it), the proposal can be included in the official document by one of the debian-policy package maintainers. If the process fails at one of these stages, the maintainers close the bug, classifying the proposal as rejected.
			

DEBIAN POLICY The documentation

				Documentation for each package is stored in /usr/share/doc/package/. This directory often contains a README.Debian file describing the Debian specific adjustments made by the package maintainer. It is, thus, wise to read this file prior to any configuration, in order to benefit from their experience. We also find a changelog.Debian.gz file describing the changes made from one version to the next by the Debian maintainer. This is not to be confused with the changelog.gz file (or equivalent), which describes the changes made by the upstream developers. The copyright file includes information about the authors and the license covering the software. Finally, we may also find a file named NEWS.Debian.gz, which allows the Debian developer to communicate important information regarding updates; if apt-listchanges is installed, then these messages are automatically displayed. All other files are specific to the software in question. We especially would like to point out the examples sub-directory, which frequently contains examples of configuration files.
			
				The Policy provides considerable coverage of the technical aspects of packaging. The size of the project also raises organizational problems; these are dealt with by the Debian Constitution, which establishes a structure and means for decision making. In other words, a formal governance system.
			
				This constitution defines a certain number of roles and positions, plus responsibilities and authorities for each. It is particularly worth noting that Debian developers always have ultimate decision making authority by a vote of general resolution, wherein a qualified majority of three quarters (75%) of votes is required for significant alterations to be made (such as those with an impact on the Foundation Documents). However, developers annually elect a âleaderâ to represent them in meetings, and ensure internal coordination between varying teams. This election is always a period of intense discussions. The Debian Project leader's (DPL) role is not formally defined by any document: candidates for this post usually propose their own definition of the position. In practice, the leader's roles include serving as a representative to the media, coordinating between âinternalâ teams, and providing overall guidance to the project, within which the developers can relate: the views of the DPL are implicitly approved by the majority of project members.
			
				Specifically, the leader has real authority; their vote resolves tie votes; they can make any decision which is not already under the authority of someone else and can delegate part of their responsibilities.
			
				Since its inception, the project has been successively led by Ian Murdock, Bruce Perens, Ian Jackson, Wichert Akkerman, Ben Collins, Bdale Garbee, Martin Michlmayr, Branden Robinson, Anthony Towns, Sam Hocevar, Steve McIntyre, Stefano Zacchiroli, Lucas Nussbaum, Neill McGovern, Mehdi Dogguy, Chris Lamb, Sam Hartman, and Jonathan Carter.
			
				The constitution also defines a âtechnical committeeâ. This committee's essential role is to decide on technical matters when the developers involved have not reached an agreement between themselves. Otherwise, this committee plays an advisory role for any developer who fails to make a decision for which they are responsible. It is important to note that they only get involved when invited to do so by one of the parties in question.
			
				Finally, the constitution defines the position of âproject secretaryâ, who is in charge of the organization of votes related to the various elections and general resolutions.
			
				The âgeneral resolutionâ (GR) procedure is fully detailed in the constitution, from the initial discussion period to the final counting of votes. The most interesting aspect of that process is that when it comes to an actual vote, developers have to rank the different ballot options between them and the winner is selected with a [Condorcet method](https://en.wikipedia.org/wiki/Condorcet_method) (more specifically, the Schulze method). For further details see: âÂ [https://www.debian.org/devel/constitution](https://www.debian.org/devel/constitution)

CULTURE Flamewar, the discussion that catches fire

				A âflamewarâ is an exceedingly impassioned debate, which frequently ends up with people attacking each other once all reasonable argumentation has been exhausted on both sides. Certain themes are more frequently subject to polemics than others (the choice of text editor, âdo you prefer vi or emacs?â, is an old favorite). The matters often provoke very rapid e-mail exchanges due to the sheer number of people with an opinion on the matter (everyone) and the very personal nature of such questions.
			
				Nothing particularly useful generally comes from such discussions; the general recommendation is to stay out of such debates, and maybe rapidly skim through their content, since reading them in full would be too time-consuming.
			
				Even if this constitution establishes a semblance of democracy, the daily reality is quite different: Debian naturally follows the free software rules of the do-ocracy: the one who does things gets to decide how to do them. A lot of time can be wasted debating the respective merits of various ways to approach a problem; the chosen solution will be the first one that is both functional and satisfyingâ¦ which will come out of the time that a competent person put into it.
			
				This is the only way to earn one's stripes: do something useful and show that one has worked well. Many Debian âadministrativeâ teams operate by co-optation, preferring volunteers who have already effectively contributed and proved their competence. The public nature of the work of those teams makes it possible for new contributors to observe and start helping without any special privilege. This is why Debian is often described as a âmeritocracyâ.
			

CULTURE Meritocracy, the reign of knowledge

				Meritocracy is a form of government in which authority is exercised by those with the greatest merit. For Debian, merit is a measure of competence, which is, itself, assessed by observation of past actions by one or more others within the project (Stefano Zacchiroli, a former project leader, speaks of âdo-ocracyâ, meaning âpower to those who get things doneâ). Their simple existence proves a certain level of competence; their achievements generally being free software, with available source code, which can be easily reviewed by peers to assess their quality.
			
				This effective operational method guarantees the quality of contributors in the âkeyâ Debian teams. This method is by no means perfect and occasionally there are those who do not accept this way of operating. The selection of developers accepted in the teams may appear a bit arbitrary, or even unfair. Furthermore, not everybody has the same definition of the service expected from these teams. For some, it is unacceptable to have to wait eight days for inclusion of a new Debian package, while others will wait patiently for three weeks without a problem. As such, there are regular complaints from the disgruntled about the âquality of serviceâ from some teams.
			

COMMUNITY Integration of new maintainers

				The team in charge of admitting new developers is the most regularly criticized. One must acknowledge that, throughout the years, the Debian project has become more and more demanding of the developers that it will accept. Some people may see some injustice in that, but we must confess that what were only little challenges at the beginning have become much greater in a community of over 1,000 people, when it comes to ensuring the quality and integrity of everything that Debian produces for its users.
			
				Furthermore, the acceptance procedure is concluded by review of the candidacy by a small team, the Debian Account Managers. These managers are, thus, particularly exposed to criticism, since they have final say in the inclusion or rejection of a volunteer within the Debian developers community. In practice, sometimes they must delay the acceptance of a person until they have learned more about the operations of the project. One can, of course, contribute to Debian before being accepted as an official developer, by being sponsored by current developers.
			

### 1.3.2.Â The Active Role of Users

				One might wonder if it is relevant to mention the users among those who work within the Debian project, but the answer is a definite yes: they play a critical role in the project. Far from being âpassiveâ, some users run development versions of Debian and regularly file bug reports to indicate problems. Others go even further and submit ideas for improvements, by filing a bug report with a severity level of âwishlistâ, or even submit corrections to the source code, called âpatchesâ (see [SectionÂ 1.3.2.3, âSending fixesâ](sect.debian-internals.en.md#sect.patch)).
			

#### 1.3.2.1.Â Reporting bugs

					The fundamental tool for submitting bugs in Debian is the Debian Bug Tracking System (Debian BTS), which is used by large parts of the project. The public part (the web interface) allows users to view all bugs reported, with the option to display a sorted list of bugs selected according to various criteria, such as: affected package, severity, status, address of the reporter, address of the maintainer in charge of it, tag, etc. It is also possible to browse the complete historical listing of all discussions regarding each of the bugs.
				
					Below the surface, the Debian BTS is e-mail based: all information that it stores comes from messages sent by the various persons involved. Any e-mail sent to [12345@bugs.debian.org](mailto:12345@bugs.debian.org) will, thus, be assigned to the history for bug number 12345. Authorized persons may âcloseâ a bug by writing a message describing the reasons for the decision to close to [12345-done@bugs.debian.org](mailto:12345-done@bugs.debian.org) (a bug is closed when the indicated problem is resolved or no longer relevant). A new bug is reported by sending an e-mail to [submit@bugs.debian.org](mailto:submit@bugs.debian.org) according to a specific format which identifies the package in question. The address [control@bugs.debian.org](mailto:control@bugs.debian.org) allows editing of all the âmeta-informationâ related to a bug.
				
					The Debian BTS has other functional features, as well, such as the use of tags for labeling bugs. For more information, see âÂ [https://www.debian.org/Bugs/](https://www.debian.org/Bugs/)

VOCABULARY Severity of a bug

					The severity of a bug formally assigns a degree of gravity to the reported problem. Effectively, not all bugs have the same importance; for instance, a typo in a manual page is not comparable to a security vulnerability in server software.
				
					Debian uses an extended scale to describe the severity of a bug. Each level is defined precisely in order to facilitate the selection thereof. âÂ [https://www.debian.org/Bugs/Developer#severities](https://www.debian.org/Bugs/Developer#severities)

					Users can also use the command line to send bug reports on a Debian package with the reportbug tool. It helps making sure the bug in question hasn't already been filed, thus preventing redundancy in the system. It reminds the user of the definitions of the severity levels, for the report to be as accurate as possible (the developer can always fine-tune these parameters later, if needed). It helps writing a complete bug report, without the user needing to know the precise syntax, by writing it and allowing the user to edit it. This report will then be sent via an e-mail server (by default, a remote one run by Debian, but reportbug can also use a local server).
				
					This tool first targets the development versions, which is where the bugs will be fixed. Effectively, changes are not welcome in a stable version of Debian, with very few exceptions for security updates or other important updates (if, for example, a package is not working at all). A correction of a minor bug in a Debian package must, thus, wait for the next stable version.
				

#### 1.3.2.2.Â Translation and documentation

					Additionally, numerous satisfied users of the service offered by Debian like to make a contribution of their own to the project. As not everyone has appropriate levels of expertise in programming, they may choose to assist with the translation and review of documentation. There are language-specific mailing lists to coordinate this work. âÂ [https://lists.debian.org/i18n.html](https://lists.debian.org/i18n.html) âÂ [https://www.debian.org/international/](https://www.debian.org/international/)

BACK TO BASICS What are i18n and l10n?

					âi18nâ and âl10nâ are the abbreviations for the words âinternationalizationâ and âlocalizationâ, respectively, preserving the initial and last letter of each word, and the number of letters in the middle.
				
					To âinternationalizeâ a program consists of modifying it so that it can be translated (localized). This involves partially rewriting a program initially written to work in one language in order to be able to open it to all languages.
				
					To âlocalizeâ a program consists of translating the original messages (frequently in English) to another language. For this, it must have already been internationalized.
				
					In summary, internationalization prepares the software for translation, which is then executed by localization.
				

#### 1.3.2.3.Â Sending fixes

					More advanced users might be able to provide a fix to a program by sending a patch.
				
					A patch is a file describing changes to be made to one or more reference files. Specifically, it will contain a list of lines to be removed or added to the code, as well as (sometimes) lines taken from the reference text, replacing the modifications in context (they allow identification of the placement of the changes if the line numbers have changed).
				
					The tool used for applying the modifications given in such a file is simply called patch. The tool that creates it is called diff, and is used as follows:
				

```bash
$ diff -u file.old file.new >file.patch
```

					The file.patch file contains the instructions for changing the content of file.old into file.new. We can send it to someone, who can then use it to recreate file.new from the two others, like this:
				

```bash
$ patch -p0 file.old <file.patch
```

					The file, file.old, is now identical to file.new.
				
					In practice, most software is currently maintained in Git repositories and contributors are thus more likely to use git to retrieve the source code and propose changes. git diff will generate a file in the same format as what diff -u would do and git apply can do the same as patch.
				

CULTURE Git

					Git is a tool for collaborative work on multiple files, while maintaining a history of modifications. The files in question are generally text files, such as a program's source code. If several people work together on the same file, git can only merge the alterations made if they were made to different portions of the file. Otherwise, these âconflictsâ must be resolved by hand.
				
					Git is a distributed system where each user has a repository with the complete history of changes. Central repositories are used to download the project (git clone) and to share the work done with others (git push). The repository can contain multiple versions of the files but only one version can be worked on at a given time: it is called the working copy (it can be changed to point to another version with git checkout). Git can show you the modifications made to the working copy (git diff), can stage changes for inclusion (git add), and can create a new entry in the version's history (git commit). It can also update the working copy to include modifications made in parallel by other users (git pull), and can record a particular configuration in the history in order to be able to easily extract it later on (git tag).
				
					Git makes it easy to handle multiple concurrent versions of a project in development without them interfering with each other. These versions are called branches. This metaphor of a tree is fairly accurate, since a program is initially developed on a common trunk. When a milestone has been reached (such as version 1.0), development continues on two branches: the development branch prepares the next major release, and the maintenance branch manages updates and fixes for version 1.0.
				
					Git is, nowadays, the most popular version control system but it is not the only one. Historically, CVS (Concurrent Versions System) was the first widely used tool but its numerous limitations contributed to the appearance of more modern free alternatives. These include, especially, Subversion (svn), Git (git), Bazaar (bzr), Fossil (fossil), and Mercurial (hg). âÂ [https://www.nongnu.org/cvs/](https://www.nongnu.org/cvs/) âÂ [https://subversion.apache.org/](https://subversion.apache.org/) âÂ [https://git-scm.com/](https://git-scm.com/) âÂ [https://bazaar.canonical.com/](https://bazaar.canonical.com/) âÂ [http://mercurial.selenic.com/](http://mercurial.selenic.com/)

					It is beyond the scope of this book to provide a detailed explanation about Git. For that, you can refer to the Pro Git book. âÂ [https://git-scm.com/book/](https://git-scm.com/book/)

					While the output of git diff is a file that can be shared with other developers, there are usually better ways to submit changes. If the developers prefer to get patches by email, they usually want patches generated with git format-patch so that they can be directly integrated in the repository with git am. This preserves commits meta-information and makes it possible to share multiple commits at once.
				
					This email-based workflow is still popular but it tends to be replaced by the usage of merge requests (or pull requests) whenever the software is hosted in a platform like GitHub or GitLab â and Debian is using GitLab on its salsa.debian.org server. On those systems, once you have created an account, you fork the repository, effectively creating a copy of the repository in your own account, and you can then clone that repository and push your own changes in it. From there, the web interface will suggest you to submit a merge request, notifying the developers of your changes, making it easy for them to review and accept your changes with a single click.
				

#### 1.3.2.4.Â Other ways of contributing

					All of these contribution mechanisms are made more efficient by users' behavior. Far from being a collection of isolated persons, users are a true community within which numerous exchanges take place. We especially note the impressive activity on the user discussion mailing list, [debian-user@lists.debian.org](mailto:debian-user@lists.debian.org) ([ChapterÂ 7, Solving Problems and Finding Relevant Information](solving-problems.en.md) discusses this in greater detail). âÂ [https://lists.debian.org/users.html](https://lists.debian.org/users.html)

					Not only do users help themselves (and others) on technical issues that directly affect them, but they also discuss the best ways to contribute to the Debian project and help it move forward â discussions that frequently result in suggestions for improvements.
				

TOOL how-can-i-help

					The how-can-i-help program lists opportunities for contributing to Debian packages that are installed locally. After each APT invocation (see [ChapterÂ 6, Maintenance and Updates: The APT Tools](apt.en.md)), it shows ways to help, by highlighting bugs tagged ânewcomerâ (which are easy entry-points for new contributors) or orphaned packages that need a new maintainer. The program can be executed directly as well.
				
					Since Debian does not expend funds on any self-promoting marketing campaigns, its users play an essential role in its diffusion, ensuring its fame via word-of-mouth.
				
					This method works quite well, since Debian fans are found at all levels of the free software community: from install parties (workshops where seasoned users assist newcomers to install the system) organized by local LUGs or âLinux User Groupsâ, to association booths at large tech conventions dealing with Linux, etc.
				
					Volunteers make posters, brochures, stickers, and other useful promotional materials for the project, which they make available to everyone, and which Debian provides freely on its website and on its wiki: âÂ [https://www.debian.org/events/material](https://www.debian.org/events/material)

### 1.3.3.Â Teams, Blends, and Sub-Projects

				From the start, Debian has been organized around the concept of source packages, each with its maintainer or group of maintainers. Many work teams have emerged over time, ensuring administration of the infrastructure, management of tasks not specific to any package in particular (quality assurance, Debian Policy, installer, etc.), with the latest series of teams growing up around sub-projects and blends.
			

#### 1.3.3.1.Â Existing Debian Sub-Projects and Blends

					To each their own Debian! A sub-project is a group of volunteers interested in adapting Debian to specific needs. Beyond the selection of a sub-group of programs intended for a particular domain (education, medicine, multimedia creation, etc.), sub-projects are also involved in improving existing packages, packaging missing software, adapting the installer, creating specific documentation, and more. While a "blend" might not be exactly the same, it works quite similar and also tries to provide a solution for groups of people intending to use Debian for a particular domain. One could say that "Debian Pure Blends" is the successor of sub-projects. âÂ [https://www.debian.org/blends/](https://www.debian.org/blends/)

VOCABULARY Sub-projects, blends, and derivative distribution

					The development process for a derivative distribution consists in starting with a particular version of Debian and making a number of modifications to it. The infrastructure used for this work is completely external to the Debian project. There isn't necessarily a policy for contributing improvements. This difference explains how a derivative distribution may âdivergeâ from its origins, and why they have to regularly resynchronize with their source in order to benefit from improvements made upstream.
				
					On the other hand, a sub-project or blend can not diverge, since all the work on it consists of directly improving Debian in order to adapt it to a specific goal.
				
					The most known distribution derived from Debian is, without a doubt, Ubuntu, but there are many. See [AppendixÂ A, Derivative Distributions](derivative-distributions.en.md) to learn about their particularities and their positioning in relationship to Debian.
				
					Here is a small selection of current released Debian Pure Blends:
				
							Debian Junior, by Ben Armstrong, offering an appealing and easy to use Debian system for children;
						
							Debian Edu, by Petter Reinholdtsen, focused on the creation of a specialized distribution for the academic and educational world;
						
							Debian Med, by Andreas Tille, dedicated to the medical field;
						
							Debian Multimedia, which deals with audio and multimedia work;
						
							Debian GIS, which takes care of Geographical Information Systems applications and users;
						
							Debian Astro, for both professionals and hobby astronomers;
						
							Debian Science, working on providing researchers and scientists a better experience using Debian;
						
							Freedombox, made to develop, design and promote personal servers running free software for private, personal communications;
						
							Debian Games, providing games in Debian from arcade and adventure to simulation and strategy;
						
							DebiChem, targeted at Chemistry, provides chemical suites and programs.
						
					The number of projects will most likely continue to grow with time and improved perception of the advantages of Debian Pure Blends. Fully supported by the existing Debian infrastructure, they can, in effect, focus on work with real added value, without worrying about remaining synchronized with Debian, since they are developed within the project.
				

#### 1.3.3.2.Â Administrative Teams

					Most administrative teams are relatively closed and recruit only by co-optation. The best means to become a part of one is to intelligently assist the current members, demonstrating that you have understood their objectives and methods of operation.
				
					The ftpmasters are in charge of the official archive of Debian packages. They maintain the program that receives packages sent by developers and automatically stores them, after some checks, on the reference server (ftp-master.debian.org).
				
					They must also verify the licenses of all new packages, in order to ensure that Debian may distribute them, prior to including them in the corpus of existing packages. When a developer wishes to remove a package, they address this team through the bug tracking system and the ftp.debian.org âpseudo-packageâ.
				

VOCABULARY The pseudo-package, a monitoring tool

					The bug tracking system, initially designed to associate bug reports with a Debian package, has proved very practical to manage other matters: lists of problems to be resolved or tasks to manage without any link to a particular Debian package. The âpseudo-packagesâ allow, thus, certain teams to use the bug tracking system without associating a real package with their team. Everyone can, thus, report issues that needs to be dealt with. For instance, the BTS has a ftp.debian.org entry that is used to report and track problems on the official package archive or simply to request removal of a package. Likewise, the www.debian.org pseudo-package refers to errors on the Debian website, and lists.debian.org gathers all the problems concerning the mailing lists.
				

TOOL GitLab, Git repository hosting and much more

					A GitLab instance, known as salsa.debian.org, is used by Debian to host the Git packaging repositories; but this software offers much more than simple hosting and Debian contributors have been quick to leverage the continuous integration features (running tests, or even building packages, on each push). Debian contributors also benefit from a cleaner contribution workflow thanks the well understood merge request process (similar to GitHub's pull requests).
				
					GitLab replaced FusionForge (which was running on a service known as alioth.debian.org) for collaborative package maintenance. This service is administered by Alexander Wirt, Bastian Blank and JÃ¶rg Jaspert. âÂ [https://salsa.debian.org/](https://salsa.debian.org/) âÂ [https://wiki.debian.org/Salsa/Doc](https://wiki.debian.org/Salsa/Doc)

					The Debian System Administrators (DSA) team ([debian-admin@lists.debian.org](mailto:debian-admin@lists.debian.org)), as one might expect, is responsible for system administration of the many servers used by the project. They ensure optimal functioning of all base services (DNS, Web, e-mail, shell, etc.), install software requested by Debian developers, and take all precautions in regards to security. âÂ [https://dsa.debian.org](https://dsa.debian.org)

TOOL Debian Package Tracker

					This is one of RaphaÃ«l's creations. The basic idea is, for a given package, to centralize as much information as possible on a single page. Thus, one can quickly check the status of a program, identify tasks to be completed, and offer one's assistance. This is why this page gathers all bug statistics, available versions in each distribution, progress of a package in the Testing distribution, the status of translations of descriptions and debconf templates, the possible availability of a new upstream version, notices of noncompliance with the latest version of the Debian Policy, information on the maintainer, and any other information that said maintainer wishes to include. âÂ [https://tracker.debian.org/](https://tracker.debian.org/)

					An e-mail subscription service completes this web interface. It automatically sends the following selected information to the list: bugs and related discussions, availability of a new version on the Debian servers, new translations available for proofreading, etc.
				
					Advanced users can, thus, follow all of this information closely and even contribute to the project, once they have got a good enough understanding of how it works.
				
					Another web interface, known as the Debian Developer's Packages Overview (DDPO), provides each developer a synopsis of the status of all Debian packages placed under their charge. âÂ [https://qa.debian.org/developer.php](https://qa.debian.org/developer.php)

					These two websites are tools developed and managed by the group responsible for quality assurance within Debian (known as Debian QA).
				
					The listmasters administer the e-mail server that manages the mailing lists. They create new lists, handle bounces (delivery failure notices), and maintain spam filters (unsolicited bulk e-mail).
				

CULTURE Traffic on the mailing lists: some figures

					The mailing lists are, without a doubt, the best testimony to activity on a project, since they keep track of everything that happens. The numbers (from April 2021) regarding our mailing lists speak for themselves: Debian hosts about 325 lists, totaling over 295,000 individual subscriptions. 396,000 e-mails are delivered every day.
				
					Each specific service has its own administration team, generally composed of volunteers who have installed it (and also frequently programmed the corresponding tools themselves). This is the case for the bug tracking system (BTS), the package tracker, salsa.debian.org (GitLab server, see sidebar [TOOL GitLab, Git repository hosting and much more](sect.debian-internals.en.md#sidebar.gitlab)), the services available on qa.debian.org, lintian.debian.org, buildd.debian.org, cdimage.debian.org, etc.
				

#### 1.3.3.3.Â Development Teams, Transversal Teams

					Unlike administrative teams, the development teams are rather widely open, even to outside contributors. Even if Debian does not have a vocation to create software, the project needs some specific programs to meet its goals. Of course, developed under a free software license, these tools make use of methods proven elsewhere in the free software world.
				
					Debian has developed little software of its own, but certain programs have assumed a starring role, and their fame has spread beyond the scope of the project. Good examples are dpkg, the Debian package management program (it is, in fact, an abbreviation of Debian PacKaGe, and generally pronounced as âdee-packageâ), and apt, a tool to automatically install any Debian package, and its dependencies, guaranteeing the consistency of the system after an upgrade (its name is an acronym for Advanced Package Tool). Their teams are, however, much smaller, since a rather high level of programming skill is required to gain an overall understanding of the operations of these types of programs.
				
					The most important team is probably that for the Debian installation program, debian-installer, which has accomplished a work of momentous proportions since its conception in 2001. Numerous contributors were needed, since it is difficult to write a single program able to install Debian on a dozen different architectures. Each one has its own mechanism for booting and its own bootloader. All of this work is coordinated on the [debian-boot@lists.debian.org](mailto:debian-boot@lists.debian.org) mailing list, under the direction of Cyril Brulebois. âÂ [https://www.debian.org/devel/debian-installer/](https://www.debian.org/devel/debian-installer/) âÂ [https://joeyh.name/blog/entry/d-i_retrospective/](https://joeyh.name/blog/entry/d-i_retrospective/)

					The (very small) debian-cd program team has an even more modest objective. Many âsmallâ contributors are responsible for their architecture, since the main developer can not know all the subtleties, nor the exact way to start the installer from the CD-ROM.
				
					Many teams must collaborate with others in the activity of packaging: [debian-qa@lists.debian.org](mailto:debian-qa@lists.debian.org) tries, for example, to ensure quality at all levels of the Debian project. The [debian-policy@lists.debian.org](mailto:debian-policy@lists.debian.org) list develops Debian Policy according to proposals from all over the place. The teams in charge of each architecture ([debian-architecture@lists.debian.org](mailto:debian-architecture@lists.debian.org)) compile all packages, adapting them to their particular architecture, if needed.
				
					Other teams manage the most important packages in order to ensure maintenance without placing too heavy a load on a single pair of shoulders; this is the case with the C library and [debian-glibc@lists.debian.org](mailto:debian-glibc@lists.debian.org), the C compiler on the [debian-gcc@lists.debian.org](mailto:debian-gcc@lists.debian.org) list, or Xorg on the [debian-x@lists.debian.org](mailto:debian-x@lists.debian.org) (this group is also known as the X Strike Force).
				[Prev1.2.Â The Foundation Documents](sect.foundation-documents.en.md)[Up](#)[Home](index.en.md)[Next1.4.Â Follow Debian News](sect.follow-debian-news.en.md)