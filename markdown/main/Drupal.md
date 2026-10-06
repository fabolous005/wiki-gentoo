<!-- source: https://wiki.gentoo.org/wiki/Drupal | group: Gentoo Wiki (Main) | wiki-title: Drupal -->
---
title: Drupal
url: https://wiki.gentoo.org/wiki/Drupal
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-04"
fingerprint: "44881c188b66f389"
license: CC BY-SA 4.0
---

# Drupal

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Drupal** is a powerful [PHP](https://wiki.gentoo.org/wiki/PHP) content management system (CMS).

## Preinstall

The Drupal package installs everything you need except a database server. However, some configuration is best done before installing. And some packages might be used for more things than Drupal and hence should be installed separately.

### USE flags

If you plan to use the default webserver, Apache 2, edit /etc/portage/make.conf and add "apache2" to your USE flags.

**`/etc/portage/make.conf`**

**Portage configuration file**

```
USE="... apache2 ..."
```
Then add the needed USE flags for PHP. Edit /etc/portage/package.use and add "gd mysql mysqli pdo" to the PHP specific USE flags.

**`/etc/portage/package.use`**

**Portage configuration file**

```
dev-lang/php gd mysql mysqli pdo
```
If you plan to use PostgreSQL instead of the default MySQL / MariaDB, add "postgres".

### Packages

You must install a database server unless you are planning to use an already existing
(remote) server. Use [MariaDB](https://wiki.gentoo.org/wiki/MariaDB) (or [MySQL](https://wiki.gentoo.org/wiki/MySQL)).

`root #``emerge --ask dev-db/mariadb`
You must also configure the database server by rerunning the prior command with the `--config` option.

`root #``emerge --ask --config dev-db/mariadb`
If you plan to use a webserver for other things than Drupal, install it separately.

`root #``emerge --ask www-servers/apache`
if you want to use [Apache](https://wiki.gentoo.org/wiki/Apache).

If you need [PHP](https://wiki.gentoo.org/wiki/PHP) for other web applications, install it separately too.

`root #``emerge --ask dev-lang/php`
PS! To enable PHP in Apache, edit /etc/conf.d/apache2 and add 
`-D PHP` to your `APACHE2_OPTS` line. You might 
also want to check out PHP\_TARGETS

### Controlling your LAMP stack

Start up your LAMP stack

`root #``rc-service mysql start``root #``rc-service apache2 start`
Set the LAMP stack to start upon boot

`root #``rc-update add mysql default``root #``rc-update add apache2 default`
If you use systemd, modify the commands above.

## Install

### Unmask

At the time of writing, [www-apps/drupal](https://packages.gentoo.org/packages/www-apps/drupal) is masked as experimental only. If you run a "stable" system, you'll have to add it to your /etc/portage/package.accept\_keywords.

**`/etc/portage/package.accept_keywords`**

**Portage configuration file**

```
www-apps/drupal
```
### Emerge

Now, install the package:

`root #``emerge --ask drupal`
Run [webapp-config](https://wiki.gentoo.org/wiki/Webapp-config) manually to actually install the files for the webserver:

`root #``webapp-config -h localhost -u apache -d /drupal -I drupal 8.8.1`
This command installs Drupal in the "drupal" subdirectory of the default vhost. If desired, change "localhost" to the hostname of another virtual host/website, and "8.8.1" to the desired Drupal version..

webapp-config can be used to install multiple Drupal sites (based on the same Gentoo package that was just emerged).

### Configure a database

You need to create a database, a database user and give that user the correct permissions on the database tables. The needed SQL code depends on the Drupal version you installed and the database server you selected.

Hence, read the "Create the database" section of the "Installing Drupal" 
documentation (on drupal.org) - for 
[Drupal 8](https://www.drupal.org/docs/8/install/) or 
[Drupal 7](https://www.drupal.org/docs/7/install/). You should read
the subsection about (SQL) commands / the command line.

### Configure Drupal

The normal way to configure (or install) Drupal, is to visit your website in a browser and follow the instructions. Go to

which will redirect you to the Drupal installer. (You can configure Drupal from the command line using Drush if you prefer.)

Read the Drupal documentation for help and tips - for 
[Drupal 9+](https://www.drupal.org/docs/getting-started/installing-drupal/) or 
[Drupal 7](https://www.drupal.org/docs/7/install/).


Release notes:
