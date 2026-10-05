<!-- source: https://wiki.gentoo.org/wiki/Piwik | group: Gentoo Wiki (Main) | wiki-title: Piwik -->
---
title: Piwik
url: https://wiki.gentoo.org/wiki/Piwik
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-09-06"
fingerprint: "82011569b57f1af3"
license: CC BY-SA 4.0
---

# Piwik

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the installation and configuration of Piwik Web Analytics software.

## Installation

### USE flags

Piwik uses the `gd`, `mysqli`, `pdo`, and `truetype` USE flags on [dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php). Be sure they are enabled by editing /etc/portage/package.use:

**`/etc/portage/package.use/php`**

### Emerge

`root #``emerge --ask --verbose dev-lang/php`
In order to see geographical IP addresses, the geoip PHP extention is required:

`root #``emerge --ask dev-php/pecl-geoip`
## Configuration

### php.ini

Piwik requires the following change in php.ini:

**`/etc/php/fpm-php5.6/php.ini`**

**FPM PHP 5.6 configuration file**

### Services

#### OpenRC

Restart the web server and php services. In the example below the Nginx server is used:

`root #````
/etc/init.d/php-fpm restart
```
`root #````
/etc/init.d/nginx restart
```
### Piwik software

Change to web server public directory:

`root #``cd /var/www/localhost/htdocs`
Download and extract piwik analytics:

### Database

#### MySQL

Create a database and user for Piwik (using mysql example):

`user $``mysql -u root -p`
mysql> CREATE DATABASE piwik;
mysql> USE piwik;
mysql> grant all privileges on piwik.\* to user@localhost identified by 'password';
mysql> grant all privileges on piwik.\* to user@'%' identified by 'password';

Create directories and change write permissions required by Piwik:

`root #``mkdir /var/www/localhost/piwikconfig``root #``chmod a+w /var/www/localhost/htdocs/piwikconfig``root #``chmod a+w /var/www/localhost/htdocs/piwik/tmp``root #``chmod a+w /var/www/localhost/htdocs/piwik/config`
In a browser navigate to piwik and follow the configuration instructions.

Change back the write permission on directories except on /var/www/localhost/htdocs/piwik/tmp:

`root #``chmod a-w /var/www/localhost/htdocs/piwikconfig``root #``chmod a-w /var/www/localhost/htdocs/piwik/config`
