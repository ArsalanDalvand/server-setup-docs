---
title: "11.7.3. Managing Accounts with LDAP"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.ldap-directory.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.ldap-directory.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.7.3. Managing Accounts with LDAP"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.http-ftp-proxy.en.md)The Debian Administrator's Handbook[Next](sect.rtc-services.en.md)

## 11.7.Â LDAP Directory

			OpenLDAP is an implementation of the LDAP protocol; in other words, it is a special-purpose database designed for storing directories. In the most common use case, using an LDAP server allows centralizing management of user accounts and the related permissions. Moreover, an LDAP database is easily replicated, which allows setting up multiple synchronized LDAP servers. When the network and the user base grows quickly, the load can then be balanced across several servers.
		
			LDAP data is structured and hierarchical. The structure is defined by âschemasâ which describe the kind of objects that the database can store, with a list of all their possible attributes. The syntax used to refer to a particular object in the database is based on this structure, which explains its complexity.
		

### 11.7.1.Â Installing

				The slapd package contains the OpenLDAP server. The ldap-utils package includes command-line tools for interacting with LDAP servers.
			
				Installing slapd usually asks only for the administrator's password and the resulting database is unlikely to suit your needs. Fortunately a simple dpkg-reconfigure slapd will let you reconfigure the LDAP database with more details:
			
						Omit OpenLDAP server configuration? No, of course, we want to configure this service.
					
						DNS domain name: âfalcot.comâ.
					
						Organization name: âFalcot Corpâ.
					
						An administrative passwords needs to be typed in.
					
						Database backend to use: âMDBâ.
					
						Do you want the database to be removed when slapd is purged? No. No point in risking losing the database in case of a mistake.
					
						Move old database? This question is only asked when the configuration is attempted while a database already exists. Only answer âyesâ if you actually want to start again from a clean database, for instance if you run dpkg-reconfigure slapd right after the initial installation.
					

BACK TO BASICS LDIF format

				An LDIF file (LDAP Data Interchange Format) is a portable text file describing the contents of an LDAP database (or a portion thereof); this can then be used to inject the data into any other LDAP server.
			
				A minimal database is now configured, as demonstrated by the following query:
			

