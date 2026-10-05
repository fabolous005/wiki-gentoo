<!-- source: https://wiki.gentoo.org/wiki/Nagios | group: Gentoo Wiki (Main) | wiki-title: Nagios -->
---
title: Nagios
url: https://wiki.gentoo.org/wiki/Nagios
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-22"
fingerprint: "92f8e87b7d8be956"
license: CC BY-SA 4.0
---

# Nagios

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Nagios** offers a complete monitoring and alerting for servers, switches, applications, and services.

Nagios is written in perl.

## Installation

### Web server

Decide which web server will be to used and set it up:

Once finished return here to and continue reading.

### USE flags

Set the proper USE flags for Nagios before it emerging it:


Do not forget to enable the right USE flags for Nagios plugins ([net-analyzer/nagios-plugins](https://packages.gentoo.org/packages/net-analyzer/nagios-plugins)).

### Emerge

Finally install [net-analyzer/nagios](https://packages.gentoo.org/packages/net-analyzer/nagios):

`root #``emerge --ask net-analyzer/nagios`
## Configuration

### Apache

Enable the Nagios module for Apache:

**`/etc/conf.d/apache2`**

```
APACHE2_OPTS="... -D NAGIOS"
```
Since Nagios requires [PHP](https://wiki.gentoo.org/wiki/PHP) for its web interface, it may needed to be enabled as well if it has not been previously. One way is to simply add `-D PHP5` to `APACHE2_OPTS` and edit /etc/php/apache2-php\<YOUR\_PHP\_VERSION>/php.ini This should be fine unless PHP is needed for purposes other than hosting Nagios.

If using Apache 2.4 (which is still marked unstable as of April 2015) the /etc/apache2/modules.d/99\_nagios3.conf file may need to be modified to fit the new authorization directives of Apache 2.4.

Remember to add the `apache` user to group `nagios`:

`root #``usermod -a -G nagios apache`
Restart the Apache service to have it recognize the group change:

`root #``rc-service apache2 restart`
### Lighttpd

Enable the Nagios configuration for Lighttpd:

**`/etc/lighttpd/lighttpd.conf`**

Configure authentication. More information on how to set this up can be found in the Lighttpd documentation.

**`/etc/lighttpd/nagios.conf`**

Restart the Lighttpd service:

`root #``rc-service lighttpd restart`
You may have to create the run folder for lighttpd

`root #````
mkdir -p /run/lighttpd;
chown lighttpd /run/lighttpd
```


### Nginx

See the [Nginx](https://wiki.gentoo.org/wiki/Nginx) guide before continuing if you don't have it already setup.

Emerge [www-servers/spawn-fcgi](https://packages.gentoo.org/packages/www-servers/spawn-fcgi) and [www-misc/fcgiwrap](https://packages.gentoo.org/packages/www-misc/fcgiwrap):

`root #``emerge --ask www-servers/spawn-fcgi www-misc/fcgiwrap`
Next, create an init script for a spawn-fcgi instance dedicated to nagios:

`root #````
ln -s /etc/init.d/spawn-fcgi /etc/init.d/spawn-fcgi.nagios
```
`root #````
cp /etc/conf.d/spawn-fcgi /etc/conf.d/spawn-fcgi.nagios
```
Then, configure our spawn-fcgi instance to launch fcgiwrap and listen on a unix socket:

**`/etc/conf.d/spawn-fcgi.nagios`**

Don't forget to add spawn-fcgi.nagios to the default runlevel and start it:

`root #````
rc-update add spawn-fcgi.nagios default
```
`root #````
rc-service spawn-fcgi.nagios start
```
You may need to change the owner of the /var/nagios folder, so fcgiwrap can access it:

`root #````
chown nagios:nagios /var/nagios
```
Nginx can now be configured to serve our nagios instance. Here's an example configuration snippet for nginx, assuming you have defined a php upstream:

**`/etc/nginx/nginx.conf`**

### Permissions

Add the user name(s) to the `nagios` group, whom are allowed access to the Nagios service:

`root #``gpasswd -a <USER_NAME> nagios`
Once done, completely sign out from all shells and re-login for the update to apply.

### Boot service

Start Nagios:

`root #``rc-service nagios start`
To start Nagios at boot time, add it the default runlevel:

`root #``rc-update add nagios default`
## Testing

Open a browser and navigate to [http://localhost/nagios](http://localhost/nagios)
