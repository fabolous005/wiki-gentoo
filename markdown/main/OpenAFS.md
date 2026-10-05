<!-- source: https://wiki.gentoo.org/wiki/OpenAFS | group: Gentoo Wiki (Main) | wiki-title: OpenAFS -->
---
title: OpenAFS
url: https://wiki.gentoo.org/wiki/OpenAFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-14"
fingerprint: f308f95b6d863fd6
license: CC BY-SA 4.0
---

# OpenAFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide shows you how to install an OpenAFS server and client on Gentoo Linux.

## Overview

### About this Document

This document provides you with all necessary steps to install an OpenAFS server on Gentoo Linux. Parts of this document are taken from the AFS FAQ and IBM's Quick Beginnings guide on AFS. Well, never reinvent the wheel. :)

### What is AFS?

AFS is a distributed filesystem that enables co-operating hosts (clients and servers) to efficiently share filesystem resources across both local area and wide area networks. Clients hold a cache for often used objects (files), to get quicker access to them.

AFS is based on a distributed file system originally developed at the Information Technology Center at Carnegie-Mellon University that was called the "Andrew File System". "Andrew" was the name of the research project at CMU - honouring the founders of the University. Once Transarc was formed and AFS became a product, the "Andrew" was dropped to indicate that AFS had gone beyond the Andrew research project and had become a supported, product quality filesystem. However, there were a number of existing cells that rooted their filesystem as /afs. At the time, changing the root of the filesystem was a non-trivial undertaking. So, to save the early AFS sites from having to rename their filesystem, AFS remained as the name and filesystem root.

### What is an AFS cell?

An AFS cell is a collection of servers grouped together administratively and presenting a single, cohesive filesystem. Typically, an AFS cell is a set of hosts that use the same Internet domain name (for example, gentoo.org) Users log into AFS client workstations which request information and files from the cell's servers on behalf of the users. Users won't know on which server a file which they are accessing, is located. They even won't notice if a server will be located to another room, since every volume can be replicated and moved to another server without any user noticing. The files are always accessible. Well, it's like NFS on steroids :)

### What are the benefits of using AFS?

The main strengths of AFS are its: caching facility (on client side, typically 100M to 1GB), security features (Kerberos 5 based, access control lists), simplicity of addressing (you just have one filesystem), scalability (add further servers to your cell as needed), communications protocol.

### Where can I get more information?

