<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Postfix_to_Database | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Postfix to Database -->
---
title: Complete Virtual Mail Server/Postfix to Database
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Postfix_to_Database
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: e237ad2a7efb2b97
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Postfix to Database

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Installing Postfix

Postfix installation is quite straight forward. A basic installation instruction for [Postfix](https://wiki.gentoo.org/wiki/Postfix) should be followed, from its article.

## Linking Postfix

Once postfix is working on its most basic level, it's time to link it to the database.

### Gaining access

#### PostgreSQL

A database-user to query the database is required:

`user $``createuser -U postgres --pwprompt postfix`
Enter password for new role: $password
Enter it again: $password

Also, the postfix user needs some permissions on the tables:

`user $``psql -U postfixadmin postfix``postfix=>````
GRANT SELECT ON alias TO postfix;
```
`postfix=>````
GRANT SELECT ON domain TO postfix;
```
`postfix=>``GRANT SELECT ON mailbox TO postfix;`
#### MySQL

If MySQL is used instead of PostgreSQL, then use the following instructions. Fetch the [SQL dump](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Postfix_to_Database/Database_Dump) and save it as genericmailsql.sql as it will be needed soon.

First, install MySQL:

`root #``emerge --ask dev-db/mysql`
Create an initial database:

`root #``/usr/bin/mysql_install_db`
Follow the on-screen instructions for adding a root password within MySQL as otherwise the database will be wide open.

Start the MySQL database server and create the mailsql database:

`root #````
/etc/init.d/mysql start
```
`root #````
mysqladmin -u root -p create mailsql
```
`root #``mysql -u root -p mailsql < genericmailsql.sql`
Fire up the mysql client to grant the proper rights to the mailsql database:

`root #``mysql -u root -p mysql``mysql>``GRANT SELECT,INSERT,UPDATE,DELETE ON mailsql.* TO mailsql@localhost IDENTIFIED BY '$password';`
Query OK, 0 rows affected (0.02 sec)

`mysql>``FLUSH PRIVILEGES;`
Query OK, 0 rows affected (0.00 sec)

`mysql>``quit`
Verify that the new mailsql user can connect to the mysql server:

`root #``mysql -u mailsql -p mailsql`
The new database has default values and tables set up for two domains. The following tables are included:

- alias - local email alias and mailman alias information.
- relocated - relocated user email address maps
- transport - default mail transport information for all domains you are hosting
- users - all user account information
- virtual - virtual domain email alias maps

The values of the `virtid` uid and gid should be those of the `vmail` user and group:

### Querying

All these queries are tested on the database to ensure the queries are written properly. This does require however that the database has been filled to be useful. When postfixadmin is used, this can be done easily from the UI. Otherwise, simple INSERT SQL queries need to be written and executed.

#### PostgreSQL

Linking postfix to a database isn't that special, postfix just executes predefined SQL routines. These SQL queries will be stored in a file per query in a directory:

`root #``mkdir /etc/postfix/pgsql`
Be aware that any errors in the configuration information in these files can be pretty tricky to track down. Be meticulous with both the contents of the files and their names. Many errors in these files will be announced in the mail log such as "user example@example.com doesn't exist" without any further explanation.

**`/etc/postfix/pgsql/virtual_mailbox_domains.cf`**

**virtual\_mailbox\_domains**

```
# virtual_mailbox_domains.cf
user            = postfix
password        = $password
dbname          = postfix
#hosts          = localhost
query           = SELECT domain FROM domain WHERE domain = '%s' AND backupmx = '0' AND active = '1';
```
It's best to test the query on the database (using copy paste) to ensure no typo's exist in the query:

`root #``psql -U postfix postfix``postfix=>``SELECT domain FROM domain WHERE domain = 'example.com' AND backupmx = '0' AND active = '1';`
domain      
----------------------
 example.com
(1 row)

**`/etc/postfix/pgsql/virtual_mailbox_maps.cf`**

**virtual\_mailbox\_maps**

```
# virtual_mailbox_maps.cf
user            = postfix
password        = $password
dbname          = postfix
#hosts          = localhost
query           = SELECT maildir FROM mailbox WHERE local_part='%u' AND domain='%d' AND active='1';
```
Also here, it's best again to execute the query on the database:

`root #``psql -U postfix postfix``postfix=>``SELECT maildir FROM mailbox WHERE local_part='testuser' AND domain='example.com' AND active='1';`
maildir
-------------------
 example.com/testuser/
(1 row)

**`/etc/postfix/pgsql/virtual_alias_maps.cf`**

**virtual\_alias\_maps**

```
# virtual_alias_maps.cf
user            = postfix
password        = $password
dbname          = postfix
#hosts          = localhost
query           = SELECT goto FROM alias WHERE address='%s' AND active='1';
```
Run the query on the database to verify its output:

`root #``psql -U postfix postfix``postfix=>``SELECT goto FROM alias WHERE address='testuser@example.com' AND active='1';`
goto       
------------------
 testuser@example.com
(1 row)

#### MySQL

Set up the necessary configurations for postfix to interact with the database for all its other transport needs. Remember to replace each value with the name of the proper user, user id, password, alias, email address, and so on.

**`/etc/postfix/mysql-aliases.cf`**

```
user         = mailsql
password     = $password
dbname       = mailsql
table        = alias
select_field = destination
where_field  = alias
hosts        = unix:/var/run/mysqld/mysqld.sock
```
**`/etc/postfix/mysql-relocated.cf`**

```
user         = mailsql
password     = $password
dbname       = mailsql
table        = relocated
select_field = destination
where_field  = email
hosts        = unix:/var/run/mysqld/mysqld.sock
```
**`/etc/postfix/mysql-transport.cf`**

**(optional)**

```
user         = mailsql
password     = $password
dbname       = mailsql
table        = transport
select_field = destination
where_field  = domain
hosts        = unix:/var/run/mysqld/mysqld.sock
```
**`/etc/postfix/mysql-virtual-gid.cf`**

**(optional)**

```
user            = mailsql
password        = $password
dbname          = mailsql
table           = users
select_field    = gid
where_field     = email
additional_conditions = and postfix = 'y'
hosts           = unix:/var/run/mysqld/mysqld.sock
```
**`/etc/postfix/mysql-virtual-maps.cf`**

```
user            = mailsql
password        = $password
dbname          = mailsql
table           = users
select_field    = maildir
where_field     = email
additional_conditions = and postfix = 'y'
hosts           = unix:/var/run/mysqld/mysqld.sock
```
**`/etc/postfix/mysql-virtual-uid.cf`**

**(optional)**

```
user            = mailsql
password        = $password
dbname          = mailsql
table           = users
select_field    = uid
where_field     = email
additional_conditions = and postfix = 'y'
hosts           = unix:/var/run/mysqld/mysqld.sock
```
**`/etc/postfix/mysql-virtual.cf`**

```
user         = mailsql
password     = $password
dbname       = mailsql
table        = 'virtual'
select_field = destination
where_field  = email
hosts        = unix:/var/run/mysqld/mysqld.sock
```
### Access rights

Only postfix should have access rights to these files, as they contain passwords:

`root #````
chown root:postfix -R /etc/postfix/
```
`root #``find /etc/postfix/ -name "*.cf" -exec chmod -c 640 '{}' \+`
### Connecting postfix to the queries

#### Postgres

**`/etc/postfix/main.cf`**

**Connect postfix to postgres**

```
#
# Settings required to support virtual mail delivery using lookups in
# the Postgres database.
#
 
# A list of all virtual domains serviced by this instance of postfix.
virtual_mailbox_domains = pgsql:/etc/postfix/pgsql/virtual_mailbox_domains.cf
 
# Look up the mailbox location based on the email address received.
virtual_mailbox_maps = pgsql:/etc/postfix/pgsql/virtual_mailbox_maps.cf
 
# Any aliases that are supported by this system
virtual_alias_maps = pgsql:/etc/postfix/pgsql/virtual_alias_maps.cf
```
#### MySQL/MariaDB

**`/etc/postfix/main.cf`**

```
## (Ensure that there are no other alias_maps definitions)
alias_maps = mysql:/etc/postfix/mysql-aliases.cf
relocated_maps = mysql:/etc/postfix/mysql-relocated.cf
 
local_transport = local
local_recipient_maps = $alias_maps $virtual_mailbox_maps unix:passwd.byname
 
virtual_transport = virtual
## (The domains listed by the mydestination should not be listed in
##  the virtual_mailbox_domains parameter)
virtual_mailbox_domains = virt-domain.com, $other-virtual-domain.com
 
virtual_minimum_uid = 1000
## (Substitute $vmail-gid with the GID of the vmail group)
virtual_gid_maps = static:$vmail-gid
virtual_mailbox_maps = mysql:/etc/postfix/mysql-virtual-maps.cf
virtual_alias_maps = mysql:/etc/postfix/mysql-virtual.cf
## (Substitute $vmail-uid with the UID of the vmail user)
virtual_uid_maps = static:$vmail-uid
```
As of Postfix 2.0.x, there were a number of significant changes over the 1.1.x release. Notably the transport, virtual-gid, and virtual-uid tables are no longer necessary. The tables are still included to support potential different use cases.

### Testing the database connection

Postfix needs to be restarted to have it connect to the database:

`root #``/etc/init.d/postfix restart`
Check for any problems in /var/log/mail.log, such as incorrect permissions.

Using telnet, the mailserver can be tested again to check whether the database is properly used:

`user $``telnet localhost 25`
Trying 127.0.0.1...
Connected to foo.example.com.
Escape character is '^\]'.
220 foo.example.com ESMTP Postfix
mail from:me@you.com
250 2.1.0 Ok
rcpt to:testuser@example.com
250 2.1.5 Ok
data
354 End data with \<CR>\<LF>.\<CR>\<LF>
Testmail to ensure Postfix is still working.
.
250 2.0.0 Ok: queued as EA94F164C7
mail from:me@you.com
250 2.1.0 Ok
rcpt to:test.user@example.com
250 2.1.5 Ok
data
354 End data with \<CR>\<LF>.\<CR>\<LF>
Testmail to ensure Postfix is still really working.
.
250 2.0.0 Ok: queued as 3A276164C7
quit
221 2.0.0 Bye
Connection closed by foreign host.

**`/var/log/mail.log`**

**Verify test messages**

```
Mar 16 19:25:15 foo postfix/smtpd[32321]: connect from unknown[127.0.0.1]
Mar 16 19:25:32 foo postfix/smtpd[32321]: EA94F164C7: client=unknown[127.0.0.1]
Mar 16 19:25:42 foo postfix/cleanup[32330]: EA94F164C7: message-id=<>
Mar 16 19:25:42 foo postfix/qmgr[31681]: EA94F164C7: from=<me@you.com>, size=215, nrcpt=1 (queue active)
Mar 16 19:25:42 foo postfix/virtual[32332]: EA94F164C7: to=<testuser@example.com>, relay=virtual, delay=22, delays=22/0.02/0/0.05, dsn=2.0.0, status=sent (delivered to maildir)
Mar 16 19:25:42 foo postfix/qmgr[31681]: EA94F164C7: removed
Mar 16 19:26:05 foo postfix/smtpd[32321]: 3A276164C7: client=unknown[127.0.0.1]
Mar 16 19:26:14 foo postfix/cleanup[32330]: 3A276164C7: message-id=<>
Mar 16 19:26:14 foo postfix/qmgr[31681]: 3A276164C7: from=<me@you.com>, size=199, nrcpt=1 (queue active)
Mar 16 19:26:15 foo postfix/smtp[32338]: 3A276164C7: to=<test.user@example.com>, orig_to=<test.user@example.com>, relay=virtual, delay=23, delays=22/0.02/0.57/0.39, dsn=2.0.0, status=sent (delivered to maildir)
Mar 16 19:26:15 foo postfix/qmgr[31681]: 3A276164C7: removed
```
