<!-- source: https://wiki.gentoo.org/wiki/Filesystem_Hierarchy_Standard | group: Gentoo Wiki (Main) | wiki-title: Filesystem Hierarchy Standard -->
---
title: Filesystem Hierarchy Standard
url: https://wiki.gentoo.org/wiki/Filesystem_Hierarchy_Standard
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-27"
fingerprint: b54cb87a27e72fe1
license: CC BY-SA 4.0
---

# Filesystem Hierarchy Standard

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

*Not to be confused with the more recently developed [Linux File System Hierarchy](https://uapi-group.org/specifications/specs/linux_file_system_hierarchy/) specification of [the UAPI Group](https://uapi-group.org/specifications/).*

The [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) (FHS) is a reference that describes the conventions used for the layout of Unix-like systems. It has been made popular by its use in Linux distributions, but it is used by other Unix-like systems as well. The FHS is maintained by the Linux Foundation. The latest version is 3.0, dated March 19, 2015<sup>[\[1\]](https://wiki.gentoo.org#cite_note-linux-foundation-1)</sup>.

Under the FHS, all files and directories appear under the root directory /, even if they are stored on different physical or virtual devices<sup>[\[1\]](https://wiki.gentoo.org#cite_note-linux-foundation-1)</sup>. Most of these directories exist in all Unix-like operating systems and are generally used in much the same way<sup>[\[2\]](https://wiki.gentoo.org#cite_note-wikipedia-2)</sup>.

The FHS is intended to support interoperability of applications, system administration tools, development tools, and scripts as well as greater uniformity of documentation for these systems<sup>[\[1\]](https://wiki.gentoo.org#cite_note-linux-foundation-1)</sup>.

**Todo:**

- Fix "ref" elements, either by creating a Template:Cite web (we may not need that), or changing the "refs"

## Standard directories

| Directory | Description | 
|---|---|
| [/](https://wiki.gentoo.org/index.php?title=/&action=edit&redlink=1) | *Primary hierarchy* root and root directory  of the entire file system hierarchy. | 
| [/bin](https://wiki.gentoo.org/wiki//bin) | Essential command binaries that need to be available in single-user mode, including to bring up the system or repair it, <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> for all users (e.g., `cat`, ls, cp). | 
| [/boot](https://wiki.gentoo.org/wiki//boot) | Boot loader files (e.g., kernels, initrd). | 
| [/dev](https://wiki.gentoo.org/wiki//dev) | Device file s (e.g., [/dev/null](https://wiki.gentoo.org/index.php?title=Null_device&action=edit&redlink=1), /dev/disk0, /dev/sda1, [/dev/tty](https://wiki.gentoo.org/index.php?title=/dev/tty&action=edit&redlink=1), [/dev/random](https://wiki.gentoo.org/index.php?title=/dev/random&action=edit&redlink=1)). | 
| [/etc](https://wiki.gentoo.org/wiki//etc) | Host-specific system-wide configuration files. There has been controversy over the meaning of the name itself. In early versions of the UNIX Implementation Document from Bell Labs, [/etc](https://wiki.gentoo.org/wiki//etc) is referred to as the *etcetera directory*,<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> as this directory historically held everything that did not belong elsewhere (however, the FHS restricts [/etc](https://wiki.gentoo.org/wiki//etc) to static configuration files and may not contain binaries).<sup>[\[5\]](https://wiki.gentoo.org#cite_note-.2Fetc-5)</sup> Since the publication of early documentation, the directory name has been re-explained in various ways. Recent interpretations include backronym s such as "Editable Text Configuration" or "Extended Tool Chest".[\[6\]](https://wiki.gentoo.org#cite_note-6) | 
| [/etc/opt](https://wiki.gentoo.org/index.php?title=/etc/opt&action=edit&redlink=1) | Configuration files for add-on packages stored in [/opt](https://wiki.gentoo.org/wiki//opt). | 
| [/etc/sgml](https://wiki.gentoo.org/index.php?title=/etc/sgml&action=edit&redlink=1) | Configuration files, such as catalogs, for software that processes SGML. | 
| [/etc/X11](https://wiki.gentoo.org/wiki/X11) | Configuration files for the [X Window System, version 11](https://wiki.gentoo.org/wiki/X11). | 
| [/etc/xml](https://wiki.gentoo.org/index.php?title=/etc/xml&action=edit&redlink=1) | Configuration files, such as catalogs, for software that processes XML. | 
| [/home](https://wiki.gentoo.org/wiki//home) | Users' [home directories](https://wiki.gentoo.org/index.php?title=Home_directory&action=edit&redlink=1), containing saved files, personal settings, etc. | 
| [/lib](https://wiki.gentoo.org/wiki//lib) | [Libraries](https://wiki.gentoo.org/index.php?title=Libraries&action=edit&redlink=1) essential for binaries in [/bin](https://wiki.gentoo.org/wiki//bin) and [/sbin](https://wiki.gentoo.org/wiki//sbin). | 
| /lib\<qual> | Alternate format essential libraries. These are typically used on systems that support more than one executable code format, such as systems supporting 32-bit and 64-bit versions of an instruction set. Such directories are optional, but if they exist, they have some requirements. | 
| [/media](https://wiki.gentoo.org/wiki//media) | Mount points for removable media such as CD-ROMs (appeared in FHS-2.3 in 2004). | 
| [/mnt](https://wiki.gentoo.org/wiki//mnt) | Temporarily mounted filesystems. | 
| [/opt](https://wiki.gentoo.org/wiki//opt) | Add-on application software Packages . <sup>[\[7\]](https://wiki.gentoo.org#cite_note-.2Fopt-7)</sup> | 
| [/proc](https://wiki.gentoo.org/wiki//proc) | Virtual filesystem providing process and kernel information as files. In Linux, corresponds to a procfs mount. Generally, automatically generated and populated by the system, on the fly. | 
| [/root](https://wiki.gentoo.org/index.php?title=/root&action=edit&redlink=1) | root user's home directory. | 
| [/run](https://wiki.gentoo.org/wiki//run) | Run-time variable data: Information about the running system since last boot, e.g., currently logged-in users and running daemons. Files under this directory must be either removed or truncated at the beginning of the boot process, but this is not necessary on systems that provide this directory as a temporary filesystem (tmpfs). | 
| [/sbin](https://wiki.gentoo.org/wiki//sbin) | Essential system binaries (e.g., `fsck`, `init`, `route`). | 
| [/srv](https://wiki.gentoo.org/wiki//srv) | Site-specific data served by this system, such as data and scripts for web servers, data offered by FTP servers, and repositories for version control systems (appeared in FHS-2.3 in 2004). | 
| [/sys](https://wiki.gentoo.org/wiki//sys) | Contains information about devices, drivers, and some kernel features. <sup>[\[8\]](https://wiki.gentoo.org#cite_note-.2Fsys-8)</sup> | 
| [/tmp](https://wiki.gentoo.org/wiki//tmp) | Directory for temporary files (see also [/var/tmp](https://wiki.gentoo.org/index.php?title=/var/tmp&action=edit&redlink=1)). Often not preserved between system reboots and may be severely size-restricted. | 
| [/usr](https://wiki.gentoo.org/wiki//usr) | *Secondary hierarchy* for read-only user data; contains the majority of multi-user utilities and applications.  Should be shareable and read-only.<sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup><sup>[\[10\]](https://wiki.gentoo.org#cite_note-10)</sup> | 
| [/usr/bin](https://wiki.gentoo.org/wiki//usr/bin) | Non-essential command binaries (not needed in single-user mode); for all users. | 
| [/usr/include](https://wiki.gentoo.org/index.php?title=/usr/include&action=edit&redlink=1) | Standard include files. | 
| [/usr/lib](https://wiki.gentoo.org/index.php?title=/usr/lib&action=edit&redlink=1) | Libraries for the binaries in [/usr/bin](https://wiki.gentoo.org/wiki//usr/bin) and [/usr/sbin](https://wiki.gentoo.org/index.php?title=/usr/sbin&action=edit&redlink=1). | 
| [/usr/libexec](https://wiki.gentoo.org/index.php?title=/usr/libexec&action=edit&redlink=1) | Binaries run by other programs that are not intended to be executed directly by users or shell scripts (optional). | 
| /usr/lib\<qual> | Alternative-format libraries (e.g., /usr/lib32 for 32-bit libraries on a 64-bit machine (optional)). | 
| [/usr/local](https://wiki.gentoo.org/index.php?title=/usr/local&action=edit&redlink=1) | *Tertiary hierarchy* for local data, specific to this host. Typically has further subdirectories (e.g., `bin`, `lib`, `share`).<sup>[\[NB 1\]](https://wiki.gentoo.org#cite_note-11)</sup> | 
| [/usr/sbin](https://wiki.gentoo.org/index.php?title=/usr/sbin&action=edit&redlink=1) | Non-essential system binaries (e.g., daemons for various network services). | 
| [/usr/share](https://wiki.gentoo.org/index.php?title=/usr/share&action=edit&redlink=1) | Architecture-independent (shared) data. | 
| [/usr/src](https://wiki.gentoo.org/index.php?title=/usr/src&action=edit&redlink=1) | Source code (e.g., the kernel source code with its header files). | 
| [/usr/X11R6](https://wiki.gentoo.org/index.php?title=/usr/X11R6&action=edit&redlink=1) | X Window System , Version 11, Release 6 (up to FHS-2.3, optional). | 
| [/var](https://wiki.gentoo.org/index.php?title=/var&action=edit&redlink=1) | Variable files: files whose content is expected to continually change during normal operation of the system, such as logs, spool files, and temporary e-mail files. | 
| [/var/cache](https://wiki.gentoo.org/index.php?title=/var/cache&action=edit&redlink=1) | Application cache data. Such data are locally generated as a result of time-consuming I/O or calculation. The application must be able to regenerate or restore the data. The cached files can be deleted without loss of data. | 
| [/var/lib](https://wiki.gentoo.org/index.php?title=/var/lib&action=edit&redlink=1) | State information. Persistent data modified by programs as they run (e.g., databases, packaging system metadata, etc.). | 
| [/var/lock](https://wiki.gentoo.org/index.php?title=/var/lock&action=edit&redlink=1) | Lock files. Files keeping track of resources currently in use. | 
| [/var/log](https://wiki.gentoo.org/index.php?title=/var/log&action=edit&redlink=1) | Log files. Various logs. | 
| [/var/mail](https://wiki.gentoo.org/index.php?title=/var/mail&action=edit&redlink=1) | Mailbox files. In some distributions, these files may be located in the deprecated /var/spool/mail. | 
| [/var/opt](https://wiki.gentoo.org/index.php?title=/var/opt&action=edit&redlink=1) | Variable data from add-on packages that are stored in [/opt](https://wiki.gentoo.org/wiki//opt). | 
| [/var/run](https://wiki.gentoo.org/index.php?title=/var/run&action=edit&redlink=1) | Run-time variable data. This directory contains system information data describing the system since it was booted. <sup>[\[11\]](https://wiki.gentoo.org#cite_note-12)</sup> In FHS 3.0, [/var/run](https://wiki.gentoo.org/index.php?title=/var/run&action=edit&redlink=1) is replaced by [/run](https://wiki.gentoo.org/wiki//run); a system should either continue to provide a [/var/run](https://wiki.gentoo.org/index.php?title=/var/run&action=edit&redlink=1) directory or provide a symbolic link from [/var/run](https://wiki.gentoo.org/index.php?title=/var/run&action=edit&redlink=1) to [/run](https://wiki.gentoo.org/wiki//run) for backwards compatibility.[\[12\]](https://wiki.gentoo.org#cite_note-13) | 
| [/var/spool](https://wiki.gentoo.org/index.php?title=/var/spool&action=edit&redlink=1) | [Spool](https://wiki.gentoo.org/index.php?title=Spooling&action=edit&redlink=1) for tasks waiting to be processed (e.g., print queues and outgoing mail queue). | 
| /var/spool/mail | (Deprecated) location for users' mailboxes. <sup>[\[13\]](https://wiki.gentoo.org#cite_note-14)</sup> | 
| [/var/tmp](https://wiki.gentoo.org/index.php?title=/var/tmp&action=edit&redlink=1) | Temporary files to be preserved between reboots. | 

## External resources

- [https://refspecs.linuxfoundation.org/FHS\_3.0/fhs-3.0.html](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html)
- [https://wiki.linuxfoundation.org/lsb/fhs](https://wiki.linuxfoundation.org/lsb/fhs)
- [https://en.wikipedia.org/wiki/Filesystem\_Hierarchy\_Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

## References

1. [↑](https://wiki.gentoo.org#cite_ref-11) Historically and strictly according to the standard, [/usr/local](https://wiki.gentoo.org/index.php?title=/usr/local&action=edit&redlink=1) is for data that must be stored on the local host (as opposed to [/usr](https://wiki.gentoo.org/wiki//usr), which may be mounted across a network). Most of the time [/usr/local](https://wiki.gentoo.org/index.php?title=/usr/local&action=edit&redlink=1) is used for installing software/data that are *not* part of the standard operating system distribution (in such case, [/usr](https://wiki.gentoo.org/wiki//usr) would only contain software/data that *are* part of the standard operating system distribution). It is possible that the FHS standard may in the future be changed to reflect this de facto convention.

1. ↑ <sup>[1.0](https://wiki.gentoo.org#cite_ref-linux-foundation_1-0)</sup> <sup>[1.1](https://wiki.gentoo.org#cite_ref-linux-foundation_1-1)</sup> <sup>[1.2](https://wiki.gentoo.org#cite_ref-linux-foundation_1-2)</sup> <sup>[1.3](https://wiki.gentoo.org#cite_ref-linux-foundation_1-3)</sup> [https://refspecs.linuxfoundation.org/FHS\_3.0/fhs/index.html](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
2. ↑ <sup>[2.0](https://wiki.gentoo.org#cite_ref-wikipedia_2-0)</sup> <sup>[2.1](https://wiki.gentoo.org#cite_ref-wikipedia_2-1)</sup> [https://en.wikipedia.org/wiki/Filesystem\_Hierarchy\_Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
4. [↑](https://wiki.gentoo.org#cite_ref-4) [Template:Cite book](https://wiki.gentoo.org/index.php?title=Template:Cite_book&action=edit&redlink=1)
5. [↑](https://wiki.gentoo.org#cite_ref-.2Fetc_5-0) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
6. [↑](https://wiki.gentoo.org#cite_ref-6) [Define - /etc?](http://ask.slashdot.org/article.pl?sid=07/03/03/028258), Posted by Cliff, 3 March 2007 - Slashdot.
7. [↑](https://wiki.gentoo.org#cite_ref-.2Fopt_7-0) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
8. [↑](https://wiki.gentoo.org#cite_ref-.2Fsys_8-0) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
9. [↑](https://wiki.gentoo.org#cite_ref-9) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
10. [↑](https://wiki.gentoo.org#cite_ref-10) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
11. [↑](https://wiki.gentoo.org#cite_ref-12) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
12. [↑](https://wiki.gentoo.org#cite_ref-13) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
13. [↑](https://wiki.gentoo.org#cite_ref-14) [Template:Cite web](https://wiki.gentoo.org/index.php?title=Template:Cite_web&action=edit&redlink=1)