Read the [AFS FAQ](http://www.angelfire.com/hi/plutonic/afs-faq.html) .

OpenAFS main page is at [www.openafs.org](http://www.openafs.org) .

AFS was originally developed by Transarc which is now owned by IBM. Since April 2005, it has been withdrawn from IBM's product catalogue. IBM branched the source of the AFS product, and made a copy of the source available for community development and maintenance in 2000. They called the release OpenAFS.

### How Can I Debug Problems?

OpenAFS has great logging facilities. However, by default it logs straight into its own logs instead of through the system logging facilities you have on your system. To have the servers log through your system logger, use the `-syslog` option for all `bos` commands.

## Upgrading from previous versions

### Introduction

This section aims to help you through the process of upgrading an existing OpenAFS installation to OpenAFS version 1.4.0 or higher (or 1.2.x starting from 1.2.13. The latter will not be handled specifically, as most people will want 1.4 for a.o. linux-2.6 support, large file support and bug fixes).

If you're dealing with a clean install of a 1.4 version of OpenAFS, then you can safely skip this chapter. However, if you're upgrading from a previous version, we strongly urge you to follow the guidelines in the next sections. The transition script in the ebuild is designed to assist you in quickly upgrading and restarting. Please note that it will (for safety reasons) not delete configuration files and startup scripts in old places, not automatically change your boot configuration to use the new scripts, etc. If you need further convincing, using an old OpenAFS kernel module together with the updated system binaries, may very well cause your kernel to freak out. So, let's read on for a clean and easy transition, shall we?





### Differences to previous versions

Traditionally, OpenAFS has used the same path-conventions that IBM TransArc labs had used, before the code was forked. Understandably, old AFS setups continue using these legacy path conventions. More recent setups conform with [FHS](https://wiki.gentoo.org/wiki/Filesystem_Hierarchy_Standard) by using standard locations (as seen in many Linux distributions). The following table is a compilation of the configure-script and the README accompanying the OpenAFS distribution tarballs:

| Directory | Purpose | Transarc Mode | Default Mode | translation to Gentoo | 
|---|---|---|---|---|
| viceetcdir | Client configuration | /usr/vice/etc | $(sysconfdir)/openafs | /etc/openafs | 
| unnamed | Client binaries | unspecified | $(bindir) | /usr/bin | 
| afsconfdir | Server configuration | /usr/afs/etc | $(sysconfdir)/openafs/server | /etc/openafs/server | 
| afssrvdir | Internal server binaries | /usr/afs/bin (servers) | $(libexecdir)/openafs | /usr/libexec/openafs | 
| afslocaldir | Server state | /usr/afs/local | $(localstatedir)/openafs | /var/lib/openafs | 
| afsdbdir | Auth/serverlist/... databases | /usr/afs/db | $(localstatedir)/openafs/db | /var/lib/openafs/db | 
| afslogdir | Log files | /usr/afs/logs | $(localstatedir)/openafs/logs | /var/lib/openafs/logs | 
| afsbosconfig | Overseer config | $(afslocaldir)/BosConfig | $(afsconfdir)/BosConfig | /etc/openafs/BosConfig | 

There are some other oddities, like binaries being put in /usr/vice/etc in Transarc mode, but this list is not intended to be comprehensive. It is rather meant to serve as a reference to those troubleshooting config file transition.

Also as a result of the path changes, the default disk cache location has been changed from /usr/vice/cache to /var/cache/openafs . Please note, however, that the directory in this path, /var/cache/openafs, is not created by the ebuild. You will need to create it yourself.

Furthermore, the init-script has been split into a client and a server part. You used to have /etc/init.d/afs , but now you'll end up with both /etc/init.d/openafs-client and /etc/init.d/openafs-server . Consequently, the configuration file /etc/conf.d/afs has been split into /etc/conf.d/openafs-client and /etc/conf.d/openafs-server . Also, options in /etc/conf.d/afs to turn either client or server on or off have been obsoleted.

Another change to the init script is that it doesn't check your disk cache setup anymore. The old code required that a separate ext2 partition be mounted at /usr/vice/cache . There were some problems with that:

- Though it's a very logical setup, your cache doesn't need to be on a separate partition. As long as you make sure that the amount of space specified in /etc/openafs/cacheinfo really is available for disk cache usage, you're safe. So there is no real problem with having the cache on your root partition.
- Some people use soft-links to point to the real disk cache location. The init script didn't like this, because then this cache location didn't turn up in /proc/mounts .
- Many prefer ext3 over ext2 nowadays. Both filesystems are valid for usage as a disk cache. Any other filesystem is unsupported (like: don't try reiserfs, you'll get a huge warning, expect failure afterwards).

### Transition to the new paths

First of all, emerging a newer OpenAFS version should not overwrite any old configuration files. The script is designed to not change any files already present on the system. So even if you have a totally messed up configuration with a mix of old and new locations, the script should not cause further problems. Also, if a running OpenAFS server is detected, the installation will abort, preventing possible database corruption.

One caveat though -- there have been ebuilds floating around the internet that partially disable the protection that Gentoo puts on /etc . These ebuilds have never been distributed by Gentoo. You might want to check the `CONFIG_PROTECT_MASK` variable in the output of the following command:

`root #``emerge info | grep "CONFIG_PROTECT_MASK"`
CONFIG\_PROTECT\_MASK="/etc/gconf /etc/terminfo /etc/texmf/web2c /etc/env.d"

Though nothing in this ebuild would touch the files in /etc/afs , upgrading will cause the removal of your older OpenAFS installation. Files in `CONFIG_PROTECT_MASK` that belong to the older installation will be removed as well.

It should be clear to the experienced user that in the case he has tweaked their system by manually adding soft links (e.g. /usr/afs/etc to /etc/openafs ), the new installation may run fine while still using the old configuration files. In this case, there has been no real transition, and cleaning up the old installation will result in a broken OpenAFS config.

Now that you know what doesn't happen, you may want to know what does:

- /usr/afs/etc is copied to /etc/openafs/server
- /usr/vice/etc is copied to /etc/openafs
- /usr/afs/local is copied to /var/lib/openafs
- /usr/afs/local/BosConfig is copied to /etc/openafs/BosConfig , while replacing occurrences of /usr/afs/bin/ with /usr/libexec/openafs , /usr/afs/etc with /etc/openafs/server and /usr/afs/bin (without the / as previously) with /usr/bin
- /usr/afs/db is copied to /var/lib/openafs/db
- The configuration file /etc/conf.d/afs is copied to /etc/conf.d/openafs-client , as all known old options were destined for client usage only.

### The upgrade itself

So you haven't got an OpenAFS server setup? Or maybe you do, the previous sections have informed you about what is going to happen, and you're still ready for it?

Let's go ahead with it then!

If you do have a server running, you want to shut it down now.

`root #``rc-service afs stop`
And then the upgrade itself.

`root #``emerge --ask openafs`
### Restarting OpenAFS

If you had an OpenAFS server running, you would have not have been forced to shut it down. Now is the time to do that.

`root #``rc-service afs stop`
As you may want keep the downtime to a minimum, so you can restart your OpenAFS server right away.

`root #``rc-service openafs-server start`
You can check whether it's running properly with the following command:

`root #``/usr/bin/bos status localhost -localauth`
Before starting the OpenAFS client again, please take time to check your cache settings. They are determined by /etc/openafs/cacheinfo . To restart your OpenAFS client installation, please type the following:

`root #``rc-service openafs-client start`
### Cleaning up afterwards

Before cleaning up, please make really sure that everything runs smoothly and that you have restarted after the upgrade (otherwise, you may still be running your old installation).

The following directories may be safely removed from the system:

- /etc/afs
- /usr/vice
- /usr/afs
- /usr/afsws

The following files are also unnecessary:

- /etc/init.d/afs
- /etc/conf.d/afs

`root #````
tar czf /root/oldafs-backup.tgz /etc/afs /usr/vice /usr/afs /usr/afsws
```
`root #````
rm -R /etc/afs /usr/vice /usr/afs /usr/afsws
```
`root #``rm /etc/init.d/afs /etc/conf.d/afs`
In case you've previously used ebuilds =openafs-1.2.13 or =openafs-1.3.85, you may also have some other unnecessary files:

- /etc/init.d/afs-client
- /etc/init.d/afs-server
- /etc/conf.d/afs-client
- /etc/conf.d/afs-server

### Init Script changes

Now most people would have their systems configured to automatically start the OpenAFS client and server on startup. Those who don't can safely skip this section. If you had your system configured to start them automatically, you will need to re-enable this, because the names of the init scripts have changed.

`root #````
rc-update del afs default
```
`root #````
rc-update add openafs-client default
```
`root #``rc-update add openafs-server default`
If you had `=openafs-1.2.13` or `=openafs-1.3.85` , you should remove afs-client and afs-server from the default runlevel, instead of afs .

### Troubleshooting: what if the automatic upgrade fails

Don't panic. You shouldn't have lost any data or configuration files. So let's analyze the situation. Please file a bug at [bugs.gentoo.org](http://bugs.gentoo.org) in any case, preferably with as much information as possible.

If you're having problems starting the client, this should help you diagnosing the problem:

- Run `dmesg` . The client normally sends error messages there.
- Check /etc/openafs/cacheinfo . It should be of the form: /afs:{path to disk cache}:{number of blocks for disk cache}. Normally, your disk cache will be located at /var/cache/openafs .
- Check the output of `lsmod` . You will want to see a line beginning with the word openafs.
- `pgrep afsd` will tell you whether afsd is running or not
- `cat /proc/mounts` should reveal whether /afs has been mounted.

If you're having problems starting the server, then these hints may be useful:

- `pgrep bosserver` tells you whether the overseer is running or not. If you have more than one overseer running, then something has gone wrong. In that case, you should try a graceful OpenAFS server shutdown with `bos shutdown localhost -localauth -wait` , check the result with `bos status localhost -localauth` , kill all remaining overseer processes and then finally check whether any server processes are still running ( `ls /usr/libexec/openafs` to get a list of them). Afterwards, do `rc-service openafs-server zap` to reset the status of the server and `rc-service openafs-server start` to try launching it again.
- If you're using OpenAFS' own logging system (which is the default setting), check out /var/lib/openafs/logs/\* . If you're using the syslog service, go check out its logs for any useful information.

## Documentation

### Getting AFS Documentation

You can get the original IBM AFS Documentation. It is very well written and you really want read it if it is up to you to administer a AFS Server.

`root #``emerge --ask app-doc/afsdoc`
You also have the option of using the documentation delivered with OpenAFS. It is installed when you have the USE flag `doc` enabled while emerging OpenAFS. It can be found in /usr/share/doc/openafs-\*/ . At the time of writing, this documentation was a work in progress. It may however document newer features in OpenAFS that aren't described in the original IBM AFS Documentation.

## Client Installation

### Building the Client

`root #``emerge --ask net-fs/openafs`
After successful compilation you're ready to go.

### A simple global-browsing client installation

If you're not part of a specific OpenAFS-cell you want to access, and you just want to try browsing globally available OpenAFS-shares, then you can just install OpenAFS, not touch the configuration at all, and start openafs-client with rc-service .

### Accessing a specific OpenAFS cell

If you need to access a specific cell, say your university's or company's own cell, then some adjustments to your configuration have to be made.

Firstly, you need to update /etc/openafs/CellServDB with the database servers for your cell. This information is normally provided by your administrator.

Secondly, in order to be able to log onto the OpenAFS cell, you need to specify its name in /etc/openafs/ThisCell .



CellServDB tells your client which server(s) it needs to contact for a specific cell. ThisCell should be quite obvious. Normally you use a name which is unique for your organisation. Your (official) domain might be a good choice.

For a quick start, you can now start openafs-client with rc-service and use `kinit; aklog` to authenticate yourself and start using your access to the cell. For automatic logons to you cell, you want to consult the appropriate section below.

### Adjusting the cache

You can house your cache on an existing filesystem (if it's ext2/3), or you may want to have a separate partition for that. The default location of the cache is /var/cache/openafs , but you can change that by editing /etc/openafs/cacheinfo . A standard size for your cache is 200MB, but more won't hurt.

### Starting AFS on startup

The following command will create the appropriate links to start your afs client on system startup.

`root #``rc-update add openafs-client default`
## Server Installation

### Installing the Kerberos Server

OpenAFS requires Kerberos 5 for authentication. The following shows how to install the MIT Kerberos server. Alternatively, the Heimdal kerberos implementation may be used.

Install the MIT Kerberos server binaries with the following command:

`root #``emerge --ask mit-krb5`
Edit the /etc/krb5.conf and /etc/kdc.conf configuration files. Replace the EXAMPLE.COM realm name with your realm name, and update the example hostnames with your actual hostnames.

Create the Kerberos database like so:

`root #``mkdir /etc/krb5kdc``root #``kdb5_util create -s`
### Building the Server

If you haven't already done so, the following command will install all necessary binaries for setting up an AFS Server *and* Client.

`root #``emerge --ask net-fs/openafs`
### Keying the Server

As of OpenAFS version 1.6.5, the OpenAFS servers support strong crypto (AES, etc.) for the service key, and will read the Kerberos keytab file directly. Create the Kerberos service key for OpenAFS and export it to a keytab for the OpenAFS server processes, before starting the OpenAFS services.

`root #``kadmin.local -q "addprinc -randkey afs/<cellname>"``root #``kadmin.local -q "ktadd -k /etc/openafs/server/rxkad.keytab afs/<cellname>"`
### Starting AFS Server

You need to run the `bosserver` command to initialize the Basic OverSeer (BOS) Server, which monitors and controls other AFS server processes on its server machine. Think of it as init for the system.

Start the OpenAFS `bosserver`.

`root #``rc-service openafs-server start`
Ensure the OpenAFS servers start on reboot:

`root #``rc-update add openafs-server default`
Verify that the BOS Server created /etc/openafs/server/CellServDB and /etc/openafs/server/ThisCell:

`root #``ls -al /etc/openafs/server/`
-rw-r--r--    1 root     root           41 Jun  4 22:21 CellServDB
-rw-r--r--    1 root     root            7 Jun  4 22:21 ThisCell

### Defining Cell Name for Server Processes

Now assign your cell's name.

Run the `bos setcellname` command to set the cell name:

`root #``bos setcellname localhost CELL_NAME -localauth`
### Starting the Database Server Process

Next use the `bos create` command to create entries for the three database server processes in the /etc/openafs/BosConfig file. The three processes run on database server machines only.

| Process | Description | 
|---|---|
| buserver | The Backup Server maintains the Backup Database | 
| ptserver | The Protection Server maintains the Protection Database | 
| vlserver | The Volume Location Server maintains the Volume Location Database (VLDB). Very important :) | 

`root #````
bos create localhost buserver simple /usr/libexec/openafs/buserver -cell CELL_NAME -localauth
```
`root #````
bos create localhost ptserver simple /usr/libexec/openafs/ptserver -cell CELL_NAME -localauth
```
`root #``bos create localhost vlserver simple /usr/libexec/openafs/vlserver -cell CELL_NAME -localauth`
You can verify that all servers are running with the `bos status` command:

`root #``bos status localhost -localauth`
Instance buserver, currently running normally.
Instance ptserver, currently running normally.
Instance vlserver, currently running normally.

### Starting the first File Server, Volume Server and Salvager

Start the `fs` process, which consists of the File Server, Volume Server and Salvager (fileserver, volserver and salvager processes).

`root #``bos create localhost fs fs /usr/libexec/openafs/fileserver /usr/libexec/openafs/volserver /usr/libexec/openafs/salvager -localauth`
Verify that all processes are running:

`root #``bos status localhost -long -localauth`
Instance buserver, (type is simple) currently running normally.
Process last started at Mon Jun  4 21:07:17 2001 (2 proc starts)
Last exit at Mon Jun  4 21:07:17 2001
Command 1 is '/usr/libexec/openafs/buserver'
  
Instance ptserver, (type is simple) currently running normally.
Process last started at Mon Jun  4 21:07:17 2001 (2 proc starts)
Last exit at Mon Jun  4 21:07:17 2001
Command 1 is '/usr/libexec/openafs/ptserver'
  
Instance vlserver, (type is simple) currently running normally.
Process last started at Mon Jun  4 21:07:17 2001 (2 proc starts)
Last exit at Mon Jun  4 21:07:17 2001
Command 1 is '/usr/libexec/openafs/vlserver'
  
Instance fs, (type is fs) currently running normally.
Auxiliary status is: file server running.
Process last started at Mon Jun  4 21:09:30 2001 (2 proc starts)
Command 1 is '/usr/libexec/openafs/fileserver'
Command 2 is '/usr/libexec/openafs/volserver'
Command 3 is '/usr/libexec/openafs/salvager'

Your next action depends on whether you have ever run AFS file server machines in the cell.

If you are installing the first AFS Server ever in the cell, create the first AFS volume, **root.afs**

`root #``vos create localhost PARTITION_NAME root.afs -localauth`
If there are existing AFS file server machines and volumes in the cell issue the `vos sncvldb` and `vos syncserv` commands to synchronize the VLDB (Volume Location Database) with the actual state of volumes on the local machine. This will copy all necessary data to your new server.

If the command fails with the message "partition /vicepa does not exist on the server", ensure that the partition is mounted before running OpenAFS servers, or mount the directory and restart the processes using `bos restart localhost -all -cell CELL_NAME -localauth` .

`root #````
vos syncvldb localhost -verbose -localauth
```
`root #``vos syncserv localhost -verbose -localauth`
### Starting the Server Portion of the Update Server

`root #``bos create localhost upserver simple "/usr/libexec/openafs/upserver -crypt /etc/openafs/server -clear /usr/libexec/openafs" -localauth`
### Creating the first Administrative Account

An administrative account is needed to complete the cell setup and perform on going administration. The first account must be created directly on the servers. Additional accounts may then be created without direct ssh access to the servers.

Four tasks need to be done to create the first administrative account.

- a Kerberos principal, by convention, in the form of USERNAME/admin
- an AFS user, by convention, the form of USERNAME.admin
- membership in the built-in AFS system::administrators group
- membership in the OpenAFS superuser list

Create the Kerberos principal. Run this following command on the Kerberos server, as root:

`root #``kadmin.local -q "addprinc USERNAME/admin"`
Create the AFS admin user. Run this command on the OpenAFS database server, as root:

`root #``pts createuser USERNAME.admin -localauth`
Add the AFS admin user to the built-in admin group. Run this command on the OpenAFS database server, as root:

`root #``pts adduser USERNAME.admin system:administrators -localauth`
Add the AFS admin user to the superuser list. Run this command on each OpenAFS server, as root:

`root #``bos adduser localhost USERNAME.admin -localauth`
### Configuring the Top Level of the AFS filespace

At this point the server configuration is complete. You will need a running AFS client to set up the top level directories in AFS and grant access rights to them. This client does not need to be installed on the OpenAFS server. You will need to obtain your administrative credentials. Root access is not required for the commands in this section.

First, obtain your administrative credentials:

`user $``kinit USERNAME/admin`
Password for USERNAME/admin@REALM: \*\*\*\*\*\*\*\*

`user $````
aklog
```
`user $``tokens`
Tokens held by the Cache Manager:
 
User's (AFS ID 1) tokens for afs@mycellname.com \[Expires Oct 21 20:26\]
   --End of list--

First you need to set some ACLs, so that any user can lookup /afs .

`user $``fs setacl /afs/.:mount/CELL_NAME:root.afs/. system:anyuser rl`
Then you need to create the root volume, mount it readonly on /afs/\<cell name> and read/write on /afs/.\<cell name> .

`user $````
vos create SERVER_NAME PARTITION_NAME root.cell
```
`user $````
fs mkmount /afs/.:mount/CELL_NAME:root.afs/CELL_NAME root.cell
```
`user $````
fs setacl /afs/.:mount/CELL_NAME:root.afs/CELL_NAME system:anyuser rl
```
`user $``fs mkmount /afs/.:mount/CELL_NAME:root.afs/.CELL_NAME root.cell -rw`

At this point, you can create volumes for your new AFS site and add them to the filespace. Users and groups should be created and directory ACLs setup to allow users to create files and directories. To create and mount a volume:

`user $````
vos create SERVER_NAME PARTITION_NAME VOLUME_NAME
```
`user $````
fs mkmount /afs/CELL_NAME/MOUNT_POINT VOLUME_NAME
```
`user $````
fs mkmount /afs/CELL_NAME/.MOUNT_POINT VOLUME_NAME -rw
```
`user $``fs setquota /afs/CELL_NAME/.MOUNT_POINT -max QUOTUM`
Finally you're done!!! You should now have a working AFS file server on your local network. Time to get a big cup of coffee and print out the AFS documentation!!!

## Basic Administration

### Disclaimer

OpenAFS is an extensive technology. Please read the AFS documentation for more information. We only list a few administrative tasks in this chapter.

### Configuring PAM to Acquire an AFS Token on Login

To use AFS you need to authenticate against the Kerberos 5 KDC (MIT, Heimdal, ShiShi Kerberos 5, or Microsoft Active Directory). However in order to login to a machine you will also need a user account, this can be local in /etc/passwd , NIS, LDAP (OpenLDAP), or a Hesiod database. PAM allows Gentoo to tie the authentication against AFS and login to the user account.

You will need to update /etc/pam.d/system-auth which is used by the other configurations. "use\_first\_pass" indicates it will be checked first against the user login, and "ignore\_root" stops the local superuser being checked so as to order to allow login if AFS or the network fails.

**`/etc/pam.d/system-auth`**

In order for `sudo` to keep the real user's token and to prevent local users gaining AFS access change /etc/pam.d/su as follows:

**`/etc/pam.d/su`**
