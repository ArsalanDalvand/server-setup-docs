---
title: "9.12. Power Management: Advanced Configuration and Power Interface (ACPI)"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.power-management.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.power-management.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.12. Power Management: Advanced Configuration and Power Interface (ACPI)"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.hotplug.en.md)The Debian Administrator's Handbook[Next](network-infrastructure.en.md)

## 9.12.Â Power Management: Advanced Configuration and Power Interface (ACPI)

			The topic of power management is often problematic. Indeed, properly suspending the computer requires that all the computer's device drivers know how to put them to standby, and that they properly reconfigure the devices upon waking. Unfortunately, there are still a few devices unable to sleep well under Linux, because their manufacturers have not provided the required specifications.
		
			Linux supports ACPI (Advanced Configuration and Power Interface) â the most recent standard in power management. The acpid package provides a daemon that looks for power management related events (switching between AC and battery power on a laptop, etc.) and that can execute various commands in response.
		

BEWARE Graphics card and standby

			The graphics card driver is often the culprit when standby doesn't work properly. In that case, it is a good idea to test the latest version of the X.org graphics server.
		
			After this overview of basic services common to many Unix systems, we will focus on the environment of the administered machines: the network. Many services are required for the network to work properly. They will be discussed in the next chapter.
		[Prev9.11.Â Hot Plugging: hotplug](sect.hotplug.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 10.Â Network Infrastructure](network-infrastructure.en.md)