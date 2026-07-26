---
title: "10.4.2. Configuring and Implementing"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.quality-of-service.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.quality-of-service.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.4.2. Configuring and Implementing"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.virtual-private-network.en.md)The Debian Administrator's Handbook[Next](sect.dynamic-routing.en.md)

## 10.4.Â Quality of Service

### 10.4.1.Â Principle and Mechanism

Quality of Service (or QoS for short) refers to a set of techniques that guarantee or improve the quality of the service provided to applications. The most popular such technique involves classifying the network traffic into categories, and differentiating the handling of traffic according to which category it belongs to. The main application of this differentiated services concept is traffic shaping, which limits the data transmission rates for connections related to some services and/or hosts so as not to saturate the available bandwidth and starve important other services. Traffic shaping is a particularly good fit for TCP traffic, since this protocol automatically adapts to available bandwidth.
			

CULTURE Net neutrality and QoS

				Network neutrality is achieved when Internet service providers treat all Internet communications equally, that is, without any access limitation based on content, user, website, destination address, etc.
			
				Quality of service can be implemented in a net neutral Internet, but only if Internet service providers can't charge a special fee for a higher-quality service.
			
				It is also possible to alter the priorities on traffic, which allows prioritizing packets related to interactive services (such as ssh and telnet) or to services that only deal with small blocks of data.
			
				The Debian kernels include the features required for QoS along with their associated modules. These modules are many, and each of them provides a different service, most notably by way of special schedulers for the queues of IP packets; the wide range of available scheduler behaviors spans the whole range of possible requirements.
			

CULTURE LARTC â Linux Advanced Routing & Traffic Control

				The Linux Advanced Routing & Traffic Control HOWTO is the reference document covering everything there is to know about network quality of service. âÂ [https://www.lartc.org/howto/](https://www.lartc.org/howto/)

### 10.4.2.Â Configuring and Implementing

				QoS parameters are set through the tc command (provided by the iproute package). Since its interface is quite complex, using higher-level tools is recommended.
			

#### 10.4.2.1.Â Reducing Latencies: wondershaper

					The main purpose of wondershaper (in the similarly-named package) is to minimize latencies independent of network load. This is achieved by limiting total traffic to a value that falls just short of the link saturation value.
				
					Once a network interface is configured, setting up this traffic limitation is achieved by running wondershaper interface download_rate upload_rate. The interface can be enp1s0, eth0 or ppp0 for example, and both rates are expressed in kilobits per second. The wondershaper remove interface command disables traffic control on the specified interface.
				
					For an Ethernet connection, historically this script would be called right after the interface is configured. This is done by adding up and down directives to the /etc/network/interfaces file allowing declared commands to be run, respectively, after the interface is configured and before it is deconfigured. Or in the PPP case, creating a script that calls wondershaper in /etc/ppp/ip-up.d/ will enable traffic control as soon as the connection is up. Below is an example using this first method:
				

ExampleÂ 10.9.Â Changes in the /etc/network/interfaces file

```bash
iface eth0 inet dhcp
    up /sbin/wondershaper eth0 500 100
    down /sbin/wondershaper remove eth0
```

GOING FURTHER Optimal configuration

					The /usr/share/doc/wondershaper/README.Debian.gz file describes, in some detail, the configuration method recommended by the package maintainer. In particular, it advises measuring the download and upload speeds so as to best evaluate real limits.
				

#### 10.4.2.2.Â Standard Configuration

					Barring a specific QoS configuration, the Linux kernel uses the pfifo_fast queue scheduler, which provides a few interesting features by itself. The priority of each processed IP packet is based on the DSCP field (Differentiated Services Code Point) of this packet; modifying this 6-bit field is enough to take advantage of the scheduling features. Refer to [https://en.wikipedia.org/wiki/Differentiated_services#Class_Selector](https://en.wikipedia.org/wiki/Differentiated_services#Class_Selector) for more information.
				
					The DSCP field can be set by applications that generate IP packets, or modified on the fly by netfilter. The following rules are sufficient to increase responsiveness for a server's SSH service, note that the DSCP field must be set in hexadecimal:
				

```bash
nft add table ip mangle
nft add rule ip mangle PREROUTING tcp sport 22 counter ip dscp set 0x04
nft add rule ip mangle PREROUTING tcp dport 22 counter ip dscp set 0x04
```

[Prev10.3.Â Virtual Private Network](sect.virtual-private-network.en.md)[Up](#)[Home](index.en.md)[Next10.5.Â Dynamic Routing](sect.dynamic-routing.en.md)