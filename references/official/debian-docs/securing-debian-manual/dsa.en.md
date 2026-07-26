---
title: "7.2.2. CVE compatibility"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/dsa.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/dsa.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "7.2.2. CVE compatibility"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch07.en.md)Securing Debian Manual[Next](ch07s03.en.md)

## 7.2.Â Debian Security Advisories

			Debian Security Advisories (DSAs) are made whenever a security vulnerability is discovered that affects a Debian package. These advisories, signed by one of the Security Team members, include information of the versions affected as well as the location of the updates. This information is: 
			
						version number for the fix.
					
						problem type.
					
						whether it is remote or locally exploitable.
					
						short description of the package.
					
						description of the problem.
					
						description of the exploit.
					
						description of the fix.
					

			DSAs are published both on [http://www.debian.org/](http://www.debian.org/) and in the [http://www.debian.org/security/](http://www.debian.org/security/). Usually this does not happen until the website is rebuilt (every four hours) so they might not be present immediately. The preferred channel is the debian-security-announce mailing list.
		
			Interested users can, however (and this is done in some Debian-related portals) use the RDF channel to download automatically the DSAs to their desktop. Some applications, such as Evolution (an email client and personal information assistant) and Multiticker (a GNOME applet), can be used to retrieve the advisories automatically. The RDF channel is available at [http://www.debian.org/security/dsa.rdf](http://www.debian.org/security/dsa.rdf).
		
			DSAs published on the website might be updated after being sent to the public-mailing lists. A common update is adding cross references to security vulnerability databases. Also, translations[[45]](#ftn.idm3210) of DSAs are not sent to the security mailing lists but are directly included in the website.
		

### 7.2.1.Â Vulnerability cross references

				Debian provides a fully [http://www.debian.org/security/crossreferences](http://www.debian.org/security/crossreferences) including all the references available for all the advisories published since 1998. This table is provided to complement the [http://cve.mitre.org/cve/refs/refmap/source-DEBIAN.html](http://cve.mitre.org/cve/refs/refmap/source-DEBIAN.html).
			
				You will notice that this table provides references to security databases such as [http://www.securityfocus.com/bid](http://www.securityfocus.com/bid), [http://www.cert.org/advisories/](http://www.cert.org/advisories/) and [http://www.kb.cert.org/vuls](http://www.kb.cert.org/vuls) as well as CVE names (see below). These references are provided for convenience use, but only CVE references are periodically reviewed and included.
			
				Advantages of adding cross references to these vulnerability databases are: 
				
							it makes it easier for Debian users to see and track which general (published) advisories have already been covered by Debian.
						
							system administrators can learn more about the vulnerability and its impact by following the cross references.
						
							this information can be used to cross-check output from vulnerability scanners that include references to CVE to remove false positives (see [SectionÂ 12.1.2.1, âVulnerability assessment scanner X says my Debian system is vulnerable!â](ch12.en.md#vulnasses-false-positive)).
						

### 7.2.2.Â CVE compatibility

				Debian Security Advisories were [http://www.debian.org/security/CVE-certificate.jpg](http://www.debian.org/security/CVE-certificate.jpg)[[46]](#ftn.idm3234) in February 24, 2004.
			
				Debian developers understand the need to provide accurate and up to date information of the security status of the Debian distribution, allowing users to manage the risk associated with new security vulnerabilities. CVE enables us to provide standardized references that allow users to develop a [https://cve.mitre.org/compatible/enterprise.html](https://cve.mitre.org/compatible/enterprise.html).
			
				The [http://cve.mitre.org](http://cve.mitre.org) project is maintained by the MITRE Corporation and provides a list of standardized names for vulnerabilities and security exposures.
			
				Debian believes that providing users with additional information related to security issues that affect the Debian distribution is extremely important. The inclusion of CVE names in advisories help users associate generic vulnerabilities with specific Debian updates, which reduces the time spent handling vulnerabilities that affect our users. Also, it eases the management of security in an environment where CVE-enabled security tools -such as network or host intrusion detection systems, or vulnerability assessment tools- are already deployed regardless of whether or not they are based on the Debian distribution.
			
				Debian provides CVE names for all DSAs released since September 1998. All of the advisories can be retrieved on the Debian web site, and announcements related to new vulnerabilities include CVE names if available at the time of their release. Advisories associated with a given CVE name can be searched directly through the Debian Security Tracker (see below).
			
				In some cases you might not find a given CVE name in published advisories, for example because: 
				
							No Debian products are affected by that vulnerability.
						
							There is not yet an advisory covering that vulnerability (the security issue might have been reported as a [http://bugs.debian.org/cgi-bin/pkgreport.cgi?tag=security](http://bugs.debian.org/cgi-bin/pkgreport.cgi?tag=security) but a fix has not been tested and uploaded).
						
							An advisory was published before a CVE name was assigned to a given vulnerability (look for an update at the web site).
						
[[45]](#idm3210)
				Translations are available in up to ten different languages.
			[[46]](#idm3234)
					The full [http://cve.mitre.org/compatible/phase2/SPI_Debian.html](http://cve.mitre.org/compatible/phase2/SPI_Debian.html) is available at CVE
				[PrevChapterÂ 7.Â Debian Security Infrastructure](ch07.en.md)[Up](#)[Home](index.en.md)[Next7.3.Â Security Tracker](ch07s03.en.md)