<!-- source: https://wiki.gentoo.org/wiki/SELinux/Labels | group: Gentoo Wiki (Main) | wiki-title: SELinux/Labels -->
---
title: SELinux/Labels
url: https://wiki.gentoo.org/wiki/SELinux/Labels
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-11"
fingerprint: "670f152726e2d181"
license: CC BY-SA 4.0
---

# SELinux/Labels

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

File labels are the most common aspect of a SELinux system that users and administrators will need to care for. As SELinux policy decisions are based on the label of a resource, making sure that the file labels are correctly set is the most important part of maintaining SELinux systems.

## Introduction

The term *label* is used for the *SELinux context* of a file or other object on a system. Whenever a document talks about a file context or file label, both actually mean the same thing. The term comes from the SELinux permissions *relabelfrom* and *relabelto* which inform the policy if a relabel operation (change of context) is allowed from a particular label (context) or towards a particular label (context).

Label values are also often abbreviated in documents. A file with `user_home_t` label (or context) actually needs to have an entire context assigned, but in the document itself the *type* part of the context is the most important one. As such, instead of talking about a file with `user_u:object_r:user_home_t:s0` label (and having to explain that the [SELinux user](https://wiki.gentoo.org/wiki/SELinux/Users_and_logins) part can be different) the type `user_home_t` alone is used.

### Labels are extended attributes

On most file systems, SELinux labels are stored as *extended attributes*. This is not always the case though - some file systems do not support extended attributes. In these cases, all files on the file system get assigned the same context, usually provided through the mount option of the file system.

### Non-file resources

On a SELinux system, everything needs to have a context / label assigned. Even resources such as TCP and UDP ports get a label. These labels are assigned by SELinux itself through policy definitions, although users can still manipulate the assigned port types if no specific port type is used yet.

## Managing file labels

Although file labels are set as extended attributes in most cases, managing file labels goes further than just ensuring that that particular extended attribute is correctly set. The SELinux user space maintains a database of file path expressions together with the file context to be assigned to these resources. This allows administrators to verify if a file context is correct or not, and to reset the context of a file.

### Listing file labels

To view the file labeling policy, **semanage fcontext** can be used with grep to filter:

`root #``semanage fcontext --list | grep repos`
/var/db/repos(/.\*)?                                all files          system\_u:object\_r:portage\_ebuild\_t

The Linux core utilities support displaying of SELinux labels.

For instance, using `ls -Z` (the `-Z` option is often used for displaying SELinux contexts):

`user $``ls -lZ /etc/resolv.conf`
-rw-r--r--. 1 root root system\_u:object\_r:net\_conf\_t 68 May  4 18:14 /etc/resolv.conf

Similarly, the `stat` command displays the label as well, here on an MLS-enabled system:

`user $``stat /etc/resolv.conf`
File: '/etc/resolv.conf'
  Size: 149             Blocks: 16         IO Block: 4096   regular file
Device: fb02h/64258d    Inode: 394204      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system\_u:object\_r:net\_conf\_t:s0
Access: 2013-01-13 13:24:01.402000000 +0100
Modify: 2014-05-04 19:43:03.563000000 +0200
Change: 2014-05-04 19:43:03.573000000 +0200

The context of a file can also be directly read through the `getfattr` command, used to obtain a file's extended attributes:

`user $``getfattr -m . -d /etc/resolv.conf`
getfattr: Removing leading '/' from absolute path names
# file: etc/resolv.conf
security.evm=0sAoTjX3aOeDQdWxbOfOUV930tWoDA
security.ima=0sAYC508o0Lz4iAA9ucVAVsvK02tV/
security.selinux="system\_u:object\_r:net\_conf\_t:s0"

In the above output, next to the [integrity](https://wiki.gentoo.org/wiki/Integrity) related extended attributes, the `security.selinux` attribute is shown, which contains the SELinux context of the file.

### Relabeling files

Relabeling files means that the context of one or more files is *reset* towards the definition stored by the SELinux user space utilities.

Such a relabeling operation is usually done through the `setfiles` or `restorecon` command:

`root #``restorecon -v /etc/resolv.conf`
Directories can also be recursively relabelled with the -R switch:

`root #``restorecon -Rv /etc/`
To relabel the entire file system, use `rlpkg` (which is a Gentoo-specific tool):

`root #``rlpkg -a -r`
The `rlpkg` application also supports relabeling all files provided by a single Gentoo package. For instance, to relabel all files provided by the `www-client/firefox-bin` package:

`root #``rlpkg www-client/firefox-bin`
### Temporarily modifying file labels

To set a file context, the `chcon` command can be used:

`root #``chcon -t net_conf_t /etc/puppet-resolv.conf`
However, the `chcon` tool does not update the SELinux user space definition list. As a result, when the administrator relabels a file, it will be reset to the original value.

### Permanently modifying file labels

To ensure that a new file context remains, even after a relabeling operation, the administrator needs to update the SELinux user space definition list first.

The definition list uses PCRE-based regular expressions and can be managed through `semanage fcontext`.

For instance, to add /etc/puppet-resolv.conf to the list and assign it the `net_conf_t` type:

`root #``semanage fcontext --add --type net_conf_t "/etc/puppet-resolv\.conf"`
The use of PCRE expressions allows for a denser set of rules that matches entire subdirectories easily:

`root #``semanage fcontext --add --type bin_t "/usr/lib/portage/bin(/.*)?"`
After updating the definition list, the context of the file(s) isn't set yet. For this, the files need to be relabeled:

`root #``restorecon -R /etc/puppet-resolv.conf /usr/lib/portage/bin`
## Managing TCP and UDP port labels

TCP and UDP ports also get assigned a particular label.

### Listing port labels

With `seinfo`, the context for a particular port can be viewed:

`user $``seinfo --portcon=80`
portcon tcp 80 system\_u:object\_r:http\_port\_t
   portcon tcp 1-511 system\_u:object\_r:reserved\_port\_t
   portcon udp 1-511 system\_u:object\_r:reserved\_port\_t

It is also possible to list all port definition declarations using `semanage port`:

`root #``semanage port -l | grep http`
http\_cache\_port\_t              tcp      3128, 8080, 8118, 10001-10010
http\_cache\_port\_t              udp      3130
http\_port\_t                    tcp      8888, 80, 443, 488, 8008, 8009, 8443
pegasus\_http\_port\_t            tcp      5988
pegasus\_https\_port\_t           tcp      5989

### Modifying port labels

If a port is not only assigned the `reserved_port_t`, `unreserved_port_t` or `hi_reserved_port_t` label, then the context for that port cannot be modified.

However, if the port only has one of these types assigned, then it is possible to set a specific type.

For instance, to assign the type `http_port_t` to port 9980, first check if it is possible:

`user $``seinfo --portcon=9980`
portcon udp 1024-65535 system\_u:object\_r:unreserved\_port\_t
   portcon tcp 1024-65535 system\_u:object\_r:unreserved\_port\_t

It is possible, so now use `semanage port` to assign it:

`root #``semanage port -a -t http_port_t -p tcp 9980`
## Managing process labels

The label of a process is decided by the SELinux policy, and is called a *domain*.

It is not possible to relabel a process. To have a process run in a different domain, it *might* be possible to launch the command using the `runcon` command. However, in the majority of cases, this will not be allowed by the SELinux policy. The domain in which a process runs is fully policy-driven (with a small configurable aspect provided for SELinux-aware applications).

### Listing process labels

The standard core utilities can display the context of a process.

For instance, to get the context of the `init` process:

`root #``ps -eZ | grep init`
system\_u:system\_r:init\_t            1 ?        00:00:01 init

## Summary

Label management is one of the most core competences that a system administrator on a SELinux system must have. As SELinux decisions are based on labels (contexts), ensuring that the right context is used on all resources is a primary concern for all administrators.
