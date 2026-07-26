---
title: "6.9.5. The Miracle Combination"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.automatic-upgrades.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.automatic-upgrades.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.9.5. The Miracle Combination"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.regular-upgrades.en.md)The Debian Administrator's Handbook[Next](sect.searching-packages.en.md)

## 6.9.Â Automatic Upgrades

			Since Falcot Corp has many computers but only limited manpower, its administrators try to make upgrades as automatic as possible. The programs in charge of these processes must therefore run with no human intervention.
		

### 6.9.1.Â Configuring dpkg

				As we have already mentioned (see sidebar [GOING FURTHER Avoiding the configuration file questions](sect.package-meta-information.en.md#sidebar.questions-conffiles)), dpkg can be instructed not to ask for confirmation when replacing a configuration file (with the --force-confdef --force-confold options). Interactions can, however, have three other sources: some come from APT itself, some are handled by debconf, and some happen on the command line due to package configuration scripts (sometimes handled by ucf).
			

### 6.9.2.Â Configuring APT

				The case of APT is simple: the -y option (or --assume-yes) tells APT to consider the answer to all its questions to be âyesâ.
			

### 6.9.3.Â Configuring debconf

				The case of debconf deserves more details. This program was, from its inception, designed to control the relevance and volume of questions displayed to the user, as well as the way they are shown. That is why its configuration requests a minimal priority for questions; only questions above the minimal priority are displayed. debconf assumes the default answer (defined by the package maintainer) for questions which it decided to skip.
			
				The other relevant configuration element is the interface used by the frontend. If you choose noninteractive out of the choices, all user interaction is disabled. If a package tries to display an informative note, it will be sent to the administrator by email.
			
				To reconfigure debconf, use the dpkg-reconfigure tool from the debconf package; the relevant command is dpkg-reconfigure debconf. Note that the configured values can be temporarily overridden with environment variables when needed (for instance, DEBIAN_FRONTEND controls the interface, as documented in the debconf(7) manual page).
			

### 6.9.4.Â Handling Command Line Interactions

				The last source of interactions, and the hardest to get rid of, is the configuration scripts run by dpkg. There is unfortunately no standard solution, and no answer is overwhelmingly better than another.
			
				The common approach is to suppress the standard input by redirecting the empty content of /dev/null into it with command </dev/null, or to feed it with an endless stream of newlines. None of these methods is 100Â % reliable, but they generally lead to the default answers being used, since most scripts consider a lack of reply as an acceptance of the default value.
			

### 6.9.5.Â The Miracle Combination

				By combining the previous elements, it is possible to design a small but rather reliable script which can handle automatic upgrades.
			

ExampleÂ 6.5.Â Non-interactive upgrade script

```bash
export DEBIAN_FRONTEND=noninteractive
yes '' | apt-get -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" dist-upgrade
```

IN PRACTICE The Falcot Corp case

				Falcot computers are a heterogeneous system, with machines having various functions. Administrators will therefore pick the most relevant solution for each computer.
			
				In practice, the servers running Bullseye are configured with the âmiracle combinationâ above, and are kept up to date automatically. Only the most critical servers (the firewalls, for instances) are set up with apticron, so that upgrades always happen under the supervision of an administrator.
			
				The office workstations in the administrative services also run Bullseye, but they are equipped with gnome-packagekit, so that users trigger the upgrades themselves. The rationale for this decision is that if upgrades happen without an explicit action, the behavior of the computer might change unexpectedly, which could cause confusion for the main users.
			
				In the lab, the few computers using Testing â to take advantage of the latest software versions â are not upgraded automatically either. Administrators only configure APT to prepare the upgrades but not enact them; when they decide to upgrade (manually), the tedious parts of refreshing package lists and downloading packages will be avoided, and administrators can focus on the really useful part.
			[Prev6.8.Â Keeping a System Up to Date](sect.regular-upgrades.en.md)[Up](#)[Home](index.en.md)[Next6.10.Â Searching for Packages](sect.searching-packages.en.md)