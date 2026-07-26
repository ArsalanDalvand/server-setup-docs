---
title: "10.2. Do periodic integrity checks"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/periodic-integrity.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/periodic-integrity.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "10.2. Do periodic integrity checks"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch10.en.md)Securing Debian Manual[Next](intrusion-detect.en.md)

## 10.2.Â Do periodic integrity checks

			Based on the baseline information you generated after installation (i.e. the snapshot described in [SectionÂ 4.19, âTaking a snapshot of the systemâ](snapshot.en.md)), you should be able to do an integrity check from time to time. An integrity check will be able to detect filesystem modifications made by an intruder or due to a system administrators mistake.
		
			Integrity checks should be, if possible, done offline.[[64]](#ftn.idm4245) That is, without using the operating system of the system to review, in order to avoid a false sense of security (i.e. false negatives) produced by, for example, installed rootkits. The integrity database that the system is checked against should also be used from read-only media.
		
			You can consider doing integrity checks online using any of the filesystem integrity tools available (described in [SectionÂ 4.17.3, âChecking file system integrityâ](ch04s17.en.md#check-integ)) if taking offline the system is not an option. However, precaution should be taken to use a read-only integrity database and also assure that the integrity checking tool (and the operating system kernel) has not been tampered with.
		
			Some of the tools mentioned in the integrity tools section, such as aide, integrit or samhain are already prepared to do periodic reviews (through the crontab in the first two cases and through a standalone daemon in samhain) and can warn the administrator through different channels (usually e-mail, but samhain can also send pages, SNMP traps or syslog alerts) when the filesystem changes.
		
			Of course, if you execute a security update of the system, the snapshot taken for the system should be re-taken to accommodate the changes done by the security update.
		[[64]](#idm4245)
				An easy way to do this is using a Live CD, such as [http://www.knoppix-std.org/](http://www.knoppix-std.org/) which includes both the file integrity tools and the integrity database for your system.
			[PrevChapterÂ 10.Â Before the compromise](ch10.en.md)[Up](#)[Home](index.en.md)[Next10.3.Â Set up Intrusion Detection](intrusion-detect.en.md)