---
title: "B.6. Security update protected by a firewall"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/fw-security-update.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/fw-security-update.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "B.6. Security update protected by a firewall"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](bind-chuser.en.md)Securing Debian Manual[Next](chroot-ssh-env.en.md)

## B.6.Â Security update protected by a firewall

			After a standard installation, a system may still have some security vulnerabilities. Unless you can download updates for the vulnerable packages on another system (or you have mirrored security.debian.org for local use), the system will have to be connected to the Internet for the downloads.
		
			However, as soon as you connect to the Internet you are exposing this system. If one of your local services is vulnerable, you might be compromised even before the update is finished! This may seem paranoid but, in fact, analysis from the [http://www.honeynet.org](http://www.honeynet.org) has shown that systems can be compromised in less than three days, even if the system is not publicly known (i.e., not published in DNS records).
		
			When doing an update on a system not protected by an external system like a firewall, it is possible to properly configure your local firewall to restrict connections involving only the security update itself. The example below shows how to set up such local firewall capabilities, which allow connections from security.debian.org only, logging all others.
		
			The following example can be use to setup a restricted firewall ruleset. Run this commands from a local console (not a remote one) to reduce the chances of locking yourself out of the system. 

```bash
# iptables -F
  # iptables -L
  Chain INPUT (policy ACCEPT)
  target     prot opt source               destination

  Chain FORWARD (policy ACCEPT)
  target     prot opt source               destination

  Chain OUTPUT (policy ACCEPT)
  target     prot opt source               destination
  # iptables -A OUTPUT -d security.debian.org --dport 80 -j ACCEPT
  # iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
  # iptables -A INPUT -p icmp -j ACCEPT
  # iptables -A INPUT -j LOG
  # iptables -A OUTPUT -j LOG
  # iptables -P INPUT DROP
  # iptables -P FORWARD DROP
  # iptables -P OUTPUT DROP
  # iptables -L
  Chain INPUT (policy DROP)
  target     prot opt source               destination
  ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0          state RELATED,ESTABLISHED
  ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0
  LOG        all  --  anywhere             anywhere           LOG level warning

  Chain FORWARD (policy DROP)
  target     prot opt source               destination

  Chain OUTPUT (policy DROP)
  target     prot opt source               destination
  ACCEPT     80   --  anywhere             security.debian.org
  LOG        all  --  anywhere             anywhere           LOG level warning
```

			Note: Using a DROP policy in the INPUT chain is the most correct thing to do, but be very careful when doing this after flushing the chain from a remote connection. When testing firewall rulesets from a remote location it is best if you run a script with the firewall ruleset (instead of introducing the ruleset line by line through the command line) and, as a precaution, keep a backdoor[[79]](#ftn.idm6407)

			Of course, you should disable any backdoors before getting the system into production. configured so that you can re-enable access to the system if you make a mistake. That way there would be no need to go to a remote location to fix a firewall ruleset that blocks you.
		
			FIXME: This needs DNS to be working properly since it is required for security.debian.org to work. You can add security.debian.org to /etc/hosts but now it is a CNAME to several hosts (there is more than one security mirror)
		
			FIXME: this will only work with HTTP URLs since ftp might need the ip_conntrack_ftp module, or use passive mode.
		[[79]](#idm6407)
				Such as knockd. Alternatively, you can open a different console and have the system ask for confirmation that there is somebody on the other side, and reset the firewall chain if no confirmation is given. The following test script could be of use: 

```bash
#!/bin/bash

while true; do
    read -n 1 -p "Are you there? " -t 30 ayt
    if [ -z "$ayt" ] ; then
        break
    fi
done

# Reset the firewall chain, user is not available
echo
echo "Resetting firewall chain!"
iptables -F
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
exit 1
```

[PrevB.5.Â Sample script to change the default Bind ins...](bind-chuser.en.md)[Up](#)[Home](index.en.md)[NextB.7.Â Chroot environment for SSH](chroot-ssh-env.en.md)