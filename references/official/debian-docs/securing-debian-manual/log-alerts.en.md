---
title: "4.13.4. Log file permissions"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/log-alerts.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/log-alerts.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.13.4. Log file permissions"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](tcpwrappers.en.md)Securing Debian Manual[Next](kernel-patches.en.md)

## 4.13.Â The importance of logs and alerts

			It is easy to see that the treatment of logs and alerts is an important issue in a secure system. Suppose a system is perfectly configured and 99% secure. If the 1% attack occurs, and there are no security measures in place to, first, detect this and, second, raise alarms, the system is not secure at all.
		
			Debian GNU/Linux provides some tools to perform log analysis, most notably swatch, [[27]](#ftn.idm1731) logcheck or log-analysis (all will need some customisation to remove unnecessary things from the report). It might also be useful, if the system is nearby, to have the system logs printed on a virtual console. This is useful since you can (from a distance) see if the system is behaving properly. Debian's /etc/syslog.conf comes with a commented default configuration; to enable it uncomment the lines and restart syslogd (/etc/init.d/syslogd restart): 

```bash
daemon,mail.*;\
        news.=crit;news.=err;news.=notice;\
        *.=debug;*.=info;\
        *.=notice;*.=warn       /dev/tty8
```

			To colorize the logs, you could take a look at colorize, ccze or glark. There is a lot to log analysis that cannot be fully covered here, so a good information resource would be books should as [http://books.google.com/books?id=UyktqN6GnWEC](http://books.google.com/books?id=UyktqN6GnWEC). In any case, even automated tools are no match for the best analysis tool: your brain.
		

### 4.13.1.Â Using and customizing logcheck

				The logcheck package in Debian is divided into the three packages logcheck (the main program), logcheck-database (a database of regular expressions for the program) and logtail (prints loglines that have not yet been read). The Debian default (in /etc/cron.d/logcheck) is that logcheck is run every hour and after reboots.
			
				This tool can be quite useful if properly customized to alert the administrator of unusual system events. Logcheck can be fully customized so that it sends mails based on events found in the logs and worthy of attention. The default installation includes profiles for ignored events and policy violations for three different setups (workstation, server and paranoid). The Debian package includes a configuration file /etc/logcheck/logcheck.conf, sourced by the program, that defines which user the checks are sent to. It also provides a way for packages that provide services to implement new policies in the directories: /etc/logcheck/cracking.d/_packagename_, /etc/logcheck/violations.d/_packagename_, /etc/logcheck/violations.ignore.d/_packagename_, /etc/logcheck/ignore.d.paranoid/_packagename_, /etc/logcheck/ignore.d.server/_packagename_, and /etc/logcheck/ignore.d.workstation/_packagename_. However, not many packages currently do so. If you have a policy that can be useful for other users, please send it as a bug report for the appropriate package (as a wishlist bug). For more information read /usr/share/doc/logcheck/README.Debian.
			
				The best way to configure logcheck is to edit its main configuration file /etc/logcheck/logcheck.conf after installation. Change the default user (root) to whom reports should be mailed. You should set the reportlevel in there, too. logcheck-database has three report levels of increasing verbosity: workstation, server, paranoid. "server" being the default level, paranoid is only recommended for high-security machines running as few services as possible and workstation for relatively sheltered, non-critical machines. If you wish to add new log files just add them to /etc/logcheck/logcheck.logfiles. It is tuned for default syslog install.
			
				Once this is done you might want to check the mails that are sent, for the first few days/weeks/months. If you find you are sent messages you do not wish to receive, just add the regular expressions (see regex(7) and egrep(1)) that correspond to these messages to the /etc/logcheck/ignore.d.reportlevel/local. Try to match the whole logline. Details on howto write rules are explained in /usr/share/doc/logcheck-database/README.logcheck-database.gz. It's an ongoing tuning process; once the messages that are sent are always relevant you can consider the tuning finished. Note that if logcheck does not find anything relevant in your system it will not mail you even if it does run (so you might get a mail only once a week, if you are lucky).
			

### 4.13.2.Â Configuring where alerts are sent

				Debian comes with a standard syslog configuration (in /etc/syslog.conf) that logs messages to the appropriate files depending on the system facility. You should be familiar with this; have a look at the syslog.conf file and the documentation if not. If you intend to maintain a secure system you should be aware of where log messages are sent so they do not go unnoticed.
			
				For example, sending messages to the console also is an interesting setup useful for many production-level systems. But for many such systems it is also important to add a new machine that will serve as loghost (i.e. it receives logs from all other systems).
			
				Root's mail should be considered also, many security controls (like snort) send alerts to root's mailbox. This mailbox usually points to the first user created in the system (check /etc/aliases). Take care to send root's mail to some place where it will be read (either locally or remotely).
			
				There are other role accounts and aliases on your system. On a small system, it's probably simplest to make sure that all such aliases point to the root account, and that mail to root is forwarded to the system administrator's personal mailbox.
			
				FIXME: It would be interesting to tell how a Debian system can send/receive SNMP traps related to security problems (jfs). Check: snmptrapfmt, snmp and snmpd.
			

### 4.13.3.Â Using a loghost

				A loghost is a host which collects syslog data remotely over the network. If one of your machines is cracked, the intruder is not able to cover the tracks, unless hacking the loghost as well. So, the loghost should be especially secure. Making a machine a loghost is simple. Just start the syslogd with 

```bash
syslogd -r
```

				 and a new loghost is born. In order to do this permanently in Debian, edit /etc/default/syslogd and change the line  

```bash
SYSLOGD=""
```

				 to 

```bash
SYSLOGD="-r"
```

				 Next, configure the other machines to send data to the loghost. Add an entry like the following to /etc/syslog.conf: 

```bash
facility.level            @your_loghost
```

				 See the documentation for what to use in place of facility and level (they should not be entered verbatim like this). If you want to log everything remotely, just write: 

```bash
*.*                       @your_loghost
```

				 into your syslog.conf. Logging remotely as well as locally is the best solution (the attacker might presume to have covered his tracks after deleting the local log files). See the syslog(3), syslogd(8) and syslog.conf(5) manpages for additional information.
			

### 4.13.4.Â Log file permissions

				It is not only important to decide how alerts are used, but also who has read/modify access to the log files (if not using a remote loghost). Security alerts which the attacker can change or disable are not worth much in the event of an intrusion. Also, you have to take into account that log files might reveal quite a lot of information about your system to an intruder who has access to them.
			
				Some log file permissions are not perfect after the installation (but of course this really depends on your local security policy). First /var/log/lastlog and /var/log/faillog do not need to be readable by normal users. In the lastlog file you can see who logged in recently, and in the faillog you see a summary of failed logins. The author recommends chmod 660 for both. Take a brief look at your log files and decide very carefully which log files to make readable/writable for a user with a UID other than 0 and a group other than 'adm' or 'root'. You can easily check this in your system with: 

```bash
#  find /var/log -type f -exec ls -l {} \; | cut -c 17-35 |sort -u
  (see to what users do files in /var/log belong)
  #  find /var/log -type f -exec ls -l {} \; | cut -c 26-34 |sort -u
  (see to what groups do files in /var/log belong)
  # find /var/log -perm +004
  (files which are readable by any user)
  #  find /var/log \! -group root \! -group adm -exec ls -ld {} \;
  (files which belong to groups not root or adm)
```

				To customize how log files are created you will probably have to customize the program that generates them. If the log file gets rotated, however, you can customize the behavior of creation and rotation.
			[[27]](#idm1731)
				there's a very good article on it written by [http://www.spitzner.net/swatch.html](http://www.spitzner.net/swatch.html)
[Prev4.12.Â Using tcpwrappers](tcpwrappers.en.md)[Up](#)[Home](index.en.md)[Next4.14.Â Adding kernel patches](kernel-patches.en.md)