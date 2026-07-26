---
title: "14.4.3. Creating a new profile"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.apparmor.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.apparmor.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "14.4.3. Creating a new profile"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.supervision.en.md)The Debian Administrator's Handbook[Next](sect.selinux.en.md)

## 14.4.Â Introduction to AppArmor

### 14.4.1.Â Principles

				AppArmor is a Mandatory Access Control (MAC) system built on Linux's LSM (Linux Security Modules) interface. In practice, the kernel queries AppArmor before each system call to know whether the process is authorized to do the given operation. Through this mechanism, AppArmor confines programs to a limited set of resources.
			
				AppArmor applies a set of rules (known as âprofileâ) on each program. The profile applied by the kernel depends on the installation path of the program being executed. Contrary to SELinux (discussed in [SectionÂ 14.5, âIntroduction to SELinuxâ](sect.selinux.en.md)), the rules applied do not depend on the user. All users face the same set of rules when they are executing the same program (but traditional user permissions still apply and might result in different behavior!).
			
				AppArmor profiles are stored in /etc/apparmor.d/ and they contain a list of access control rules on resources that each program can make use of. The profiles are compiled and loaded into the kernel by the apparmor_parser command. Each profile can be loaded either in enforcing or complaining mode. The former enforces the policy and reports violation attempts, while the latter does not enforce the policy but still logs the system calls that would have been denied.
			

### 14.4.2.Â Enabling AppArmor and managing AppArmor profiles

				AppArmor support is built into the standard kernels provided by Debian. Enabling AppArmor is thus just a matter of installing some packages by executing apt install apparmor apparmor-profiles apparmor-utils with root privileges.
			
				AppArmor is functional after the installation, and aa-status will confirm it quickly:
			

```bash
# aa-status
apparmor module is loaded.
32 profiles are loaded.
15 profiles are in enforce mode.
   /usr/bin/man
[...]
17 profiles are in complain mode.
   /usr/sbin/dnsmasq
[...]
1 processes have profiles defined.
1 processes are in enforce mode.
   /usr/sbin/libvirtd (468) libvirtd
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.
```

NOTE More AppArmor profiles

				The apparmor-profiles package contains profiles managed by the upstream AppArmor community. To get even more profiles you can install apparmor-profiles-extra which contains profiles developed by Ubuntu and Debian.
			
				The state of each profile can be switched between enforcing and complaining with calls to aa-enforce and aa-complain giving as parameter either the path of the executable or the path to the policy file. Additionally a profile can be entirely disabled with aa-disable or put in audit mode (to log accepted system calls too) with aa-audit.
			

```bash
# aa-enforce /usr/bin/pidgin
Setting /usr/bin/pidgin to enforce mode.
# aa-complain /usr/sbin/dnsmasq
Setting /usr/sbin/dnsmasq to complain mode.
```

### 14.4.3.Â Creating a new profile

				Even though creating an AppArmor profile is rather easy, most programs do not have one. This section will show you how to create a new profile from scratch just by using the target program and letting AppArmor monitor the system call it makes and the resources it accesses.
			
				The most important programs that need to be confined are the network facing programs as those are the most likely targets of remote attackers. That is why AppArmor conveniently provides an aa-unconfined command to list the programs which have no associated profile and which expose an open network socket. With the --paranoid option you get all unconfined processes that have at least one active network connection.
			

```bash
# aa-unconfined
451 /usr/bin/containerd not confined
467 /usr/sbin/sshd (sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups) not confined
892 /usr/sbin/exim4 not confined
```

				In the following example, we will thus try to create a profile for /sbin/dhclient (there already is a profile shipped by apparmor-profiles, so you can compare your results to the official one). For this we will use aa-genprof dhclient. It will invite you to use the application in another window and when done to come back to aa-genprof to scan for AppArmor events in the system logs and convert those logs into access rules. For each logged event, it will make one or more rule suggestions that you can either approve or further edit in multiple ways:
			

