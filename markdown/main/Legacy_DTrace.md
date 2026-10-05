<!-- source: https://wiki.gentoo.org/wiki/Legacy_DTrace | group: Gentoo Wiki (Main) | wiki-title: Legacy DTrace -->
---
title: Legacy DTrace
url: https://wiki.gentoo.org/wiki/Legacy_DTrace
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-18"
fingerprint: "1e10317e9be7bbaf"
license: CC BY-SA 4.0
---

# Legacy DTrace

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## DTrace on Gentoo

### Overlay repository

The ebuilds for DTrace are placed (in time of writing) in GitHub repository.

To activate the repository just add new repository configuration to your Portage config.

**`/etc/portage/repos.conf/dtrace`**

Then sync Portage tree.

### Installation

DTrace support on Gentoo consists of kernel sources with patches and userspace utilities. It is packed in 3 packages:

- libctf => library for CTF debug info handling, needed for both the kernel and user utilities
- dtrace-user => userspace part, mainly the dtrace(1) command and libdtrace
- dtrace-sources => kernel 4.14.x sources containing dtrace patches and working initial .config

The default installation can be done installing just the dtrace-user:

`root #``emerge --ask dtrace-user`
### Kernel

Build the kernel your way. The main difference to normal building is executing make ctf before kernel installation. This will generate CTF debug data for DTrace to work.

Example steps:

`root #````
cd /usr/src/linux-4.14.28-dtrace
```
`root #````
make oldconfig
```
`root #````
make
```
`root #````
make ctf
```
`root #````
make install
```
`root #````
make modules_install
```
### Test it

When everything passed correctly you may try some basic DTrace one-liner.

`root #````
dtrace -n 'proc:::exec-success { trace(curpsinfo->pr_psargs); }'
```
dtrace: description 'proc:::exec-success ' matched 1 probe
CPU     ID                    FUNCTION:NAME
  3   2017  do\_execveat\_common:exec-success   /usr/bin/konsole                 
  3   2017  do\_execveat\_common:exec-success   git --version                    
  3   2017  do\_execveat\_common:exec-success   grep --color=auto                
  3   2017  do\_execveat\_common:exec-success   grep --exclude-dir=.cvs          
  3   2017  do\_execveat\_common:exec-success   dircolors -b                     
  3   2017  do\_execveat\_common:exec-success   ls --color -d .

`Ctrl`+

`c`

## References

To start learning DTrace check some informative guides:
