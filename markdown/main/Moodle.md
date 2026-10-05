<!-- source: https://wiki.gentoo.org/wiki/Moodle | group: Gentoo Wiki (Main) | wiki-title: Moodle -->
---
title: Moodle
url: https://wiki.gentoo.org/wiki/Moodle
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: c3810050d1cebac4
license: CC BY-SA 4.0
---

# Moodle

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Moodle** is a LAMP stack content management system for educators.

## Installation

### USE flags


| [imap](https://packages.gentoo.org/useflags/imap) | Add support for IMAP (Internet Mail Application Protocol) | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [mssql](https://packages.gentoo.org/useflags/mssql) | Add support for Microsoft SQL Server database | 
| [mysqli](https://packages.gentoo.org/useflags/mysqli) | Add support for the improved mySQL libraries | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Add ODBC Support (Open DataBase Connectivity) | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [vhosts](https://packages.gentoo.org/useflags/vhosts) | Add support for installing web-based applications into a virtual-hosting environment | 

### System Arrangement

`root #`
echo "www-apps/moodle mysqli" >> /etc/portage/package.use
echo "app-eselect/eselect-php apache2" >> /etc/portage/package.use
echo "dev-lang/php gd xmlrpc zip mysqli intl curl soap apache2" >> /etc/portage/package.use
echo "www-apps/moodle" >> /etc/portage/package.accept_keywords

### Merge

`root #``emerge --ask moodle`
### Adjust Settings

Add -D PHP5 in APACHE2\_OPTS to /etc/conf.d/apache

`root #`
APACHEPHP=$(cat /etc/conf.d/apache2 | grep -c PHP); if [ "$APACHEPHP" = 1 ] ;\
 then echo "done"; else sed -i -e 's/S="/S="-D PHP5 /' /etc/conf.d/apache2; fi

Link moodle to the web root.

`root #``ln -s /usr/share/webapps/moodle/2.5.1/htdocs/ /var/www/localhost/htdocs/moodle`
### Mysql

Create a database for moodle to interact with.

`root #````
mysql -u root -p
```
mysql> SET GLOBAL binlog\_format = 'ROW';
mysql> CREATE DATABASE IF NOT EXISTS \`moodle\_db\` DEFAULT CHARACTER SET \`utf8\` COLLATE \`utf8\_unicode\_ci\`;
mysql> CREATE USER 'moodle\_user'@'localhost' IDENTIFIED BY 'changeme';
mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER ON \`moodle\_db\`.\* TO 'moodle\_user'@'localhost' IDENTIFIED BY 'changeme';
mysql> \q

### Adjust Configs

`root #``nano /var/www/localhost/htdocs/moodle/config.php`
Adjust $CFG->dbpass to your password. If you intend on using this program with a domain name, insert domain name now.



## Web end

Point your browser at [http://localhost/moodle](http://localhost/moodle)

- continue
- continue
- continue (probably bottom left under spell checker module install)
- fill out & update profile
- name your site, save changes