```bash
$ ldapsearch -x -b dc=falcot,dc=com
# extended LDIF
#
# LDAPv3
# base <dc=falcot,dc=com> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# falcot.com
dn: dc=falcot,dc=com
objectClass: top
objectClass: dcObject
objectClass: organization
o: Falcot Corp
dc: falcot

# admin, falcot.com
dn: cn=admin,dc=falcot,dc=com
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

				The query returned two objects: the organization itself, and the administrative user.
			

### 11.7.2.Â Filling in the Directory

				Since an empty database is not particularly useful, we are going to inject into it all the existing directories; this includes the users, groups, services and hosts databases.
			
				The migrationtools package provides a set of scripts dedicated to extract data from the standard Unix directories (/etc/passwd, /etc/group, /etc/services, /etc/hosts and so on), convert this data, and inject it into the LDAP database.
			
				Once the package is installed, the /etc/migrationtools/migrate_common.ph must be edited; the IGNORE_UID_BELOW and IGNORE_GID_BELOW options need to be enabled (uncommenting them is enough), and DEFAULT_MAIL_DOMAIN/DEFAULT_BASE need to be updated.
			
				The actual migration operation is handled by the migrate_all_online.sh command, as follows:
			

```bash
# cd /usr/share/migrationtools
# PERL5LIB="${PERL5LIB}:/etc/migrationtools" LDAPADD="/usr/bin/ldapadd -c" ETC_ALIASES=/dev/null ./migrate_all_online.sh
```

				The migrate_all_online.sh asks a few questions about the LDAP database into which the data is to be migrated. [TableÂ 11.1](sect.ldap-directory.en.md#tab-migrate-all) summarizes the answers given in the Falcot use-case.
			

TableÂ 11.1.Â Answers to questions asked by the migrate_all_online.sh script

QuestionAnswerX.500 naming contextdc=falcot,dc=comLDAP server hostnamelocalhostManager DNcn=admin,dc=falcot,dc=comBind credentialsthe administrative passwordCreate DUAConfigProfileno
				You might notice that we extend the PERL5LIB variable. This is due to Debian bug report #982666. âÂ [https://bugs.debian.org/982666](https://bugs.debian.org/982666)

				As you might have also noticed, we deliberately ignore migration of the /etc/aliases file, since the standard schema as provided by Debian does not include the structures that this script uses to describe email aliases. Should we want to integrate this data into the directory, the /etc/ldap/schema/misc.schema file should be added to the standard schema.
			

TOOL Browsing an LDAP directory

				The jxplorer command (in the package of the same name) is a graphical tool allowing to browse and edit an LDAP database. It is an interesting tool that provides an administrator with a good overview of the hierarchical structure of the LDAP data.
			
				Also note the use of the -c option to the ldapadd command; this option requests that processing doesn't stop in case of error. Using this option is required because converting the /etc/services often generates a few errors that can safely be ignored.
			

### 11.7.3.Â Managing Accounts with LDAP

				Now the LDAP database contains some useful information, the time has come to make use of this data. This section focuses on how to configure a Linux system so that the various system directories use the LDAP database.
			

#### 11.7.3.1.Â Configuring NSS

					The NSS system (Name Service Switch, see sidebar [GOING FURTHER NSS and system databases](sect.user-group-databases.en.md#sidebar.intro-nss)) is a modular system designed to define or fetch information for system directories. Using LDAP as a source of data for NSS requires installing the libnss-ldap package. Its installation asks a few questions; the answers are summarized in [TableÂ 11.2](sect.ldap-directory.en.md#tab-libnss-ldap).
				

TableÂ 11.2.Â Configuring the libnss-ldap package:

QuestionAnswerLDAP server URI (Uniform Resource Identifier) ldapi://ldap.falcot.com Distinguished name of the search base dc=falcot,dc=com LDAP version to use 3 LDAP account for root cn=admin,dc=falcot,dc=com LDAP root account passwordthe administrative passwordAllow LDAP admin account behave like local root? yes Does the LDAP database require login?no
					The /etc/nsswitch.conf file then needs to be modified, so as to configure NSS to use the freshly-installed ldap module. You can use the example provided in /usr/share/doc/libnss-ldap/examples/nsswitch.ldap or edit your existing configuration.
				

ExampleÂ 11.23.Â The /etc/nsswitch.conf file

```bash
#ident $Id: nsswitch.ldap,v 2.4 2003/10/02 02:36:25 lukeh Exp $
#
# An example file that could be copied over to /etc/nsswitch.conf; it
# uses LDAP conjunction with files.
#
# "hosts:" and "services:" in this file are used only if the
# /etc/netconfig file has a "-" for nametoaddr_libs of "inet" transports.

# the following lines obviate the "+" entry in /etc/passwd and /etc/group.
passwd:         files ldap
shadow:         files ldap
group:          files ldap

# consult DNS first, we will need it to resolve the LDAP host. (If we
# can't resolve it, we're in infinite recursion, because libldap calls
# gethostbyname(). Careful!)
hosts:          dns ldap

# LDAP is nominally authoritative for the following maps.
services:   ldap [NOTFOUND=return] files
networks:   ldap [NOTFOUND=return] files
protocols:  ldap [NOTFOUND=return] files
rpc:        ldap [NOTFOUND=return] files
ethers:     ldap [NOTFOUND=return] files

# no support for netmasks, bootparams, publickey yet.
netmasks:   files
bootparams: files
publickey:  files
automount:  files

# I'm pretty sure nsswitch.conf is consulted directly by sendmail,
# here, so we can't do much here. Instead, use bbense's LDAP
# rules ofr sendmail.
aliases:    files
sendmailvars:   files

