---
title: "11.8.6. Adding WebRTC"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.rtc-services.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.rtc-services.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.8.6. Adding WebRTC"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.ldap-directory.en.md)The Debian Administrator's Handbook[Next](advanced-administration.en.md)

## 11.8.Â Real-Time Communication Services

			Real-Time Communication (RTC) services include voice, video/webcam, instant messaging (IM) and desktop sharing. This chapter gives a brief introduction to three of the services required to operate RTC, including a TURN server, SIP server and XMPP server. Comprehensive details of how to plan, install and manage these services are available in the Real-Time Communications Quick Start Guide which includes examples specific to Debian. âÂ [https://rtcquickstart.org](https://rtcquickstart.org)

			Both SIP and XMPP can provide the same functionality. SIP is slightly more well known for voice and video while XMPP is traditionally regarded as an IM protocol. In fact, they can both be used for any of these purposes. To maximize connectivity options, it is recommended to run both in parallel.
		
			These services rely on X.509 certificates both for authentication and confidentiality purposes. See [SectionÂ 10.2, âX.509 certificatesâ](sect.x509-cert.en.md) for more information.
		

### 11.8.1.Â DNS settings for RTC services

				RTC services require DNS SRV and NAPTR records. A sample configuration that can be placed in the zone file for falcot.com (see also [ExampleÂ 10.13, âExcerpt of /etc/bind/db.falcot.comâ](sect.domain-name-servers.en.md#example.bind-db.falcot.com):
			

```bash
; the server where everything will run
server1            IN     A      198.51.100.19
server1            IN     AAAA   2001:DB8:1000:2000::19

; IPv4 only for TURN for now, some clients are buggy with IPv6
turn-server        IN     A      198.51.100.19

; IPv4 and IPv6 addresses for SIP
sip-proxy          IN     A      198.51.100.19
sip-proxy          IN     AAAA   2001:DB8:1000:2000::19

; IPv4 and IPv6 addresses for XMPP
xmpp-gw            IN     A      198.51.100.19
xmpp-gw            IN     AAAA   2001:DB8:1000:2000::19

; DNS SRV and NAPTR for STUN / TURN
_stun._udp  IN SRV    0 1 3467 turn-server.falcot.com.
_turn._udp  IN SRV    0 1 3467 turn-server.falcot.com.
@           IN NAPTR  10 0 "s" "RELAY:turn.udp" "" _turn._udp.falcot.com.

; DNS SRV and NAPTR records for SIP
_sips._tcp  IN SRV    0 1 5061 sip-proxy.falcot.com.
@           IN NAPTR  10 0 "s" "SIPS+D2T" "" _sips._tcp.falcot.com.

; DNS SRV records for XMPP Server and Client modes:
_xmpp-client._tcp  IN     SRV    5 0 5222 xmpp-gw.falcot.com.
_xmpp-server._tcp  IN     SRV    5 0 5269 xmpp-gw.falcot.com.
```

### 11.8.2.Â TURN Server

				TURN is a service that helps clients behind NAT routers and firewalls to discover the most efficient way to communicate with other clients and to relay the media streams if no direct media path can be found. It is highly recommended that the TURN server is installed before any of the other RTC services are offered to end users.
			
				TURN and the related ICE protocol are open standards. To benefit from these protocols, maximizing connectivity and minimizing user frustration, it is important to ensure that all client software supports ICE and TURN.
			
				For the ICE algorithm to work effectively, the server must have two public IPv4 addresses.
			
				Install the coturn package and edit the /etc/turnserver.conf configuration file. By default, a SQLite database is configured in /var/db/turndb for user account settings, but PostgreSQL, MySQL or Redis can be set up instead if preferred. The most important thing to do is insert the IP addresses of the server.
			
				The server can be started running turnserver from the coturn package. We want the server to be an automatically started system service. This is the default behavior using systemd. Only when using the older SysVinit you have to edit the /etc/default/coturn file like this:
			

```bash
#
# Uncomment it if you want to have the turnserver running as
# an automatic system service daemon
#
TURNSERVER_ENABLED=1
```

				By default, the TURN server uses anonymous access. We have to add the users we want to use:
			

```bash
# turnadmin -a -u roland -p secret_password -r falcot.com
# turnadmin -A -u admin -p secret_password
```

				We use the argument -a to add a normal user and -A to add an admin user.
			

### 11.8.3.Â SIP Proxy Server

				A SIP proxy server manages the incoming and outgoing SIP connections between other organizations, SIP trunking providers, SIP PBXes such as Asterisk, SIP phones, SIP-based softphones and WebRTC applications.
			
				It is strongly recommended to install and configure the SIP proxy before attempting a SIP PBX setup. The SIP proxy normalizes a lot of the traffic reaching the PBX and provides greater connectivity and resilience.
			

#### 11.8.3.1.Â Install the SIP proxy

					Install the kamailio package and the package for the database backend. The Falcot administrators chose MySQL, so they install kamailio-mysql-modules and mariadb-server. /etc/kamailio/kamctlrc is the configuration file for the control tools kamctl and kamdbctl. You need to edit and set the SIP_DOMAIN to your SIP service domain and set the DBENGINE to MySQL, another database backend can be used.
				

```bash
[...]
## your SIP domain
SIP_DOMAIN=sip.falcot.com

## chrooted directory
# CHROOT_DIR="/path/to/chrooted/directory"

## database type: MYSQL, PGSQL, ORACLE, DB_BERKELEY, DBTEXT, or SQLITE
# by default none is loaded
#
# If you want to setup a database with kamdbctl, you must at least specify
# this parameter.
DBENGINE=MYSQL
[...]
```

					Now we focus on the configuration file /etc/kamailio/kamailio.cfg. Falcot needs user authentication and persistent user location, so they add the following #!define directives at the top of that file:
				

```bash
#!KAMAILIO
#
# Kamailio (OpenSER) SIP Server v5.2 - default configuration script
#     - web: https://www.kamailio.org
#     - git: https://github.com/kamailio/kamailio
#!define WITH_MYSQL
#!define WITH_AUTH
#!define WITH_USRLOCDB
[...]
```

					Kamailio needs a database structure that we can create running kamdbctl create as root. Finally, we can add some users with kamctl.
				

```bash
# kamdbctl create
[...]
# kamctl add roland secret_password
```

					Once everything is properly configured you can start or restart the service with systemctl restart kamailio, you can connect with a SIP client providing the IP address and the port (5090 is the default port). The users have the following id: roland@sip.falcot.com, and they can login using a client (see [SectionÂ 13.9, âReal-Time Communications softwareâ](sect.rtc-clients.en.md)).
				

### 11.8.4.Â XMPP Server

				An XMPP server manages connectivity between local XMPP users and XMPP users in other domains on the public Internet.
			

VOCABULARY XMPP or Jabber?

				XMPP is sometimes referred to as Jabber. In fact, Jabber is a trademark and XMPP is the official name of the standard.
			
prosody is a popular XMPP server that operates reliably on Debian servers.
			

#### 11.8.4.1.Â Install the XMPP server

					Install the prosody package.
				
					Review the /etc/prosody/prosody.cfg.lua configuration file. The most important thing to do is insert JIDs of the users who are permitted to manage the server.
				

```bash
admins = { "joe@falcot.com" }
```

					An individual configuration file is also needed for each domain. Copy the sample from /etc/prosody/conf.avail/example.com.cfg.lua and use it as a starting point. Here is falcot.com.cfg.lua:
				

```bash
VirtualHost "falcot.com"
        enabled = true
        ssl = {
                key = "/etc/ssl/private/falcot.com.key";
                certificate = "/etc/ssl/certs/falcot.com.pem";
                }

-- Set up a MUC (multi-user chat) room server on conference.example.com:
Component "conference.falcot.com" "muc"
```

					To enable the domain, there must be a symlink from /etc/prosody/conf.d/. Create it that way:
				

```bash
# ln -s /etc/prosody/conf.avail/falcot.com.cfg.lua /etc/prosody/conf.d/
```

					Restart the service to use the new configuration.
				

#### 11.8.4.2.Â Managing the XMPP server

					Some management operations can be performed using the prosodyctl command line utility. For example, to add the administrator account specified in /etc/prosody/prosody.cfg.lua:
				

```bash
# prosodyctl adduser joe@falcot.com
```

					See the [Prosody online documentation](https://prosody.im/doc/configure) for more details about how to customize the configuration.
				

### 11.8.5.Â Running services on port 443

				Some administrators prefer to run all of their RTC services on port 443. This helps users to connect from remote locations such as hotels and airports where other ports may be blocked or Internet traffic is routed through HTTP proxy servers.
			
				To use this strategy, each service (SIP, XMPP and TURN) needs a different IP address. All the services can still be on the same host as Linux supports multiple IP addresses on a single host. The port number, 443, must be specified in the configuration files for each process and also in the DNS SRV records.
			

### 11.8.6.Â Adding WebRTC

				Falcot wants to let customers make phone calls directly from the web site. The Falcot administrators also want to use WebRTC as part of their disaster recovery plan, so staff can use web browsers at home to log in to the company phone system and work normally in an emergency.
			

IN PRACTICE Try WebRTC

				If you have not tried WebRTC before, there are various sites that give an online demonstration and test facilities. âÂ [https://sip5060.net/test-calls](https://sip5060.net/test-calls)

				WebRTC is a rapidly evolving technology and it is essential to use packages from the Testing distribution. Another option is to compile the software.
			
				WebRTC uses a simple API to provide browsers and mobile applications with RTC, it is free software and it is being developed by Google. âÂ [https://webrtc.org](https://webrtc.org)

				A very flexible approach is using GStreamer's WebRTC implementation. It enables pipeline-based multimedia applications, which allows developing interesting and highly efficient applications. A good starting point is the following demo by Centricular, the main company that is developing it: âÂ [https://gitlab.freedesktop.org/gstreamer/gst-examples](https://gitlab.freedesktop.org/gstreamer/gst-examples)

				More advanced click-to-call web sites typically use server-side scripting to generate the config.js file dynamically. The [DruCall](https://www.drupal.org/project/drucall) source code demonstrates how to do this with PHP.
			
				This chapter sampled only a fraction of the available server software; however, most of the common network services were described. Now it is time for an even more technical chapter: we'll go into deeper detail for some concepts, describe massive deployments and virtualization.
			[Prev11.7.Â LDAP Directory](sect.ldap-directory.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 12.Â Advanced Administration](advanced-administration.en.md)