<!-- source: https://wiki.gentoo.org/wiki/PostgreSQL/QuickStart | group: Gentoo Wiki (Main) | wiki-title: PostgreSQL/QuickStart -->
---
title: PostgreSQL/QuickStart
url: https://wiki.gentoo.org/wiki/PostgreSQL/QuickStart
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-25"
fingerprint: a188b94edde313d0
license: CC BY-SA 4.0
---

# PostgreSQL/QuickStart

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a quick start guide covering the installation and configuration of PostgreSQL. This guide is complementary to the official documentation; it does not mean to supplant it.

## Introduction

[PostgreSQL](https://www.postgresql.org/) is a free and open source relational database management system (RDBMS). It supports such things as transactions, schemata and foreign keys, and is often touted to adhere to the SQL standards more strictly and be more secure, by default, than any other database, commercial or otherwise.

Visit the [About](https://www.postgresql.org/about/) page on postgresql.org for more information.

### What this article will cover

This article will guide you through the Gentoo specific steps to install the PostgreSQL RDBMS.

The ebuild covered by this article is [dev-db/postgresql](https://packages.gentoo.org/packages/dev-db/postgresql).

### About the ebuilds

The PostgreSQL ebuilds in Portage feature slotting based on the major version. This allows you to have two major versions of PostgreSQL operating simultaneously; 9.0-9.4 libraries and servers can be installed and serve at the same time. This is useful in such circumstances where you need to move data from an older database to a new database, or need to have a production and a testing database on the same machine. Also, this prevents a database, corresponding libraries or executables from being overwritten by an incompatible update. That would require migration which is described in this guide.

Additionally, bug and security fixes, which are delivered via minor version updates, can be applied without fear of corrupting the database or the PostgreSQL installation itself; 9.3.4 can be updated to 9.3.5 as they are guaranteed to be compatible and require no more interaction from you than to emerge it and restart the server process neither migration, reconfiguration nor initialization are necessary.

Read the [PostgreSQL Versioning Policy](http://www.postgresql.org/support/versioning) for more information.

### What this article will not cover

There is quite a bit that will not be covered. The [official documentation](http://www.postgresql.org/docs/) is somewhere in the neighborhood of 2,000 pages. So, a lot of details will be left out in this quick start guide. Only Gentoo specific issues will be covered and some basic configuration guidelines.

## Installation

### USE flags


| [+icu](https://packages.gentoo.org/useflags/+icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [+lz4](https://packages.gentoo.org/useflags/+lz4) | Enable support for lz4 compression (as implemented in app-arch/lz4) | 
| [+numa](https://packages.gentoo.org/useflags/+numa) | Add support for NUMA | 
| [+readline](https://packages.gentoo.org/useflags/+readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [+server](https://packages.gentoo.org/useflags/+server) | Disable to build and install the clients and libraries only. | 
| [+uring](https://packages.gentoo.org/useflags/+uring) | Build with support for sys-libs/liburing AIO backend | 
| [+zstd](https://packages.gentoo.org/useflags/+zstd) | Enable support for ZSTD compression | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [llvm](https://packages.gentoo.org/useflags/llvm) | Add support for llvm JIT engine | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [oauth](https://packages.gentoo.org/useflags/oauth) | Enable OAuth2 authentication support | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [perl](https://packages.gentoo.org/useflags/perl) | Add optional support/bindings for the Perl language | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [tcl](https://packages.gentoo.org/useflags/tcl) | Add support the Tcl language | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [uuid](https://packages.gentoo.org/useflags/uuid) | Enable server side UUID generation (via dev-libs/ossp-uuid). | 
| [xml](https://packages.gentoo.org/useflags/xml) | Add support for XML files | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

Information on relevant USE flags:

- doc: Include the [online documentation](http://www.postgresql.org/docs/) to be stored on your system
- pg\_legacytimestamp: Use the older, floating-point method for formatting timestamps instead of the higher resolution 64-bit integer method. Unless you had a previous installation that utilized this deprecated method, leave this USE flag disabled. Flipping 'pg\_legacytimestamp' will require you to do a dump and restore if any of your databases utilize timestamps. The two methods are incompatible with each other.
- readline: You really want this enabled. Disabling removes command line editing and history in psql.
- selinux: This can only be enabled by using the SELinux profile.
- uuid: Include support to generate a 128 bit random unique identifier. This is useful for merging databases together so the chances of collisions become extremely low.

### Emerge

`root #``emerge --ask dev-db/postgresql`
\[ebuild N \] dev-db/postgresql-9.3.5 USE="doc -kerberos -ldap -pg\_legacytimestamp
  nls perl python -pg\_legacytimestamp (-selinux) readline ssl -tcl -threads
  -uuid -xml zlib" LINGUAS="-af -cs -de -es -fa -fr -hr -hu -it -ko -nb -pl
  -pt\_BR -ro -ru -sk -sl -sv -tr -zh\_CN -zh\_TW" 0 kB

You may receive a notice regarding that any of the above packages are blocked by any or all of the following packages: **dev-db/postgresql-libs**, **dev-db/postgresql-client**, or **dev-db/libpq**. These packages were **not maintained** and obsoleted (removed from the Gentoo ebuild repository). Refer to the section on migration from the previous ebuilds to the new ones to know how to handle this situation.

### Preparing to initialize the database cluster

Once the packages have finished emerging, you may want to edit /etc/conf.d/postgresql-9.3. There are three lines that effect the defaults of the server and **cannot** be changed later without deleting the directory that contains the database cluster and reinitializing.

*PGDATA* defines where to place the configuration files. *DATA\_DIR* defines where to create the database cluster and related files. *PG\_INITDB\_OPTS* may contain any [extra options](http://www.postgresql.org/docs/current/static/app-initdb.html) you would care to set. The extra options are **not** required as the reasonable defaults are, ahem, reasonable.

In the following example, *PGDATA* states that the configuration files are to be located in /etc/postgresql-9.3/ . *DATA\_DIR* states that the database cluster should be installed to /var/lib/postgresql/9.3/data/ , which is the default. If you decide to stray from the default, bear in mind that it is a **very good idea** to keep the major version in the path. *PG\_INITDB\_OPTS* states that the default locale should be *en\_US.UTF-8* . That is, U.S. English ordering and formatting, and UTF-8 character encoding.

**`/etc/conf.d/postgresql-9.3`**

**Example content**

```
# Location of configuration files
PGDATA="/etc/postgresql-9.3/"
# Where the data directory is located/to be created
DATA_DIR="/var/lib/postgresql/9.3/data"
# Additional options to pass to initdb.
# See 'man initdb' for available options.
PG_INITDB_OPTS="--locale=en_US.UTF-8"
```
There are six locale options that can be set to override *--locale=* . The following table lists the six options that, if used, are to be formatted as: `--option=lo_LO.ENCODING`.

| Option | Effects | 
|---|---|
| lc-collate | String sort order | 
| lc-ctype | Character classification (What is a letter? Its upper-case equivalent?) | 
| lc-messages | Language of messages | 
| lc-monetary | Formatting of currency amounts | 
| lc-numeric | Formatting of numbers | 
| lc-time | Formatting of dates and times | 

So, if you would like the default to be English, but you want messages in, say, Swedish, then your *PG\_INITDB\_OPTS* would look like so:

A complete list of language and character encodings supported by the server can be found in the documentation, but your system must also support the respective languages and character encodings. Compare the output of `locale -a` to the [encodings](http://www.postgresql.org/docs/current/static/multibyte.html) in the documentation.

You can change your locale and encoding selections at database [creation time.](http://www.postgresql.org/docs/current/static/sql-createdatabase.html) In order to change the locale for a database after you have created it, you must drop the database and start over again.

`root #``emerge --config dev-db/postgresql:9.3`
This will create the database cluster and store all the related server files into *PGDATA* and *DATA\_DIR* .

## Configuration

### Where the configuration files are located

Sample configuration files can be found in /usr/share/postgresql-9.3 (or whatever version), see the trouble shooting section for the script.

This time the focus is upon the files in the *PGDATA* directory, /etc/postgresql-9.3 , instead with primary focus on the postgresql.conf and pg\_hba.conf files.

### postgresql.conf

This is the main configuration file. The line that you may find of immediate interest is *listen\_addresses* . This variable defines to which addresses PostgreSQL will bind. By default, only localhost and the Unix socket are bound. Changing *listen\_addresses* is not enough to enable remote connections. That will be covered in the next section. The [official documentation](http://www.postgresql.org/docs/current/static/runtime-config.html) is fairly easy to understand and is exhaustive on all the settings available. It would behoove you to read that in addition to what is covered here as some things may change.

Of secondary interest is the logging destination. By default, everything is logged to postmaster.log in the *DATA\_DIR* directory. There is an entire subsection of postgresql.conf that covers a slew of options for how, what and where to log. The subsection is marked: ERROR REPORTING AND LOGGING.

Other than *listen\_addresses* and the logging options, the rest of the defaults in postgresql.conf are reasonable enough to get you going.

### pg\_hba.conf

The pg\_hba.conf file states who is allowed to connect to the database server and which authentication method must be used to establish the connection. Again, the documentation is quite exhaustive on the settings and what they all mean, but a few things are covered here for clarification.

**`pg_hba.conf`**

**Default settings**

As has been mentioned before, by default the server is secure. Kind of. There is only one database role that is available for log in by default: *postgres* . And, the only way to initiate a connection to the database is through the /run/postgresql/.s.PGSQL.5432 Unix socket, which is owned by the *postgres* system user and system group, or via localhost. Now for the "kind of" bit: Any user on the system can make a connection to the database through the localhost. Even as the *postgres* database superuser.

The *trust* method is fine for playing on a local system. If more than one user is working on your system, you will probably use *peer* (\[[https://www.postgresql.org/docs/current/auth-peer.html](https://www.postgresql.org/docs/current/auth-peer.html) | s. docu for more details).
For remote connection, use *scram-sha-256* and allow only hosts in your network.  
(The *trust* method is what allows any user to log on as any user without a password. It specifies just what it implies: Trust all connections for the given type to the given database from the given database user (but not the system user) from the given location without a password. This is what allows any user on the system to log on as any user through the localhost connection from the get go. This is not as dangerous as it seems, but does pose a serious security risk in most circumstances.)

The two methods you will most likely use are: *peer* for local connection (have a look to ident map) and *scram-sha-256* (*md5* is broken!  The password method only specifies that a password is required to start the connection and the password is sent "in-the-clear". This method is fine when such information will never leave the machine, such as connecting via the Unix socket or localhost. The md5 method is like password, but protects the password by using an md5 hash.)

At this point, this author would like to bring your attention to the last two lines, four lines including comments, of the pg\_hba.conf file. PostgreSQL has native support for IPv6 regardless of your desires for such support. Additionally, IPv4 addresses are automatically mapped to IPv6 addresses, *i.e.* , 127.0.0.1 will be mapped to ::FFFF:127.0.0.1 and as "pure" IPv6 ::FFFF:7F00:0001.

There seems to be some misunderstanding, though, as to how host names are mapped to IP addresses. Let us take a look at the /etc/hosts file.

**`/etc/hosts`**

From the example above you can see that both an IPv4 and an IPv6 IP address are mapped to localhost. When `psql` refers to this file, it will grab the first match and use that as the address; in this case 127.0.0.1. When PostgreSQL parses this, it will match the IPv6 formatted address as well, e.g. ::ffff:127.0.0.1. If, however, the IPv6 address appears first, then `psql` will map to ::1 alone; ::1 is not the same as ::ffff:127.0.0.1. As such, if you do not have ::1 as a permitted means of access, `psql` will not be able to establish a connection. Furthermore, your kernel needs to support the IPv6 protocol.

So, it is better to specify IP addresses alone to `psql` and in pg\_hba.conf rather than to rely on /etc/hosts to be ordered properly, and it removes any doubt as to which IP addresses are allowed or to which server you will connect.

## Starting the server

### Give it a go!

Now start PostgreSQL and set the password for the database superuser *postgres*.

Change 'trust' to 'password' for the 'host' (not the 'local', Unix domain socket) connections.

`root #``nano -w /etc/postgresql-9.3/pg_hba.conf`
Now start the database:

`root #``/etc/init.d/postgresql-9.3 start`
postgresql-9.3  | \* Starting PostgreSQL ...                             \[ ok \]

Or with systemd:

`root #``systemctl start postgresql-9.3`
Open a connection to the server and set the password:

`root #``psql -U postgres`
psql (9.3.5)
Type "help" for help.

`postgres=#``\password`
Enter new password:
Enter it again:

`postgres=#``\q`
Change 'trust' to 'password' for the local connection:

`root #``nano -w /etc/postgresql-9.3/pg_hba.conf`
Now have the database reload the configuration:

`root #``/etc/init.d/postgresql-9.3 reload`
postgresql-9.3 | \* Reloading PostgreSQL configuration ...               \[ ok \]

Or with systemd:

`root #``systemctl restart postgresql-9.3`
Finally, once everything works as it should, have PostgreSQL start at boot:

`root #``# rc-update add postgresql-9.3 default`
\* service postgresql-9.3 added to runlevel default

At this point you are ready to continue on with the official [PostgreSQL Tutorial](http://www.postgresql.org/docs/current/static/tutorial.html). The tutorial will guide you through creating roles, databases, schemata and all that fun and useful stuff.

## Migrating PostgreSQL

### When you need to migrate

Migration is required when you are moving from one major version to another, *e.g.* from PostgreSQL 17.6 to 18.1, (but not from 17.6 to 17.8). In this case you must migrate your data so that it is compatible with the new major version. The rest of this section will use a migration of postgres-17 (**old**) to postgres-18 (**new**) as an example.

### Perform the migration

Postgres provides a `pg_upgrade` utility which will do most of the work required for the migration. Always use the version of `pg_upgrade` from the **new** version of postgres. (In our example, postgres-18.)

Migration using `pg_upgrade` requires the **old** and the **new** binary files as well as the data files.

All steps should done as user *postgres*.

First do a check:

`user $````
/usr/lib64/postgresql-18/bin/pg_upgrade --check \
 --old-datadir '/var/lib/postgresql/17/data/' \
 --new-datadir '/var/lib/postgresql/18/data/' \
 --old-bindir '/usr/lib64/postgresql-17/bin/' \
 --new-bindir '/usr/lib64/postgresql-18/bin/'
```
If something looks wrong, you'll (usually) get a hint on what to do. See the troubleshooting guide below for common problems.

Otherwise you will get a message like the following.

\*Clusters are compatible\*

Now you can trigger the migration for real:

`user $````
/usr/lib64/postgresql-18/bin/pg_upgrade \
 --old-datadir '/var/lib/postgresql/17/data/' \
 --new-datadir '/var/lib/postgresql/18/data/' \
 --old-bindir '/usr/lib64/postgresql-17/bin/' \
 --new-bindir '/usr/lib64/postgresql-18/bin/'
```
```
Performing Consistency Checks
-----------------------------
Checking cluster versions                                     ok
Checking database user is the install user                    ok
Checking database connection settings                         ok
Checking for prepared transactions                            ok
Checking for system-defined composite types in user tables    ok
Checking for reg* data types in user tables                   ok
Checking for contrib/isn with bigint-passing mismatch         ok
Checking for incompatible "aclitem" data type in user tables  ok
Creating dump of global objects                               ok
Creating dump of database schemas
                                                              ok
Checking for presence of required libraries                   ok
Checking database user is the install user                    ok
Checking for prepared transactions                            ok
Checking for new cluster tablespace directories               ok
If pg_upgrade fails after this point, you must re-initdb the
new cluster before continuing.
Performing Upgrade
------------------
Setting locale and encoding for new cluster                   ok
Analyzing all rows in the new cluster                         ok
Freezing all rows in the new cluster                          ok
Deleting files from new pg_xact                               ok
Copying old pg_xact to new server                             ok
Setting oldest XID for new cluster                            ok
Setting next transaction ID and epoch for new cluster         ok
Deleting files from new pg_multixact/offsets                  ok
Copying old pg_multixact/offsets to new server                ok
Deleting files from new pg_multixact/members                  ok
Copying old pg_multixact/members to new server                ok
Setting next multixact ID and offset for new cluster          ok
Resetting WAL archives                                        ok
Setting frozenxid and minmxid counters in new cluster         ok
Restoring global objects in the new cluster                   ok
Restoring database schemas in the new cluster
                                                              ok
Copying user relation files
                                                              ok
Setting next OID for new cluster                              ok
Sync data directory to disk                                   ok
Creating script to delete old cluster                         ok
Checking for extension updates                                ok
Upgrade Complete
----------------
Optimizer statistics are not transferred by pg_upgrade.
Once you start the new server, consider running:
    /usr/lib64/postgresql-16/bin/vacuumdb --all --analyze-in-stages
Running this script will delete the old cluster's data files:
    ./delete_old_cluster.sh
```
There are more information about this tool in the [PostgreSQL documentation](https://www.postgresql.org/docs/current/pgupgrade.html).
(in particular, you will want to take a look at 'copy' or 'link')

#### Post migration steps

There are still a few things that should be done to complete the migration. (`pg_upgrade` writes a corresponding note at the end and generates the necessary scripts)

Update extensions:

`user $``psql -f update_extensions.sql`
No statistics are transferred during migration. It therefore makes sense to execute this command:

`user $``/usr/lib64/postgresql-18/bin/vacuumdb --all --analyze-in-stages`
Cleanup:

`user $` `./delete_old_cluster.sh` #### Troubleshooting

##### Old cluster uses data checksums but the new one does not

This means you have done the `initdb` or `emerge --config postgresql:15` without checksums enabled. You can fix this by enabling checksums on the **new** version:

`user $``pg_checksums18 --enable '/var/lib/postgresql/18/data'`
##### Old cluster does not use data checksums but the new one does

1. enable checksum on the **old** cluster: `pg_checksums17 --enable '/var/lib/postgresql/17/data'`

**OR**

1. remove checksum from the new cluster: `pg_checksums18 --disable '/var/lib/postgresql/18/data'`
2. do the migration
3. enable checksum on the new cluster: `pg_checksums18 --enable '/var/lib/postgresql/18/data'`

## Utilities

### pgAgent

[pgAgent](https://www.pgadmin.org/docs/pgadmin4/development/pgagent.html) is a job scheduling agent for PostgreSQL.
It is available as [dev-db/pgagent](https://packages.gentoo.org/packages/dev-db/pgagent).

Prior to pgAdmin v1.9, pgAgent shipped as part of pgAdmin.

### pgbouncer

[PgBouncer](https://www.pgbouncer.org/) is a connection pooling service. It is available as [dev-db/pgbouncer](https://packages.gentoo.org/packages/dev-db/pgbouncer).

Its main design goal is improving performance of short-lived connections.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Troubleshooting

### Obsolete ebuilds

If any of the following PostgreSQL ebuilds are installed on the system, then the system is using a very old, obsolete installation of PostgreSQL and should migrate now: **dev-db/postgresql-libs**, **dev-db/postgresql-client**, **dev-db/libpq**, and **dev-db/postgresql** older than 9.0.

The split ebuilds named **dev-db/postgresql-docs**, **dev-db/postgresql-base**, and/or **dev-db/postgresql-server** have been unified into a single atomic ebuild: [dev-db/postgresql](https://packages.gentoo.org/packages/dev-db/postgresql). Nothing is required to move from the split ebuilds to the unified ebuild other than to emerge the unified ebuild.

This article [does cover migrating](https://wiki.gentoo.org#Migrating_PostgreSQL) from the old ebuilds to the new ones.

### Server lacks instrumentation functions

This problem is easy to solve, with the solution depending on the version you are using. What is difficult about it is finding the answer. What is required is an import from a file that already exists on the storage drive: adminpack.sql . To resolve this issue, run one of the following commands appropriate to the version you have.

For PostgreSQL 9.0 and earlier:

`user $``psql -U postgres --file /usr/share/postgresql-9.0/contrib/adminpack.sql`
For PostgreSQL 9.1 and later:

`user $``psql -U postgres -c "CREATE EXTENSION adminpack;"`
### Missing config files in /etc/postgresql-9.x and /var/lib/postgresql/9.x/data

You can try to move the config files into the directory, where x is your postgresql version number, as follows:

`root #``cp /var/lib/postgresql/9.x/data/*.conf /etc/postgresql-9.x/`
If those files are missing you will need to initialize them.

First su into the postgres user:

`root #``su postgres`
Then, as the postgres user, run `initdb` and specify the data directory:

`user $``initdb -D /var/lib/postgresql/9.x/data/`
This will generate your configuration files and allow you to copy them over to /etc/postgresql-9.x/ as shown in the first command in this section.

### "ERROR: timezone directory stack overflow" or "FATAL: too many private dirs demanded"

This is caused by a symlink loop. To fix it, type the following:

`root #``rm /usr/share/zoneinfo/posix`
Keep in mind that any update to zoneinfo will recreate this symlink, thus requiring you to remove it again.

### systemd

Force the service to start on bootup:

`root #``systemctl enable postgresql-9.4`
To start service immediately:

`root #``systemctl start postgresql-9.4`
If an error occurs check if the /run/postgresql/ directory is there. If not, then create it like so:

`root #``systemd-tmpfiles --create`
Check the config file permissions:

`root #``ls -l /etc/postgresql-9.*/*``root #``chown postgres:postgres /etc/postgresql-9.*/*``root #``chmod 600 /etc/postgresql-9.*/*`
#### Service file and changes to it

Systemd service files (postgresql-@SLOT@-.service) can be found in /lib/systemd/system/:

example config change:

**`/lib/systemd/system/postgresql-@SLOT@.service`**

**Example config port change**

```
[Service]
Environment=PGPORT=5433
```
This will override the setting appearing in /lib/systemd/system/postgresql-@SLOT@.service.

(source: postgresql-10.service file)

#### RemoveIPC

PostgreSQL recommends in wiki.postgresql.org/wiki/Systemd to change the RemoveIPC setting to no in /etc/systemd/logind.conf:

**`/etc/systemd/logind.conf`**

**RemoveIPC**

```
RemoveIPC=no
```
(source: wiki.postgresql.org/wiki/Systemd)
