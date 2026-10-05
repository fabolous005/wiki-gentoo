<!-- source: https://wiki.gentoo.org/wiki/PostgreSQL | group: Gentoo Wiki (Main) | wiki-title: PostgreSQL -->
---
title: PostgreSQL
url: https://wiki.gentoo.org/wiki/PostgreSQL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-08"
fingerprint: "7ddf297095c822c6"
license: CC BY-SA 4.0
---

# PostgreSQL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)




**PostgreSQL** is a free and open source relational database management system (RDBMS). It supports such things as transactions, schemata and foreign keys, and is often touted to more strictly adhere to the SQL standards and to be more secure, by default, than any other database, commercial or otherwise.

Visit the [About](https://www.postgresql.org/about/) page on postgresql.org for more information.

## Installation, Upgrading, and Migration

See [Gentoo PostgreSQL Quick Start Guide](https://wiki.gentoo.org/wiki/PostgreSQL/QuickStart) for details about installing, upgrading, or migrating PostgreSQL.

## User managment

### Show existing users

`user $``psql -U postgres -d postgres````
psql (16.1)
Type "help" for help.
\du
                             List of roles
 Role name |                         Attributes                         
-----------+------------------------------------------------------------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS
```


### Adding a User

Create a new user account (called user1) and assign a password:

`user $``psql -U postgres -d postgres`
psql (16.1)
Type "help" for help.
postgres=# CREATE ROLE user1 WITH LOGIN;
CREATE ROLE
postgres=# \password user1
Enter new password: 
Enter it again:

See [Chapter 20. Database Roles](https://www.postgresql.org/docs/current/user-manag.html) of the official PostgreSQL documentation for more on role management.

## database managment

### Show existing databases

`user $``psql -U postgres -d postgres -l` ```
                                                        List of databases
     Name     |   Owner   | Encoding | Locale Provider |  Collate   |   Ctype    | ICU Locale | ICU Rules |    Access privileges    
--------------+-----------+----------+-----------------+------------+------------+------------+-----------+-------------------------
 postgres     | postgres  | UTF8     | libc            | de_DE.utf8 | de_DE.utf8 |            |           | postgres=CTc/postgres  +
              |           |          |                 |            |            |            |           | =T/postgres            
 template0    | postgres  | UTF8     | libc            | de_DE.utf8 | de_DE.utf8 |            |           | =c/postgres            +
              |           |          |                 |            |            |            |           | postgres=CTc/postgres
 template1    | postgres  | UTF8     | libc            | de_DE.utf8 | de_DE.utf8 |            |           | postgres=CTc/postgres  +
              |           |          |                 |            |            |            |           | =c/postgres
(3 rows)
\q
```
**OR**

`user $``psql -U postgres -d postgres````
psql (16.1)
Type "help" for help.
\l
     Name     |   Owner   | Encoding | Locale Provider |  Collate   |   Ctype    | ICU Locale | ICU Rules |    Access privileges    
--------------+-----------+----------+-----------------+------------+------------+------------+-----------+-------------------------
 postgres     | postgres  | UTF8     | libc            | de_DE.utf8 | de_DE.utf8 |            |           | postgres=CTc/postgres  +
              |           |          |                 |            |            |            |           | =T/postgres            
 template0    | postgres  | UTF8     | libc            | de_DE.utf8 | de_DE.utf8 |            |           | =c/postgres            +
              |           |          |                 |            |            |            |           | postgres=CTc/postgres
 template1    | postgres  | UTF8     | libc            | de_DE.utf8 | de_DE.utf8 |            |           | postgres=CTc/postgres  +
              |           |          |                 |            |            |            |           | =c/postgres
(3 rows)
\q
```
### Create a new database

user need the right to do this.

`user $``psql -U postgres -d postgres`
psql (16.1)
Type "help" for help.
postgres=# CREATE DATABASE otherdb WITH OWNER user1; -- username has all privileges on otherdb
CREATE DATABASE
-- connect to the new database otherdb as user1
postgres=# \c otherdb user1
You are now connected to database "otherdb" as user "user1".
\q

### Give a other user rights on the new database

(user1 should exist. see above)

connect to the new database as user1 (owner):

`user $` `psql -U user1 -d otherdb`
psql16 (16.1)
Type "help" for help.
otherdb=# GRANT CONNECT ON DATABASE otherdb TO user1; -- user1 can now connect to otherdb
GRANT
otherdb=# GRANT SELECT ON test TO user1; -- user1 can now query (SELECT statements) the test table on otherdb.
GRANT

See the PostgreSQL documentation for more on [GRANT](https://www.postgresql.org/docs/current/sql-grant.html) and [REVOKE](https://www.postgresql.org/docs/current/sql-revoke.html),

### Changing the default encoding for new databases

When creating a new database (e.g. with `createdb mydb` or `CREATE DATABASE`) PostgreSQL actually copies a template database. There are two predefined templates: template0 is vanilla, while template1 is meant as an on-site template changeable by the administrator and is used by default. In order to change the default encoding of new databases, one of the options is to change on-site template1. To do this, log into PostgreSQL shell (`psql`) and execute the following:

1\. First we need to drop template1. As templates cannot be dropped, we first need to change it to an ordinary database:

UPDATE pg\_database SET datistemplate = FALSE WHERE datname = 'template1';

2\. After that, it is possible to drop it:

DROP DATABASE template1;

3\. The next step is to create a new database from template0 with a new default encoding. (Gotcha: In PostgreSQL, Unicode is synonymous with UTF-8.)

CREATE DATABASE template1 WITH TEMPLATE = template0 ENCODING = 'UNICODE';

4\. Now we need to change template1 back to the template:

UPDATE pg\_database SET datistemplate = TRUE WHERE datname = 'template1';

5\. (OPTIONAL) If you do not want anyone connecting to this template, set datallowconn to FALSE:

UPDATE pg\_database SET datallowconn = FALSE WHERE datname = 'template1';

Now you can create a new database by running from regular shell:

`user $``createdb -U postgres testdb`
If you log in back to psql and check the databases, you should see the proper encoding of your new database:

`user $``psql -U postgres -d postgres````
psql (9.1.1)
Type "help" for help.
postgres=# \l
                              List of databases
  Name    |  Owner   | Encoding  | Collation | Ctype |   Access privileges
----------+----------+-----------+-----------+-------+----------------------
testdb    | postgres | UTF8      | C         | C     |
postgres  | postgres | SQL_ASCII | C         | C     |
template0 | postgres | SQL_ASCII | C         | C     | =c/postgres
                                                     : postgres=CTc/postgres
template1 | postgres | UTF8      | C         | C     |
```
