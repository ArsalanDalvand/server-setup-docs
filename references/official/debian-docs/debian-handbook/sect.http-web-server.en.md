---
title: "11.2.5. Log Analyzers"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.http-web-server.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.http-web-server.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.2.5. Log Analyzers"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](network-services.en.md)The Debian Administrator's Handbook[Next](sect.ftp-file-server.en.md)

## 11.2.Â Web Server (HTTP)

			The Falcot Corp administrators decided to use the Apache HTTP server, included in Debian Bullseye at version 2.4.52.
		

ALTERNATIVE Other web servers

			Apache is probably the most widely-known web server, but there are others. Its major opponent is nginx, which comes in different packages with different sets of features, like nginx-light, nginx-full, or nginx-extras. Another alternative is lighttpd. All of them can offer very good or even better performance under certain workloads or in certain situations, but not all provide the same number of available features and modules.
		

### 11.2.1.Â Installing Apache

				Installing the apache2 package is all that is needed. It contains all the modules, including the Multi-Processing Modules (MPMs) that affect how Apache handles parallel processing of many requests, which used to be provided in separate apache2-mpm-* packages. It will also pull apache2-utils containing the command line utilities that we will discover later.
			
				The MPM in use affects significantly the way Apache will handle concurrent requests. With the worker MPM, it uses threads (lightweight processes), whereas with the prefork MPM it uses a pool of processes created in advance. With the event MPM it also uses threads, but the inactive connections (notably those kept open by the HTTP keep-alive feature) are handed back to a dedicated management thread.
			
				The Falcot administrators also install libapache2-mod-php so as to include the PHP 7.4 support in Apache. This causes the default event MPM to be disabled, and prefork to be used instead. To use the event MPM one can use php-fpm.
			

SECURITY Execution under the www-data user

				By default, Apache handles incoming requests under the identity of the www-data user as defined in /etc/apache2/envvars. This means that a security vulnerability in a CGI script executed by Apache (for a dynamic page) won't compromise the whole system, but only the files owned by this particular user.
			
				Using the suexec modules, provided by apache2-suexec-* packages, allows bypassing this rule so that some CGI scripts are executed under the identity of another user. This is configured with a SuexecUserGroup usergroup directive in the Apache configuration.
			
				Another possibility is to use a dedicated MPM, such as the one provided by libapache2-mpm-itk. This particular one has a slightly different behavior: it allows âisolatingâ virtual hosts (actually, sets of pages) so that they each run as a different user. A vulnerability in one website therefore cannot compromise files belonging to the owner of another website.
			

