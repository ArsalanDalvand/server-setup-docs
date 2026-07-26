---
title: "8.5.1. Point to Point tunneling"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/vpn.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/vpn.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "8.5.1. Point to Point tunneling"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch08s04.en.md)Securing Debian Manual[Next](ch08s06.en.md)

## 8.5.Â Virtual Private Networks

			A virtual private network (VPN) is a group of two or more computer systems, typically connected to a private network with limited public network access, that communicate securely over a public network. VPNs may connect a single computer to a private network (client-server), or a remote LAN to a private network (server-server). VPNs often include the use of encryption, strong authentication of remote users or hosts, and methods for hiding the private network's topology.
		
			Debian provides quite a few packages to set up encrypted virtual private networks: 
			
vtun

tunnelv (non-US section)
					
cipe-source, cipe-common

tinc

secvpn

pptpd

openvpn

openswan ([http://www.openswan.org/](http://www.openswan.org/))
					

			FIXME: Update the information here since it was written with FreeSWAN in mind. Check Bug #237764 and Message-Id: <200412101215.04040.rmayr@debian.org>.
		
			The OpenSWAN package is probably the best choice overall, since it promises to interoperate with almost anything that uses the IP security protocol, IPsec (RFC 2411). However, the other packages listed above can also help you get a secure tunnel up in a hurry. The point to point tunneling protocol (PPTP) is a proprietary Microsoft protocol for VPN. It is supported under Linux, but is known to have serious security issues.
		
			For more information see the [http://www.tldp.org/HOWTO/VPN-Masquerade-HOWTO.html](http://www.tldp.org/HOWTO/VPN-Masquerade-HOWTO.html) (covers IPsec and PPTP), [http://www.tldp.org/HOWTO/VPN-HOWTO.html](http://www.tldp.org/HOWTO/VPN-HOWTO.html) (covers PPP over SSH), [http://www.tldp.org/HOWTO/mini/Cipe+Masq.html](http://www.tldp.org/HOWTO/mini/Cipe+Masq.html), and [http://www.tldp.org/HOWTO/mini/ppp-ssh/index.html](http://www.tldp.org/HOWTO/mini/ppp-ssh/index.html).
		
			Also worth checking out is [http://yavipin.sourceforge.net/](http://yavipin.sourceforge.net/), but no Debian packages seem to be available yet.
		

### 8.5.1.Â Point to Point tunneling

				If you want to provide a tunneling server for a mixed environment (both Microsoft operating systems and Linux clients) and IPsec is not an option (since it's only provided for Windows 2000 and Windows XP), you can use PoPToP (Point to Point Tunneling Server), provided in the pptpd package.
			
				If you want to use Microsoft's authentication and encryption with the server provided in the ppp package, note the following from the FAQ: 

```bash
It is only necessary to use PPP 2.3.8 if you want Microsoft compatible
MSCHAPv2/MPPE authentication and encryption. The reason for this is that
the MSCHAPv2/MPPE patch currently supplied (19990813) is against PPP
2.3.8. If you don't need Microsoft compatible authentication/encryption
any 2.3.x PPP source will be fine.
```

				However, you also have to apply the kernel patch provided by the kernel-patch-mppe package, which provides the pp_mppe module for pppd.
			
				Take into account that the encryption in ppptp forces you to store user passwords in clear text, and that the MS-CHAPv2 protocol contains [http://mopo.informatik.uni-freiburg.de/pptp_mschapv2/](http://mopo.informatik.uni-freiburg.de/pptp_mschapv2/).
			[Prev8.4.Â Auditing source code](ch08s04.en.md)[Up](#)[Home](index.en.md)[Next8.6.Â Public Key Infrastructure (PKI)](ch08s06.en.md)