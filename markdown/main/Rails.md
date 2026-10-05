<!-- source: https://wiki.gentoo.org/wiki/Rails | group: Gentoo Wiki (Main) | wiki-title: Rails -->
---
title: USE flags for dev-ruby/rails Ruby on Rails is a web-application and persistence framework
url: https://wiki.gentoo.org/wiki/Rails
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-27"
fingerprint: "65d6172257d86777"
license: CC BY-SA 4.0
---

# Rails

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Rails** or **Ruby on Rails** is an [MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) web application framework written in [Ruby](https://wiki.gentoo.org/wiki/Ruby).

# Installation

## USE flags


## Emerge

Install [dev-ruby/rails](https://packages.gentoo.org/packages/dev-ruby/rails):

`root #``emerge --ask dev-ruby/rails`
## Setup

`root /var/www #````
rails new ror
```
`root /var/www #````
cd ror
```
`root /var/www/ror #````
rails server
```
Point a web browser to [http://0.0.0.0:3000](http://0.0.0.0:3000). are now riding rails via WEBrick. This is only for testing, not production.  WEBrick could probably be used for production if it were behind [nginx](https://wiki.gentoo.org/wiki/Nginx) or an accelerator proxy such as [varnish](https://wiki.gentoo.org/wiki/Varnish).

### Configuration

Rails is not eselect aware, this might come in handy to resolving some issues, but be aware that bundle install will blast things away.

`root #``eselect rails list``root #``eselect rails set 1`
## Passenger via apache

Emerge passenger:

`root #``emerge --ask www-apache/passenger`
Add `-D PASSENGER` to the `APACHE2_OPTS` variable in Apache's config:

**`/etc/conf.d/apache2`**

**add -D PASSENGER to apache's config**

Passenger needs Apache to relax the rules a little bit. Edit the /etc/apache2/modules.d/30\_mod\_passenger.conf file and insert relaxed settings before the closing \</IfDefine> tag:

**`/etc/apache2/modules.d/30_mod_passenger.conf`**

**apache 2.2**

```
<Directory />
	Options FollowSymLinks
	AllowOverride all
	Order allow,deny
	Allow from all
</Directory>
</IfDefine>
```
**`/etc/apache2/modules.d/30_mod_passenger.conf`**

**apache 2.4**

```
<Directory />
	Options FollowSymLinks
	AllowOverride all
	Require all granted
</Directory>
</IfDefine>
```
Backup the original Apache vhost:

`root #``mv /etc/apache2/vhosts.d/00_default_vhost.conf /etc/apache2/vhosts.d/00_default_vhost.conf.backup`
Drop in the passenger vhost file for Apache:

**`/etc/apache2/vhosts.d/00_default_vhost.conf`**

```
<IfDefine DEFAULT_VHOST>
Listen 80
NameVirtualHost *:80
<VirtualHost *:80>
      DocumentRoot /var/www/ror/public    
#  RailsBaseURI /
  RailsEnv development
      <Directory /var/www/ror/public>
Options -MultiViews
      </Directory>
</VirtualHost>
</IfDefine>
```
`root #``/etc/init.d/apache2 restart`
Point a web browser to [http://0.0.0.0](http://0.0.0.0) or [http://127.0.0.1](http://127.0.0.1) or [http://localhost](http://localhost) and your riding rails via passenger now.

## See also

- [Ruby](https://wiki.gentoo.org/wiki/Ruby) - The programming language used to build Rails.