# Note: there is no support for netgroups on Solaris (yet)
netgroup:   ldap [NOTFOUND=return] files
```

					The ldap module is usually inserted before others, and it will therefore be queried first. The notable exception is the hosts service since contacting the LDAP server requires consulting DNS first (to resolve ldap.falcot.com). Without this exception, a hostname query would try to ask the LDAP server; this would trigger a name resolution for the LDAP server, and so on in an infinite loop.
				
					If the LDAP server should be considered authoritative (and the local files used by the files module disregarded), services can be configured with the following syntax:
				
service: ldap [NOTFOUND=return] files.
				
					If the requested entry does not exist in the LDAP database, the query will return a ânot existingâ reply even if the resource does exist in one of the local files; these local files will only be used when the LDAP service is down.
				

#### 11.7.3.2.Â Configuring PAM

					This section describes a PAM configuration (see sidebar [BEHIND THE SCENES /etc/environment and /etc/default/locale](basic-configuration.en.md#sidebar.intro-pam)) that will allow applications to perform the required authentications against the LDAP database.
				

CAUTION Broken authentication

					Changing the standard PAM configuration used by various programs is a sensitive operation. A mistake can lead to broken authentication, which could prevent logging in. Keeping a root shell open is therefore a good precaution. If configuration errors occur, they can be then fixed and the services restarted with minimal effort.
				
					The LDAP module for PAM is provided by the libpam-ldap package. Installing this package asks a few questions very similar to those in libnss-ldap; some configuration parameters (such as the URI for the LDAP server) are even actually shared with the libnss-ldap package. Answers are summarized in [TableÂ 11.3](sect.ldap-directory.en.md#tab-libpam-ldap).
				

TableÂ 11.3.Â Configuration of libpam-ldap

QuestionAnswerAllow LDAP admin account to behave like local root?Yes. This allows using the usual passwd command for changing passwords stored in the LDAP database.Does the LDAP database require logging in?noLDAP account for root: cn=admin,dc=falcot,dc=com LDAP administrative password:the LDAP database administrative passwordLocal encryption algorithm to use for passwords: crypt PAM profiles to enable:LDAP Authentication is among the enabled profiles
					Installing libpam-ldap automatically adapts the default PAM configuration defined in the /etc/pam.d/common-auth, /etc/pam.d/common-password and /etc/pam.d/common-account files. This mechanism uses the dedicated pam-auth-update tool (provided by the libpam-runtime package). This tool can also be run by the administrator should they wish to enable or disable PAM modules.
				

#### 11.7.3.3.Â Securing LDAP Data Exchanges

					By default, the LDAP protocol transits on the network as cleartext; this includes the (encrypted) passwords. Since the encrypted passwords can be extracted from the network, they can be vulnerable to dictionary-type attacks. This can be avoided by using an extra encryption layer; enabling this layer is the topic of this section.
				

##### 11.7.3.3.1.Â Configuring the Server

						The first step is to create a key pair (comprising a public key and a private key) for the LDAP server. The Falcot administrators reuse easy-rsa to generate it (see [SectionÂ 10.2.2, âPublic Key Infrastructure: easy-rsaâ](sect.x509-cert.en.md#sect.easy-rsa)). Running ./easyrsa build-server-full ldap.falcot.com nopass will ask you about the âcommon nameâ. The answer to that question must be the fully-qualified hostname for the LDAP server; in our case, ldap.falcot.com.
					
						This command creates a certificate in the pki/issued/ldap.falcot.com.crt file; the corresponding private key is stored in pki/private/ldap.falcot.com.key.
					
						Now these keys have to be installed in their standard location, and we must make sure that the private file is readable by the LDAP server which runs under the openldap user identity:
					

```bash
# adduser openldap ssl-cert
Adding user `openldap' to group `ssl-cert' ...
Adding user openldap to group ssl-cert
Done.
# mv pki/private/ldap.falcot.com.key /etc/ssl/private/ldap.falcot.com.key
# chown root.ssl-cert /etc/ssl/private/ldap.falcot.com.key
# chmod 0640 /etc/ssl/private/ldap.falcot.com.key
# mv pki/issued/ldap.falcot.com.crt /etc/ssl/certs/ldap.falcot.com.pem
# chown root.root /etc/ssl/certs/ldap.falcot.com.pem
# chmod 0644 /etc/ssl/certs/ldap.falcot.com.pem
```

						The slapd daemon also needs to be told to use these keys for encryption. The LDAP server configuration is managed dynamically: the configuration can be updated with normal LDAP operations on the cn=config object hierarchy, and the server updates /etc/ldap/slapd.d in real time to make the configuration persistent. ldapmodify is thus the right tool to update the configuration:
					

ExampleÂ 11.24.Â Configuring slapd for encryption

```bash
# cat >ssl.ldif <<END
dn: cn=config
changetype: modify
add: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/ssl/private/ldap.falcot.com.key
-
add: olcTLSCertificateFile
olcTLSCertificateFile: /etc/ssl/certs/ldap.falcot.com.pem
END
# ldapmodify -Y EXTERNAL -H ldapi:/// -f ssl.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "cn=config"
# systemctl restart slapd.service
# ldapsearch -Y EXTERNAL -H ldapi:/// -b cn=config -s base | grep TLS
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
olcTLSCertificateFile: /etc/ssl/certs/ldap.falcot.com.pem
olcTLSCertificateKeyFile: /etc/ssl/certs/ldap.falcot.com.key
```

TOOL ldapvi to edit an LDAP directory

						With ldapvi, you can display an LDIF output of any part of the LDAP directory, make some changes in the text editor, and let the tool do the corresponding LDAP operations for you.
					
						It is thus a convenient way to update the configuration of the LDAP server, simply by editing the cn=config hierarchy.
					

```bash
# ldapvi -Y EXTERNAL -h ldapi:/// -b cn=config
```

						The last step for enabling encryption involves changing the SLAPD_SERVICES variable in the /etc/default/slapd file. We'll play it safe and disable unsecured LDAP altogether.
					

ExampleÂ 11.25.Â The /etc/default/slapd file

```bash
# Default location of the slapd.conf file or slapd.d cn=config directory. If
# empty, use the compiled-in default (/etc/ldap/slapd.d with a fallback to
# /etc/ldap/slapd.conf).
SLAPD_CONF=

