<!-- source: https://wiki.gentoo.org/wiki/MediaWiki | group: Gentoo Wiki (Main) | wiki-title: MediaWiki -->
---
title: MediaWiki
url: https://wiki.gentoo.org/wiki/MediaWiki
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-07"
fingerprint: d3f7f111e9862094
license: CC BY-SA 4.0
---

# MediaWiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Resources**

**MediaWiki** is a [PHP](https://wiki.gentoo.org/wiki/PHP)-powered web application used by the Gentoo wiki and the various Wikimedia Project websites (including Wikipedia).

## Installation

### Prerequisites

- Install [PHP](https://wiki.gentoo.org/wiki/PHP). Enable the *xmlreader* USE flag, because MediaWiki requires it:

- `root #``echo "dev-lang/php xmlreader" >> /etc/portage/package.use`

- Install a web server and set it up for use of PHP:
  - [Apache](https://wiki.gentoo.org/wiki/Apache) — an efficient, extensible [web server](https://wiki.gentoo.org/wiki/Category:Web_Servers). It is one of the most popular web servers used the Internet.
  - [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) — a fast and lightweight [web server](https://wiki.gentoo.org/wiki/Category:Web_servers).
  - [Nginx](https://wiki.gentoo.org/wiki/Nginx) — a robust, small, high performance [web server](https://wiki.gentoo.org/wiki/Category:Web_servers) and reverse proxy server.

- Install [MySQL](https://wiki.gentoo.org/wiki/MySQL) ([MariaDB](https://wiki.gentoo.org/wiki/MariaDB) is a suitable alternative and the default virtual/mysql provider on Gentoo). Create a database for MediaWiki:

`root #````
mysql -u root -p
```
mysql> CREATE DATABASE IF NOT EXISTS \`mediawiki\` DEFAULT CHARACTER SET \`utf8\` COLLATE \`utf8\_unicode\_ci\`;
mysql> CREATE USER 'mediawiki'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER ON \`mediawiki\`.\* TO 'mediawiki'@'localhost' IDENTIFIED BY 'password';
mysql> FLUSH PRIVILEGES;
mysql> \q

- If the database (MariaDB or MySQL) server is on a different server than MediaWiki, set these USE flags:

**`/etc/portage/package.use`**

### www-apps/mediawiki


| [+sqlite](https://packages.gentoo.org/useflags/+sqlite) | Add support for sqlite - embedded sql database | 
| [imagemagick](https://packages.gentoo.org/useflags/imagemagick) | Enable optional support for the ImageMagick or GraphicsMagick image converter | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [vhosts](https://packages.gentoo.org/useflags/vhosts) | Add support for installing web-based applications into a virtual-hosting environment | 

Install [www-apps/mediawiki](https://packages.gentoo.org/packages/www-apps/mediawiki):

`root #``emerge --ask www-apps/mediawiki`
## Setup

- Copy mediawiki files from /usr/share/webapps/mediawiki/{version}/htdocs to /var/www/localhost/htdocs/mediawiki
- Point a browser at [http://127.0.0.1/mediawiki](http://127.0.0.1/mediawiki) and follow the instructions.
  - If there is no GUI on the computer running mediawiki, then log in from another machine using http://\<server>/mediawiki instead, where \<server> is the name or IP address of the server (see [/etc/hosts](https://wiki.gentoo.org/wiki/Handbook:Parts/Installation/System/en#The_hosts_file)).
- Page "Welcome to MediaWiki!":
  - "Database name" is "mediawiki"
  - "Database username" is "mediawiki"
  - "Database password" is "changeme" (or what else is setup)
- Page "Connect to database":
  - Database character set" should be "UTF-8"
- Page "Name"
  - "Name of wiki" has to be set
  - Setup admin user and password
- Page "Complete!"
  - Download the LocalSettings.php configuration and move it to /var/www/localhost/htdocs/mediawiki/LocalSettings.php.
- Point a browser at [http://127.0.0.1/mediawiki/](http://127.0.0.1/mediawiki/) (or the address used above) to see the newly installed and configured wiki.

## Advanced configuration

- To use a [shorter URL](https://www.mediawiki.org/wiki/Manual:Short_URL/Apache) make the following modifications:

add this somewhere (does not matter exactly where):

**`/var/www/localhost/htdocs/mediawiki/LocalSettings.php`**

```
$wgArticlePath = "/wiki/$1"; 
$wgUsePathInfo = true;
```
and add this between \<Directory> tags:

**`/etc/apache2/vhosts.d/default_vhost.include`**

```
RewriteEngine On
RewriteRule ^/?wiki(/.*)?$ %{DOCUMENT_ROOT}/mediawiki/index.php [L]
```
- By default the landing page in MediaWiki is unlocked for anyone to edit. Login as admin. By the top right of the page is an arrow that points down, click it and the 3rd option down is "protect" to lock down the page.

- Set an avatar for the wiki. Add to the bottom of /var/www/localhost/htdocs/mediawiki/LocalSettings.php:

- FILE**`/var/www/localhost/htdocs/mediawiki/LocalSettings.php`** $wgLogo = "/mediawiki/wiki.png";
