<!-- source: https://wiki.gentoo.org/wiki/Nfs-utils | group: Gentoo Wiki (Main) | wiki-title: Nfs-utils -->
---
title: nfs-utils
url: https://wiki.gentoo.org/wiki/Nfs-utils
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-22"
fingerprint: fe19b15bb3067bc5
license: CC BY-SA 4.0
---

# nfs-utils

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Network File System (NFS)** is a file system protocol that allows client machines to access network attached filesystems (called exports) from a host system. NFS is supported by the Linux kernel and userspace daemons and utilities are found in the [net-fs/nfs-utils](https://packages.gentoo.org/packages/net-fs/nfs-utils) package.

## Installation

### Kernel

NFS server support is not required for NFS clients. Conversely NFS client support is not required for NFS servers. Inotify support is only required for NFSv4. NFSv3 is only required for compatibility with legacy clients e.g. the BusyBox mount command does not support NFSv4.

#### Client support

Client kernel support must be enabled on each system connecting to the host running the NFS exports.

**Enabling NFS client support**

#### Server support

Server kernel support is only necessary on the system hosting the NFS exports. For local testing purposes, it can be helpful to also enable client support as defined in the previous section on the server as well.

**Enabling NFS server support**

### USE flags


| [+libmount](https://packages.gentoo.org/useflags/+libmount) | Link mount.nfs with libmount | 
| [+nfsv3](https://packages.gentoo.org/useflags/+nfsv3) | Enable support for NFSv2 and NFSv3 | 
| [+nfsv4](https://packages.gentoo.org/useflags/+nfsv4) | Enable support for NFSv4 (includes NFSv4.1 and NFSv4.2) | 
| [+uuid](https://packages.gentoo.org/useflags/+uuid) | Support UUID lookups in rpc.mountd | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [junction](https://packages.gentoo.org/useflags/junction) | Enable NFS junction support in nfsref | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add ldap support | 
| [nfsdctl](https://packages.gentoo.org/useflags/nfsdctl) | Build 'nfsdctl' utility to control nfsd. | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 

### Emerge

Install [net-fs/nfs-utils](https://packages.gentoo.org/packages/net-fs/nfs-utils):

`root #``emerge --ask net-fs/nfs-utils`
## Configuration

### Server

The following table describes the filesystems that will be exported by the server:

| Device | Mount directory | Description | 
|---|---|---|
| /dev/sdb1 | /home | Filesystem containing user home directories. | 
| /dev/sdc1 | /data | Filesystem containing user data. | 

#### Virtual root

The filesystems to be exported can be made available under a single directory. This directory is known as the virtual root directory:

`root #``mkdir /export`
Create directories in the virtual root directory for the filesystems (e.g. /home and /data) that are to be exported:

`root #````
mkdir /export/home
```
`root #````
mkdir /export/data
```
The filesystems to be exported need to be made available under their respective directories in the virtual root directory. This is accomplished with the  `--bind` option of the mount command (if you need also mount something that is mounted inside, use `--rbind` instead:

`root #````
mount --bind /home /export/home
```
`root #````
mount --bind /data /export/data
```
To make the above mounts persistent, add the following to [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab):

**`/etc/fstab`**

#### Exports

The filesystems to be made accessible for clients are specified in /etc/exports. This file consists of the directories to be exported, the clients allowed to access those directories, and a list options for each client. Refer to man exports for more information about the NFS export configuration options.

The following table briefly describes the server options used in the configuration below:

| Option | Description | 
|---|---|
| `insecure` | The server will require that client requests originate on unprivileged ports (those above 1024). This option is required when mounting exported directories from OS X or by the nfs:/ kioslave in KDE. The default is to use privileged ports. | 
| `rw` | The client will have read and write access to the exported directory. The default is to allow read-only access. | 
| `sync` | The server must wait until filesystem changes are committed to storage before responding to further client requests. This is the default. | 
| `no_subtree_check` | The server will not verify that a file requested by a client is in the appropriate filesystem and exported tree. This is the default. | 
| `crossmnt` | The server will reveal filesystems that are mounted under the virtual root directory that would otherwise be hidden when a client mounts the virtual root directory. | 
| `fsid=0` | This option is required to uniquely identify the virtual root directory. | 

If changes are made to /etc/exports after the NFS server has started, issue the following command to propagate the changes to clients:

`root #``exportfs -rv`
##### IPv4

Configuration grants access to the exported local shares, access is granted to the clients in the`192.0.2.0/24` IP network. Client access can also be specified as a single host (IP address or fully qualified domain name), NIS netgroup, or with a single `*` character which grants all clients access.

**`/etc/exports`**

##### IPv6

IPv6 only configuration. Allowed IPv6 prefixes are put after the already configured IPv4 networks.
The above configuration grants access to the exported directories by IP network, in this case `2001:db8:1::/64`. 
These IP networks are allowed to access the exported shares on the NFS server:

**`/etc/exports`**

##### Dual stack configuration

IPv4 and IPv6 networks which are allowed to access the exported shares on the NFS server, here `192.0.2.0/24` and `2001:db8:1::/64`.

**`/etc/exports`**

#### Daemon

##### OpenRC

The NFS daemon on OpenRC is configured via the `OPTS_RPC_NFSD` variable:

**`/etc/conf.d/nfs`**

```
OPTS_RPC_NFSD="8 -V 3 -V 4 -V 4.1"
```
##### systemd

The NFS daemon on systemd is configured via the /etc/nfs.conf config file:

**`/etc/nfs.conf`**

```
[nfsd]
threads=4
vers3=on
vers4=on
vers4.1=on
```
The option `threads=4` is the number of NFS server threads to start, 8 threads are started by default. The options `vers3=on`, `vers4=on` and `vers4.1=on` enable NFS versions 3, 4, and 4.1. Refer to man nfsd for more information about the NFS daemon configuration options. Technical differences between major NFS versions [explained in the wikipedia article](https://en.wikipedia.org/wiki/Network_File_System#Versions_and_variations).

#### Service

##### OpenRC

To start the NFS server:

`root #``rc-service nfs start`
\* Starting rpcbind ...                                                   \[ ok \]
 \* Starting NFS statd ...                                                 \[ ok \]
 \* Starting idmapd ...                                                    \[ ok \]
 \* Exporting NFS directories ...                                          \[ ok \]
 \* Starting NFS mountd ...                                                \[ ok \]
 \* Starting NFS daemon ...                                                \[ ok \]
 \* Starting NFS smnotify ...                                              \[ ok \]

The above output shows that many other services are also started along with the nfs service. To stop all NFS services, stop the rpcbind service:

`root #``rc-service rpcbind stop`
To start the NFS server at boot:

`root #``rc-update add nfs default`
##### systemd

To start the NFS server:

`root #``systemctl start rpcbind nfs-server`
To start the NFS server at boot:

`root #``systemctl enable rpcbind nfs-server`
### Client

#### Service

##### OpenRC

To be able to mount exported directories, start the NFS client:

`root #``rc-service nfsclient start`
\* Starting rpcbind                                                       \[ ok \]
 \* Starting NFS statd                                                     \[ ok \]
 \* Starting NFS sm-notify                                                 \[ ok \]

To start the NFS client at boot:

`root #``rc-update add nfsclient default`
##### systemd

The nfs-client service will be started automatically when systemd detects that exported directories are being mounted.

#### Mounting exports

Mount the exported directories:

`root #````
mount 192.0.2.1:/home /home
```
`root #````
mount 192.0.2.1:/data /data
```
To make the above mounts persistent, add the following to /etc/fstab:

**`/etc/fstab`**

`root #````
mount 192.0.2.1:/home -t nfs4 -o _netdev,rsize=1048576,wsize=1048576,vers=4
```
The virtual root directory can be mounted instead of each individual exported directory. This will make all exported directories available to the client:

`root #``mount 192.0.2.1:/ /mnt`
To make the above mount persistent, add the following to /etc/fstab:

**`/etc/fstab`**

When using /etc/fstab to mount the exported directories, add the netmount service to the default runlevel:

`root #``rc-update add netmount default`
If the NFS server or client support NFSv3 only, the *full* path to the exported directory (e.g. /export/home or /export/data) needs to be specified when mounting:

`root #````
mount 192.0.2.1:/export/home /home
```
`root #````
mount 192.0.2.1:/export/data /data
```
The same applies when mounting the virtual root directory:

`root #``mount 192.0.2.1:/export /mnt`
When mounting exported directories on an IPv6 network, enclose the IPv6 NFS server address in square brackets:

`root #````
mount [2001:db8:1::1]:/home /home
```
`root #````
mount [2001:db8:1::1]:/data /data
```
When mounting a link-local IPv6 address, the outgoing local network interface must also be specified:

`root #````
mount [fe80::215:c5ff:fb3e:e2b1%eth0]:/home /home
```
`root #````
mount [fe80::215:c5ff:fb3e:e2b1%eth0]:/data /data
```
With NFSv4, the virtual root directory can be rather 'invisible' depending on server configuration; you may need to use relative path:

`root #````
mount -t nfs4 192.0.2.1:home /home
```
`root #````
mount -t nfs4 192.0.2.1:data /data
```
`root #````
mount 192.0.2.1:/home /home -o rsize=1048576,wsize=1048576,vers=4
```
For persistence:

**`/etc/fstab`**

### Kerberos

It is possible to identify NFS client using [Kerberos GSS](https://wiki.gentoo.org/wiki/Kerberos_Server). This will require a few modifications. In the following instruction, it is supposed that Kerberos is already configured on the same server as NFS (which hostname is *server.domain.tld*) and that the client (*client.domain.tld*) is able to kinit to it. The Kerberos default realm it *DOMAIN\_REALM.TLD*.

First, enable the following kernel option (`CONFIG_RPCSEC_GSS_KRB5`) for both server and client. Note that this option may not appear if all cryptographic dependencies are not selected. See kernel option dependencies for more information:

**Enabling Kerberos for RPC**

Then, create principals for the NFS service for both the server and the client. On the server, execute:

`root #````
kadmin.local add_principal -randkey nfs/server.domain.tld
```
`root #````
kadmin.local add_principal -randkey nfs/client.domain.tld
```
Each computer must have its password saved in a local keytab. The easiest way to do it is (on the server):

`root #````
kadmin.local ktadd nfs/server.domain.tld
```
`root #````
kadmin.local ktadd -k /root/krb5.keytab nfs/client.domain.tld
```
and then transfer the /root/krb5.keytab to the client, with the name /etc/krb5.keytab. Note that the file should be owned by root with `0600` mode.

The service rpc.gssd must run at client side. The following line must appear in /etc/conf.d/nfsclient of the client:

**`/etc/conf.d/nfsclient`**

The services rpc.idmapd and rpc.svcgssd must run at server side. The following line must appear in /etc/conf.d/nfs of the server:

**`/etc/conf.d/nfs`**

The rpc.idmapd service must be correctly configured (on the server):

**`/etc/idmapd.conf`**

Add `sec=krb5` to the export options.

**`/etc/exports`**

It is also possible to increase security with `sec=krb5i` (user authentication and integrity checking) or even `sec=krb5p` (user authentication, integrity checking and NFS traffic encryption). The more security, the more resources are needed.

The same option must be added to the mount command at client side.

#### Encryption

For the three security modes `krb`, `krb5i`, and `krb5p` the client and server must agree upon the encryption scheme to use. For Linux clients and servers the scheme used is determined by four things: which schemes are enabled in the client's kernel, which are enabled in the server's, and which are enabled in the client and server Kerberos configurations.

**Kernel Configuration**

First, you must enable the basic building blocks that will be used for the schemes. You should stick to strong algorithms. As of early 2025, AES + SHA2 is a decent choice. Camellia + CMAC is probably OK. Avoid DES. Avoid SHA1 unless forced by compatibility reasons.

**Enabling the basic algorithms**

**Putting the pieces together**

There are a *lot* of acceleration options. Those available to you will depend upon your architecture and processor type. A rule of thumb is "if in doubt, turn it on."

**Optional acceleration (x86 example)**

**Optional acceleration (ARM64 example)**

#### Kerberos Configuration

Kerberos must also be configured with the permitted and preferred encryption types. This goes into the /etc/krb5.conf file. An example:

**`/etc/krb5.conf`**

**Encryption types example**

It is also possible to allow encoding types per-service ("principal"). See the kadmin documentation under set string. Also, the kdc.conf supported\_enctypes parameter can control, on a per-domain basis, the encryption types that kadmind will use when generating long-term keys.

Which encryption types are "weak" is hard-coded into the Kerberos libraries; see the MIT Kerberos Consortium [Encryption Types](https://web.mit.edu/kerberos/krb5-latest/doc/admin/enctypes.html) documentation for a list of supported and weak types.

The kernel and Kerberos configurations do not need to be identical, and your only indication they differ will be that things are mysteriously not working.

See the [krb5.conf](https://web.mit.edu/kerberos/krb5-latest/doc/admin/conf_files/krb5_conf.html) page for a fuller explanation of the configuration options. See the "Troubleshooting" section, below, for encryption type mismatch errors.

### Minimal NFS V4 only server/client setup

This is a setup for a minimal NFS v4 only server/client configuration without id mapping and Kerberos support. The purpose is to minimise the number of running daemons and TCP/UDP open ports by removing all rpc\* need.

Set USEFLAG to remove NFS v3 support in order to remove the init.d portmap dependency:

**`/etc/portage/package.use`**

```
 -nfsv3
```
#### Minimal Server

##### Openrc

On server side we make rpcbind don't bind on TCP/UDP ports resulting to only one NFS daemon listening on only one port (2049 by default for NFS):

**`/etc/conf.d/nfs`**

```
NFS_NEEDED_SERVICES="!rpc.statd"
OPTS_RPC_NFSD="8 -V 4.2 -V 4.1 -V 4 -N 3"
OPTS_RPC_MOUNTD="-u -n"
```
#### Minimal Client

##### Openrc

On client side we remove unnecessary client daemons resulting to only nfsclient service and no open port at all.

**`/etc/conf.d/nfsclient`**

```
rc_need="!rpc.statd !rpc.idmapd"
```
## NFS usage stats

### Server

`root #``nfsstat -s`
Server rpc stats:
calls      badcalls   badfmt     badauth    badclnt
16418983   619        9          610        0
 
Server nfs v4:
null             compound
113       0%     16418909 99%
 
Server nfs v4 operations:
op0-unused       op1-unused       op2-future       access           close
0         0%     0         0%     0         0%     4566793   8%     105140    0%
commit           create           delegpurge       delegreturn      getattr
7234      0%     818       0%     0         0%     3602      0%     13580634 25%
getfh            link             lock             lockt            locku
354535    0%     0         0%     0         0%     0         0%     0         0%
lookup           lookup\_root      nverify          open             openattr
269769    0%     0         0%     0         0%     105456    0%     0         0%
open\_conf        open\_dgrd        putfh            putpubfh         putrootfh
0         0%     317       0%     15906016 29%     0         0%     45        0%
read             readdir          readlink         remove           rename
1922910   3%     369810    0%     47        0%     3919      0%     793       0%
renew            restorefh        savefh           secinfo          setattr
0         0%     0         0%     793       0%     0         0%     1269      0%
setcltid         setcltidconf     verify           write            rellockowner
0         0%     0         0%     0         0%     274789    0%     0         0%
bc\_ctl           bind\_conn        exchange\_id      create\_ses       destroy\_ses
0         0%     0         0%     64        0%     59        0%     35        0%
free\_stateid     getdirdeleg      getdevinfo       getdevlist       layoutcommit
0         0%     0         0%     0         0%     0         0%     0         0%
layoutget        layoutreturn     secinfononam     sequence         set\_ssv
0         0%     0         0%     4         0%     16418727 30%     0         0%
test\_stateid     want\_deleg       destroy\_clid     reclaim\_comp     allocate
0         0%     0         0%     17        0%     41        0%     0         0%
copy             copy\_notify      deallocate       ioadvise         layouterror
0         0%     0         0%     0         0%     0         0%     0         0%
layoutstats      offloadcancel    offloadstatus    readplus         seek
0         0%     0         0%     0         0%     0         0%     0         0%
write\_same
0         0%

### Client

`root #``nfsstat -c`
Client rpc stats:
calls      retrans    authrefrsh
7002       2          7002
Client nfs v4:
null             read             write            commit           open
64        1%     1         0%     0         0%     0         0%     1         0%
open\_conf        open\_noat        open\_dgrd        close            setattr
0         0%     1         0%     0         0%     1         0%     0         0%
fsinfo           renew            setclntid        confirm          lock
125       2%     0         0%     8         0%     0         0%     0         0%
lockt            locku            access           getattr          lookup
0         0%     0         0%     101       2%     522      10%     90        1%
lookup\_root      remove           rename           link             symlink
45        0%     0         0%     0         0%     0         0%     0         0%
create           pathconf         statfs           readlink         readdir
0         0%     80        1%     6         0%     0         0%     109       2%
server\_caps      delegreturn      getacl           setacl           fs\_locations
205       4%     1         0%     2154     44%     0         0%     0         0%
rel\_lkowner      secinfo          fsid\_present     exchange\_id      create\_session
0         0%     0         0%     0         0%     121       2%     84        1%
destroy\_session  sequence         get\_lease\_time   reclaim\_comp     layoutget
60        1%     960      19%     21        0%     63        1%     0         0%
getdevinfo       layoutcommit     layoutreturn     secinfo\_no       test\_stateid
0         0%     0         0%     0         0%     3         0%     0         0%
free\_stateid     getdevicelist    bind\_conn\_to\_ses destroy\_clientid seek
0         0%     0         0%     0         0%     39        0%     0         0%
allocate         deallocate       layoutstats      clone
0         0%     0         0%     0         0%     0         0%

## Troubleshooting

- Verify that the NFS server is running and listening for connections:

`root #``ss -tulpn | grep rpc````
udp   UNCONN 0      0            0.0.0.0:111        0.0.0.0:*    users:(("rpcbind",pid=4020,fd=6))
udp   UNCONN 0      0            0.0.0.0:49657      0.0.0.0:*    users:(("rpc.mountd",pid=4149,fd=4))
udp   UNCONN 0      0            0.0.0.0:58082      0.0.0.0:*    users:(("rpc.mountd",pid=4149,fd=12))
udp   UNCONN 0      0          127.0.0.1:834        0.0.0.0:*    users:(("rpc.statd",pid=4050,fd=5))
udp   UNCONN 0      0            0.0.0.0:34042      0.0.0.0:*    users:(("rpc.statd",pid=4050,fd=8))
udp   UNCONN 0      0            0.0.0.0:35152      0.0.0.0:*    users:(("rpc.mountd",pid=4149,fd=8))
udp   UNCONN 0      0                  *:111              *:*    users:(("rpcbind",pid=4020,fd=8))
udp   UNCONN 0      0                  *:49463            *:*    users:(("rpc.mountd",pid=4149,fd=14))
udp   UNCONN 0      0                  *:43316            *:*    users:(("rpc.mountd",pid=4149,fd=10))
udp   UNCONN 0      0                  *:44048            *:*    users:(("rpc.mountd",pid=4149,fd=6))
udp   UNCONN 0      0                  *:44332            *:*    users:(("rpc.statd",pid=4050,fd=10))
tcp   LISTEN 0      0            0.0.0.0:52271      0.0.0.0:*    users:(("rpc.mountd",pid=4149,fd=5))
tcp   LISTEN 0      0            0.0.0.0:41965      0.0.0.0:*    users:(("rpc.mountd",pid=4149,fd=9))
tcp   LISTEN 0      0            0.0.0.0:111        0.0.0.0:*    users:(("rpcbind",pid=4020,fd=7))
tcp   LISTEN 0      0            0.0.0.0:48527      0.0.0.0:*    users:(("rpc.mountd",pid=4149,fd=13))
tcp   LISTEN 0      0            0.0.0.0:53559      0.0.0.0:*    users:(("rpc.statd",pid=4050,fd=9))
tcp   LISTEN 0      0                  *:52293            *:*    users:(("rpc.mountd",pid=4149,fd=7))
tcp   LISTEN 0      0                  *:43983            *:*    users:(("rpc.mountd",pid=4149,fd=15))
tcp   LISTEN 0      0                  *:111              *:*    users:(("rpcbind",pid=4020,fd=9))
tcp   LISTEN 0      0                  *:40105            *:*    users:(("rpc.statd",pid=4050,fd=11))
tcp   LISTEN 0      0                  *:38481            *:*    users:(("rpc.mountd",pid=4149,fd=11))
```
- Verify which NFS daemons are running:

`root #``rpcinfo -p````
   program vers proto   port  service
    100000    4   tcp    111  portmapper
    100000    3   tcp    111  portmapper
    100000    2   tcp    111  portmapper
    100000    4   udp    111  portmapper
    100000    3   udp    111  portmapper
    100000    2   udp    111  portmapper
    100024    1   udp  57655  status
    100024    1   tcp  34950  status
    100003    2   tcp   2049  nfs
    100003    3   tcp   2049  nfs
    100003    4   tcp   2049  nfs
    100003    2   udp   2049  nfs
    100003    3   udp   2049  nfs
    100003    4   udp   2049  nfs
    100021    1   udp  44208  nlockmgr
    100021    3   udp  44208  nlockmgr
    100021    4   udp  44208  nlockmgr
    100021    1   tcp  44043  nlockmgr
    100021    3   tcp  44043  nlockmgr
    100021    4   tcp  44043  nlockmgr
```
- List the exported directories from the NFS server:

`root #``exportfs -v`
/export       	192.0.2.0/24(rw,wdelay,crossmnt,insecure,root\_squash,no\_subtree\_check,fsid=0,sec=sys,no\_all\_squash)
/export/home  	192.0.2.0/24(rw,wdelay,insecure,root\_squash,no\_subtree\_check,sec=sys,no\_all\_squash)
/export/data  	192.0.2.0/24(rw,wdelay,insecure,root\_squash,no\_subtree\_check,sec=sys,no\_all\_squash)

- List the current open connections to the NFS server:

`user $``ss -tun|grep -E 'Sta|2049'`
Netid State Recv-Q Send-Q       Local Address:Port     Peer Address:Port   Process
tcp   ESTAB 0      0                192.0.2.1:2049       192.0.2.10:1012

- Verify that the exported directories are mounted by the NFS client:

`user $``ss -tun|grep -E 'Sta|2049'`
Netid State Recv-Q Send-Q       Local Address:Port     Peer Address:Port   Process
tcp   ESTAB 0      0              192.0.2.10:1012         192.0.2.1:2049

### Unresponsiveness of the system

The system may become unresponsive during shutdown when the NFS client attempts to unmount exported directories *after* [udev](https://wiki.gentoo.org/wiki/Udev) has stopped. To prevent this a [local.d](https://wiki.gentoo.org/wiki/Local.d) script can be used to forcibly unmount the exported directories during shutdown.

Create the file nfs.stop:

**`/etc/local.d/nfs.stop`**

Set the according file bits:

`root #``chmod a+x /etc/local.d/nfs.stop`
### Client refuses to mount NFS filesystem

If you do this: mount myserver:/somefilesystem, and the client responds with mount.nfs: Operation not permitted for myserver:/somefilesystem on /the\_fstab/mount\_point, and the server log says

**`/var/log/messages`**

then you have an encryption type mismatch. RPC debug won't help much; you'll just see stuff like

**`/var/log/messages`**

but it won't tell you \_which\_ two types the kernel wants.

Here's something that should help:

**`/proc/net/rpc/gss_krb5_enctypes`**

Not very impressive, I admit. However, there is a secret decoding ring:

**`/usr/include/krb5/krb5.h`**

```
/* per Kerberos v5 protocol spec */
// ...
#define ENCTYPE_NULL            0x0000
#define ENCTYPE_DES_CBC_CRC     0x0001  /**< @deprecated no longer supported */
#define ENCTYPE_DES_CBC_MD4     0x0002  /**< @deprecated no longer supported */
#define ENCTYPE_DES3_CBC_ENV    0x000f  /**< DES-3 cbc mode, CMS enveloped data */
// ...
#define ENCTYPE_DES3_CBC_SHA1               0x0010
#define ENCTYPE_AES128_CTS_HMAC_SHA1_96     0x0011 /**< RFC 3962 */
#define ENCTYPE_AES256_CTS_HMAC_SHA1_96     0x0012 /**< RFC 3962 */
#define ENCTYPE_AES128_CTS_HMAC_SHA256_128  0x0013 /**< RFC 8009 */
#define ENCTYPE_AES256_CTS_HMAC_SHA384_192  0x0014 /**< RFC 8009 */
```
Translating the 18 & 17 to hexidecimal, we end up with 0x0012 (ENCTYPE\_AES256\_CTS\_HMAC\_SHA1\_96) and 0x0011 (ENCTYPE\_AES128\_CTS\_HMAC\_SHA1\_96). So two SHA1 algorithms. If you've eliminated them from the Client's krb5.conf then that's your problem.

## See also

- [Samba](https://wiki.gentoo.org/wiki/Samba) — a re-implementation of the [SMB/CIFS](https://wiki.gentoo.org/wiki/Smbnetfs) networking protocol, a Microsoft Windows alternative to Network File System ([NFS](https://wiki.gentoo.org/wiki/NFS)).
