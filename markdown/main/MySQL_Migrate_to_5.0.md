<!-- source: https://wiki.gentoo.org/wiki/MySQL/Migrate_to_5.0 | group: Gentoo Wiki (Main) | wiki-title: MySQL/Migrate to 5.0 -->
---
title: MySQL/Migrate to 5.0
url: https://wiki.gentoo.org/wiki/MySQL/Migrate_to_5.0
hostname: gentoo.org
sitename: MySQL/Migrate to 5.0
date: "2024-03-07"
fingerprint: e2b871448186b387
license: CC BY-SA 4.0
---

# MySQL/Migrate to 5.0

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This document describes how to upgrade to MySQL 4.\* and to 5.0.\*.

## Straight upgrade, suggested for 4.1 => 5.0 migration

The myisam storage engine in version 4.1 was already mature enough to allow a direct upgrade to the next major version of MySQL.

For this step two shells are required because locks belong to the mysql session.

`root #````
quickpkg dev-db/mysql
```
`root #````
alias MYSQL="mysql --user=root --password='your_password'"
```
This next step should be done in the second shell:

`root #``mysql --user=root --password='your_password'``mysql>``FLUSH TABLES WITH READ LOCK;`
Return to the first shell to run this command:

`root #``tar -cjpvf ~/mysql.$(date +%F"T"%H-%M).tar.bz2 /etc/conf.d/mysql /etc/mysql/my.cnf "${DATADIR}"`
The following commands should be done in the second shell:

`mysql>``UNLOCK TABLES;``mysql>``quit`
Return to the first shell for the rest of the upgrade:

`root #````
tar -tjvf ~/mysql.*.tar.bz2
```
`root #````
emerge -av ">dev-db/mysql-5.0"
```
`root #````
dispatch-conf
```
`root #````
revdep-rebuild
```
`root #````
/etc/init.d/mysql restart
```
`root #````
mysql_upgrade_shell --user=root --password='your_password' --protocol=tcp --datadir="${DATADIR}"
```
`root #````
/etc/init.d/mysql restart
```
`root #````
unset DATADIR
```
`root #``unalias MYSQL`
## Upgrading from old versions of MySQL

Users upgrading from an old version (\<4.0.24) of MySQL will first have to install MySQL 4.0.25. If you are already running a more recent version, you can skip this section and continue with backing up the databases.

`root #``emerge -av --buildpkg "<mysql-4.1"`
## Creating a backup of your current data

One of the most important tasks that every database administrator has to perform is backing up data. Here we go:

`root #````
mysqldump \
--password='your_password' \
-hlocalhost \
--all-databases \
--opt \
--allow-keywords \
--flush-logs \
--hex-blob \
--master-data \
--max_allowed_packet=16M \
--quote-names \
```
-uroot \

--result-file=BACKUP_MYSQL_4.0.SQL
Now a file named BACKUP\_MYSQL\_4.0.SQL should exist, which can be used later to recreate your data. The data is described in the MySQL dialect of SQL, the Structured Query Language.

Now would also be a good time to see if the backup you have created is working.

## Upgrading from recent versions of MySQL

If you have skipped step #1, you now have to create a backup package (of the database server, not the data) of the currently installed version:

`root #``quickpkg dev-db/mysql`
Now it's time to clean out the current version and all of its data:

`root #````
/etc/init.d/mysql stop
```
`root #````
emerge -C mysql
```
`root #````
tar cjpvf ~/mysql.$(date +%F"T"%H-%M).tar.bz2 /etc/mysql/my.cnf /var/lib/mysql/
```
`root #````
ls -l ~/mysql.*
```
`root #``rm -rf /var/lib/mysql/ /var/log/mysql`
After you get rid of your old MySQL installation, you can now install the new version. Note that `revdep-rebuild` is necessary for rebuilding packages linking against MySQL.

`root #``emerge -av ">mysql-4.1"`
Update your config files using dispatch-conf:

`root #````
dispatch-conf
```
`root #``revdep-rebuild`
Now configure the newly installed version of MySQL and restart the daemon:

`root #````
emerge --config =mysql-4.1.<micro_version>
```
`root #``/etc/init.d/mysql start`
Finally you can import the backup you have created during step #2.

Older mysqldump utilities may export tables in the wrong order when foreign keys are involved. To work around this problem, surround the SQL with the following statements:

Next, import the backup.

`root #````
cat BACKUP_MYSQL_4.0.SQL \
```
`root #````
 mysql \
--password='your_password' \
-hlocalhost \
--max_allowed_packet=16M
```
-uroot \

