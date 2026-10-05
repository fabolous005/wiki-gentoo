<!-- source: https://wiki.gentoo.org/wiki/Icinga | group: Gentoo Wiki (Main) | wiki-title: Icinga -->
---
title: Icinga
url: https://wiki.gentoo.org/wiki/Icinga
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: aa08fe7a57eda38f
license: CC BY-SA 4.0
---

# Icinga

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

Icinga is a fork of [Nagios](https://wiki.gentoo.org/wiki/Nagios) and is an extremely capable monitoring system.  Installation of the basic system is pretty easy but there are lots of really useful add ons.  This howto provides a copy n pastable install for many of those addons and integration.

Add hosts and service checks into Icinga itself is not covered but at the end of this install you will have pretty much all the components you could need. Fight with the usage rather than the install.

To make the most of this howto, ensure that the "you should be able to ..." steps actually work before progressing to the next bit. This system is huge and it is easy to get lost.

## Prerequisites

This Howto assumes the following are up and running. Here I use a single vhost configuration file which is made up from all the various module definitions plus a few extras. If you prefer you should be able to follow the modular approach which is the proper Gentoo way.

- Install Apache including a method of authentication, and ideally SSL encryption
  - Disable the default vhosts in /etc/conf.d/apache2
  - Create a separate vhost in /etc/apache2/vhosts.d/ (see below for a sample config file)
- Install MySQL, with phpmyadmin (always handy) and a root password configured
- Consider using my package.use (see below) to get workable USE flags

## Installation and configuration

- Install [net-analyzer/icinga](https://packages.gentoo.org/packages/net-analyzer/icinga) 1.10.2

- Add some extras:
  - [net-analyzer/nagios-check\_ipmi\_sensor](https://packages.gentoo.org/packages/net-analyzer/nagios-check_ipmi_sensor)
  - [net-analyzer/nagios-check\_mysql\_health](https://packages.gentoo.org/packages/net-analyzer/nagios-check_mysql_health)
  - [sys-power/apcupsd](https://packages.gentoo.org/packages/sys-power/apcupsd) nls -cgi -gnome -snmp -usb (this failed to compile for me unless I disabled some USE flags)
  - [net-analyzer/nagios-plugins-snmp](https://packages.gentoo.org/packages/net-analyzer/nagios-plugins-snmp)

- Add the apache user to the icinga group and restart Apache:

`root #````
usermod -G icinga apache
```
`root #````
/etc/init.d/apache2 restart
```
- Amend the CGI configuration as required. Replace all occurrences of "icingaadmin" in the permission settings with a valid user that the web server authenticates or \* which means any authenticated user.

**`/etc/icinga/cgi.cfg`**

**Icinga CGI settings changed from defaults**

```
lowercase_user_name=1
first_day_of_week=1
date_format=euro
```
- Start the daemon

`root #``/etc/init.d/icinga start`
- At this point Icinga should be up and running and the web interface should be usable at [https://monitor.example.co.uk/icinga](https://monitor.example.co.uk/icinga). The localhost should be monitored and probably showing an error for the http service (its switched off - we are only using https here).

## IDOUtils

- Setup the IDO related config

`root #````
cd /etc/icinga
```
`root #````
cp ido2db.cfg-sample ido2db.cfg
```
`root #````
cp idomod.cfg-sample idomod.cfg
```
`root #````
chown icinga:icinga ido*
```
`root #````
cd modules
```
`root #````
cp idoutils.cfg-sample idoutils.cfg
```
`root #````
chown icinga:icinga ido*
```
- Create the DB. The root password is echoed to the screen so watch out for shoulder surfers. A log file is spat out at the end.

`root #````
cd /usr/share/icinga/contrib/db/scripts
```
`root #````
sh create_mysqldb.sh
```
- Start daemons (The Icinga service will startup ico2db itself)

`root #``/etc/init.d/icinga restart`
- Check /var/log/icinga.log that IDO is happy
- Add to default run level

`root #````
rc-update add ido2db
```
`root #````
rc-update add icinga
```
## Icinga-web

- [net-analyzer/icinga-web](https://packages.gentoo.org/packages/net-analyzer/icinga-web) 1.9.0 USE="apache2 mysql pnp -postgres"
- rrdtool may need [media-fonts/dejavu](https://packages.gentoo.org/packages/media-fonts/dejavu) installing

- Create Database

`root #````
echo "CREATE DATABASE icinga_web;" | mysql -p 
```
`root #````
echo "GRANT SELECT,UPDATE,INSERT,DELETE ON icinga_web.* TO 'icinga_web'@'localhost' IDENTIFIED BY 'icinga_web';" | mysql -p
```
`root #````
echo "FLUSH PRIVILEGES;" | mysql -p
```
`root #````
cat /usr/share/icinga/icinga-web/contrib/mysql.sql | mysql icinga_web -p
```
- Change /etc/apache2/modules.d/00\_default\_settings.conf and set servertokens to Minimal and restart Apache

- Here we are using the web server to authenticate, so add this section to auth.xml. I put it after the default section. It seems to work fine but more research needed to find out exactly how it works.

**`/etc/icinga-web/auth.xml`**

**Icinga-web auth config - Use web server auth**

```
<setting name="provider">
    <ae:parameter name="http-basic-authentication">
         <ae:parameter name="auth_create">true</ae:parameter>
         <ae:parameter name="auth_update">true</ae:parameter>
    </ae:parameter>        
</setting>
```
- At this point you should be able to point your browser at: [https://monitor.example.co.uk/icinga-web](https://monitor.example.co.uk/icinga-web), connect and see localhost being monitored.  If there is no data in Icinga-Web then ensure that the ido2db service is running

### Admin users

As we are using web server auth, then the root user becomes unavailable. Edit the database directly to enable another admin user:

- DB: icinga\_web, Table: nsm\_user. Find the user\_id corresponding to the user to "upgrade"
- In the nsm\_user\_role table, add two additional entries for that user\_id with role\_id = 2 and 3
- Log the user out and in again and the admin menu should appear

### PNP4Nagios

The pnp USE flag for icinga-web pulls in PNP4nagios Bulk Mode with npcdmod - this means minimal extra configuration within Icinga. A module is loaded and that grads the process data and passes it to a daemon called npcd. npcd then proceesses that data and puts it into the rrds for the web interface

- Create rra config, no need to edit this, defaults are fine

`root #````
cd /etc/pnp/
```
`root #````
cp rra.cfg-sample rra.cfg
```
- Edit npcd config

**`/etc/pnp/npcd.cfg`**

**NPCD config changes**

```
# Changed from default (/var/lib/perfdata.dump) - JG 13 Aug 2013
perfdata_file = /var/spool/npcdmod/prefdata.dump
```
- Create a directory for npcdmod which runs from within Icinga to dump its data file into, npcd then gets data from there and processes it

`root #````
mkdir /var/spool/npcdmod/
```
`root #````
chown icinga:icinga /var/spool/npcdmod/
```
- Site local changes are made in config\_local.php, leave all the other config files alone. On upgrades diff against them for any new parameters and add to the local config as needed. The ebuild correctly sets icinga perfdata but does not set nagios\_base correctly

**`/etc/pnp/config_local.php`**

**PNP config file changes**

```
# Default was for Nagios JG 13 Aug 2013
$conf['nagios_base'] = "/icinga/cgi-bin";
```
- Create the PNP module configuration

**`/etc/icinga/modules/pnp.cfg`**

**PNP broker module configuration**

- Amend icinga config to process prefdata

**`/etc/icinga/icinga.cfg`**

**Icinga config file changes**

```
process_performance_data=1
```
- Setup the npcd service

`root #````
rc-update add npcd default
```
`root #````
/etc/init.d/npcd start
```
- Restart the icinga service to enable the module and start gathering data

`root #``/etc/init.d/icinga restart`
- Wait for a while for the graphs to be generated and start filling with data - this will take at least five minutes.  You should see that npcd is mentioned in /var/log/icinga/icinga.log.  Look for "npcdmod: Ready to run to have some fun!"
  - Icinga-web: Expand the heading on a host or service (little blue square icon with an arrow) there should be a PNP section with links to the graphs.
  - Icinga classic does not have built in support for PNP as such but the extra actions can be configured in a template for this which is not covered here.

### Debugging PNP

Debugging pnp can be hard. A script is available to help:

`root #````
cd /usr/src 
```
`root #````
perl verify_pnp_config -c /etc/icinga/icinga.cfg -m npcdmod -p /etc/pnp/
```
I think that the script requires you to explicitly state the module using the broker\_module parameter within the icinga.cfg, so you may have to convert back to that temporarily to run this test.

## mk-livestatus

The Nagvis developers have deprecated using the ndomy driver and only recommend using mk-livestatus

- Create this module config file

**`/etc/icinga/modules/mk-livestatus.cfg`**

**mk-livestatus module config for icinga**

- Restart Icinga

`root #``/etc/init.d/icinga restart`
- Check /var/log/icinga/icinga.log for this: "livestatus: Finished initialization. Further log messages go to /var/log/icinga/livestatus.log"

## Nagvis

Nagviz in Portage lags upstream quite badly - its the only package used here that does this. So I use a local ebuild. See below for how to create this. If Portage catches up then ignore that step and install directly.

- Create config from sample

`root #``cp /etc/nagvis/nagvis.ini.php-sample /etc/nagvis/nagvis.ini.php`
- Set some permissions

`root #````
chown apache /var/nagvis
```
`root #````
mkdir /etc/nagvis/profiles
```
`root #````
chown apache /etc/nagvis/profiles
```
- Edit config

**`/etc/nagvis/nagvis.ini.php`**

**Snippets from Nagvis config**

```
; This will make new users into admins by default.  Login with each user you want to be a manager and then revert it to Guests
; After that a manager can maintain the permissions from within the application
; See http://docs.nagvis.org/1.7/en_US/index.html
logonmodule="LogonMixed"
logonenvvar="REMOTE_USER"
logonenvcreateuser="1"
logonenvcreaterole=""Administrators"
; Disable the demo maps
;maps="demo-germany,demo-ham-racks,demo-load,demo-muc-srv1,demo-geomap,demo-automap"
[paths]
base="/usr/share/nagvis/"
htmlbase="/nagvis"
htmlcgi="/icinga/cgi-bin"
[defaults]
backend="live_1"
[index]
showrotations=0
[backend_live_1]
backendtype="mklivestatus"
socket="unix:/var/lib/icinga/rw/live"
```
- Remove demo data (put them somewhere for reference)

`root #``mv /etc/nagvis/maps/*.cfg /tmp/`
- Create a simple map - as you add systems to Icinga, this map will grow with it.

**`/etc/nagvis/maps`**

**Example map**

- Set permissions

`root #``chown -r apache:apache /etc/nagvis/maps`
- Test it - [https://monitor.example.co.uk/nagvis](https://monitor.example.co.uk/nagvis).  You should go straight though using web server authentication and become an admin.  To reset the permissions, you can delete /etc/nagvis/auth.db - it will be recreated from scratch for you. Change logonenvcreaterole in nagvis.ini.cfg to change how future users will be created.

- Add a cronk to display Nagviz in icinga-web

**`/etc/icinga-web/cronks.xml`**

**Icinga-web cronk snippet for Nagvis in an iFrame**

```
<cronk name="iFrameViewNagvis">
   <ae:parameter name="module">Cronks</ae:parameter>
   <ae:parameter name="action">System.IframeView</ae:parameter>
   <ae:parameter name="hide">false</ae:parameter>
   <ae:parameter name="description">Nagvis Maps views</ae:parameter>
   <ae:parameter name="name">Nagvis Display</ae:parameter>
   <ae:parameter name="image">cronks.Info2</ae:parameter>
   <ae:parameter name="categories">data</ae:parameter>
   <ae:parameter name="position">310</ae:parameter>
   <ae:parameter name="ae:parameter">
       <ae:parameter name="url"><![CDATA[/nagvis/frontend/nagvis-js/]]></ae:parameter>
       <ae:parameter name="user">admin</ae:parameter>
       <ae:parameter name="password">admin</ae:parameter>
   </ae:parameter>
</cronk>
```
- Clear the cache to enable the new cronk

`root #``/usr/share/icinga/icinga-web/bin/clearcache.sh`
## Icinga Mobile

 **Work in progress** 

- Install a JVM first e.g. [dev-java/oracle-jdk-bin](https://packages.gentoo.org/packages/dev-java/oracle-jdk-bin)
- Homepage: [https://www.icinga.org/about/icinga-mobile/](https://www.icinga.org/about/icinga-mobile/)

`root #````
cd icinga-mobile
```
`root #````
autoconf
```
- Generate an API key in Icinga web
- Edit lib/Model/IcingaConfiguration.js and set the apikey
- Install the web app `root #``./configure --with-web-user=apache --with-web-group=apache --with-web-apache-path=/etc/apache2/modules.d --prefix=/usr/local/icinga-mobile``root #``make``root #``make install``root #``make install-apache-config``root #``/usr/bin/install -c -b -m 664 -o apache -g apache etc/apache/icinga-mobile.conf /etc/apache2/modules.d/icinga-mobile.conf`

- Point your browser at [https://monitor.example.co.uk/m](https://monitor.example.co.uk/m)

## NagiosQL

- Download and untar source, patch with SP2.
- Move to /var/www/localhost/htdocs
- Symlink to ./nagiosql
- Set `date.timezone` in php.ini

**`/etc/php/apache2/php.ini`**

**Snippet from php.ini**

```
[Date]
date.timezone = Europe/London
```
- Create /etc/icinga/nagiosql and chown icinga:icinga and chmod g+s
- Temporary: `chown apache:apache /var/www/localhost/htdocs/nagiosql/config`
- Run the setup wizard: https:// /nagiosql
- Set NagiosQL path values:
  - Config: /etc/icinga/nagiosql and /etc/icinga
- Remove the install directory
- Temp disable the http -> https redirection so that you can login and amend the config, reload apache. You will need to amend the browser URL to set https: Administration -> Settings -> Server protocol -> https
- Put the redirector back and reload apache
- NagiosQL needs to write to this directory - this may need to be fixed every time Icinga is upgraded:

`root #``chmod g+w /var/lib/icinga/spool/checkresults`
- Change Icinga over to using the objects defined in NagiosQL in /etc/icinga/icinga.conf. Set this from within the web GUI itself: Administration -> Config Targets -> localhost

## Notes

- Snags with Pango seemingly needing to access /root? - reboot fixes it! This manifests itself as graph errors when accessing PNP4Ngios pages
- Handy SQL - use in phpmyadmin:
  - Find/Replace:

- Embedded Perl (ePN). If you enable this note that Icinga prefers this to disable it (rather than nagios):

\# icinga: -epn

### Sample Apache configuration

The following sample Apache vhost configuration includes settings for mod\_auth\_kerb and SSL. Replace monitor and example.co.uk and make sure the SSLCert lines correspond to your environment. Note use of SetEnvIf to automatically "login" certain systems with a username where Kerberos fails - handy for automated systems to access this web server without having to be Kerberized. Replace the authentication settings in the Location / section for your environment.

**`/etc/conf.d/apache2`**

**Snippet from Apache startup configuration file**

```
APACHE2_OPTS="-D INFO -D SSL -D LANGUAGE -D PHP5 -D AUTH_KERB"
```
**`/etc/apache2/vhosts.d/10_mon.conf`**

**Sample Icinga and addons Apache vhost definition**

```
# Monitoring
# JG 9 Aug 2013
Listen 443
<VirtualHost _default_:443>
    DocumentRoot "/var/www/localhost/htdocs"
    ServerName   monitoring.example.co.uk:443
    ServerAdmin  admin@example.co.uk
      
    ErrorLog     /var/log/apache2/mon_error_log
    TransferLog  /var/log/apache2/mon_access_log
  
    SSLEngine           on
    SSLHonorCipherOrder On
    SSLCipherSuite      ECDHE-RSA-AES128-SHA256:AES128-GCM-SHA256:RC4:HIGH:!MD5:!aNULL:!EDH
      
    SSLCertificateFile      /etc/apache2/ssl/server.crt
    SSLCertificateKeyFile   /etc/apache2/ssl/server.key
# Authorization/Authentication
    <Location />
        AuthType Kerberos
        AuthName "Login"
        Krb5Keytab /etc/krb5.keytab
        KrbAuthRealms EXAMPLE.CO.UK
        KrbMethodNegotiate On
        KrbMethodK5Passwd On                                                                                                                                                                         
        KrbServiceName HTTP/monitor@EXAMPLE.CO.UK
        Require valid-user                                                                                                                                                                           
        Order allow,deny
        Allow from 127.0.0.1
        Satisfy any
        SetEnvIf Remote_Addr 127\.0\.0\.1 REMOTE_USER=Mon
    </Location>
    <Directory "/var/www/localhost/htdocs">
        Options Indexes FollowSymlinks
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
    ScriptAlias /cgi-bin/ "/var/www/localhost/cgi-bin/"
    <Directory "/var/www/localhost/cgi-bin">
        AllowOverride None
        Options None
        Order allow,deny
        Allow from all
    </Directory>
# Icinga
    ScriptAlias /icinga/cgi-bin/ /usr/lib/icinga/cgi-bin/
    Alias /icinga /usr/share/icinga/htdocs
    <Directory "/usr/share/icinga/htdocs/">
        Order allow,deny
        Allow from all
    </Directory>
    <Directory "/usr/lib/icinga/cgi-bin/">
        Options ExecCGI
        Order allow,deny
        Allow from all
    </Directory>
#### icinga-Web - taken from the module
# Matching for module stylesheet and images
AliasMatch "^/icinga-web/modules/([A-Za-z0-9]+)/resources/styles/([A-Za-z0-9]+\.css)$" "/usr/share/icinga/icinga-web/app/modules/$1/pub/styles/$2"
AliasMatch "^/icinga-web/modules/([A-Za-z0-9]+)/resources/images/([A-Za-z_\-0-9]+\.(?:png|gif|jpg))$" "/usr/share/icinga/icinga-web/app/modules/$1/pub/images/$2"
# Matching for Icinga Web and the ext3 framework
Alias /icinga-web/js/ext3/ /usr/share/icinga/icinga-web/lib/ext3/
Alias /icinga-web/ /usr/share/icinga/icinga-web/pub/
RedirectMatch "^/icinga-web$" /icinga-web/
# Access to where the styles are located
<DirectoryMatch "^/usr/share/icinga/icinga-web/app/modules/\w+/pub/styles/">
    Options -Indexes -MultiViews
    Order allow,deny
    Allow from all
</DirectoryMatch>
# Access to where the images are located
<DirectoryMatch "^/usr/share/icinga/icinga-web/app/modules/\w+/pub/images/">
    Options -Indexes -MultiViews
    Order allow,deny
    Allow from all
</DirectoryMatch>
# Access to the ext3 library
<Directory "/usr/share/icinga/icinga-web/lib/ext3/">
    Options -Indexes -MultiViews
    Order allow,deny
    Allow from all
</Directory>
# Access to the public web folder of Icinga Web
<Directory "/usr/share/icinga/icinga-web/pub/">
    DirectoryIndex index.php
    Options -MultiViews -Indexes +FollowSymLinks
    RewriteEngine On
    # /icinga-web is base for all rewrite rules
    RewriteBase /icinga-web
    # If the requested URL does not exist (it's likely an agavi route),
    # pass it as path info to index.php, the Agavi dispatch script.
    RewriteRule ^$ index.php?/ [QSA,L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ".*" index.php?/$0 [QSA,L]
    <IfModule mod_deflate.c>
        # Insert filter
        SetOutputFilter DEFLATE
        # Netscape 4.x has some problems...
        BrowserMatch ^Mozilla/4 gzip-only-text/html
        # Netscape 4.06-4.08 have some more problems
        BrowserMatch ^Mozilla/4\.0[678] no-gzip
        # MSIE masquerades as Netscape, but it is fine
        BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
        # Don't compress images
        SetEnvIfNoCase Request_URI \
        \.(?:gif|jpe?g|png)$ no-gzip dont-vary
        # Make sure proxies don't deliver the wrong content
        <IfModule mod_headers.c>
            Header append Vary User-Agent env=!dont-vary
        </IfModule>
    </IfModule>
    <IfDefine APACHE2>
        AcceptPathInfo On
    </IfDefine>
    Order allow,deny
    Allow from all
</Directory>
# /icinga-web
# PNP4Nagios
    Alias /pnp4nagios /usr/share/pnp/
    <Directory /usr/share/pnp>
        AllowOverride AuthConfig
        Order allow,deny
        Allow from all
        RewriteEngine On
        Options FollowSymLinks
        RewriteBase /pnp4nagios
        # Protect application and system files from being viewed
        RewriteRule ^(application|modules|system) - [F,L]
        # Allow any files or directories that exist to be displayed directly
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        # Rewrite all other URLs to index.php/URL
        RewriteRule .* index.php/$0 [PT,L]
    </Directory>
# /PNP4Nagios
# Nagvis
    Alias /nagvis "/usr/share/nagvis/"
    <Directory "/usr/share/nagvis/">
        Options FollowSymLinks
        AllowOverride None
        Order allow,deny
        Allow from all
    </Directory>
# /Nagvis
# Icinga Mobile
Alias /m /usr/local/icinga-mobile/
<Directory /usr/local/icinga-mobile/>
        DirectoryIndex index.html
        Options FollowSymLinks
        AllowOverride all
        Order allow,deny
        Allow from all
</Directory>
# /Icinga Mobile
</VirtualHost>
```
### Local ebuild for Nagvis

**2017-02-22**, the information in this section is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=Icinga&action=edit).

I cobbled this together from the existing ebuild and it works OK for me.

- Create a local overlay directory structure

`root #````
mkdir -p /usr/local/portage/net-analyzer/nagvis/files
```
`root #````
mkdir /usr/local/portage/metadata
```
`root #````
mkdir /usr/local/portage/profiles
```
- Name the repo

**`/usr/local/portage/profiles/repo-name`**

**Overlay name**

- Set layout

**`/usr/local/portage/metadata/layout.conf`**

**Overlay layout file**

```
masters=gentoo
```
- Ebuild files

**`/usr/local/portage/net-analyzer/nagvis/nagvis-1.7.10.ebuild`**

**Ebuild**

```
# Copyright 1999-2009 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
# $Header: $
EAPI=4
inherit eutils confutils depend.php depend.apache
DESCRIPTION="NagVis is a visualization addon for the well known network managment system Nagios."
HOMEPAGE="http://www.nagvis.org/"
SRC_URI="mirror://sourceforge/${PN}/${P}.tar.gz"
LICENSE="GPL-2"
SLOT="0"
KEYWORDS="~amd64 ~ppc ~x86"
IUSE="apache2"
DEPEND=""
RDEPEND="|| ( net-analyzer/nagios net-analyzer/icinga )
        apache2? ( dev-lang/php[apache2] )
        >=media-gfx/graphviz-2.14
        net-analyzer/mk-livestatus
        dev-lang/php[gd,nls,json,session,pdo,sqlite,sockets,mysql,unicode,xml]"
need_php_httpd
want_apache2
pkg_setup() {
        confutils_require_built_with_all dev-lang/php gd nls json session pdo sqlite sockets mysql unicode xml
        depend.apache_pkg_setup
}
src_prepare() {
        epatch "${FILESDIR}"/${P}-base-path.patch
        epatch "${FILESDIR}"/${P}-global-definitions.patch
        grep -Rl "/usr/local" "${S}"/* | xargs sed -i s:/usr/local:/usr:g ||die
        sed -i s:@NAGVIS_WEB@:/nagvis:g "${S}"/etc/apache2-nagvis.conf-sample ||die
        sed -i s:@NAGVIS_PATH@:/usr/share/nagvis/:g "${S}"/etc/apache2-nagvis.conf-sample ||die
        sed -i s:/usr/nagios/var/rw/live:/var/nagios/rw/live:g "${S}"/etc/nagvis.ini.php-sample ||die
}
src_install() {
        dodoc README INSTALL
        insinto /usr/share/nagvis
        doins -r share/{config.php,index.php,frontend,netmap,server,userfiles}
        doins -r docs
        diropts -o apache -g root
        dodir /var/nagvis/tmpl/{cache,compile}
        diropts
        dosym /var/nagvis /usr/share/nagvis/var
        if use apache2 ; then
                insinto "${APACHE_MODULES_CONFDIR}"
                newins etc/apache2-nagvis.conf-sample 98_${PN}.conf
        fi
        insinto /etc/nagvis
        doins -r etc/{conf.d,geomap,.htaccess,nagvis.ini.php-sample}
        fowners apache:root /etc/nagvis
        fperms 0664 /etc/nagvis/nagvis.ini.php-sample
        dosym /etc/nagvis /usr/share/nagvis/etc
        diropts -o apache -g root -m0775
        insopts -o apache -g root -m0664
        doins -r etc/maps
        diropts
        insopts
        # move image maps dir from usr to var and symlink it back
        dodir /var/nagvis/userfiles/images
        mv "${D}"/usr/share/nagvis/userfiles/images/maps "${D}"/var/nagvis/userfiles/images/ ||die
        fowners apache:root /var/nagvis/userfiles/images/maps
        dosym /var/nagvis/userfiles/images/maps /usr/share/nagvis/userfiles/images/maps
}
pkg_postinst() {
        elog "Before running NagVis for the first time, you will need to set up"
        elog "/etc/nagvis/nagvis.ini.php"
        elog "A sample is in"
        elog "/etc/nagvis/nagvis.ini.php-sample"
        if use apache2 ; then
                elog
                elog "For web interface make sure to add -D NAGVIS to APACHE2_OPTS in"
                elog "/etc/conf.d/apache2 and to restart apache2. A default configuration"
                elog "has been placed at /etc/apache2/modules.d/98_${PN}.conf"
        fi
        elog ""
        elog "Default user/password are: nagiosadmin/nagiosadmin"
        elog "                                 guest/guest"
}
```
**`/usr/local/portage/net-analyzer/nagvis/files/nagvis-1.7.10-global-definitions.patch`**

**patch 1**

```
--- a/share/server/core/defines/global.php      2013-07-17 14:40:39.768577131 +0200
+++ b/share/server/core/defines/global.php      2013-07-17 14:41:44.492006012 +0200
@@ -65,7 +65,7 @@
 define('DEBUGLEVEL', 6);
 
 // Path to the debug file
-define('DEBUGFILE', '../../../var/nagvis-debug.log');
+define('DEBUGFILE', '../../var/nagvis-debug.log');
 
 // It is possible to define a conf.d directory for splitting the main
 // configuration in several files. Only the values defined in the CONST_MAINCFG
@@ -79,14 +79,14 @@
 // The last value wins.
 //
 // Path to the main configuration file
-define('CONST_MAINCFG', '../../../etc/nagvis.ini.php');
-define('CONST_MAINCFG_CACHE', '../../../var/nagvis-conf');
+define('CONST_MAINCFG', '../../etc/nagvis.ini.php');
+define('CONST_MAINCFG_CACHE', '../../var/nagvis-conf');
 
 // Path to the main configuration conf.d directory
-define('CONST_MAINCFG_DIR', '../../../etc/conf.d');
+define('CONST_MAINCFG_DIR', '../../etc/conf.d');
 
 // The directory below the NagVis root which is shared by the webserver
-define('HTDOCS_DIR', 'share');
+define('HTDOCS_DIR', '');
 
 // Needed minimal PHP version
 define('CONST_NEEDED_PHP_VERSION', '5.0');
```
**`/usr/local/portage/net-analyzer/nagvis/files/nagvis-1.7.10-base-path.patch`**

**Patch 2**

```
--- a/share/server/core/classes/GlobalMainCfg.php       2013-07-17 14:37:00.475733872 +0200
+++ b/share/server/core/classes/GlobalMainCfg.php       2013-07-17 14:37:46.745756044 +0200
@@ -1391,8 +1391,8 @@
      * @author Roman Kyrylych <rkyrylych@op5.com>
      */
     private function getBasePath() {
-        // Go 3 levels up from nagvis/share/nagvis to nagvis base path
-        return realpath(dirname($_SERVER['SCRIPT_FILENAME']) . '/../../..') . '/';
+        // Go 2 levels up from share/nagvis to nagvis base path
+        return realpath(dirname($_SERVER['SCRIPT_FILENAME']) . '/../..') . '/';
         // Note: the method below causes problems when <docroot>/nagvis is a symlink to <nagvis-base>/share
         // return realpath(dirname(dirname(dirname($_SERVER['SCRIPT_FILENAME'])))).'/';
     }
```
- Digest the ebuild

`root #``ebuild /usr/local/portage/net-analyzer/nagvis/nagvis-1.7.10.ebuild digest`
- Add the repo to Portage

**`/etc/portage/make.conf`**

**Standard Gentoo make.conf snippet**

```
PORTDIR_OVERLAY="/usr/local/portage"
```
- If you use eix then update its database

`root #``eix-update`
- You should now be able to:

`root #``emerge -va nagvis`
These are the packages that would be merged, in order:
Calculating dependencies... done!
\[ebuild  N     \] net-analyzer/nagvis-1.7.10::x-portage  USE="apache2" 0 kB
Total: 1 package (1 new), Size of downloads: 0 kB
Would you like to merge these packages? \[Yes/No\]

### USE flags

These are the flags I generally use and will provide a good starting point

**`/etc/portage/package.use`**

**USE flags**

## References

- Links to docs for Icinga are provided from within the classic web and icinga web interface