```bash
# aa-genprof dhclient
Writing updated profile for /usr/sbin/dhclient.
Setting /usr/sbin/dhclient to complain mode.

Before you begin, you may wish to check if a
profile already exists for the application you
wish to confine. See the following wiki page for
more information:
https://gitlab.com/apparmor/apparmor/wikis/Profiles

Profiling: /usr/sbin/dhclient

Please start the application to be profiled in
another window and exercise its functionality now.

Once completed, select the "Scan" option below in 
order to scan the system logs for AppArmor events. 

For each AppArmor event, you will be given the 
opportunity to choose whether the access should be 
allowed or denied.

[(S)can system log for AppArmor events] / (F)inish
S
Reading log entries from /var/log/syslog.

Profile:  /usr/sbin/dhclient 
Execute:  /usr/sbin/dhclient-script
Severity: unknown

(I)nherit / (C)hild / (P)rofile / (N)amed / (U)nconfined / (X) ix On / (D)eny / Abo(r)t / (F)inish
P

Should AppArmor sanitise the environment when
switching profiles?

Sanitising environment is more secure,
but some applications depend on the presence
of LD_PRELOAD or LD_LIBRARY_PATH.

[(Y)es] / (N)o
Y
Writing updated profile for /usr/sbin/dhclient-script.
Complain-mode changes:

Profile:    /usr/sbin/dhclient 
Capability: net_raw
Severity:   8

 [1 - capability net_raw,]
(A)llow / [(D)eny] / (I)gnore / Audi(t) / Abo(r)t / (F)inish
A
Adding capability net_raw, to profile.

Profile:    /usr/sbin/dhclient
Capability: net_bind_service
Severity:   8

 [1 - #include <abstractions/nis>]
  2 - capability net_bind_service,
(A)llow / [(D)eny] / (I)gnore / Audi(t) / Abo(r)t / (F)inish
A
Adding #include <abstractions/nis> to profile.

Profile:  /usr/sbin/dhclient 
Path:     /etc/ssl/openssl.cnf
New Mode: owner r
Severity: 2

 [1 - #include <abstractions/lightdm>]
  2 - #include <abstractions/openssl>
  3 - #include <abstractions/ssl_keys>
  4 - owner /etc/ssl/openssl.cnf r,
(A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N)ew / Audi(t) / (O)wner permissions off / Abo(r)t / (F)inish
2

Profile:  /usr/sbin/dhclient
Path:     /etc/ssl/openssl.cnf
New Mode: owner r
Severity: 2

  1 - #include <abstractions/lightdm> 
 [2 - #include <abstractions/openssl>]
  3 - #include <abstractions/ssl_keys>
  4 - owner /etc/ssl/openssl.cnf r, 
[(A)llow] / (D)eny / (I)gnore / (G)lob / Glob with (E)xtension / (N)ew / Abo(r)t / (F)inish / (M)ore
A
[...]
Profile:  /usr/sbin/dhclient-script 
Path:     /usr/bin/dash
New Mode: owner r
Severity: unknown

  1 - #include <abstractions/gvfs-open>
 [2 - #include <abstractions/lightdm>]
  3 - #include <abstractions/ubuntu-browsers.d/plugins-common>
  4 - #include <abstractions/xdg-open>
  5 - owner /usr/bin/dash r,
(A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N)ew / Audi(t) / (O)wner permissions off / Abo(r)t / (F)inish
A
Adding #include <abstractions/lightdm> to profile.
Deleted 2 previous matching profile entries.

= Changed Local Profiles =

The following local profiles were changed. Would you like to save them?

 [1 - /usr/sbin/dhclient]
  2 - /usr/sbin/dhclient-script
(S)ave Changes / Save Selec(t)ed Profile / [(V)iew Changes] / View Changes b/w (C)lean profiles / Abo(r)t
S
Writing updated profile for /usr/sbin/dhclient.
Writing updated profile for /usr/sbin/dhclient-script.

Profiling: /usr/sbin/dhclient

Please start the application to be profiled in
another window and exercise its functionality now.

Once completed, select the "Scan" option below in
order to scan the system logs for AppArmor events.

For each AppArmor event, you will be given the
opportunity to choose whether the access should be
allowed or denied.

[(S)can system log for AppArmor events] / (F)inish
F
Setting /usr/sbin/dhclient to enforce mode.
Setting /usr/sbin/dhclient-script to enforce mode.

Reloaded AppArmor profiles in enforce mode.

Please consider contributing your new profile!
See the following wiki page for more information:
https://gitlab.com/apparmor/apparmor/wikis/Profiles

Finished generating profile for /usr/sbin/dhclient.
```

				Note that the program does not display back the control characters that you type but for the clarity of the explanation we have included them in the previous transcript.
			

[](#aa-genprof-execute)

						The first event detected is the execution of another program. In that case, you have multiple choices: you can run the program with the profile of the parent process (the âInheritâ choice), you can run it with its own dedicated profile (the âProfileâ and the âNamedâ choices, differing only by the possibility to use an arbitrary profile name), you can run it with a sub-profile of the parent process (the âChildâ choice), you can run it without any profile (the âUnconfinedâ choice) or you can decide to not run it at all (the âDenyâ choice).
					
						Note that when you opt to run it under a dedicated profile that doesn't exist yet, the tool will create the missing profile for you and will make rule suggestions for that profile in the same run.
					

[](#aa-genprof-capability)

						At the kernel level, the special powers of the root user have been split in âcapabilitiesâ. When a system call requires a specific capability, AppArmor will verify whether the profile allows the program to make use of this capability.
					

[](#aa-genprof-read)

						Here the program seeks read permissions for /etc/ssl/openssl.cnf. aa-genprof detected that this permission was also granted by multiple âabstractionsâ and offers them as alternative choices. An abstraction provides a reusable set of access rules grouping together multiple resources that are commonly used together. In this specific case, the file is generally accessed through the nameservice related functions of the C library and we type â2â to first select the â#include <abstractions/openssl>â choice and then âAâ to allow it.
					

[](#aa-genprof-other-profile)

						Notice that this access request is not part of the dhclient profile but of the new profile that we created when we allowed /usr/sbin/dhclient-script to run with its own profile.
					
						After having gone through all the logged events, the program offers to save all the profiles that were created during the run. In this case, we have two profiles that we save at once with âSaveâ (but you can save them individually too) before leaving the program with âFinishâ.
					
aa-genprof is in fact only a smart wrapper around aa-logprof: it creates an empty profile, loads it in complain mode and then run aa-logprof which is a tool to update a profile based on the profile violations that have been logged. So you can re-run that tool later to improve the profile that you just created.
			
				If you want the generated profile to be complete, you should use the program in all the ways that it is legitimately used. In the case of dhclient, it means running it via Network Manager, running it via ifupdown, running it manually, etc. In the end, you might get a /etc/apparmor.d/usr.sbin.dhclient close to the profile shipped by apparmor-profiles in /usr/share/apparmor/extra-profiles/sbin.dhclient.
			
				And /etc/apparmor.d/usr.sbin.dhclient-script might be similar to /usr/share/apparmor/extra-profiles/sbin.dhclient, shipped in apparmor-profiles too.
			[Prev14.3.Â Supervision: Prevention, Detection, Deterre...](sect.supervision.en.md)[Up](#)[Home](index.en.md)[Next14.5.Â Introduction to SELinux](sect.selinux.en.md)