---
title: "14.6.7. Legal Liability"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.other-security-considerations.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.other-security-considerations.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "14.6.7. Legal Liability"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.selinux.en.md)The Debian Administrator's Handbook[Next](sect.dealing-with-compromised-machine.en.md)

## 14.6.Â Other Security-Related Considerations

			Security is not just a technical problem; more than anything, it is about good practices and understanding the risks. This section reviews some of the more common risks, as well as a few best practices which should, depending on the case, increase security or lessen the impact of a successful attack.
		

### 14.6.1.Â Inherent Risks of Web Applications

				The universal character of web applications led to their proliferation. Several are often run in parallel: a webmail, a wiki, some groupware system, forums, a photo gallery, a blog, and so on. Many of those applications rely on the âLAMPâ (Linux, Apache, MySQL, PHP) stack. Unfortunately, many of those applications were also written without much consideration for security problems. Data coming from outside is, too often, used with little or no validation. Providing specially-crafted values can be used to subvert a call to a command so that another one is executed instead. Many of the most obvious problems have been fixed as time has passed, but new security problems pop up regularly.
			

VOCABULARY SQL injection

				When a program inserts data into SQL queries in an insecure manner, it becomes vulnerable to SQL injections; this name covers the act of changing a parameter in such a way that the actual query executed by the program is different from the intended one, either to damage the database or to access data that should normally not be accessible. âÂ [https://en.wikipedia.org/wiki/SQL_Injection](https://en.wikipedia.org/wiki/SQL_Injection)

				Updating web applications regularly is therefore a must, lest any cracker (whether a professional attacker or a âscript kiddyâ) can exploit a known vulnerability. The actual risk depends on the case, and ranges from data destruction to arbitrary code execution, including web site defacement.
			

### 14.6.2.Â Knowing What To Expect

				A vulnerability in a web application is often used as a starting point for cracking attempts. What follows is a short review of possible consequences.
			

QUICK LOOK Filtering HTTP queries

				Apache 2 includes modules allowing filtering incoming HTTP queries. This allows blocking some attack vectors. For instance, limiting the length of parameters can prevent buffer overflows. More generally, one can validate parameters before they are even passed to the web application and restrict access along many criteria. This can even be combined with dynamic firewall updates, so that a client infringing one of the rules is banned from accessing the web server for a given period of time.
			
				Setting up these checks can be a long and cumbersome task, but it can pay off when the web application to be deployed has a dubious track record where security is concerned.
			
mod-security2 (in the libapache2-mod-security2 package) is the main such module. It even comes with many ready-to-use rules of its own (in the modsecurity-crs package) that you can easily enable.
			
				The consequences of an intrusion will have various levels of obviousness depending on the motivations of the attacker. âScript-kiddiesâ only apply recipes they find on web sites; most often, they deface a web page or delete data. In more subtle cases, they add invisible contents to web pages so as to improve referrals to their own sites in search engines.
			
				A more advanced attacker will go beyond that. A disaster scenario could go on in the following fashion: the attacker gains the ability to execute commands as the www-data user, but executing a command requires many manipulations. To make their life easier, they install other web applications specially designed to remotely execute many kinds of commands, such as browsing the filesystem, examining permissions, uploading or downloading files, executing commands, and even provide a network shell. Often, the vulnerability will allow running a wget command that will download some malware into /tmp/, then executing it. The malware is often downloaded from a foreign website that was previously compromised, in order to cover tracks and make it harder to find out the actual origin of the attack.
			
				At this point, the attacker has enough freedom of movement that they often install an IRC bot (a robot that connects to an IRC server and can be controlled by this channel). This bot is often used to share illegal files (unauthorized copies of movies or software, and so on). A determined attacker may want to go even further. The www-data account does not allow full access to the machine, and the attacker will try to obtain administrator privileges. Now, this should not be possible, but if the web application was not up-to-date, chances are that the kernel and other programs are outdated too; this sometimes follows a decision from the administrator who, despite knowing about the vulnerability, neglected to upgrade the system since there are no local users. The attacker can then take advantage of this second vulnerability to get root access.
			

VOCABULARY Privilege escalation

				This term covers anything that can be used to obtain more permissions than a given user should normally have. The sudo program is designed for precisely the purpose of giving administrative rights to some users. But the same term is also used to describe the act of an attacker exploiting a vulnerability to obtain undue rights.
			
				Now the attacker owns the machine; they will usually try to keep this privileged access for as long as possible. This involves installing a rootkit, a program that will replace some components of the system so that the attacker will be able to obtain the administrator privileges again at a later time (see also [QUICK LOOK The checksecurity and chkrootkit/rkhunter packages](sect.supervision.en.md#sidebar.the-checksecurity-and-chkrootkit-rkhunter-packages)); the rootkit also tries hiding its own existence as well as any traces of the intrusion. A subverted ps program will omit to list some processes, netstat will not list some of the active connections, and so on. Using the root permissions, the attacker was able to observe the whole system, but didn't find important data; so they will try accessing other machines in the corporate network. Analyzing the administrator's account and the history files, the attacker finds what machines are routinely accessed. By replacing sudo or ssh with a subverted program, the attacker can intercept some of the administrator's passwords, which they will use on the detected serversâ¦ and the intrusion can propagate from then on.
			
				This is a nightmare scenario which can be prevented by several measures. The next few sections describe some of these measures.
			

### 14.6.3.Â Choosing the Software Wisely

				Once the potential security problems are known, they must be taken into account at each step of the process of deploying a service, especially when choosing the software to install. Many web sites keep a list of recently-discovered vulnerabilities, which can give an idea of a security track record before some particular software is deployed. Of course, this information must be balanced against the popularity of said software: a more widely-used program is a more tempting target, and it will be more closely scrutinized as a consequence. On the other hand, a niche program may be full of security holes that never get publicized due to a lack of interest in a security audit.
			

VOCABULARY Security audit

				A security audit is the process of thoroughly reading and analyzing the source code of some software, looking for potential security vulnerabilities it could contain. Such audits are usually proactive and they are conducted to ensure a program meets certain security requirements.
			
				In the free software world, there is generally ample room for choice, and choosing one piece of software over another should be a decision based on the criteria that apply locally. More features imply an increased risk of a vulnerability hiding in the code; picking the most advanced program for a task may actually be counter-productive, and a better approach is usually to pick the simplest program that meets the requirements.
			

VOCABULARY Zero-day exploit

				A zero-day exploit attack is hard to prevent; the term covers a vulnerability that is not yet known to the authors of the program.
			

### 14.6.4.Â Managing a Machine as a Whole

				Most Linux distributions install by default a number of Unix services and many tools. In many cases, these services and tools are not required for the actual purposes for which the administrator set up the machine. As a general guideline in security matters, unneeded software is best uninstalled. Indeed, there is no point in securing an FTP server, if a vulnerability in a different, unused service can be used to get administrator privileges on the whole machine.
			
				By the same reasoning, firewalls will often be configured to only allow access to services that are meant to be publicly accessible.
			
				Current computers are powerful enough to allow hosting several services on the same physical machine. From an economic viewpoint, such a possibility is interesting: only one computer to administrate, lower energy consumption, and so on. From the security point of view, however, such a choice can be a problem. One compromised service can bring access to the whole machine, which in turn compromises the other services hosted on the same computer. This risk can be mitigated by isolating the services. This can be attained either with virtualization (each service being hosted in a dedicated virtual machine or container), or with AppArmor/SELinux (each service daemon having an adequately designed set of permissions).
			

### 14.6.5.Â Users Are Players

				Discussing security immediately brings to mind protection against attacks by anonymous crackers hiding in the Internet jungle; but an often-forgotten fact is that risks also come from inside: an employee about to leave the company could download sensitive files on the important projects and sell them to competitors, a negligent salesman could leave their desk without locking their session during a meeting with a new prospect, a clumsy user could delete the wrong directory by mistake, and so on.
			
				The response to these risks can involve technical solutions: no more than the required permissions should be granted to users, and regular backups are a must. But in many cases, the appropriate protection is going to involve training users to avoid the risks.
			

QUICK LOOK autolog

				The autolog package provides a program that automatically disconnects inactive users after a configurable delay. It also allows killing user processes that persist after a session ends, thereby preventing users from running daemons.
			

### 14.6.6.Â Physical Security

				There is no point in securing the services and networks if the computers themselves are not protected. Important data deserve being stored on hot-swappable hard disks in RAID arrays, because hard disks fail eventually and data availability is a must. But if any pizza delivery boy can enter the building, sneak into the server room and run away with a few selected hard disks, an important part of security is not fulfilled. Who can enter the server room? Is access monitored? These questions deserve consideration (and an answer) when physical security is being evaluated.
			
				Physical security also includes taking into consideration the risks for accidents such as fires. This particular risk is what justifies storing the backup media in a separate building, or at least in a fire-proof strongbox.
			

### 14.6.7.Â Legal Liability

				An administrator is, more or less implicitly, trusted by their users as well as the users of the network in general. They should therefore avoid any negligence that malevolent people could exploit.
			
				An attacker taking control of your machine then using it as a forward base (known as a ârelay systemâ) from which to perform other nefarious activities could cause legal trouble for you, since the attacked party would initially see the attack coming from your system, and therefore consider you as the attacker (or as an accomplice). In many cases, the attacker will use your server as a relay to send spam, which shouldn't have much impact (except potentially registration on black lists that could restrict your ability to send legitimate emails), but won't be pleasant, nevertheless. In other cases, more important trouble can be caused from your machine, for instance, denial of service attacks. This will sometimes induce loss of revenue, since the legitimate services will be unavailable and data can be destroyed; sometimes this will also imply a real cost, because the attacked party can start legal proceedings against you. Rights-holders can sue you if an unauthorized copy of a work protected by copyright law is shared from your server, as well as other companies compelled by service level agreements if they are bound to pay penalties following the attack from your machine.
			
				When these situations occur, claiming innocence is not usually enough; at the very least, you will need convincing evidence showing suspect activity on your system coming from a given IP address. This won't be possible if you neglect the recommendations of this chapter and let the attacker obtain access to a privileged account (root, in particular) and use it to cover their tracks.
			[Prev14.5.Â Introduction to SELinux](sect.selinux.en.md)[Up](#)[Home](index.en.md)[Next14.7.Â Dealing with a Compromised Machine](sect.dealing-with-compromised-machine.en.md)