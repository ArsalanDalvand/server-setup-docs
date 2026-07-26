---
title: "6.5.2.synaptic"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.apt-frontends.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.apt-frontends.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.5.2.synaptic"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.apt-file.en.md)The Debian Administrator's Handbook[Next](sect.package-authentication.en.md)

## 6.5.Â Frontends: aptitude, synaptic

			APT is a C++ program whose code mainly resides in the libapt-pkg shared library. Using a shared library facilitates the creation of user interfaces (frontends), since the code contained in the library can easily be reused. Historically, apt-get was only designed as a test frontend for libapt-pkg but its success tends to obscure this fact.
		

### 6.5.1.Â aptitude

aptitude is an interactive program that can be used in semi-graphical mode on the console. You can browse the list of installed and available packages, look up all the available information, and select packages to install or remove. The program is designed specifically to be used by administrators, so that its default behaviors are designed to be much more intelligent than apt-get's, and its interface much easier to understand.
			

FigureÂ 6.1.Â The aptitude package manager

				When it starts, aptitude shows a list of packages sorted by state (installed, non-installed, or installed but not available on the mirrors â other sections display tasks, virtual packages, and new packages that appeared recently on mirrors). To facilitate thematic browsing, other views are available. In all cases, aptitude displays a list combining categories and packages on the screen. Categories are organized through a tree structure, whose branches can respectively be unfolded or closed with the Enter, [ and ] keys. + should be used to mark a package for installation, - to mark it for removal and _ to purge it (note that these keys can also be used for categories, in which case the corresponding actions will be applied to all the packages of the category). u updates the lists of available packages and Shift+u prepares a global system upgrade. g switches to a summary view of the requested changes (and typing g again will apply the changes), and q quits the current view. If you are in the initial view, this will effectively close aptitude.
			

DOCUMENTATION aptitude

				This section does not cover the finer details of using aptitude. It rather focuses on giving you a survival kit to use it. But it is well documented and we advise you to use its complete manual available in the aptitude-doc-en package (see /usr/share/doc/aptitude/html/en/index.html) or at [https://www.debian.org/doc/manuals/aptitude/](https://www.debian.org/doc/manuals/aptitude/).
			
				To search for a package, you can type / followed by a search pattern. This pattern matches the name of the package, but can also be applied to the description (if preceded by ~d), to the section (with ~s) or to other characteristics detailed in the documentation. The same patterns can filter the list of displayed packages: type the l key (as in limit) and enter the pattern.
			
				Managing the âautomatic flagâ of Debian packages (see [SectionÂ 6.2.7, âTracking Automatically Installed Packagesâ](sect.apt-get.en.md#sect.automatic-tracking)) is a breeze with aptitude. It is possible to browse the list of installed packages and mark packages as automatic with Shift+m or to remove the mark with the m key. âAutomatic packagesâ are displayed with an âAâ in the list of packages. This feature also offers a simple way to visualize the packages in use on a machine, without all the libraries and dependencies that you don't really care about. The related pattern that can be used with l (to activate the filter mode) is ~i!~M. It specifies that you only want to see installed packages (~i) not marked as automatic (!~M).
			

TOOL Using aptitude on the command-line interface

				Most of aptitude's features are accessible via the interactive interface as well as via command-lines. These command-lines will seem familiar to regular users of apt-get and apt-cache.
			
				The advanced features of aptitude are also available on the command-line. You can use the same package search patterns as in the interactive version. For example, if you want to cleanup the list of âmanually installedâ packages, and if you know that none of the locally installed programs require any particular libraries or Perl modules, you can mark the corresponding packages as automatic with a single command:
			

```bash
# aptitude markauto '~slibs|~sperl'
```

				Here, you can clearly see the power of the search pattern system of aptitude, which enables the instant selection of all the packages in the libs and perl sections.
			
				Beware, if some packages are marked as automatic and if no other package depends on them, they will be removed immediately (after a confirmation request).
			

#### 6.5.1.1.Â Managing Recommendations, Suggestions and Tasks

					Another interesting feature of aptitude is the fact that it respects recommendations between packages while still giving users the choice not to install them on a case by case basis. For example, the gnome package recommends transmission-gtk (among others). When you select the former for installation, the latter will also be selected (and marked as automatic if not already installed on the system). Typing g will make it obvious: transmission-gtk appears on the summary screen of pending actions in the list of packages installed automatically to satisfy dependencies. However, you can decide not to install it by deselecting it before confirming the operations.
				
					Note that this recommendation tracking feature does not apply to upgrades. For instance, if a new version of gnome recommends a package that it did not recommend formerly, the package won't be marked for installation. However, it will be listed on the upgrade screen so that the administrator can still select it for installation.
				
					Suggestions between packages are also taken into account, but in a manner adapted to their specific status. For example, since gnome suggests empathy, the latter will be displayed on the summary screen of pending actions (in the section of packages suggested by other packages). This way, it is visible and the administrator can decide whether to take the suggestion into account or not. Since it is only a suggestion and not a dependency or a recommendation, the package will not be selected automatically â its selection requires a manual intervention from the user (thus, the package will not be marked as automatic).
				
					In the same spirit, remember that aptitude makes intelligent use of the concept of task. Since tasks are displayed as categories in the screens of packages lists, you can either select a full task for installation or removal, or browse the list of packages included in the task to select a smaller subset.
				

#### 6.5.1.2.Â Better Solver Algorithms

					To conclude this section, let's note that aptitude has more elaborate algorithms compared to apt-get when it comes to resolving difficult situations. When a set of actions is requested and when these combined actions would lead to an incoherent system, aptitude evaluates several possible scenarios and presents them in order of decreasing relevance. However, these algorithms are not fail-proof. Fortunately there is always the possibility to manually select the actions to perform. When the currently selected actions lead to contradictions, the upper part of the screen indicates a number of âbrokenâ packages (and you can directly navigate to those packages by pressing b). It is then possible to manually build a solution for the problems found. In particular, you can get access to the different available versions by simply selecting the package with Enter. If the selection of one of these versions solves the problem, you should not hesitate to use the function. When the number of broken packages gets down to zero, you can safely go to the summary screen of pending actions for a last check before you apply them.
				

NOTE aptitude's log

					Like dpkg, aptitude keeps a trace of executed actions in its logfile (/var/log/aptitude). However, since both commands work at a very different level, you cannot find the same information in their respective logfiles. While dpkg logs all the operations executed on individual packages step by step, aptitude gives a broader view of high-level operations like a system-wide upgrade.
				
					Beware, this logfile only contains a summary of operations performed by aptitude. If other frontends (or even dpkg itself) are occasionally used, then aptitude's log will only contain a partial view of the operations, so you can't rely on it to build a trustworthy history of the system.
				

### 6.5.2.Â synaptic

synaptic is a graphical package manager for Debian which features a clean and efficient graphical interface based on GTK+/GNOME. Its many ready-to-use filters give fast access to newly available packages, installed packages, upgradable packages, obsolete packages and so on. If you browse through these lists, you can select the operations to be done on the packages (install, upgrade, remove, purge); these operations are not performed immediately, but put into a task list. A single click on a button then validates the operations, and they are performed in one go.
			

FigureÂ 6.2.Â synaptic package manager

[Prev6.4.Â The apt-file Command](sect.apt-file.en.md)[Up](#)[Home](index.en.md)[Next6.6.Â Checking Package Authenticity](sect.package-authentication.en.md)