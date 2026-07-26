---
title: "10.3.4. PPTP"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.virtual-private-network.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.virtual-private-network.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.3.4. PPTP"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.x509-cert.en.md)The Debian Administrator's Handbook[Next](sect.quality-of-service.en.md)

## 10.3.Â Virtual Private Network

			A Virtual Private Network (VPN for short) is a way to link two different local networks through the Internet by way of a tunnel; the tunnel is usually encrypted for confidentiality. VPNs are often used to integrate a remote machine within a company's local network.
		
			Several tools provide this functionality. OpenVPN is an efficient solution, easy to deploy and maintain, based on SSL/TLS. Another possibility is using IPsec to encrypt IP traffic between two machines; this encryption is transparent, which means that applications running on these hosts need not be modified to take the VPN into account. SSH can also be used to provide a VPN, in addition to its more conventional features. Finally, a VPN can be established using Microsoft's PPTP protocol. Other solutions exist, but are beyond the focus of this book.
		

### 10.3.1.Â OpenVPN

				OpenVPN is a piece of software dedicated to creating virtual private networks. Its setup involves creating virtual network interfaces on the VPN server and on the client(s); both tun (for IP-level tunnels) and tap (for Ethernet-level tunnels) interfaces are supported. In practice, tun interfaces will most often be used except when the VPN clients are meant to be integrated into the server's local network by way of an Ethernet bridge.
			
				OpenVPN relies on OpenSSL for all the SSL/TLS cryptography and associated features (confidentiality, authentication, integrity, non-repudiation). It can be configured either with a shared private key or using X.509 certificates based on a public key infrastructure. The latter configuration is strongly preferred since it allows greater flexibility when faced with a growing number of roaming users accessing the VPN.
			