QUICK LOOK List of modules

				The full list of Apache standard modules can be found online. âÂ [https://httpd.apache.org/docs/2.4/mod/](https://httpd.apache.org/docs/2.4/mod/)

				Apache is a modular server, and many features are implemented by external modules that the main program loads during its initialization. The default configuration only enables the most common modules, but enabling new modules is a simple matter of running a2enmod module; to disable a module, the command is a2dismod module. These programs actually only create (or delete) symbolic links in /etc/apache2/mods-enabled/, pointing at the actual files (stored in /etc/apache2/mods-available/).
			

IN PRACTICE Checking the configuration

				The mod_info module (a2enmod info) allows to access the comprehensive Apache server configuration and information via browser visiting http://localhost/server-info. Because it might contain sensitive information, access is only allowed from the local host by default. âÂ [https://httpd.apache.org/docs/2.4/mod/mod_info.html](https://httpd.apache.org/docs/2.4/mod/mod_info.html)

				With its default configuration, the web server listens on port 80 (as configured in /etc/apache2/ports.conf), and serves pages from the /var/www/html/ directory (as configured in /etc/apache2/sites-enabled/000-default.conf).
			

### 11.2.2.Â Adding support for SSL

				Apache 2.4 includes the SSL module (mod_ssl) required for secure HTTP (HTTPS) out of the box. It just needs to be enabled with a2enmod ssl, then the required directives have to be added to the configuration files. A configuration example is provided in /etc/apache2/sites-available/default-ssl.conf. âÂ [https://httpd.apache.org/docs/2.4/mod/mod_ssl.html](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html)

				If you want to generate trusted certificates, you can follow section [SectionÂ 10.2.1, âCreating gratis trusted certificatesâ](sect.x509-cert.en.md#sect.trusted-certs) and then adjust the following variables:
			

```bash
SSLCertificateFile      /etc/letsencrypt/live/DOMAIN/fullchain.pem
SSLCertificateKeyFile   /etc/letsencrypt/live/DOMAIN/privkey.pem
SSLCertificateChainFile /etc/letsencrypt/live/DOMAIN/chain.pem
SSLCACertificateFile    /etc/ssl/certs/ca-certificates.crt
```

				Some extra care must be taken if you want to favor SSL connections with Perfect Forward Secrecy (those connections use ephemeral session keys ensuring that a compromise of the server's secret key does not result in the compromise of old encrypted traffic that could have been stored while sniffing on the network). Have a look at Mozilla's recommendations in particular: âÂ [https://wiki.mozilla.org/Security/Server_Side_TLS#Apache](https://wiki.mozilla.org/Security/Server_Side_TLS#Apache)

				As an alternative to the standard SSL module, there is an extension module called mod_gnutls, which is shipped with the libapache2-mod-gnutls package and enabled with the a2enmod gnutls command. Unfortunately the version packaged for Debian had serious issues and even security implications and is therefor not part of the Debian Bullseye release. âÂ [https://mod.gnutls.org/](https://mod.gnutls.org/)

### 11.2.3.Â Configuring Virtual Hosts

				A virtual host is an extra identity for the web server.
			
				Apache considers two different kinds of virtual hosts: those that are based on the IP address (or the port), and those that rely on the domain name of the web server. The first method requires allocating a different IP address (or port) for each site, whereas the second one can work on a single IP address (and port), and the sites are differentiated by the hostname sent by the HTTP client (which only works in version 1.1 of the HTTP protocol â fortunately that version is old enough that all clients use it already).
			
				The (increasing) scarcity of IPv4 addresses usually favors the second method; however, it is made more complex if the virtual hosts need to provide HTTPS too, since the SSL protocol hasn't always provided for name-based virtual hosting; the SNI extension (Server Name Indication) that allows such a combination is not handled by all browsers. When several HTTPS sites need to run on the same server, they will usually be differentiated either by running on a different port or on a different IP address (IPv6 can help there).
			
				The default configuration for ApacheÂ 2 enables name-based virtual hosts. In addition, a default virtual host is defined in the /etc/apache2/sites-enabled/000-default.conf file; this virtual host will be used if no host matching the request sent by the client is found.
			

CAUTION First virtual host

				Requests concerning unknown virtual hosts will always be served by the first defined virtual host, which is why we defined www.falcot.com first here.
			

QUICK LOOK Apache supports SNI

				The Apache server supports an SSL protocol extension called Server Name Indication (SNI). This extension allows the browser to send the hostname of the web server during the establishment of the SSL connection, much earlier than the HTTP request itself, which was previously used to identify the requested virtual host among those hosted on the same server (with the same IP address and port). This allows Apache to select the most appropriate SSL certificate for the transaction to proceed.
			
				Before SNI, Apache would always use the certificate defined in the default virtual host. Clients trying to access another virtual host would then display warnings, since the certificate they received didn't match the website they were trying to access. Fortunately, most browsers now work with SNI; this includes Microsoft Internet Explorer starting with version 7.0 (starting on Vista), Mozilla Firefox starting with version 2.0, Apple Safari since version 3.2.1, and all versions of Google Chrome.
			
				The Apache package provided in Debian is built with support for SNI; no particular configuration is therefore needed.
			
				Each extra virtual host is then described by a file stored in /etc/apache2/sites-available/. Setting up a website for the falcot.org domain is therefore a simple matter of creating the following file, then enabling the virtual host with a2ensite www.falcot.org.
			

ExampleÂ 11.13.Â The /etc/apache2/sites-available/www.falcot.org.conf file

```bash
<VirtualHost *:80>
ServerName   www.falcot.org
ServerAlias  falcot.org
DocumentRoot /srv/www/www.falcot.org
</VirtualHost>
```

				The Apache server, as configured so far, uses the same log files for all virtual hosts (although this could be changed by adding CustomLog directives in the definitions of the virtual hosts). It therefore makes good sense to customize the format of this log file to have it include the name of the virtual host. This can be done by creating a /etc/apache2/conf-available/customlog.conf file that defines a new format for all log files (with the LogFormat directive) and by enabling it with a2enconf customlog. The CustomLog line must also be removed (or commented out) from the /etc/apache2/sites-available/000-default.conf file.
			

ExampleÂ 11.14.Â The /etc/apache2/conf-available/customlog.conf file

```bash
# New log format including (virtual) host name
LogFormat "%v %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" vhost

# Now let's use this "vhost" format by default
CustomLog /var/log/apache2/access.log vhost
```

### 11.2.4.Â Common Directives

				This section briefly reviews some of the commonly-used Apache configuration directives.
			
				The main configuration file usually includes several Directory blocks; they allow specifying different behaviors for the server depending on the location of the file being served. Such a block commonly includes Options and AllowOverride directives.
			

ExampleÂ 11.15.Â Directory block

```bash
<Directory /srv/www>
Options Includes FollowSymlinks
AllowOverride All
DirectoryIndex index.php index.html index.htm
</Directory>
```

				The DirectoryIndex directive contains a list of files to try when the client request matches a directory. The first existing file in the list is used and sent as a response.
			
				The Options directive is followed by a list of options to enable. The None value disables all options; correspondingly, All enables them all except MultiViews. Available options include:
			
ExecCGI indicates that CGI scripts can be executed.
					
FollowSymlinks tells the server that symbolic links can be followed, and that the response should contain the contents of the target of such links.
					
SymlinksIfOwnerMatch also tells the server to follow symbolic links, but only when the link and its target have the same owner.
					
Includes enables Server Side Includes (SSI for short). These are directives embedded in HTML pages and executed on the fly for each request.
					
IncludesNOEXEC allows Server Side Includes (SSI) but disables the exec command and limits the include directive to text/markup files.
					
Indexes tells the server to list the contents of a directory if the HTTP request sent by the client points at a directory without an index file (i.e., when no files mentioned by the DirectoryIndex directive exists in this directory).
					
MultiViews enables content negotiation; this can be used by the server to return a web page matching the preferred language as configured in the browser.
					

BACK TO BASICS .htaccess file

				The .htaccess file contains Apache configuration directives enforced each time a request concerns an element of the directory where it is stored. The scope of these directives also recurses to all the subdirectories within.
			
				Most of the directives that can occur in a Directory block are also legal in a .htaccess file.
			
				The AllowOverride directive lists all the options that can be enabled or disabled by way of a .htaccess file. A common use of this option is to restrict ExecCGI, so that the administrator chooses which users are allowed to run programs under the web server's identity (the www-data user).
			

#### 11.2.4.1.Â Requiring Authentication

					In some circumstances, access to part of a website needs to be restricted, so only legitimate users who provide a username and a password are granted access to the contents. These feature are provided by the mod_auth* modules.
				

ExampleÂ 11.16.Â .htaccess file requiring authentication

```bash
Require valid-user
AuthName "Private directory"
AuthType Basic
AuthUserFile /etc/apache2/authfiles/htpasswd-private
```

SECURITY No security

					The authentication system used in the above example (Basic) has minimal security as the password is sent in clear text (it is only encoded as base64, which is a simple encoding rather than an encryption method). It should also be noted that the documents âprotectedâ by this mechanism also go over the network in the clear. If security is important, the whole HTTP connection should be encrypted with SSL.
				
					The /etc/apache2/authfiles/htpasswd-private file contains a list of users and passwords; it is commonly manipulated with the htpasswd command. For example, the following command is used to add a user or change their password:
				

```bash
# htpasswd /etc/apache2/authfiles/htpasswd-private user
New password:
Re-type new password:
Adding password for user user
```

#### 11.2.4.2.Â Restricting Access

					The Require directive controls access restrictions for a directory (and its subdirectories, recursively). âÂ [https://httpd.apache.org/docs/2.4/howto/access.html](https://httpd.apache.org/docs/2.4/howto/access.html)

					It can be used to restrict access based on many criteria; we will stop at describing access restriction based on the IP address of the client, but it can be made much more powerful than that, especially when several Require directives are combined within a RequireAll block.
				

ExampleÂ 11.17.Â Only allow from the local network

```bash
Require ip 192.168.0.0/16
```

ALTERNATIVE Old syntax

					The Require syntax is only available in ApacheÂ 2.4 (the version shipped since Jessie). For users of Wheezy, the ApacheÂ 2.2 syntax is different, and we describe it here mainly for reference, although it can also be made available in ApacheÂ 2.4 using the mod_access_compat module.
				
					The Allow from and Deny from directives control access restrictions for a directory (and its subdirectories, recursively).
				
					The Order directive tells the server of the order in which the Allow from and Deny from directives are applied; the last one that matches takes precedence. In concrete terms, Order deny,allow allows access if no Deny from applies, or if an Allow from directive does. Conversely, Order allow,deny rejects access if no Allow from directive matches (or if a Deny from directive applies).
				
					The Allow from and Deny from directives can be followed by an IP address, a network (such as 192.168.0.0/255.255.255.0, 192.168.0.0/24 or even 192.168.0), a hostname or a domain name, or the all keyword, designating everyone.
				
					For instance, to reject connections by default but allow them from the local network, you could use this:
				

```bash
Order deny,allow
Allow from 192.168.0.0/16
Deny from all
```

### 11.2.5.Â Log Analyzers

				A log analyzer is frequently installed on a web server; since the former provides the administrators with a precise idea of the usage patterns of the latter.
			
				The Falcot Corp administrators selected AWStats (Advanced Web Statistics) to analyze their Apache log files.
			
				The first configuration step is the customization of the /etc/awstats/awstats.conf file. The Falcot administrators keep it unchanged apart from the following parameters:
			

```bash
LogFile="/var/log/apache2/access.log"
LogFormat = "%virtualname %host %other %logname %time1 %methodurl %code %bytesd %refererquot %uaquot"
SiteDomain="www.falcot.com"
HostAliases="falcot.com REGEX[^.*\.falcot\.com$]"
DNSLookup=1
LoadPlugin="tooltips"
```

				All these parameters are documented by comments in the template file. In particular, the LogFile and LogFormat parameters describe the location and format of the log file and the information it contains; SiteDomain and HostAliases list the various names under which the main web site is known.
			
				For high traffic sites, DNSLookup should usually not be set to 1; for smaller sites, such as the Falcot one described above, this setting allows getting more readable reports that include full machine names instead of raw IP addresses.
			

SECURITY Access to statistics

				AWStats makes its statistics available on the website with no restrictions by default, but restrictions can be set up so that only a few (probably internal) IP addresses can access them; the list of allowed IP addresses needs to be defined in the AllowAccessFromWebToFollowingIPAddresses parameter
			
				AWStats will also be enabled for other virtual hosts; each virtual host needs its own configuration file, such as /etc/awstats/awstats.www.falcot.org.conf.
			

ExampleÂ 11.18.Â AWStats configuration file for a virtual host

```bash
Include "/etc/awstats/awstats.conf"
SiteDomain="www.falcot.org"
HostAliases="falcot.org"
```

				AWStats uses many icons stored in the /usr/share/awstats/icon/ directory. In order for these icons to be available on the web site, the Apache configuration needs to be adapted to include the following directive (check out /usr/share/doc/awstats/examples/apache.conf for a more detailed example):
			

```bash
Alias /awstats-icon/ /usr/share/awstats/icon/
```

				After a few minutes (and once the script has been run a few times), the results are available online: âÂ [http://www.falcot.com/cgi-bin/awstats.pl](http://www.falcot.com/cgi-bin/awstats.pl)âÂ [http://www.falcot.org/cgi-bin/awstats.pl](http://www.falcot.org/cgi-bin/awstats.pl)

CAUTION Log file rotation

				In order for the statistics to take all the logs into account, AWStats needs to be run right before the Apache log files are rotated. Looking at the prerotate directive of /etc/logrotate.d/apache2 file, this is solved by the awstats package by putting a scipt in /etc/logrotate.d/httpd-prerotate running /usr/share/awstats/tools/update.sh.
			
				Note also that the log files created by logrotate need to be readable by everyone, especially AWStats. This is ensured by changing /etc/logrotate.d/apache2 to include create 644 root adm (instead of the default 640 permissions) for example. But please refer to /usr/share/doc/awstats/README.Debian.gz for a more extensive documentation about this topic and alternatives.
			[PrevChapterÂ 11.Â Network Services: Postfix, Apache, NF...](network-services.en.md)[Up](#)[Home](index.en.md)[Next11.3.Â FTP File Server](sect.ftp-file-server.en.md)