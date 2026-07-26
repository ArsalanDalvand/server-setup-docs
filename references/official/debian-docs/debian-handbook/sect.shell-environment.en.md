---
title: "8.6. Shell Environment"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.shell-environment.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.shell-environment.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.6. Shell Environment"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.creating-accounts.en.md)The Debian Administrator's Handbook[Next](sect.config-printing.en.md)

## 8.6.Â Shell Environment

			Command interpreters (or shells) can be a user's first point of contact with the computer, and they must therefore be rather friendly. Most of them use initialization scripts that allow configuration of their behavior (automatic completion, prompt text, etc.).
		
bash, the standard shell, uses the /etc/bash.bashrc initialization script for âinteractiveâ shells, and /etc/profile for âloginâ shells.
		
			In simple terms, a login shell is invoked when you login to the console either locally or remotely via ssh, or when you run an explicit bash --login command. Regardless of whether it is a login shell or not, a shell can be interactive (in an xterm-type terminal for instance); or non-interactive (when executing a script).
		

DISCOVERY Other shells, other scripts

			Each command interpreter has a specific syntax and its own configuration files. Thus, zsh uses the files in /etc/zsh/; tcsh uses /etc/csh.cshrc, /etc/csh.login and /etc/csh.logout. The man pages for these programs document which files they use.
		
			For bash, it is useful to install and activate âautomatic completionâ. The package bash-completion contains these completions for most common programs and is usually enabled if the user's .bashrc configuration file was copied from /etc/skel/.bashrc. Otherwise it can be enabled via /etc/bash.bashrc (simply uncomment a few lines) or /etc/profile.
		

BACK TO BASICS Automatic completion

			Many command interpreters provide a completion feature, which allows the shell to automatically complete a partially typed command name or argument when the user hits the Tab key. This lets users work more efficiently and be less error-prone.
		
			This function is very powerful and flexible. It is possible to configure its behavior according to each command. Thus, the first argument following apt will be proposed according to the syntax of this command, even if it does not match any file (in this case, the possible choices are install, remove, upgrade, etc.).
		

BACK TO BASICS The tilde, a shortcut to HOME

			The tilde is often used to indicate the directory to which the environment variable, HOME, points (being the user's home directory, such as /home/rhertzog/). Command interpreters automatically make the substitution: ~/hello.txt becomes /home/rhertzog/hello.txt.
		
			The tilde also allows access to another user's home directory. Thus, ~rmas/bonjour.txt is synonymous with /home/rmas/bonjour.txt.
		
			In addition to these common scripts, each user can create their own ~/.bashrc and ~/.bash_profile to configure their shell. The most common changes are the addition of aliases; these are words that are automatically replaced with the execution of a command, which makes it faster to invoke that command. For instance, you could create the la alias for the command ls -la | less command; then you only have to type la to inspect the contents of a directory in detail. Please note that the shell needs to be reinitialized after adding an alias, e.g. by starting a new shell.
		

BACK TO BASICS Environment variables

			Environment variables allow storage of global settings for the shell or various other programs called. They are contextual (each process has its own set of environment variables) but inheritable. This last characteristic offers the possibility for a login shell to declare variables which will be passed down to all programs it executes.
		
			Setting default environment variables is an important element of shell configuration. Leaving aside the variables specific to a shell, it is preferable to place system wide variables in the /etc/environment file, since it is used by the various programs likely to initiate a shell session. Variables typically defined there include ORGANIZATION, which usually contains the name of the company or organization, and HTTP_PROXY, which indicates the existence and location of an HTTP proxy. Other options include to set system wide variables via scripts in /etc/profile.d, or session wide variables via .pam_environment or .profile, where the latter can override any definition contained in the first. The file /etc/default/locale is meant to contain system wide locale related environment variables.
		

TIP All shells configured identically

			Users often want to configure their login and interactive shells in the same way. To do this, they choose to interpret (or âsourceâ) the content from ~/.bashrc in the ~/.bash_profile file. However, the default ~/.profile file is sourcing ~/.bashrc already and you don't need to create ~/.bash_profile for this purpose anymore, except you want to customize your shell further (see bash(1) for which files are read by bash for login shells). It is also possible to do the same with files common to all users (by calling /etc/bash.bashrc from /etc/profile, which is also done by default now).
		[Prev8.5.Â Creating Accounts](sect.creating-accounts.en.md)[Up](#)[Home](index.en.md)[Next8.7.Â Printer Configuration](sect.config-printing.en.md)