#### 10.3.1.1.Â Configuring the OpenVPN Server

					After all certificates have been created (follow the instructions from [SectionÂ 10.2.2, âPublic Key Infrastructure: easy-rsaâ](sect.x509-cert.en.md#sect.easy-rsa)), they need to be copied where appropriate: the root certificate's public key (pki/ca.crt) will be stored on all machines (both server and clients) as /etc/ssl/certs/Falcot_CA.crt. The server's certificate is installed only on the server (pki/issued/vpn.falcot.com.crt goes to /etc/ssl/certs/vpn.falcot.com.crt, and pki/private/vpn.falcot.com.key goes to /etc/ssl/private/vpn.falcot.com.key with restricted permissions so that only the administrator can read it), with the corresponding Diffie-Hellman parameters (pki/dh.pem) installed to /etc/openvpn/dh.pem. Client certificates are installed on the corresponding VPN client in a similar fashion.
				
					By default, the OpenVPN initialization script tries starting all virtual private networks defined in /etc/openvpn/*.conf. Setting up a VPN server is therefore a matter of storing a corresponding configuration file in this directory. A good starting point is /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz, which leads to a rather standard server. Of course, some parameters need to be adapted: ca, cert, key and dh need to describe the selected locations (respectively, /etc/ssl/certs/Falcot_CA.crt, /etc/ssl/vpn.falcot.com.crt, /etc/ssl/private/vpn.falcot.com.key and /etc/openvpn/dh.pem). The server 10.8.0.0 255.255.255.0 directive defines the subnet to be used by the VPN; the server uses the first IP address in that range (10.8.0.1) and the rest of the addresses are allocated to clients.
				
					With this configuration, starting OpenVPN creates the virtual network interface, usually under the tun0 name. However, firewalls are often configured at the same time as the real network interfaces, which happens before OpenVPN starts. Good practice therefore recommends creating a persistent virtual network interface, and configuring OpenVPN to use this pre-existing interface. This further allows choosing the name for this interface. To this end, openvpn --mktun --dev vpn --dev-type tun creates a virtual network interface named vpn with type tun; this command can easily be integrated in the firewall configuration script, or in an up directive of the /etc/network/interfaces file, or a udev rule can be added to that end. The OpenVPN configuration file must also be updated accordingly, with the dev vpn and dev-type tun directives.
				
					Barring further action, VPN clients can only access the VPN server itself by way of the 10.8.0.1 address. Granting the clients access to the local network (192.168.0.0/24), requires adding a push route 192.168.0.0 255.255.255.0 directive to the OpenVPN configuration so that VPN clients automatically get a network route telling them that this network is reachable by way of the VPN. Furthermore, machines on the local network also need to be informed that the route to the VPN goes through the VPN server (this automatically works when the VPN server is installed on the gateway). Alternatively, the VPN server can be configured to perform IP masquerading so that connections coming from VPN clients appear as if they are coming from the VPN server instead (see [SectionÂ 10.1, âGatewayâ](network-infrastructure.en.md#sect.gateway)).
				

#### 10.3.1.2.Â Configuring the OpenVPN Client

					Setting up an OpenVPN client also requires creating a configuration file in /etc/openvpn/. A standard configuration can be obtained by using /usr/share/doc/openvpn/examples/sample-config-files/client.conf as a starting point. The remote vpn.falcot.com 1194 directive describes the address and port of the OpenVPN server; the ca, cert and key also need to be adapted to describe the locations of the key files.
				
					If the VPN should not be started automatically on boot, set the AUTOSTART directive to none in the /etc/default/openvpn file. Starting or stopping a given VPN connection is always possible with the commands systemctl start openvpn@name and systemctl stop openvpn@name (where the connection name matches the one defined in /etc/openvpn/name.conf).
				
					The network-manager-openvpn-gnome package contains an extension to Network Manager (see [SectionÂ 8.2.5, âAutomatic Network Configuration for Roaming Usersâ](sect.network-config.en.md#sect.roaming-network-config)) that allows managing OpenVPN virtual private networks. This allows every user to configure OpenVPN connections graphically and to control them from the network management icon.
				

### 10.3.2.Â Virtual Private Network with SSH

				There are actually two ways of creating a virtual private network with SSH. The historic one involves establishing a PPP layer over the SSH link. This method is described in a HOWTO document: âÂ [https://www.tldp.org/HOWTO/ppp-ssh/](https://www.tldp.org/HOWTO/ppp-ssh/)

				The second method is more recent, and was introduced with OpenSSHÂ 4.3; it is now possible for OpenSSH to create virtual network interfaces (tun*) on both sides of an SSH connection, and these virtual interfaces can be configured exactly as if they were physical interfaces. The tunneling system must first be enabled by setting PermitTunnel to âyesâ in the SSH server configuration file (/etc/ssh/sshd_config). When establishing the SSH connection, the creation of a tunnel must be explicitly requested with the -w any:any option (any can be replaced with the desired tun device number). This requires the user to have administrator privilege on both sides, so as to be able to create the network device (in other words, the connection must be established as root).
			
				Both methods for creating a virtual private network over SSH are quite straightforward. However, the VPN they provide is not the most efficient available; in particular, it does not handle high levels of traffic very well.
			
				The explanation is that when a TCP/IP stack is encapsulated within a TCP/IP connection (for SSH), the TCP protocol is used twice, once for the SSH connection and once within the tunnel. This leads to problems, especially due to the way TCP adapts to network conditions by altering timeout delays. The following site describes the problem in more detail: âÂ [http://sites.inka.de/sites/bigred/devel/tcp-tcp.html](http://sites.inka.de/sites/bigred/devel/tcp-tcp.html)

				VPNs over SSH should therefore be restricted to one-off tunnels with no performance constraints.
			

### 10.3.3.Â IPsec

				IPsec, despite being the standard in IP VPNs, is rather more involved in its implementation. The IPsec engine itself is integrated in the Linux kernel; the required user-space parts, the control and configuration tools, are provided by the libreswan package or the strongswan package. Here we describe briefly the first of these options.
			
				First, we install the libreswan package. In concrete terms, each host's /etc/ipsec.conf contains the parameters for IPsec tunnels (or Security Associations, in the IPsec terminology) that the host is concerned with. There are many configuration examples in /usr/share/doc/libreswan/, but Libreswan's online documentation has more examples with explanations: âÂ [https://libreswan.org/wiki/](https://libreswan.org/wiki/)

				The IPsec service can be controlled with systemctl; for example, systemctl start ipsec will start the IPsec service.
			
				In spite of its status as the reference, the complexity of setting up IPsec restricts its usage in practice. OpenVPN-based solutions will generally be preferred when the required tunnels are neither too many nor too dynamic.
			

CAUTION IPsec and NAT

				NATing firewalls and IPsec do not work well together: since IPsec signs the packets, any change on these packets that the firewall might perform will void the signature, and the packets will be rejected at their destination. Various IPsec implementations now include the NAT-T technique (for NAT Traversal), which basically encapsulates the IPsec packet within a standard UDP packet.
			

SECURITY IPsec and firewalls

				The standard mode of operation of IPsec involves data exchanges on UDP portÂ 500 for key exchanges (also on UDP portÂ 4500 in the case that NAT-T is in use). Moreover, IPsec packets use two dedicated IP protocols that the firewall must let through; reception of these packets is based on their protocol numbers, 50 (ESP) and 51 (AH).
			

### 10.3.4.Â PPTP

				PPTP (for Point-to-Point Tunneling Protocol) uses two communication channels, one for control data and one for payload data; the latter uses the GRE protocol (Generic Routing Encapsulation). A standard PPP link is then set up over the data exchange channel.
			

#### 10.3.4.1.Â Configuring the Client

					The pptp-linux package contains an easily-configured PPTP client for Linux. The following instructions take their inspiration from the official documentation: âÂ [http://pptpclient.sourceforge.net/howto-debian.phtml](http://pptpclient.sourceforge.net/howto-debian.phtml)

					The Falcot administrators created several files: /etc/ppp/options.pptp, /etc/ppp/peers/falcot, /etc/ppp/ip-up.d/falcot, and /etc/ppp/ip-down.d/falcot.
				

ExampleÂ 10.2.Â The /etc/ppp/options.pptp file

```bash
# PPP options used for a PPTP connection
lock
noauth
nobsdcomp
nodeflate
```

ExampleÂ 10.3.Â The /etc/ppp/peers/falcot file

```bash
# vpn.falcot.com is the PPTP server
pty "pptp vpn.falcot.com --nolaunchpppd"
# the connection will identify as the "vpn" user
user vpn
remotename pptp
# encryption is needed
require-mppe-128
file /etc/ppp/options.pptp
ipparam falcot
```

ExampleÂ 10.4.Â The /etc/ppp/ip-up.d/falcot file

```bash
# Create the route to the Falcot network
if [ "$6" = "falcot" ]; then
  # 192.168.0.0/24 is the (remote) Falcot network
  ip route add 192.168.0.0/24 dev $1
fi
```

ExampleÂ 10.5.Â The /etc/ppp/ip-down.d/falcot file

```bash
# Delete the route to the Falcot network
if [ "$6" = "falcot" ]; then
  # 192.168.0.0/24 is the (remote) Falcot network
  ip route del 192.168.0.0/24 dev $1
fi
```

SECURITY MPPE

					Securing PPTP involves using the MPPE feature (Microsoft Point-to-Point Encryption), which is available in official Debian kernels as a module.
				

#### 10.3.4.2.Â Configuring the Server

CAUTION PPTP and firewalls

					Intermediate firewalls need to be configured to let through IP packets using protocol 47 (GRE). Moreover, the PPTP server's port 1723 needs to be open so that the communication channel can happen.
				
pptpd is the PPTP server for Linux. Its main configuration file, /etc/pptpd.conf, requires very few changes: localip (local IP address) and remoteip (remote IP address). In the example below, the PPTP server always uses the 192.168.0.199 address, and PPTP clients receive IP addresses from 192.168.0.200 to 192.168.0.250.
				

ExampleÂ 10.6.Â The /etc/pptpd.conf file

```bash
[..]
# TAG: localip
# TAG: remoteip
#       Specifies the local and remote IP address ranges.
#
#       These options are ignored if delegate option is set.
#
#       Any addresses work as long as the local machine takes care of the
#       routing.  But if you want to use MS-Windows networking, you should
#       use IP addresses out of the LAN address space and use the proxyarp
#       option in the pppd options file, or run bcrelay.
#
#       You can specify single IP addresses seperated by commas or you can
#       specify ranges, or both. For example:
#
#               192.168.0.234,192.168.0.245-249,192.168.0.254
#
#       IMPORTANT RESTRICTIONS:
#
#       1. No spaces are permitted between commas or within addresses.
#
#       2. If you give more IP addresses than the value of connections,
#          it will start at the beginning of the list and go until it
#          gets connections IPs.  Others will be ignored.
#
#       3. No shortcuts in ranges! ie. 234-8 does not mean 234 to 238,
#          you must type 234-238 if you mean this.
#
#       4. If you give a single localIP, that's ok - all local IPs will
#          be set to the given one. You MUST still give at least one remote
#          IP for each simultaneous client.
#
# (Recommended)
#localip 192.168.0.1
#remoteip 192.168.0.234-238,192.168.0.245
# or
#localip 192.168.0.234-238,192.168.0.245
#remoteip 192.168.1.234-238,192.168.1.245
localip 192.168.0.199
remoteip 192.168.0.200-250
```

					The PPP configuration used by the PPTP server also requires a few changes in /etc/ppp/pptpd-options. The important parameters are the server name (pptp), the domain name (falcot.com), and the IP addresses for DNS and WINS servers.
				

ExampleÂ 10.7.Â The /etc/ppp/pptpd-options file

```bash
# Enable connection debugging facilities.
# (see your syslog configuration for where pppd sends to)
#debug

# Name of the local system for authentication purposes
# (must match the second field in /etc/ppp/chap-secrets entries)
name pptpd

# Optional: domain name to use for authentication
## change the domainname to your local domain
domain falcot.com

# Authentication
## these are reasonable defaults for WinXXXX clients
## for the security related settings
auth
refuse-pap
refuse-chap
refuse-mschap
# Require the peer to authenticate itself using MS-CHAPv2 [Microsoft
# Challenge Handshake Authentication Protocol, Version 2] authentication.
require-mschap-v2
# Require MPPE 128-bit encryption
# (note that MPPE requires the use of MSCHAP-V2 during authentication)
require-mppe-128

# Network and Routing
## Fill in your addresses
ms-dns 192.168.0.1
ms-wins 192.168.0.1

## Fill in your netmask
netmask 255.255.255.0

## some defaults
nodefaultroute
proxyarp
lock
```

					The last step involves registering the vpn user (and the associated password) in the /etc/ppp/chap-secrets file. Contrary to other instances where an asterisk (*) would work, the server name must be filled explicitly here. Furthermore, Windows PPTP clients identify themselves under the DOMAIN\\USER form, instead of only providing a user name. This explains why the file also mentions the FALCOT\\vpn user. It is also possible to specify individual IP addresses for users; an asterisk in this field specifies that dynamic addressing should be used.
				

ExampleÂ 10.8.Â The /etc/ppp/chap-secrets file

```bash
# Secrets for authentication using CHAP
# client        server  secret      IP addresses
vpn             pptp    f@Lc3au     *
FALCOT\\vpn     pptp    f@Lc3au     *
```

SECURITY PPTP vulnerabilities

					Microsoft's first PPTP implementation drew severe criticism because it had many security vulnerabilities; most have since then been fixed in more recent versions. The configuration documented in this section uses the latest version of the protocol. Be aware though that removing some options (such as require-mppe-128 and require-mschap-v2) would make the service vulnerable again.
				[Prev10.2.Â X.509 certificates](sect.x509-cert.en.md)[Up](#)[Home](index.en.md)[Next10.4.Â Quality of Service](sect.quality-of-service.en.md)