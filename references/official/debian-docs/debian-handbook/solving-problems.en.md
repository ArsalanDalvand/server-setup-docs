---
title: "7.1.5. Tutorials (HOWTO)"
source: "https://www.debian.org/doc/manuals/debian-handbook/solving-problems.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/solving-problems.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "7.1.5. Tutorials (HOWTO)"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.searching-packages.en.md)The Debian Administrator's Handbook[Next](sect.common-procedures.en.md)

# ChapterÂ 7.Â Solving Problems and Finding Relevant Information

[7.1. Documentation Sources](solving-problems.en.md#sect.documentation-sources)[7.1.1. Manual Pages](solving-problems.en.md#sect.manual-pages)[7.1.2. info Documents](solving-problems.en.md#id-1.10.4.5)[7.1.3. Specific Documentation](solving-problems.en.md#id-1.10.4.6)[7.1.4. Websites](solving-problems.en.md#id-1.10.4.7)[7.1.5. Tutorials (HOWTO)](solving-problems.en.md#id-1.10.4.8)[7.2. Common Procedures](sect.common-procedures.en.md)[7.2.1. Configuring a Program](sect.common-procedures.en.md#id-1.10.5.5)[7.2.2. Monitoring What Daemons Are Doing](sect.common-procedures.en.md#id-1.10.5.6)[7.2.3. Asking for Help on a Mailing List](sect.common-procedures.en.md#id-1.10.5.7)[7.2.4. Reporting a Bug When a Problem Is Too Difficult](sect.common-procedures.en.md#id-1.10.5.8)
		For an administrator, the most important skill is to be able to cope with any situation, known or unknown. This chapter gives a number of methods that will â hopefully â allow you to isolate the cause of any problem that you will encounter, so that you may be able to resolve them.
	

## 7.1.Â Documentation Sources

			Before you can understand what is really going on when there is a problem, you need to know the theoretical role played by each program involved in the problem. To do this, the best reflex to have is consult their documentation; but since these documentations are many and can be scattered far and wide, you should know all the places where they can be found.
		

### 7.1.1.Â Manual Pages

CULTURE RTFM

				This acronym stands for âRead the F***ing Manualâ, but can also be expanded in a friendlier variant, âRead the Fine Manualâ. This phrase is sometimes used in (terse) responses to questions from newbies. It is rather abrupt, and betrays a certain annoyance at a question asked by someone who has not even bothered to read the documentation. Some say that this classic response is better than no response at all (since it indicates that the documentation contains the information sought), or than a more verbose and angry answer.
			
				In any case, if someone responds âRTFMâ to you, it is often wise not to take offense. Since this answer may be perceived as vexing, you might want to try and avoid receiving it. If the information that you need is not in the manual, which can happen, you might want to say so, preferably in your initial question. You should also describe the various steps that you have personally taken to find information before you raised a question on a forum. Following Eric Raymond's guidelines is a good way to avoid the most common mistakes and get useful answers. âÂ [http://catb.org/~esr/faqs/smart-questions.html](http://catb.org/~esr/faqs/smart-questions.html)

				Manual pages, while relatively terse in style, contain a great deal of essential information. We will quickly go over the command for viewing them, provided by the man-db package. Simply type man manual-page â the manual page usually goes by the same name as the command whose documentation is sought. For example, to learn about the possible options for the cp command, you would type the man cp command at the shell prompt (see sidebar [BACK TO BASICS The shell, a command line interpreter](solving-problems.en.md#sidebar.shell)).
			

BACK TO BASICS The shell, a command line interpreter

				A command line interpreter, also called a âshellâ, is a program that executes commands that are either entered by the user or stored in a script. In interactive mode, it displays a prompt (usually ending in $ for a normal user, or by # for an administrator) indicating that it is ready to read a new command. [AppendixÂ B, Short Remedial Course](short-remedial-course.en.md) describes the basics of using the shell.
			
				The default and most commonly used shell is bash (Bourne Again SHell), but there are others, including dash, csh, tcsh and zsh.
			
				Among other things, most shells offer help (type help) and assistance during input at the prompt, such as the completion of command or file names (which you can generally activate by pressing the tab key), or recalling previous commands (history management; i.e. check out the mappings for "page up" and "page down" in /etc/inputrc).
			
				Man pages not only document commands and programs accessible from the command line, but also configuration files, system calls, library functions, and so forth. Sometimes names can collide. For example, the shell's read command has the same name as the read system call. This is why manual pages are organized in numbered sections:
			1
							commands that can be executed from the command line;
						2
							system calls (functions provided by the kernel);
						3
							library functions (provided by system libraries);
						4
							devices (on Unix-like systems, these are special files, usually placed in the /dev/ directory);
						5
							configuration files (formats and conventions);
						6
							games;
						7
							sets of macros and standards;
						8
							system administration commands;
						9
							kernel routines.
						
				It is possible to specify the section of the manual page that you are looking for: to view the documentation for the read system call, you would type man 2 read. When no section is explicitly specified, the first section that has a manual page with the requested name will be shown. Thus, man shadow returns shadow(5) because there are no manual pages for shadow in sections 1 to 4.
			

TIP whatis

				If you do not want to look at the full manual page, but only a short description to confirm that it is what you are looking for, simply enter whatis command.
			

```bash
$ whatis scp
scp (1)              - OpenSSH secure file copy
```

				This short description is included in the NAME section at the beginning of all manual pages.
			
				Of course, if you do not know the names of the commands, the manual is not going to be of much use to you. This is the purpose of the apropos command, which helps you conduct a search in the manual pages, or more specifically in their short descriptions. Each manual page begins essentially with a one line summary. apropos returns a list of manual pages whose summary mentions the requested keyword(s). If you choose them well, you will find the name of the command that you need.
			

ExampleÂ 7.1.Â Finding cp with apropos

```bash
$ apropos "copy file"
cp (1)               - copy files and directories
cp (1posix)          - copy files
cpio (1)             - copy files to and from archives
exec (1posix)        - execute commands and open, close, or copy file descriptors
install (1)          - copy files and set attributes
ntfscp (8)           - copy file to an NTFS volume.
```

TIP Browsing by following links

				Many manual pages have a âSEE ALSOâ section, usually at the end. It refers to other manual pages relevant to similar commands, or to external documentation. In this way, it is possible to find relevant documentation even when the first choice is not optimal.
			
				The man command is not the only means of consulting the manual pages, since khelpcenter and konqueror (by KDE) and yelp (under GNOME) programs also offer this possibility. There is also a web interface, provided by the man2html package, which allows you to view manual pages in a web browser. On a computer where this package is installed, use this URL after following the instructions in /usr/share/doc/man2html/README.Debian: âÂ [http://localhost/cgi-bin/man/man2html](http://localhost/cgi-bin/man/man2html)

				This utility requires a web server. This is why you should choose to install this package on one of your servers: all users of the local network could benefit from this service (including non-Linux machines), and this will allow you not to set up an HTTP server on each workstation. If your server is also accessible from other networks, it may be desirable to restrict access to this service only to users of the local network.
			
				Last but not least, you can view all manual pages available in Debian (even those that are not installed on your machine) on the manpages.debian.org service. It offers each manual page in multiple versions, one for each Debian release. âÂ [https://manpages.debian.org](https://manpages.debian.org)

DEBIAN POLICY Required man pages

				Debian requires each program to have a manual page. If the upstream author does not provide one, the Debian package maintainer will usually write a minimal page that will at the very least direct the reader to the location of the original documentation.
			

### 7.1.2.Â info Documents

				The GNU project has written manuals for most of its programs in the info format; this is why many manual pages refer to the corresponding info documentation. This format offers some advantages, but the default program to view these documents (it is called info) is also slightly more complex. You would be well advised to use pinfo instead (from the pinfo package).
			
				The info documentation has a hierarchical structure, and if you invoke pinfo without parameters, it will display a list of the nodes available at the first level. Usually, nodes bear the name of the corresponding commands.
			
				With pinfo navigating between these nodes is easy to achieve with the arrow keys. Alternatively, you could also use a graphical browser, which is a lot more user-friendly. Again, konqueror and yelp work; the info2www package also provides a web interface. âÂ [http://localhost/cgi-bin/info2www](http://localhost/cgi-bin/info2www)

				Note that the info system is not suitable for translation, unlike the man page system. info documents are thus almost always in English. However, when you ask the pinfo program to display a non-existing info page, it will fall back on the man page by the same name (if it exists), which might be translated.
			

### 7.1.3.Â Specific Documentation

				Each package includes its own documentation. Even the least well documented programs generally have a README file containing some interesting and/or important information. This documentation is installed in the /usr/share/doc/package/ directory (where package represents the name of the package). If the documentation is particularly large, it may not be included in the program's main package, but might be offloaded to a dedicated package which is usually named package-doc. The main package generally recommends the documentation package so that you can easily find it.
			
				The /usr/share/doc/package/ directory also contains some files provided by Debian which complete the documentation by specifying the package's particularities or improvements compared to a traditional installation of the software. The README.Debian file also indicates all of the adaptations that were made to comply with the Debian Policy. The changelog.Debian.gz file allows the user to follow the modifications made to the package over time: it is very useful to try to understand what has changed between two installed versions that do not have the same behavior. Finally, there is sometimes a NEWS.Debian.gz file which documents the major changes in the program that may directly concern the administrator (see [SectionÂ 6.7.2, âHandling Problems after an Upgradeâ](sect.dist-upgrade.en.md#sect.dist-upgrade-problems)).
			

### 7.1.4.Â Websites

				In most cases, free software programs have websites that are used to distribute it and to bring together the community of its developers and users. These sites are frequently loaded with relevant information in various forms: official documentation, FAQ (Frequently Asked Questions), mailing list archives, etc. Problems that you may encounter have often already been the subject of many questions; the FAQ or mailing list archives may have a solution for it. A good mastery of search engines will prove immensely valuable to find relevant pages quickly (by restricting the search to the Internet domain or sub-domain dedicated to the program). If the search returns too many pages or if the results do not match what you seek, you can add the keyword debian to limit results and target relevant information.
			

TIP From error to solution

				If the software returns a very specific error message, enter it into the search engine (between double quotes, ", in order to search not for individual keywords, but for the complete phrase). In most cases, the first links returned will contain the answer that you need.
			
				In other cases, you will get very general errors, such as âPermission deniedâ. In this case, it is best to check the permissions of the elements involved (files, user ID, groups, etc.).
			
				If you do not know the address for the software's website, there are various means of getting it. First, check if there is a Homepage field in the package's meta-information (apt show package). Alternately, the package description may contain a link to the program's official website. If no URL is indicated, look at /usr/share/doc/package/copyright. The Debian maintainer generally indicates in this file where they got the program's source code, and this is likely to be the website that you need to find. If at this stage your search is still unfruitful, consult a free software directory, such as FSF's Free Software Directory, or search directly with a search engine, such as Google, DuckDuckGo, Yahoo, etc. âÂ [https://directory.fsf.org/wiki/Main_Page](https://directory.fsf.org/wiki/Main_Page)

				You might also want to check the Debian wiki, a collaborative website where anybody, even new visitors, can make suggestions directly from their browsers. It is used equally by developers who design and specify their projects, and by users who share their knowledge by writing documents collaboratively. âÂ [https://wiki.debian.org/](https://wiki.debian.org/)

### 7.1.5.Â Tutorials (HOWTO)

				A HOWTO is a document that describes, in concrete terms and step by step, âhow toâ reach a predefined goal. The covered goals are relatively varied, but often technical in nature: for example, setting up IP Masquerading, configuring software RAID, installing a Samba server, etc. These documents often attempt to cover all of the potential problems likely to occur during the implementation of a given technology.
			
				Many such tutorials are managed by the Linux Documentation Project (LDP), whose website hosts all of these documents: âÂ [https://www.tldp.org/](https://www.tldp.org/)

				Debian also provides tutorials for its users: âÂ [https://www.debian.org/doc/](https://www.debian.org/doc/)

				All these documents should be taken with a grain of salt. They are often several years old; the information they contain is sometimes obsolete. This phenomenon is even more frequent for their translations, since updates are neither systematic nor instant after the publication of a new version of the original documents. Further many tutorials nowadays are provided by bloggers, sharing their individual solution with the interested reader. They often lack important information, i.e. the reason why some configuration has been chosen over another, or why some option has been enabled or disabled. Because blogging and creating personal websites made it so easy to share, many of these often short tutorials exist, but only a few are actively maintained and well-kept. This can make it hard to find the "right" one for you. This is all part of the joys of working in a volunteer environment and without constraintsâ¦
			[Prev6.10.Â Searching for Packages](sect.searching-packages.en.md)[Up](#)[Home](index.en.md)[Next7.2.Â Common Procedures](sect.common-procedures.en.md)