---
title: "14.1. Defining a Security Policy"
source: "https://www.debian.org/doc/manuals/debian-handbook/security.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/security.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "14.1. Defining a Security Policy"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.rtc-clients.en.md)The Debian Administrator's Handbook[Next](sect.firewall-packet-filtering.en.md)

# ChapterÂ 14.Â Security

[14.1. Defining a Security Policy](security.en.md#sect.defining-security-policy)[14.2. Firewall or Packet Filtering](sect.firewall-packet-filtering.en.md)[14.2.1. nftables Behavior](sect.firewall-packet-filtering.en.md#sect.netfilter)[14.2.2. Moving from iptables to nftables](sect.firewall-packet-filtering.en.md#id-1.17.5.14)[14.2.3. Syntax of nft](sect.firewall-packet-filtering.en.md#sect.nftables)[14.2.4. Installing the Rules at Each Boot](sect.firewall-packet-filtering.en.md#sect.install-rules-at-boot)[14.3. Supervision: Prevention, Detection, Deterrence](sect.supervision.en.md)[14.3.1. Monitoring Logs with logcheck](sect.supervision.en.md#sect.logcheck)[14.3.2. Monitoring Activity](sect.supervision.en.md#sect.monitoring-activity)[14.3.3. Avoiding Intrusion](sect.supervision.en.md#id-1.17.6.6)[14.3.4. Detecting Changes](sect.supervision.en.md#id-1.17.6.7)[14.3.5. Detecting Intrusion (IDS/NIDS)](sect.supervision.en.md#sect.intrusion-detection)[14.4. Introduction to AppArmor](sect.apparmor.en.md)[14.4.1. Principles](sect.apparmor.en.md#sect.apparmor-principles)[14.4.2. Enabling AppArmor and managing AppArmor profiles](sect.apparmor.en.md#sect.apparmor-setup)[14.4.3. Creating a new profile](sect.apparmor.en.md#sect.apparmor-new-profile)[14.5. Introduction to SELinux](sect.selinux.en.md)[14.5.1. Principles](sect.selinux.en.md#sect.selinux-principles)[14.5.2. Setting Up SELinux](sect.selinux.en.md#sect.selinux-setup)[14.5.3. Managing an SELinux System](sect.selinux.en.md#sect.selinux-management)[14.5.4. Adapting the Rules](sect.selinux.en.md#sect.selinux-custom-rules)[14.6. Other Security-Related Considerations](sect.other-security-considerations.en.md)[14.6.1. Inherent Risks of Web Applications](sect.other-security-considerations.en.md#id-1.17.9.3)[14.6.2. Knowing What To Expect](sect.other-security-considerations.en.md#id-1.17.9.4)[14.6.3. Choosing the Software Wisely](sect.other-security-considerations.en.md#sect.choosing-the-software-wisely)[14.6.4. Managing a Machine as a Whole](sect.other-security-considerations.en.md#sect.managing-a-machine-as-a-whole)[14.6.5. Users Are Players](sect.other-security-considerations.en.md#sect.users-are-players)[14.6.6. Physical Security](sect.other-security-considerations.en.md#sect.physical-security)[14.6.7. Legal Liability](sect.other-security-considerations.en.md#id-1.17.9.9)[14.7. Dealing with a Compromised Machine](sect.dealing-with-compromised-machine.en.md)[14.7.1. Detecting and Seeing the Cracker's Intrusion](sect.dealing-with-compromised-machine.en.md#id-1.17.10.3)[14.7.2. Putting the Server Off-Line](sect.dealing-with-compromised-machine.en.md#id-1.17.10.4)[14.7.3. Keeping Everything that Could Be Used as Evidence](sect.dealing-with-compromised-machine.en.md#sect.keeping-everything-that-could-be-used-as-evidence)[14.7.4. Re-installing](sect.dealing-with-compromised-machine.en.md#id-1.17.10.6)[14.7.5. Forensic Analysis](sect.dealing-with-compromised-machine.en.md#sect.forensic-analysis)[14.7.6. Reconstituting the Attack Scenario](sect.dealing-with-compromised-machine.en.md#sect.reconstituting-the-attack-scenario)
		An information system can have a varying level of importance depending on the environment. In some cases, it is vital to a company's survival. It must therefore be protected from various kinds of risks. The process of evaluating these risks, defining and implementing the protection is collectively known as the âsecurity processâ.
	

## 14.1.Â Defining a Security Policy

CAUTION Scope of this chapter

			Security is a vast and very sensitive subject, so we cannot claim to describe it in any kind of comprehensive manner in the course of a single chapter. We will only delineate a few important points and describe some of the tools and methods that can be of use in the security domain. For further reading, literature abounds, and entire books have been devoted to the subject. 
		
			The word âsecurityâ itself covers a vast range of concepts, tools and procedures, none of which apply universally. Choosing among them requires a precise idea of what your goals are. Securing a system starts with answering a few questions. Rushing headlong into implementing an arbitrary set of tools runs the risk of focusing on the wrong aspects of security.
		
			The very first thing to determine is therefore the goal. A good approach to help with that determination starts with the following questions:
		
What are we trying to protect? The security policy will be different depending on whether we want to protect computers or data. In the latter case, we also need to know which data.
				
					What are we trying to protect against? Is it leakage of confidential data? Accidental data loss? Revenue loss caused by disruption of service?
				
					Also, who are we trying to protect against? Security measures will be quite different for guarding against a typo by a regular user of the system than they would be when protecting against a determined attacker group.
				
			The term âriskâ is customarily used to refer collectively to these three factors: what to protect, what needs to be prevented from happening, and who will try to make it happen. Modeling the risk requires answers to these three questions. From this risk model, a security policy can be constructed, and the policy can be implemented with concrete actions.
		

NOTE Permanent questioning

			Bruce Schneier, a world expert in security matters (not only computer security) tries to counter one of security's most important myths with a motto: âSecurity is a process, not a productâ. Assets to be protected change in time, and so do threats and the means available to potential attackers. Even if a security policy has initially been perfectly designed and implemented, one should never rest on one's laurels. The risk components evolve, and the response to that risk must evolve accordingly.
		
			Extra constraints are also worth taking into account, as they can restrict the range of available policies. How far are we willing to go to secure a system? This question has a major impact on the policy to implement. The answer is too often only defined in terms of monetary costs, but the other elements should also be considered, such as the amount of inconvenience imposed on system users or performance degradation.
		
			Once the risk has been modeled, one can start thinking about designing an actual security policy.
		

NOTE Extreme policies

			There are cases where the choice of actions required to secure a system is extremely simple.
		
			For instance, if the system to be protected only comprises a second-hand computer, the sole use of which is to add a few numbers at the end of the day, deciding not to do anything special to protect it would be quite reasonable. The intrinsic value of the system is low. The value of the data is zero since they are not stored on the computer. A potential attacker infiltrating this âsystemâ would only gain an unwieldy calculator. The cost of securing such a system would probably be greater than the cost of a breach.
		
			At the other end of the spectrum, we might want to protect the confidentiality of secret data in the most comprehensive way possible, trumping any other consideration. In this case, an appropriate response would be the total destruction of these data (securely erasing the files, shredding of the hard disks to bits, then dissolving these bits in acid, and so on). If there is an additional requirement that data must be kept in store for future use (although not necessarily readily available), and if cost still isn't a factor, then a starting point would be storing the data on iridiumâplatinum alloy plates stored in bomb-proof bunkers under various mountains in the world, each of which being (of course) both entirely secret and guarded by entire armiesâ¦
		
			Extreme though these examples may seem, they would, nevertheless, be an adequate response to defined risks, insofar as they are the outcome of a thought process that takes into account the goals to reach and the constraints to fulfill. When coming from a reasoned decision, no security policy is less respectable than any other.
		
			In most cases, the information system can be segmented in consistent and mostly independent subsets. Each subsystem will have its own requirements and constraints, and so the risk assessment and the design of the security policy should be undertaken separately for each. A good principle to keep in mind is that a short and well-defined perimeter is easier to defend than a long and winding frontier. The network organization should also be designed accordingly: the sensitive services should be concentrated on a small number of machines, and these machines should only be accessible via a minimal number of check-points; securing these check-points will be easier than securing all the sensitive machines against the entirety of the outside world. It is at this point that the usefulness of network filtering (including by firewalls) becomes apparent. This filtering can be implemented with dedicated hardware, but a possibly simpler and more flexible solution is to use a software firewall such as the one integrated in the Linux kernel.
		[Prev13.9.Â Real-Time Communications software](sect.rtc-clients.en.md)[Up](#)[Home](index.en.md)[Next14.2.Â Firewall or Packet Filtering](sect.firewall-packet-filtering.en.md)