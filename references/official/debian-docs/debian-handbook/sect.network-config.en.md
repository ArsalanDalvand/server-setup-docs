---
title: "8.2.5. Automatic Network Configuration for Roaming Users"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.network-config.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.network-config.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.2.5. Automatic Network Configuration for Roaming Users"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](basic-configuration.en.md)The Debian Administrator's Handbook[Next](sect.hostname-name-service.en.md)

## 8.2.Â Configuring the Network

BACK TO BASICS Essential network concepts (Ethernet, IP address, subnet, broadcast)

			Most modern local networks use the Ethernet protocol, where data is split into small blocks called frames and transmitted on the wire one frame at a time. Data speeds vary from 10Â Mb/s for older Ethernet cards to 100Â Gb/s in the newest cards (with the most common rate currently growing from 100Â Mb/s to 10Â Gb/s). The most widely used cables are called 10BASE-T, 100BASE-T, 1000BASE-T, 10GBASE-T and 40GBASE-T, depending on the throughput they can reliably provide (the T stands for âtwisted pairâ); those cables end in an RJ45 connector. There are other cable types, used mostly for speeds of 10Â Gb/s and above.
		
			An IP address is a number used to identify a network interface on a computer on a local network or the Internet. In the currently most widespread version of IP (IPv4), this number is encoded in 32 bits, and is usually represented as 4 numbers separated by periods (e.g. 192.168.0.1), each number being between 0 and 255 (inclusive, which corresponds to 8 bits of data). The next version of the protocol, IPv6, extends this addressing space to 128 bits, and the addresses are generally represented as a series of hexadecimal numbers separated by colons (e.g., 2001:0db8:13bb:0002:0000:0000:0000:0020, or 2001:db8:13bb:2::20 for short).
		
			A subnet mask (netmask) defines in its binary code which portion of an IP address corresponds to the network, the remainder specifying the machine. In the example of configuring a static IPv4 address given here, the subnet mask, 255.255.255.0 (24 â1âs followed by 8 â0âs in binary representation) indicates that the first 24 bits of the IP address correspond to the network address, and the other 8 are specific to the machine. In IPv6, for readability, only the number of â1âs is expressed; the netmask for an IPv6 network could, thus, be 64.
		
			The network address is an IP address in which the part describing the machine's number is 0. The range of IPv4 addresses in a complete network is often indicated by the syntax, a.b.c.d/e, in which a.b.c.d is the network address and e is the number of bits affected to the network part in an IP address. The example network would thus be written: 192.168.0.0/24. The syntax is similar in IPv6: 2001:db8:13bb:2::/64.
		
			A router is a machine that connects several networks to each other. All traffic coming through a router is guided to the correct network. To do this, the router analyzes incoming packets and redirects them according to the IP address of their destination. The router is often known as a gateway; in this configuration, it works as a machine that helps reach out beyond a local network (towards an extended network, such as the Internet).
		
			The special broadcast address connects all the stations in a network. Almost never âroutedâ, it only functions on the network in question. Specifically, it means that a data packet addressed to the broadcast never passes through the router.
		
			This chapter focuses on IPv4 addresses, since they are currently the most commonly used. The details of the IPv6 protocol are approached in [SectionÂ 10.6, âIPv6â](sect.ipv6.en.md), but the concepts remain the same.
		
			The network is automatically configured during the initial installation. If Network Manager gets installed (which is generally the case for full desktop installations), then it might be that no configuration is actually required (for example, if you rely on DHCP on a wired connection and have no specific requirements). If a configuration is required (for example, for a WiFi interface), then it will create the appropriate file in /etc/NetworkManager/system-connections/.
		