# System account to run the slapd server under. If empty the server
# will run as root.
SLAPD_USER="openldap"

# System group to run the slapd server under. If empty the server will
# run in the primary group of its user.
SLAPD_GROUP="openldap"

# Path to the pid file of the slapd server. If not set the init.d script
# will try to figure it out from $SLAPD_CONF (/etc/ldap/slapd.d by
# default)
SLAPD_PIDFILE=

# slapd normally serves ldap only on all TCP-ports 389. slapd can also
# service requests on TCP-port 636 (ldaps) and requests via unix
# sockets.
# Example usage:
# SLAPD_SERVICES="ldap://127.0.0.1:389/ ldaps:/// ldapi:///"
SLAPD_SERVICES="ldaps:/// ldapi:///"

# If SLAPD_NO_START is set, the init script will not start or restart
# slapd (but stop will still work).  Uncomment this if you are
# starting slapd via some other means or if you don't want slapd normally
# started at boot.
#SLAPD_NO_START=1

# If SLAPD_SENTINEL_FILE is set to path to a file and that file exists,
# the init script will not start or restart slapd (but stop will still
# work).  Use this for temporarily disabling startup of slapd (when doing
# maintenance, for example, or through a configuration management system)
# when you don't want to edit a configuration file.
SLAPD_SENTINEL_FILE=/etc/ldap/noslapd

# For Kerberos authentication (via SASL), slapd by default uses the system
# keytab file (/etc/krb5.keytab).  To use a different keytab file,
# uncomment this line and change the path.
#export KRB5_KTNAME=/etc/krb5.keytab

# Additional options to pass to slapd
SLAPD_OPTIONS=""
```

##### 11.7.3.3.2.Â Configuring the Client

						On the client side, the configuration for the libpam-ldap and libnss-ldap modules needs to be modified to use an ldaps:// URI.
					
						LDAP clients also need to be able to authenticate the server. In an X.509 public key infrastructure, public certificates are signed by the key of a certificate authority (CA). With easy-rsa, the Falcot administrators have created their own CA and they now need to configure the system to trust the signatures of Falcot's CA. This can be done by putting the CA certificate in /usr/local/share/ca-certificates and running update-ca-certificates.
					

```bash
# cp pki/ca.crt /usr/local/share/ca-certificates/falcot.crt
# update-ca-certificates
Updating certificates in /etc/ssl/certs...
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...

Adding debian:falcot.pem
done.
done.
```

						Last but not least, the default LDAP URI and default base DN used by the various command line tools can be modified in /etc/ldap/ldap.conf. This will save quite some typing.
					

ExampleÂ 11.26.Â The /etc/ldap/ldap.conf file

```bash
#
# LDAP Defaults
#

# See ldap.conf(5) for details
# This file should be world readable but not world writable.

#BASE   dc=example,dc=com
#URI    ldap://ldap.example.com ldap://ldap-provider.example.com:666

#SIZELIMIT      12
#TIMELIMIT      15
#DEREF          never

# TLS certificates (needed for GnuTLS)
TLS_CACERT      /etc/ssl/certs/ca-certificates.crt
```

[Prev11.6.Â HTTP/FTP Proxy](sect.http-ftp-proxy.en.md)[Up](#)[Home](index.en.md)[Next11.8.Â Real-Time Communication Services](sect.rtc-services.en.md)