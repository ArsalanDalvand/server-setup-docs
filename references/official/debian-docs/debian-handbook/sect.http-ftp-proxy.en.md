---
title: "11.6.3. Configuring a Filter"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.http-ftp-proxy.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.http-ftp-proxy.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.6.3. Configuring a Filter"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.windows-file-server-with-samba.en.md)The Debian Administrator's Handbook[Next](sect.ldap-directory.en.md)

## 11.6.Â HTTP/FTP Proxy

			An HTTP/FTP proxy acts as an intermediary for HTTP and/or FTP connections. Its role is twofold:
		
					Caching: recently downloaded documents are copied locally, which avoids multiple downloads.
				
					Filtering server: if use of the proxy is mandated (and outgoing connections are blocked unless they go through the proxy), then the proxy can determine whether or not the request is to be granted.
				
			Falcot Corp selected Squid as their proxy server.
		

### 11.6.1.Â Installing

				The squid Debian package only contains the modular (caching) proxy. Turning it into a filtering server requires installing the additional squidguard package. In addition, squid-cgi provides a querying and administration interface for a Squid proxy.
			
				Prior to installing, care should be taken to check that the system can identify its own complete name: the hostname -f must return a fully-qualified name (including a domain). If it does not, then the /etc/hosts file should be edited to contain the full name of the system (for instance, arrakis.falcot.com). The official computer name should be validated with the network administrator in order to avoid potential name conflicts.
			

### 11.6.2.Â Configuring a Cache

				Enabling the caching server feature is a simple matter of editing the /etc/squid/squid.conf configuration file and allowing machines from the local network to run queries through the proxy. The following example shows the modifications made by the Falcot Corp administrators:
			

ExampleÂ 11.22.Â The /etc/squid/squid.conf file (excerpts)

```bash
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
#
include /etc/squid/conf.d/*

# Example rule allowing access from your local networks.
# Adapt localnet in the ACL section to list your (internal) IP networks
# from where browsing should be allowed

acl our_networks src 192.168.1.0/24 192.168.2.0/24
http_access allow our_networks
http_access allow localhost
# And finally deny all other access to this proxy
http_access deny all
```

### 11.6.3.Â Configuring a Filter

squid itself does not perform the filtering; this action is delegated to squidGuard. The former must then be configured to interact with the latter. This involves adding the following directive to the /etc/squid/squid.conf file:
			

```bash
url_rewrite_program /usr/bin/squidGuard -c /etc/squid/squidGuard.conf
```

				The /usr/lib/cgi-bin/squidGuard.cgi CGI program also needs to be installed, using /usr/share/doc/squidguard/examples/squidGuard.cgi as a starting point. Required modifications to this script are the $proxy and $proxymaster variables (the name of the proxy and the administrator's contact email, respectively). The $image and $redirect variables should point to existing images representing the rejection of a query.
			
				The filter is enabled with the systemctl reload squid command. However, since the squidguard package does no filtering by default, it is the administrator's task to define the policy. This can be done by creating the /etc/squid/squidGuard.conf file (using /etc/squidguard/squidGuard.conf.default as template if required).
			
				The working database must be regenerated with update-squidguard after each change of the squidGuard configuration file (or one of the lists of domains or URLs it mentions). The configuration file syntax is documented on the following website: âÂ [http://www.squidguard.org/Doc/configure.html](http://www.squidguard.org/Doc/configure.html)

ALTERNATIVE E2guardian (a DansGuardian Fork)

				The e2guardian package, a DansGuardian fork, is an alternative to squidguard. This software does not simply handle a blacklist of forbidden URLs, it can also take advantage of content and anti-virus scanning and it provides broad authentication support.
			[Prev11.5.Â Setting Up Windows Shares with Samba](sect.windows-file-server-with-samba.en.md)[Up](#)[Home](index.en.md)[Next11.7.Â LDAP Directory](sect.ldap-directory.en.md)