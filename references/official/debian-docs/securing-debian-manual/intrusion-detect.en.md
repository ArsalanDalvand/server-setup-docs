---
title: "10.3.2. Host based intrusion detection"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/intrusion-detect.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/intrusion-detect.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "10.3.2. Host based intrusion detection"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](periodic-integrity.en.md)Securing Debian Manual[Next](ch10s04.en.md)

## 10.3.Â Set up Intrusion Detection

			Debian GNU/Linux includes tools for intrusion detection, which is the practice of detecting inappropriate or malicious activity on your local system, or other systems in your private network. This kind of defense is important if the system is very critical or you are truly paranoid. The most common approaches to intrusion detection are statistical anomaly detection and pattern-matching detection.
		
			Always be aware that in order to really improve the system's security with the introduction of any of these tools, you need to have an alert+response mechanism in place. Intrusion detection is a waste of time if you are not going to alert anyone.
		
			When a particular attack has been detected, most intrusion detection tools will either log the event with syslogd or send e-mail to the root user (the mail recipient is usually configurable). An administrator has to properly configure the tools so that false positives do not trigger alerts. Alerts may also indicate an ongoing attack and might not be useful, say, one day later, since the attack might have already succeeded. So be sure that there is a proper policy on handling alerts and that the technical mechanisms to implement this policy are in place.
		
			An interesting source of information is [http://www.cert.org/tech_tips/intruder_detection_checklist.html](http://www.cert.org/tech_tips/intruder_detection_checklist.html)

### 10.3.1.Â Network based intrusion detection

				Network based intrusion detection tools monitor the traffic on a network segment and use this information as a data source. Specifically, the packets on the network are examined, and they are checked to see if they match a certain signature.
			
snort is a flexible packet sniffer or logger that detects attacks using an attack signature dictionary. It detects a variety of attacks and probes, such as buffer overflows, stealth port scans, CGI attacks, SMB probes, and much more. snort also has real-time alerting capability. You can use snort for a range of hosts on your network as well as for your own host. This is a tool which should be installed on every router to keep an eye on your network. Just install it with apt-get install snort, follow the questions, and watch it log. For a little broader security framework, see [http://www.prelude-ids.org](http://www.prelude-ids.org).
			
				Debian's snort package has many security checks enabled by default. However, you should customize the setup to take into account the particular services you run on your system. You may also want to seek additional checks specific to these services.
			
				There are other, simpler tools that can be used to detect network attacks. portsentry is an interesting package that can tip you off to port scans against your hosts. Other tools like ippl or iplogger will also detect some IP (TCP and ICMP) attacks, even if they do not provide the kind of advanced techniques snort does.
			
				You can test any of these tools with the Debian package idswakeup, a shell script which generates false alarms, and includes many common attack signatures.
			

### 10.3.2.Â Host based intrusion detection

				Host based intrusion detection involves loading software on the system to be monitored which uses log files and/or the systems auditing programs as a data source. It looks for suspicious processes, monitors host access, and may even monitor changes to critical system files.
			
tiger is an older intrusion detection tool which has been ported to Debian since the Woody branch. tiger provides checks of common issues related to security break-ins, like password strength, file system problems, communicating processes, and other ways root might be compromised. This package includes new Debian-specific security checks including: MD5sums checks of installed files, locations of files not belonging to packages, and analysis of local listening processes. The default installation sets up tiger to run each day, generating a report that is sent to the superuser about possible compromises of the system.
			
				Log analysis tools, such as logcheck can also be used to detect intrusion attempts. See [SectionÂ 4.13.1, âUsing and customizing logcheckâ](log-alerts.en.md#custom-logcheck).
			
				In addition, packages which monitor file system integrity (see [SectionÂ 4.17.3, âChecking file system integrityâ](ch04s17.en.md#check-integ)) can be quite useful in detecting anomalies in a secured environment. It is most likely that an effective intrusion will modify some files in the local file system in order to circumvent local security policy, install Trojans, or create users. Such events can be detected with file system integrity checkers.
			[Prev10.2.Â Do periodic integrity checks](periodic-integrity.en.md)[Up](#)[Home](index.en.md)[Next10.4.Â Avoiding root-kits](ch10s04.en.md)