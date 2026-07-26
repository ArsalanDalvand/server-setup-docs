---
title: "12.4.2. Setting Up Nagios"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.monitoring.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.monitoring.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "12.4.2. Setting Up Nagios"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.automated-installation.en.md)The Debian Administrator's Handbook[Next](workstation.en.md)

## 12.4.Â Monitoring

			Monitoring is a generic term, and the various involved activities have several goals: on the one hand, following usage of the resources provided by a machine allows anticipating saturation and the subsequent required upgrades; on the other hand, alerting the administrator as soon as a service is unavailable or not working properly means that the problems that do happen can be fixed sooner.
		
Munin covers the first area, by displaying graphical charts for historical values of a number of parameters (used RAM, occupied disk space, processor load, network traffic, Apache/MySQL load, and so on). Nagios covers the second area, by regularly checking that the services are working and available, and sending alerts through the appropriate channels (e-mails, text messages, and so on). Both have a modular design, which makes it easy to create new plug-ins to monitor specific parameters or services.
		

ALTERNATIVE Zabbix, an integrated monitoring tool

			Although Munin and Nagios are in very common use, they are not the only players in the monitoring field, and each of them only handles half of the task (graphing on one side, alerting on the other). Zabbix, on the other hand, integrates both parts of monitoring; it also has a web interface for configuring the most common aspects. It has grown by leaps and bounds during the last few years, and can now be considered a viable contender. On the monitoring server, you would install zabbix-server-pgsql (or zabbix-server-mysql), possibly together with zabbix-frontend-php to have a web interface. On the hosts to monitor you would install zabbix-agent feeding data back to the server. âÂ [https://zabbix.com/](https://zabbix.com/)

ALTERNATIVE Icinga, a Nagios fork

			Spurred by divergences in opinions concerning the development model for Nagios (which is controlled by a company), a number of developers forked Nagios and use Icinga as their new name. Icinga is still compatible â so far â with Nagios configurations and plugins, but it also adds extra features. âÂ [https://icinga.com/](https://icinga.com/)

### 12.4.1.Â Setting Up Munin

				The purpose of Munin is to monitor many machines; therefore, it quite naturally uses a client/server architecture. The central host â the grapher â collects data from all the monitored hosts, and generates historical graphs.
			

#### 12.4.1.1.Â Configuring Hosts To Monitor

					The first step is to install the munin-node package. The daemon installed by this package listens on port 4949 and sends back the data collected by all the active plugins. Each plugin is a simple program returning a description of the collected data as well as the latest measured value. Plugins are stored in /usr/share/munin/plugins/, but only those with a symbolic link in /etc/munin/plugins/ are really used.
				
					When the package is installed, a set of active plugins is determined based on the available software and the current configuration of the host. However, this auto-configuration depends on a feature that each plugin must provide, and it is usually a good idea to review and tweak the results by hand. Browsing the Plugin Gallery can be interesting even though not all plugins have comprehensive documentation. âÂ [https://gallery.munin-monitoring.org](https://gallery.munin-monitoring.org)

					However, all plugins are scripts and most are rather simple and well-commented. Browsing /etc/munin/plugins/ is therefore a good way of getting an idea of what each plugin is about and determining which should be removed. Similarly, enabling an interesting plugin found in /usr/share/munin/plugins/ is a simple matter of setting up a symbolic link with ln -sf /usr/share/munin/plugins/plugin /etc/munin/plugins/. Note that when a plugin name ends with an underscore â_â, the plugin requires a parameter. This parameter must be stored in the name of the symbolic link; for instance, the âif_â plugin must be enabled with a if_eth0 symbolic link, and it will monitor network traffic on the eth0 interface.
				
					Once all plugins are correctly set up, the daemon configuration must be updated to describe access control for the collected data. This involves allow directives in the /etc/munin/munin-node.conf file. The default configuration is allow ^127\.0\.0\.1$, and only allows access to the local host. An administrator will usually add a similar line containing the IP address of the grapher host, then restart the daemon with systemctl restart munin-node.
				

GOING FURTHER Creating local plugins

					Munin does include detailed documentation on how plugins should behave, and how to develop new plugins. âÂ [https://guide.munin-monitoring.org/en/latest/plugin/writing.html](https://guide.munin-monitoring.org/en/latest/plugin/writing.html)

					A plugin is best tested when run in the same conditions as it would be when triggered by munin-node; this can be simulated by running munin-run plugin as root. A potential second parameter given to this command (such as config) is passed to the plugin as a parameter.
				
					When a plugin is invoked with the config parameter, it must describe itself by returning a set of fields:
				

```bash
# munin-run load config
graph_title Load average
graph_args --base 1000 -l 0
graph_vlabel load
graph_scale no
graph_category system
load.label load
graph_info The load average of the machine describes how many processes are in the run-queue (scheduled to run "immediately").
load.info 5 minute load average
```

					The various available fields are described by the âPlugin referenceâ available as part of the âMunin guideâ. âÂ [https://munin.readthedocs.org/en/latest/reference/plugin.html](https://munin.readthedocs.org/en/latest/reference/plugin.html)

					When invoked without a parameter, the plugin simply returns the last measured values; for instance, executing sudo munin-run load could return load.value 0.12.
				
					Finally, when a plugin is invoked with the autoconf parameter, it should return âyesâ (and a 0 exit status) or ânoâ (with a 1 exit status) according to whether the plugin should be enabled on this host.
				

#### 12.4.1.2.Â Configuring the Grapher

					The âgrapherâ is simply the computer that aggregates the data and generates the corresponding graphs. The required software is in the munin package. The standard configuration runs munin-cron (once every 5 minutes), which gathers data from all the hosts listed in /etc/munin/munin.conf (only the local host is listed by default), saves the historical data in RRD files (Round Robin Database, a file format designed to store data varying in time) stored under /var/lib/munin/ and generates an HTML page with the graphs in /var/cache/munin/www/.
				
					All monitored machines must therefore be listed in the /etc/munin/munin.conf configuration file. Each machine is listed as a full section with a name matching the machine and at least an address entry giving the corresponding IP address.
				

```bash
[ftp.falcot.com]
    address 192.168.0.12
    use_node_name yes
```

					Sections can be more complex, and describe extra graphs that could be created by combining data coming from several machines. The samples provided in the configuration file are good starting points for customization.
				
					The last step is to publish the generated pages; this involves configuring a web server so that the contents of /var/cache/munin/www/ are made available on a website. Access to this website will often be restricted, using either an authentication mechanism or IP-based access control. See [SectionÂ 11.2, âWeb Server (HTTP)â](sect.http-web-server.en.md) for the relevant details.
				

### 12.4.2.Â Setting Up Nagios

				Unlike Munin, Nagios does not necessarily require installing anything on the monitored hosts; most of the time, Nagios is used to check the availability of network services. For instance, Nagios can connect to a web server and check that a given web page can be obtained within a given time.
			

#### 12.4.2.1.Â Installing

					The first step in setting up Nagios is to install the nagios4 and monitoring-plugins packages. Installing the packages configures the web interface and the Apache server. The authz_groupfile and auth_digest Apache modules must be enabled, for that execute:
				

```bash
# a2enmod authz_groupfile
Considering dependency authz_core for authz_groupfile:
Module authz_core already enabled
Module authz_core already enabled
Enabling module authz_groupfile.
To activate the new configuration, you need to run:
  systemctl restart apache2
# a2enmod auth_digest
Considering dependency authn_core for auth_digest:
Module authn_core already enabled
Enabling module auth_digest.
To activate the new configuration, you need to run:
  systemctl restart apache2
# systemctl restart apache2
```

					Adding other users is a simple matter of inserting them in the /etc/nagios4/hdigest.users file.
				
					Pointing a browser at http://server/nagios4/ displays the web interface; in particular, note that Nagios already monitors some parameters of the machine where it runs. However, some interactive features such as adding comments to a host do not work. These features are disabled in the default configuration for Nagios, which is very restrictive for security reasons.
				
					Enabling some features involves editing /etc/nagios4/nagios.cfg. We also need to set up write permissions for the directory used by Nagios, with commands such as the following:
				

```bash
# systemctl stop nagios4
# dpkg-statoverride --update --add nagios www-data 2710 /var/lib/nagios4/rw
# dpkg-statoverride --update --add nagios nagios 751 /var/lib/nagios4
# systemctl start nagios4
```

#### 12.4.2.2.Â Configuring

					The Nagios web interface is rather nice, but it does not allow configuration, nor can it be used to add monitored hosts and services. The whole configuration is managed via files referenced in the central configuration file, /etc/nagios4/nagios.cfg.
				
					These files should not be dived into without some understanding of the Nagios concepts. The configuration lists objects of the following types:
				
							a host is a machine to be monitored;
						
							a hostgroup is a set of hosts that should be grouped together for display, or to factor some common configuration elements;
						
							a service is a testable element related to a host or a host group. It will most often be a check for a network service, but it can also involve checking that some parameters are within an acceptable range (for instance, free disk space or processor load);
						
							a servicegroup is a set of services that should be grouped together for display;
						
							a contact is a person who can receive alerts;
						
							a contactgroup is a set of such contacts;
						
							a timeperiod is a range of time during which some services have to be checked;
						
							a command is the command line invoked to check a given service.
						
					According to its type, each object has a number of properties that can be customized. A full list would be too long to include, but the most important properties are the relations between the objects.
				
					A service uses a command to check the state of a feature on a host (or a hostgroup) within a timeperiod. In case of a problem, Nagios sends an alert to all members of the contactgroup linked to the service. Each member is sent the alert according to the channel described in the matching contact object.
				
					An inheritance system allows easy sharing of a set of properties across many objects without duplicating information. Moreover, the initial configuration includes a number of standard objects; in many cases, defining new hosts, services and contacts is a simple matter of deriving from the provided generic objects. The files in /etc/nagios4/conf.d/ are a good source of information on how they work.
				
					The Falcot Corp administrators use the following configuration:
				

ExampleÂ 12.5.Â /etc/nagios4/conf.d/falcot.cfg file

```bash
define contact{
    name                            generic-contact
    service_notification_period     24x7
    host_notification_period        24x7
    service_notification_options    w,u,c,r
    host_notification_options       d,u,r
    service_notification_commands   notify-service-by-email
    host_notification_commands      notify-host-by-email
    register                        0 ; Template only
}
define contact{
    use             generic-contact
    contact_name    rhertzog
    alias           Raphael Hertzog
    email           hertzog@debian.org
}
define contact{
    use             generic-contact
    contact_name    rmas
    alias           Roland Mas
    email           lolando@debian.org
}

define contactgroup{
    contactgroup_name     falcot-admins
    alias                 Falcot Administrators
    members               rhertzog,rmas
}

define host{
    use                   generic-host ; Name of host template to use
    host_name             www-host
    alias                 www.falcot.com
    address               192.168.0.5
    contact_groups        falcot-admins
    hostgroups            debian-servers,ssh-servers
}
define host{
    use                   generic-host ; Name of host template to use
    host_name             ftp-host
    alias                 ftp.falcot.com
    address               192.168.0.12
    contact_groups        falcot-admins
    hostgroups            debian-servers,ssh-servers
}

# 'check_ftp' command with custom parameters
define command{
    command_name          check_ftp2
    command_line          /usr/lib/nagios/plugins/check_ftp -H $HOSTADDRESS$ -w 20 -c 30 -t 35
}

# Generic Falcot service
define service{
    name                  falcot-service
    use                   generic-service
    contact_groups        falcot-admins
    register              0
}

# Services to check on www-host
define service{
    use                   falcot-service
    host_name             www-host
    service_description   HTTP
    check_command         check_http
}
define service{
    use                   falcot-service
    host_name             www-host
    service_description   HTTPS
    check_command         check_https
}
define service{
    use                   falcot-service
    host_name             www-host
    service_description   SMTP
    check_command         check_smtp
}

# Services to check on ftp-host
define service{
    use                   falcot-service
    host_name             ftp-host
    service_description   FTP
    check_command         check_ftp2
}
```

					This configuration file describes two monitored hosts. The first one is the web server, and the checks are made on the HTTP (80) and secure-HTTP (443) ports. Nagios also checks that an SMTP server runs on port 25. The second host is the FTP server, and the check includes making sure that a reply comes within 20 seconds. Beyond this delay, a warning is emitted; beyond 30 seconds, the alert is deemed critical. The Nagios web interface also shows that the SSH service is monitored: this comes from the hosts belonging to the ssh-servers hostgroup. The matching standard service is defined in /etc/nagios4/conf.d/services_nagios2.cfg.
				
					Note the use of inheritance: an object is made to inherit from another object with the âuse parent-nameâ. The parent object must be identifiable, which requires giving it a âname identifierâ property. If the parent object is not meant to be a real object, but only to serve as a parent, giving it a âregister 0â property tells Nagios not to consider it, and therefore to ignore the lack of some parameters that would otherwise be required.
				

DOCUMENTATION List of object properties

					A more in-depth understanding of the various ways in which Nagios can be configured can be obtained from the documentation hosted on [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/). It includes a list of all object types, with all the properties they can have. It also explains how to create new plugins.
				

GOING FURTHER Remote tests with NRPE

					Many Nagios plugins allow checking some parameters local to a host; if many machines need these checks while a central installation gathers them, the NRPE (Nagios Remote Plugin Executor) plugin needs to be deployed. The nagios-nrpe-plugin package needs to be installed on the Nagios server, and nagios-nrpe-server on the hosts where local tests need to run. The latter gets its configuration from /etc/nagios/nrpe.cfg. This file should list the tests that can be started remotely, and the IP addresses of the machines allowed to trigger them. On the Nagios side, enabling these remote tests is a simple matter of adding matching services using the new check_nrpe command.
				[Prev12.3.Â Automated Installation](sect.automated-installation.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 13.Â Workstation](workstation.en.md)