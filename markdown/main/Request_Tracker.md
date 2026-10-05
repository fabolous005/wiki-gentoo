<!-- source: https://wiki.gentoo.org/wiki/Request_Tracker | group: Gentoo Wiki (Main) | wiki-title: Request Tracker -->
---
title: Request Tracker
url: https://wiki.gentoo.org/wiki/Request_Tracker
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-16"
fingerprint: e29d6d5af9873b46
license: CC BY-SA 4.0
---

# Request Tracker

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

- Update Apache FastCGI instructions
- Update Lighttpd instructions
- Add instructions about fetching emails/generating tickets

[Request Tracker (RT)](https://www.bestpractical.com/rt/) is a battle-tested issue tracking system which thousands of organizations use for bug tracking, help desk ticketing, customer service, workflow processes, change management, network operations, youth counselling and even more. [Organizations around the world](https://www.bestpractical.com/rt/who.html) have been running smoothly thanks to RT for over 10 years.

## About this guide

This guide was written using the latest version of RT available, which at the time of this writing is 4.2.9.

This guide assumes familiarity with [Apache](https://wiki.gentoo.org/wiki/Apache) or [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) and will not delve into the details of either.

Whether or not virtual hosting is used holds no bearing on the bulk of this guide. It will be noted if there's something significantly different that must be done in a virtual hosting environment.

## Installation

### USE flags


| [+apache](https://packages.gentoo.org/useflags/+apache) | Add www-servers/apache support | 
| [+postgres](https://packages.gentoo.org/useflags/+postgres) | Add support for the postgresql database | 
| [fastcgi](https://packages.gentoo.org/useflags/fastcgi) | Add support for the FastCGI interface | 
| [lighttpd](https://packages.gentoo.org/useflags/lighttpd) | Add www-servers/lighttpd support | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [nginx](https://packages.gentoo.org/useflags/nginx) | Add www-servers/nginx support | 
| [vhosts](https://packages.gentoo.org/useflags/vhosts) | Add support for installing web-based applications into a virtual-hosting environment | 

### Requirements

RT requires a database backend and works equally well with either [MySQL](https://wiki.gentoo.org/wiki/MySQL) or [PostgreSQL](https://wiki.gentoo.org/wiki/PostgreSQL). Enable at most one of their USE flags:

`root #``echo "www-apps/rt mysql" >> /etc/portage/package.use``root #``echo "www-apps/rt postgres" >> /etc/portage/package.use`
RT also requires a Web server. The default is to run on [Apache](https://wiki.gentoo.org/wiki/Apache), but [lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) is also documented. To use lighttpd, you must enable its USE flag:

`root #``echo "www-apps/rt lighttpd" >> /etc/portage/package.use`
### Emerge

Many of the packages RT depends on, including RT's own package, are keyword masked. Use the following command to have a patch automatically generated.

`root #``emerge -av --autounmask-write =www-apps/rt-4.2.9`
Once the previous command has finished, use dispatch-conf to apply the patch:

`root #``dispatch-conf`
Run emerge again:

`root #``emerge -av www-apps/rt`
When the `vhosts` USE flag is enabled, run webapp-config to finish the installation:

`root #``webapp-config -I -h localhost -d rt rt 4.2.9`
## Setup and configuration

### Database

RT provides a script called rt-setup-database which creates the initial database and a database user.

`root #``/var/www/localhost/rt-4.2.9/sbin/rt-setup-database --action init --dba dbasuperuser --prompt-for-dba-password`
In order to create or update your RT database, this script needs to connect to your
Pg instance on localhost (port '') as postgres
Please specify that user's database password below. If the user has no database
password, just press return.
Password: 
Working with:
Type:   Pg
Host:   localhost
Port:
Name:   rt4
User:   rt\_user
DBA:    postgres
Now creating a Pg database rt4 for RT.
Done.
Now populating database schema.
Done.
Now inserting database ACLs.
Done.
Now inserting RT core system objects.
Done.
Now inserting data.
Done inserting data.
Done.

### Configuring RT

RT uses an overlay system for configuration. This means that the default configuration is declared in /var/www/localhost/rt-4.2.9/etc/RT\_Config.pm, and that custom configurations are declared in /var/www/localhost/rt-4.2.9/etc/RT\_SiteConfig.pm. RT\_SiteConfig.pm will not exist until manually created. Any custom configuration in RT\_SiteConfig.pm will be preserved in upgrades, while the default configurations, RT\_Config.pm, will be overwritten.

Either copy certain sections from RT\_Config.pm to RT\_SiteConfig.pm, or create a full config from scratch.

`root #````
cd /var/www/localhost/rt-4.2.9/etc
```
`root #````
cp RT_Config.pm RT_SiteConfig.pm
```
`root #````
chmod u+w RT_SiteConfig.pm
```
`root #``$EDITOR RT_SiteConfig.pm`
The configuration file is well documented, but the [official documentation](https://www.bestpractical.com/docs/) can also be consulted.

#### Sendmail alternatives

When not using a full-blown SMTP server locally, use a lightweight client to send the emails instead as long as it provides a sendmail-compatible executable. Mail options are specified in RT\_SiteConfig.pm.

### Configuring the web server

Request Tracker can be run on any [PSGI compliant server](http://plackperl.org/). However, [Apache](https://wiki.gentoo.org/wiki/Apache) and [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) are proven platforms.

#### Apache

Only information pertinent to RT will be covered. Additional information about [Apache](https://wiki.gentoo.org/wiki/Apache) is covered elsewhere.

There's little information about which method works better for RT on Apache, and benchmarks have shown mod\_perl and FastCGI to be nearly equal.

##### mod\_perl

Save the following snippet within the individual `VirtualHost` tags RT is installed to or /etc/apache2/vhosts.d/default\_vhost.include:

Instruct Apache to start with mod\_perl enabled:

**`/etc/conf.d/apache2`**

It may be necessary to change the owner and group of RT's Mason data directory:

`root #``chown -R apache:apache /var/www/localhost/rt-4.2.9/var/mason_data`
##### mod\_fastcgi

NOTE: When using `mod_fastcgi`, instruct webapp-config to install `rt` with appropriate permissions. Edit /etc/vhosts/webapp-config:

**`/etc/vhosts/webapp-config`**

Save the following snippet within the individual `VirtualHost` tags RT is installed to or /etc/apache2/vhosts.d/default\_vhost.include.

Edit /etc/conf.d/apache2 to instruct `apache2` to start with `FASTCGI` and enabled:

**`/etc/conf.d/apache2`**

To have apache start on boot:

`root #``rc-update add apache2 default`
Restart apache so that all changes made so far will take effect:

`root #``/etc/init.d/apache2 restart`
#### lighttpd (untested)

RT is able to run on `lighttpd` + `fastcgi`. The ebuild will install an init script /etc/init.d/rt and a config file /etc/conf.d/rt.

NOTE: To use `mod_fastcgi`, instruct webapp-config to install `rt` with appropriate permissions. Edit /etc/vhosts/webapp-config:

**`/etc/vhosts/webapp-config`**

Edit /etc/conf.d/rt to set `RTPATH` to the root of the installation. Everything else in that file can be left at there defaults normally.

Also note that, under the default configuration, the socket in `$FCGI_SOCKET_PATH` is owned by rt:lighttpd, and is chmod-ded to g+rwx. This means that user `lighttpd` needs to be in the `rt` group. One way to do that is to use `vigr`. To change that behaviour, edit /etc/init.d/rt to suit.

Edit /etc/lighttpd.conf to enable `mod_fastcgi`:

- Uncomment `mod_fastcgi` under `server.modules`
- set `server.document-root`
- set `fastcgi.server` to something like this:

**`/etc/lighttpd.conf`**

Be sure to set the correct path to socket (same as `$FCGI_SOCKET_PATH` in /etc/conf.d/rt).

Now, start `rt` and `lighttpd`:

`root #````
/etc/init.d/rt start
```
`root #``/etc/init.d/lighttpd start`
If things don't seem to be working, check the `lighttpd` logs in /var/log/lighttpd and edit /etc/init.d/rt as per the comments in the file to make the `rt` daemon more verbose.

Note: this initscript should work with any `fastcgi`-enabled webserver.

## Feeding emails into RT

There are a variety of methods to feed email into RT. Use an MTA, such as Postfix, Exim, or the real Sendmail, whenever possible. Follow the [MTA On Same Server](https://wiki.gentoo.org#MTA_On_Same_Server) portion of this section.

However, if the system is only fetching email from a remote server, an MTA is optional, just 2 or 3 smaller utilities are required. Follow the [Without An MTA](https://wiki.gentoo.org#Without_An_MTA) portion of this section.

### MTA on same server

TODO

### Without a MTA

There are 2 utilities needed: [net-mail/fetchmail](https://packages.gentoo.org/packages/net-mail/fetchmail) and [mail-mta/msmtp](https://packages.gentoo.org/packages/mail-mta/msmtp). When using aliases delivered to the same email box, [mail-filter/procmail](https://packages.gentoo.org/packages/mail-filter/procmail) becomes necessary.

**`/etc/fetchmailrc`**

**Configuration file for fetchmail**

**`/var/www/localhost/etc/rt-4.2.9/etc/procmailrc`**

**Configuration for procmail**

## Log in

Use a browser to log into RT. Username is `root`, and password is `password`. Change the password.

## Special thanks

Thank you to all those who worked on the [original version](https://rt-wiki.bestpractical.com/wiki/GentooInstallGuide) of this guide.