NOTE NetworkManager

			If Network Manager is particularly recommended in roaming setups (see [SectionÂ 8.2.5, âAutomatic Network Configuration for Roaming Usersâ](sect.network-config.en.md#sect.roaming-network-config)), it is also perfectly usable as the default network management tool. You can create âSystem connectionsâ that are used as soon as the computer boots either manually with a .ini-like file in /etc/NetworkManager/system-connections/ or through a graphical tool (nm-connection-editor). If you were using ifupdown, just remember to deactivate the entries in /etc/network/interfaces that you want Network Manager to handle. âÂ [https://wiki.gnome.org/Projects/NetworkManager/SystemSettings](https://wiki.gnome.org/Projects/NetworkManager/SystemSettings) âÂ [https://developer.gnome.org/NetworkManager/1.30/ref-settings.html](https://developer.gnome.org/NetworkManager/1.30/ref-settings.html)

			If Network Manager is not installed, then the installer will configure ifupdown by creating the /etc/network/interfaces file. A line starting with auto gives a list of interfaces to be automatically configured on boot by the networking service. When there are many interfaces, it is good practice to keep the configuration in different files inside /etc/network/interfaces.d/ as described in sidebar [BACK TO BASICS Directories ending in .d](sect.apt-get.en.md#sidebar.directory.d).
		
			In a server context, ifupdown is thus the network configuration tool that you usually get. That is why we will cover it in the next sections. For more information about the syntax of the configuration file please read interfaces(5).
		

### 8.2.1.Â Ethernet Interface

				If the computer has an Ethernet card, the IP network that is associated with it must be configured by choosing from one of two methods. The simplest method is dynamic configuration with DHCP, and it requires a DHCP server on the local network. It may indicate a desired hostname, corresponding to the hostname setting in the example below. The DHCP server then sends configuration settings for the appropriate network.
			

ExampleÂ 8.1.Â DHCP configuration

```bash
auto enp0s31f6
iface enp0s31f6 inet dhcp
  hostname arrakis
```

IN PRACTICE Names of network interfaces

				By default, the kernel attributes generic names such as eth0 (for wired Ethernet) or wlan0 (for WiFi) to the network interfaces. The number in those names is a simple incremental counter representing the order in which they have been detected. With modern hardware, that order (at least in theory) might change for each reboot and thus the default names are not reliable.
			
				Fortunately, systemd and udev are able to rename the interfaces as soon as they appear. The default name policy is defined by /lib/systemd/network/99-default.link (see systemd.link(5) for an explanation of the NamePolicy entry in that file). In practice, the names are often based on the device's physical location (as guessed by where they are connected) and you will see names starting with en for wired ethernet and wl for WiFi. In the example above, the rest of the name indicates, in abbreviated form, a PCI (p) bus number (0), a slot number (s31), a function number (f6). Thus the name becomes predictable.
			
				Obviously, you are free to override this policy and/or to complement it to customize the names of some specific interfaces. You can find out the names of the network interfaces in the output of ip addr (or as filenames in /sys/class/net/).
			
				In some corner cases it might be necessary to disable the predictable naming of network devices as described above. Besides changing the default udev rule it is also possible to boot the system using the net.ifnames=0 (restores the default kernel behavior) and biosdevname=0 kernel parameters to achieve that.
			
				The predecessor of the predictable name scheme was called âpersistent name schemeâ. It was done using a udev rule to assign the device name based on the MAC address. This scheme has been abandoned and with the release of Debian 10 Buster users have been encouraged to migrate to predictable network device names. âÂ [https://www.debian.org/releases/buster/amd64/release-notes/ch-information.en.html#migrate-interface-names](https://www.debian.org/releases/buster/amd64/release-notes/ch-information.en.html#migrate-interface-names)

				A âstaticâ configuration must indicate network settings in a fixed manner. This includes at least the IP address and subnet mask; network and broadcast addresses are also sometimes listed. A router connecting to the exterior will be specified as a gateway.
			

ExampleÂ 8.2.Â Static configuration

```bash
auto enp0s31f6
iface enp0s31f6 inet static
  address 192.168.0.3/24
  broadcast 192.168.0.255
  network 192.168.0.0
  gateway 192.168.0.1
```

NOTE Multiple addresses

				It is possible not only to associate several interfaces to a single, physical network card, but also several IP addresses to a single interface. To assign them, just create several iface entries for the same device. Remember also that an IP address may correspond to any number of names via DNS, and that a name may also correspond to any number of numerical IP addresses.
			
				As you can guess, the configurations can be rather complex, but these options are only used in very special cases. The examples cited here are typical of the usual configurations.
			

### 8.2.2.Â Wireless Interface

				Getting wireless network cards to work can be a bit more challenging. First of all, they often require the installation of proprietary firmwares which are not installed by default in Debian. Then wireless networks rely on cryptography to restrict access to authorized users only, this implies storing some secret key in the network configuration. Let's tackle those topics one by one.
			

#### 8.2.2.1.Â Installing the required firmwares

					First you have to enable the non-free repository in APT's sources.list file: see [SectionÂ 6.1, âFilling in the sources.list Fileâ](apt.en.md#sect.apt-sources.list) for details about this file. Many firmware are proprietary and are thus located in this repository. You can try to skip this step if you want, but if the next step doesn't find the required firmware, retry after having enabled the non-free section.
				
					Then you have to install the appropriate firmware-* packages. If you don't know which package you need, you can install the isenkram package and run its isenkram-autoinstall-firmware command. The packages are often named after the hardware manufacturer or the corresponding kernel module: firmware-iwlwifi for Intel wireless cards, firmware-atheros for Qualcomm Atheros, firmware-ralink for Ralink, etc. A reboot is then recommended because the kernel driver usually looks for the firmware files when it is first loaded and no longer afterwards.
				

#### 8.2.2.2.Â Wireless specific entries in /etc/network/interfaces

ifupdown is able to manage wireless interfaces but it needs the help of the wpasupplicant package which provides the required integration between ifupdown and the wpa_supplicant command used to configure the wireless interfaces (when using WPA/WPA2 encryption). The usual entry in /etc/network/interfaces needs to be extended with two supplementary parameters to specify the name of the wireless network (aka its SSID) and the Pre-Shared Key (PSK).
				

ExampleÂ 8.3.Â DHCP configuration for a wireless interface

```bash
auto wlp4s0
iface wlp4s0 inet dhcp
  wpa-ssid Falcot
  wpa-psk ccb290fd4fe6b22935cbae31449e050edd02ad44627b16ce0151668f5f53c01b
```

					The wpa-psk parameter can contain either the plain text passphrase or its hashed version generated with wpa_passphrase SSID passphrase. If you use an unencrypted wireless connection, then you should put a wpa-key-mgmt NONE and no wpa-psk entry. For more information about the possible configuration options, have a look at /usr/share/doc/wpasupplicant/README.Debian.gz.
				
					At this point, you should consider restricting the read permissions on /etc/network/interfaces to the root user only since the file contains a private key that not all users should have access to.
				

HISTORY WEP encryption

					Usage of the deprecated WEP encryption protocol is possible with the wireless-tools package. See /usr/share/doc/wireless-tools/README.Debian for instructions.
				

### 8.2.3.Â Connecting with PPP through a PSTN Modem

				A point to point (PPP) connection establishes an intermittent connection; this is the most common solution for connections made with a telephone modem (âPSTN modemâ, since the connection goes over the public switched telephone network).
			
				A connection by telephone modem requires an account with an access provider, including a telephone number, username, password, and, sometimes the authentication protocol to be used. Such a connection is configured using the pppconfig tool in the Debian package of the same name. By default, it sets up a connection named provider (as in Internet service provider). When in doubt about the authentication protocol, choose PAP: it is offered by the majority of Internet service providers.
			
				After configuration, it is possible to connect using the pon command (giving it the name of the connection as a parameter, when the default value of provider is not appropriate). The link is disconnected with the poff command. These two commands can be executed by the root user, or by any other user, provided they are in the dip group.
			

### 8.2.4.Â Connecting through an ADSL Modem

				The generic term âADSL modemâ covers a multitude of devices with very different functions. The modems that are simplest to use with Linux are those that have an Ethernet interface (and not only a USB interface). These tend to be popular; most ADSL Internet service providers lend (or lease) a âboxâ with Ethernet interfaces. Depending on the type of modem, the configuration required can vary widely.
			

#### 8.2.4.1.Â Modems Supporting PPPOE

					Some Ethernet modems work with the PPPOE protocol (Point to Point Protocol over Ethernet). The pppoeconf tool (from the package with the same name) will configure the connection. To do so, it modifies the /etc/ppp/peers/dsl-provider file with the settings provided and records the login information in the /etc/ppp/pap-secrets and /etc/ppp/chap-secrets files. It is recommended to accept all modifications that it proposes.
				
					Once this configuration is complete, you can open the ADSL connection with the command, pon dsl-provider and disconnect with poff dsl-provider.
				

TIP Starting ppp at boot

					PPP connections over ADSL are, by definition, intermittent. Since they are usually not billed according to time, there are few downsides to the temptation of keeping them always open. The standard means to do so is to use the init system.
				
					With systemd, adding an automatically restarting task for the ADSL connection is a simple matter of creating a âunit fileâ such as /etc/systemd/system/adsl-connection.service, with contents such as the following:
				

```bash
[Unit]
Description=ADSL connection

[Service]
Type=forking
ExecStart=/usr/sbin/pppd call dsl-provider
Restart=always

[Install]
WantedBy=multi-user.target
```

					Once this unit file has been defined, it needs to be enabled with systemctl enable adsl-connection. Then the loop can be started manually with systemctl start adsl-connection; it will also be started automatically on boot.
				
					On systems not using systemd (including Wheezy and earlier versions of Debian), the standard SystemV init works differently. On such systems, all that is needed is to add a line such as the following at the end of the /etc/inittab file; then, any time the connection is disconnected, init will reconnect it.
				

```bash
adsl:2345:respawn:/usr/sbin/pppd call dsl-provider
```

					For ADSL connections that auto-disconnect on a daily basis, this method reduces the duration of the interruption.
				

#### 8.2.4.2.Â Modems Supporting PPTP

					The PPTP (Point-to-Point Tunneling Protocol) protocol was created by Microsoft. Deployed at the beginning of ADSL, it was quickly replaced by PPPOE. If this protocol is forced on you, see [SectionÂ 10.3.4, âPPTPâ](sect.virtual-private-network.en.md#sect.pptp).
				

#### 8.2.4.3.Â Modems Supporting DHCP

					When a modem is connected to the computer by an Ethernet cable (crossover cable) you typically configure a network connection by DHCP on the computer; the modem automatically acts as a gateway by default and takes care of routing (meaning that it manages the network traffic between the computer and the Internet).
				

BACK TO BASICS Crossover cable for a direct Ethernet connection

					Computer network cards expect to receive data on specific wires in the cable, and send their data on others. When you connect a computer to a local network, you usually connect a cable (straight or crossover) between the network card and a repeater or switch. However, if you want to connect two computers directly (without an intermediary switch or repeater), you must route the signal sent by one card to the receiving side of the other card, and vice-versa. This is the purpose of a crossover cable, and the reason it is used.
				
					Note that this distinction has become almost irrelevant over time, as modern network cards are able to detect the type of cable present and adapt accordingly, so it won't be unusual that both kinds of cable will work in a given location.
				
					Most âADSL routersâ on the market can be used like this, as do most of the ADSL modems provided by Internet services providers.
				

### 8.2.5.Â Automatic Network Configuration for Roaming Users

				Many Falcot engineers have a laptop computer that, for professional purposes, they also use at home. The network configuration to use differs according to location. At home, it may be a wifi network (protected by a WPA key), while the workplace uses a wired network for greater security and more bandwidth.
			
				To avoid having to manually connect or disconnect the corresponding network interfaces, administrators installed the network-manager package on these roaming machines. This software enables a user to easily switch from one network to another using a small icon displayed in the notification area of their graphical desktop. Clicking on this icon displays a list of available networks (both wired and wireless), so they can simply choose the network they wish to use. The program saves the configuration for the networks to which the user has already connected, and automatically switches to the best available network when the current connection drops.
			
				In order to do this, the program is structured in two parts: a daemon running as root handles activation and configuration of network interfaces and a user interface controls this daemon. PolicyKit handles the required authorizations to control this program and Debian configured PolicyKit in such a way so that members of the netdev group can add or change Network Manager connections.
			
				Network Manager knows how to handle various types of connections (DHCP, manual configuration, local network), but only if the configuration is set with the program itself. This is why it will systematically ignore all network interfaces in /etc/network/interfaces and /etc/network/interfaces.d/ for which it is not suited. Since Network Manager doesn't give details when no network connections are shown, the easy way is to delete from /etc/network/interfaces any configuration for all interfaces that must be managed by Network Manager.
			
				Note that this program is installed by default when the âDesktop Environmentâ task is chosen during initial installation.
			[PrevChapterÂ 8.Â Basic Configuration: Network, Accounts...](basic-configuration.en.md)[Up](#)[Home](index.en.md)[Next8.3.Â Setting the Hostname and Configuring the Nam...](sect.hostname-name-service.en.md)