<!-- source: https://wiki.gentoo.org/wiki/S390/z/VM_tips_and_tricks | group: Gentoo Wiki (Main) | wiki-title: S390/z/VM tips and tricks -->
---
title: S390/z/VM tips and tricks
url: https://wiki.gentoo.org/wiki/S390/z/VM_tips_and_tricks
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-01"
fingerprint: "173b9eeb2c837abf"
license: CC BY-SA 4.0
---

# S390/z/VM tips and tricks

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

These are some tips and tricks that are useful for z/VM.

## CMS

See [IBM's z/VM documentation for all CMS commands](https://www.ibm.com/docs/en/zvm/7.1?topic=reference-cms-commands).

| COMMAND | DESCRIPTION | EXAMPLE | 
|---|---|---|
| LISTFILES | List files |  | 
| FILELIST | another list files, interactive |  | 
| ERASE | Erases files | ERASE FILENAME DISK | 
| COPY | Copies files | FIXME! | 
| RENAME | Renames files | FIXME! | 

## CP

See [IBM's z/VM documentation for all CP commands](https://www.ibm.com/docs/en/zvm/7.1?topic=reference-cp-commands).

| COMMAND | DESCRIPTION | EXAMPLE | 
|---|---|---|
| DISCONNECT | Disconnect from the current VM, but leave the VM running |  | 
| LOGOFF | Shut the current VM off |  | 
| IPL | Restart the VM and launch the Initial Program Loader (i.e. the bootloader) |  | 
| I | Shortcut for the IPL command |  | 
| IPL `NUMBER` | Restart the VM and IPL a specific image | IPL 150 | 
| IPL CMS | Restart the VM and launch CMS |  | 
| QUERY `ARGS` | Query general VM state |  | 
| Q `ARGS` | Shortcut to the `QUERY` command |  | 
| Q CPUS | List allocated cpus |  | 
| Q STORAGE | List allocated memory |  | 
| Q DASD | List allocated disks |  | 
| Q DISK | List attached disks |  | 
| Q ACCESSED | List disks available under CP/CMS |  | 
| Q OSA | List allocated network devices |  | 
| Q READER ALL | Show all queued files in the reader spool queue |  | 
| LINK `user rdev ldev mode` PASS=`password` | Link a DASD from a different VM into the current one. | LINK lgentoo4 150 152 RR PASS=... | 
| DETACH `dev` | Detach a DASD that was previously linked in. | DETACH 152 | 
| VI VMSG `NUMBER ARGS` | After launching the IPL, select `NUMBER` and append `ARGS` to the program's command line (e.g. the Linux kernel's command line) | VI VMSG 0 init=/bin/bb | 
| CP SET RUN ON | Maintain VM running when disconnecting from z/VM |  | 

## Linux

To run CP commands from Linux while in the x3270 session, type: `#CP` , e.g. `COMMAND``#CP SYSTEM CLEAR`. This would stop the VM.

## References

- [https://www.ibm.com/docs/en/zvm/](https://www.ibm.com/docs/en/zvm/)
- [https://sar.informatik.hu-berlin.de/summit07/02-1\_Intro\_.pdf](https://sar.informatik.hu-berlin.de/summit07/02-1_Intro_.pdf)
- [CP/CMS Wikipedia Page](https://en.wikipedia.org/wiki/CP/CMS)
