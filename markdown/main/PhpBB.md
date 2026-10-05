<!-- source: https://wiki.gentoo.org/wiki/PhpBB | group: Gentoo Wiki (Main) | wiki-title: PhpBB -->
---
title: phpBB
url: https://wiki.gentoo.org/wiki/PhpBB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-19"
fingerprint: "7287091891a63bc4"
license: CC BY-SA 4.0
---

# phpBB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**phpBB** is open-source bulletin board software, an internet forum package, that runs on the [PHP](https://wiki.gentoo.org/wiki/PHP) scripting language.

## Install

Before installing **phpBB**, it is recommended to peruse the configuration options for [Apache](https://wiki.gentoo.org/wiki/Apache)/[nginx](https://wiki.gentoo.org/wiki/Nginx), [MariaDB](https://wiki.gentoo.org/wiki/MariaDB)/[MySQL](https://wiki.gentoo.org/wiki/MySQL), and [PHP](https://wiki.gentoo.org/wiki/PHP) as well.

### USE flags


| [ftp](https://packages.gentoo.org/useflags/ftp) | Add FTP (File Transfer Protocol) support | 
| [gd](https://packages.gentoo.org/useflags/gd) | Add support for media-libs/gd (to generate graphics on the fly) | 
| [mssql](https://packages.gentoo.org/useflags/mssql) | Add support for Microsoft SQL Server database | 
| [mysqli](https://packages.gentoo.org/useflags/mysqli) | Add support for the improved mySQL libraries | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [vhosts](https://packages.gentoo.org/useflags/vhosts) | Add support for installing web-based applications into a virtual-hosting environment | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

When using Apache, it is important to enable PHP support for [www-servers/apache](https://packages.gentoo.org/packages/www-servers/apache) by installing [dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php) with the `apache2` USE-flag.

For nginx, the `fpm` flag should be enabled on [dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php).

With MariaDB/MySQL, [dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php) will need to have been built with the `mysql` and `mysqli` flags enabled.

### Emerge

`root #``emerge --ask www-apps/phpBB`
## Configuration

### Enabling Apache PHP support

**`/etc/conf.d/apache2`**

**Enabling the PHP module**

```
APACHE2_OPTS="... -D PHP"
```
### Creating a database

Refer to [MySQL/Startup Guide](https://wiki.gentoo.org/wiki/MySQL/Startup_Guide) for detailed instructions.

`root #``/etc/init.d/mysql start``user $````
mysql -u root -p
```
mysql> CREATE DATABASE IF NOT EXISTS \<database> DEFAULT CHARACTER SET utf8 COLLATE utf8\_unicode\_ci;
mysql> CREATE USER '\<username>'@'localhost' IDENTIFIED BY '\<password>';
mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER ON \<database>.\* TO '\<username>'@'localhost' IDENTIFIED BY '\<password>';
mysql> quit

### Completing the phpBB install

Browsing to the root of the install, [http://localhost/phpBB](http://localhost/phpBB) for example (`https` for SSL installs), should now bring up the phpBB introduction web page with an overview and install tabs, which will work as a guide through the rest of the installation process.
