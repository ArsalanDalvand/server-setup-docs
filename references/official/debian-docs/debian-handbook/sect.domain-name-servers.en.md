---
title: "10.7.2. Configuring bind"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.domain-name-servers.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.domain-name-servers.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.7.2. Configuring bind"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.ipv6.en.md)The Debian Administrator's Handbook[Next](sect.dhcp.en.md)

## 10.7.Â Domain Name Servers (DNS)

			The Domain Name Service (DNS) is a fundamental component of the Internet: it maps host names to IP addresses (and vice-versa), which allows the use of www.debian.org instead of 130.89.148.77 or 2001:67c:2564:a119::77.
		
			DNS records are organized in zones; each zone matches either a domain (or a subdomain) or an IP address range (since IP addresses are generally allocated in consecutive ranges). A primary server is authoritative on the contents of a zone; secondary servers, usually hosted on separate machines, provide regularly refreshed copies of the primary zone.
		
			Each zone can contain records of various kinds (Resource Records), these are some of the most common:
		
A (address record): IPv4 address. This is the most common form to point a domain to an IPv4 address.
				
CNAME (canonical name record): alias
				
MX (mail exchange): an email server. This information is used by other email servers to find where to send email addressed to a given address. Each MX record has a priority. The highest-priority server (with the lowest number) is tried first (see sidebar [BACK TO BASICS SMTP](network-services.en.md#sidebar.smtp)); other servers are contacted in order of decreasing priority if the first one does not reply.
				
PTR (pointer): mapping of an IP address to a name. Such a record is stored in a âreverse DNSâ zone named after the IP address range. For example, 1.168.192.in-addr.arpa is the zone containing the reverse mapping for all addresses in the 192.168.1.0/24 range.
				
AAAA (IPv6 address record): IPv6 address.
				
NS (name server): maps a name to a name server. Each domain must have at least one NS record. These records point at a DNS server that can answer queries concerning this domain; they usually point at the primary and secondary servers for the domain. These records also allow DNS delegation; for instance, the falcot.com zone can include an NS record for internal.falcot.com, which means that the internal.falcot.com zone is handled by another server. Of course, this server must declare an internal.falcot.com zone.
				

### 10.7.1.Â DNS software

				The reference name server, Bind, was developed and is maintained by ISC (the Internet Software Consortium). It is provided in Debian by the bind9 package. VersionÂ 9 brings two major changes compared to previous versions. First, the DNS server can now run under an unprivileged user, so that a security vulnerability in the server does not grant root privileges to the attacker (as was seen repeatedly with versionsÂ 8.x).
			
				Furthermore, Bind supports the DNSSEC standard for signing (and therefore authenticating) DNS records, which allows blocking any spoofing of this data during man-in-the-middle attacks.
			

CULTURE DNSSEC

				The DNSSEC norm is quite complex; this partly explains why it is not in widespread usage yet (even if it perfectly coexists with DNS servers unaware of DNSSEC). To understand all the ins and outs, you should check the following article. âÂ [https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions)

### 10.7.2.Â Configuring bind

				Configuration files for bind, irrespective of version, have the same structure.
			
				The Falcot administrators created a primary falcot.com zone to store information related to this domain, and a 168.192.in-addr.arpa zone for reverse mapping of IP addresses in the local networks.
			

CAUTION Names of reverse zones

				Reverse zones have a particular name. The zone covering the 192.168.0.0/16 network needs to be named 168.192.in-addr.arpa: the IP address components are reversed, and followed by the in-addr.arpa suffix.
			
				For IPv6 networks, the suffix is ip6.arpa and the IP address components which are reversed are each character in the full hexadecimal representation of the IP address. As such, the 2001:0bc8:31a0::/48 network would use a zone named 0.a.1.3.8.c.b.0.1.0.0.2.ip6.arpa.
			

TIP Testing the DNS server

				The host command (in the bind9-host package) queries a DNS server, and can be used to test the server configuration. For example, host machine.falcot.com localhost checks the local server's reply for the machine.falcot.com query. host ipaddress localhost tests the reverse resolution.
			
				The following configuration excerpts, taken from the Falcot files, can serve as starting points to configure a DNS server:
			

ExampleÂ 10.12.Â Excerpt of /etc/bind/named.conf.local

```bash
zone "falcot.com" {
        type master;
        file "/etc/bind/db.falcot.com";
        allow-query { any; };
        allow-transfer {
                195.20.105.149/32 ; // ns0.xname.org
                193.23.158.13/32 ; // ns1.xname.org
        };
};

zone "internal.falcot.com" {
        type master;
        file "/etc/bind/db.internal.falcot.com";
        allow-query { 192.168.0.0/16; };
};

zone "168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.192.168";
        allow-query { 192.168.0.0/16; };
};
```

ExampleÂ 10.13.Â Excerpt of /etc/bind/db.falcot.com

```bash
; falcot.com Zone 
; admin.falcot.com. => zone contact: admin@falcot.com
$TTL    604800
@       IN      SOA     falcot.com. admin.falcot.com. (
                        20040121        ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
; The @ refers to the zone name ("falcot.com" here)
; or to $ORIGIN if that directive has been used
;
@       IN      NS      ns
@       IN      NS      ns0.xname.org.

internal IN      NS      192.168.0.2

@       IN      A       212.94.201.10
@       IN      MX      5 mail
@       IN      MX      10 mail2

ns      IN      A       212.94.201.10
mail    IN      A       212.94.201.10
mail2   IN      A       212.94.201.11
www     IN      A       212.94.201.11

dns     IN      CNAME   ns
```

CAUTION Syntax of a name

				The syntax of machine names follows strict rules. For instance, machine implies machine.domain. If the domain name should not be appended to a name, said name must be written as machine. (with a dot as suffix). Indicating a DNS name outside the current domain therefore requires a syntax such as machine.otherdomain.com. (with the final dot).
			

ExampleÂ 10.14.Â Excerpt of /etc/bind/db.192.168

```bash
; Reverse zone for 192.168.0.0/16
; admin.falcot.com. => zone contact: admin@falcot.com
$TTL    604800
@       IN      SOA     ns.internal.falcot.com. admin.falcot.com. (
                        20040121        ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

        IN      NS      ns.internal.falcot.com.

; 192.168.0.1 -> arrakis
1.0     IN      PTR     arrakis.internal.falcot.com.
; 192.168.0.2 -> neptune
2.0     IN      PTR     neptune.internal.falcot.com.

; 192.168.3.1 -> pau
1.3     IN      PTR     pau.internal.falcot.com.
```

[Prev10.6.Â IPv6](sect.ipv6.en.md)[Up](#)[Home](index.en.md)[Next10.8.Â DHCP](sect.dhcp.en.md)