`root #````
mysql_fix_privilege_tables \
--user=root \
```
--defaults-file=/etc/mysql/my.cnf \

--password='your_password'
If you restart your MySQL daemon now and everything goes as expected, you have a fully working version of 4.1.x.

`root #``/etc/init.d/mysql restart`
If you encountered any problems during the upgrade process, please report them on [Bugzilla](https://bugs.gentoo.org) .

## Recover the old installation of MySQL 4.0

If you are not happy with MySQL 4.1, it's possible to go back to MySQL 4.0.

`root #````
/etc/init.d/mysql stop
```
`root #````
emerge -C mysql
```
`root #````
rm -rf /var/lib/mysql/ /var/log/mysql
```
`root #``emerge --usepkgonly "<mysql-4.1"`
Replace \<timestamp> with the one used when creating the backup:

`root #````
tar -xjpvf mysql.<timestamp>.tar.bz2 -C /
```
`root #``/etc/init.d/mysql start`
## On charset conversion:

### Introduction

This chapter is not intended to be an exhaustive guide on how to do such conversions, rather a short list of hints on which the reader can elaborate.

Converting a database may be a complex task and difficulty increases with data variancy. Things like serialized object and blobs are one example where it's difficult to keeps pieces together.

### Indexes

Every utf-8 character is considered 3 bytes long within an index. Indexes in MySQL can be up to 1000 bytes long (767 bytes for InnoDB tables). Note that the limits are measured in bytes, whereas the length of a column is interpreted as number of characters.

MySQL can also create indexes on parts of a column, this can be of some help. Below are some examples:

`user $``mysql -uroot -p'your_password' test``mysql>``SHOW variables LIKE "version" \G````
*************************** 1. row ***************************
Variable_name: version
    Value: 5.0.24-log
1 row in set (0.00 sec)
```
`mysql>````
CREATE TABLE t1 (
```
->   c1 varchar(255) NOT NULL default *,*
->   c2 varchar(255) NOT NULL default

->   ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.01 sec)

`mysql>````
ALTER TABLE t1
->   ADD INDEX idx1 ( c1 , c2 );
```
ERROR 1071 (42000): Specified key was too long; max key length is 1000 bytes

`mysql>````
ALTER TABLE t1
->   ADD INDEX idx1 ( c1(165) , c2(165) );
```
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

`mysql>````
CREATE TABLE t2 (
```
->   c1 varchar(255) NOT NULL default *,*
->   c2 varchar(255) NOT NULL default

->   ) ENGINE=MyISAM DEFAULT CHARSET=sjis;
Query OK, 0 rows affected (0.00 sec)

`mysql>````
ALTER TABLE t2
->   ADD INDEX idx1 ( c1(250) , c2(250) );
```
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

`mysql>````
CREATE TABLE t3 (
```
->   c1 varchar(255) NOT NULL default *,*
->   c2 varchar(255) NOT NULL default

->   ) ENGINE=MyISAM DEFAULT CHARSET=latin1;
Query OK, 0 rows affected (0.00 sec)

`mysql>````
ALTER TABLE t3
->   ADD INDEX idx1 ( c1 , c2 );
```
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

### Environment

The system must be configured to support the UTF-8 locale. You will find more information in the [UTF-8](https://wiki.gentoo.org/wiki/UTF-8) and [Localization Guide](https://wiki.gentoo.org/wiki/Localization/Guide) documents.

In this example, we set some shell environment variables to make use of the English UTF-8 locale in /etc/env.d/02locale :

**`/etc/env.d/02locale`**

Be sure to run `env-update && source /etc/profile` afterward.

### iconv

`iconv`, provided by`sys-libs/glibc` , is used to convert text files from one charset to another. The`app-text/recode` package can be used as well.

`user $``iconv -f ISO-8859-15 -t UTF-8 file1.sql > file2.sql`
From Japanese to utf8:

`user $``iconv -f ISO2022JP -t UTF-8 file1.sql > file2.sql`
`iconv` can be used to recode a sql dump even if the environment is not set to utf8.

### SQL Mangling

It's possible to use the `CONVERT()` and `CAST()` MySQL functions to convert data in your SQL scripts.

### Apache (webserver)

To use utf-8 with apache, you need to adjust the following variables in httpd.conf : AddDefaultCharset, CharsetDefault, CharsetSourceEnc. If your source html files aren't encoded in utf-8, they **must** be converted with `iconv` or `recode` .
