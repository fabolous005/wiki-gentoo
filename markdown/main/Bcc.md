<!-- source: https://wiki.gentoo.org/wiki/Bcc | group: Gentoo Wiki (Main) | wiki-title: Bcc -->
---
title: bcc
url: https://wiki.gentoo.org/wiki/Bcc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-07"
fingerprint: a2c2aa72d354b2ea
license: CC BY-SA 4.0
---

# bcc

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**bcc** is a toolkit for creating efficient kernel tracing and manipulation programs, and includes several useful tools and examples. It makes use of [extended BPF](https://en.wikipedia.org/wiki/EBPF) (Berkeley Packet Filters), formally known as eBPF, a new feature that was first added to Linux 3.15. Much of what BCC uses requires Linux 4.1 and above.

## Installation

### Kernel configuration

KERNEL

```
General setup --->
  BPF subsystem --->
    -*- Enable bpf() system call 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BPF_SYSCALL</code> to find this item.
    [*] Enable BPF Just In Time compiler [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BPF_JIT</code> to find this item.
Networking support --->
  Networking options --->
    [*] QoS and/or fair queueing --->
      <M> BPF-based classifier [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NET_CLS_BPF</code> to find this item.
      [*] Actions --->
        <M> BPF based action [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NET_ACT_BPF</code> to find this item.


### USE flags


### USE flags for
            [dev-util/bcc](https://packages.gentoo.org/packages/dev-util/bcc)
            
            Tools for BPF-based Linux IO analysis, networking, monitoring, and more

| [+lua](https://packages.gentoo.org/useflags/+lua) | Enable Lua scripting support | 
| [+python](https://packages.gentoo.org/useflags/+python) | Add optional support/bindings for the Python language | 
| [debuginfod](https://packages.gentoo.org/useflags/debuginfod) | Enable debuginfod support via dev-libs/elfutils libdebuginfod | 
| [lzma](https://packages.gentoo.org/useflags/lzma) | Support for LZMA compression algorithm | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask dev-util/bcc`
## Usage

### biolatency

**biolatency** is a tool to summarize block device I/O latency as a histogram.

`root #``biolatency````
Tracing block device I/O... Hit Ctrl-C to end.
^C
     usecs               : count     distribution
         0 -> 1          : 0        |                                        |
         2 -> 3          : 0        |                                        |
         4 -> 7          : 0        |                                        |
         8 -> 15         : 0        |                                        |
        16 -> 31         : 13       |*****                                   |
        32 -> 63         : 94       |****************************************|
        64 -> 127        : 28       |***********                             |
       128 -> 255        : 1        |                                        |
       256 -> 511        : 0        |                                        |
       512 -> 1023       : 0        |                                        |
      1024 -> 2047       : 0        |                                        |
      2048 -> 4095       : 2        |                                        |
      4096 -> 8191       : 5        |**                                      |
      8192 -> 16383      : 7        |**                                      |
```
### execsnoop

**execsnoop** shows any new processes that are started after the execsnoop is run. The following example is the result of `emerge -vp app-editors/nano`.

`root #``execsnoop````
COMM             PID     PPID    RET ARGS
emerge           23937   23276     0 /usr/sbin/emerge -vp app-editors/nano
emerge           23937   23276     0 /usr/lib/python-exec/python3.14/emerge -vp app-editors/nano
python3.14       23938   23937     0 /usr/bin/python3.14 -c from multiprocessing.resource_tracker import main;main(10)
python3.14       23939   23937     0 /usr/bin/python3.14 -c from multiprocessing.forkserver import main; main(10, 12, ['__main__'], **{'sys_path': ['/usr/lib/python-exec/python3.14', '/usr
bash             23940   23939     0 /bin/bash -c [[ $(< <(echo foo) ) == foo ]]
ionice           23944   23939     0 /usr/sbin/ionice -c 3 -p 23937
ionice           23945   23939     0 /usr/sbin/ionice -c 3 -p 23939
renice           23946   23939     0 /usr/sbin/renice -n 19 23937
renice           23947   23939     0 /usr/sbin/renice -n 19 23939
stty             23948   23937     0 /usr/sbin/stty size
ldconfig         23950   23949     0 /sbin/ldconfig -p
find             23951   23937     0 /usr/sbin/find /etc/portage -name .* -type d -prune -o -name ._cfg????_* ! -name .*~ ! -iname .*.bak -print0
stty             23958   23937     0 /usr/sbin/stty size
```
### runqlat

**runqlat** analyzes the run queue (scheduler) latency as a histogram.

`root #``runqlat -m 5````
Tracing run queue latency... Hit Ctrl-C to end.
     msecs               : count     distribution
         0 -> 1          : 82161    |****************************************|
         2 -> 3          : 7        |                                        |
     msecs               : count     distribution
         0 -> 1          : 81322    |****************************************|
         2 -> 3          : 14       |                                        |
         4 -> 7          : 5        |                                        |
^C
     msecs               : count     distribution
         0 -> 1          : 17190    |****************************************|
```
### stackcount

`root #``stackcount submit_bio````
Tracing 1 functions for "submit_bio"... Hit Ctrl-C to end.
^C
  submit_bio
  xlog_state_release_iclog
  xlog_force_lsn
  xfs_log_force_seq
  xfs_file_fsync
  do_fsync
  __x64_sys_fsync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
    1
  submit_bio
  xlog_state_release_iclog
  xlog_force_lsn
  xfs_log_force_seq
  xfs_file_fsync
  do_fsync
  __x64_sys_fsync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
  [unknown]
    1
  submit_bio
  xlog_state_release_iclog
  xlog_force_lsn
  xfs_log_force_seq
  xfs_file_fsync
  do_fsync
  __x64_sys_fdatasync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
    1
  submit_bio
  iomap_ioend_writeback_submit
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  filemap_fdatawrite_wbc
  __filemap_fdatawrite_range
  file_write_and_wait_range
  xfs_file_fsync
  do_fsync
  __x64_sys_fsync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
  [unknown]
    1
  submit_bio
  xlog_state_release_iclog
  xlog_force_lsn
  xfs_log_force_seq
  do_fsync
  __x64_sys_fsync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
    1
  submit_bio
  iomap_ioend_writeback_submit
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  filemap_fdatawrite_wbc
  __filemap_fdatawrite_range
  file_write_and_wait_range
  xfs_file_fsync
  do_fsync
  __x64_sys_fdatasync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
    1
  submit_bio
  iomap_ioend_writeback_submit
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  filemap_fdatawrite_wbc
  __filemap_fdatawrite_range
  file_write_and_wait_range
  xfs_file_fsync
  do_fsync
  __x64_sys_fsync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
  [unknown]
    1
  submit_bio
  iomap_ioend_writeback_submit
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  filemap_fdatawrite_wbc
  __filemap_fdatawrite_range
  file_write_and_wait_range
  xfs_file_fsync
  do_fsync
  __x64_sys_fdatasync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
    2
  submit_bio
  xlog_state_release_iclog
  xlog_write_get_more_iclog_space
  xlog_write_partial
  xlog_write
  xlog_cil_push_work
  process_one_work
  worker_thread
  kthread
  ret_from_fork
  ret_from_fork_asm
    2
  submit_bio
  iomap_ioend_writeback_submit
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  __writeback_single_inode
  writeback_sb_inodes
  __writeback_inodes_wb
  wb_writeback
  wb_workfn
  process_one_work
  worker_thread
  kthread
  ret_from_fork
  ret_from_fork_asm
    3
  submit_bio
  iomap_ioend_writeback_submit
  iomap_add_to_ioend
  xfs_writeback_range
  iomap_writeback_folio
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  __writeback_single_inode
  writeback_sb_inodes
  __writeback_inodes_wb
  wb_writeback
  wb_workfn
  process_one_work
  worker_thread
  kthread
  ret_from_fork
  ret_from_fork_asm
    3
  submit_bio
  submit_bio_wait
  blkdev_issue_flush
  xfs_file_fsync
  do_fsync
  __x64_sys_fdatasync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
    4
  submit_bio
  iomap_ioend_writeback_submit
  iomap_add_to_ioend
  xfs_writeback_range
  iomap_writeback_folio
  iomap_writepages
  xfs_vm_writepages
  do_writepages
  filemap_fdatawrite_wbc
  __filemap_fdatawrite_range
  file_write_and_wait_range
  xfs_file_fsync
  do_fsync
  __x64_sys_fdatasync
  do_syscall_64
  entry_SYSCALL_64_after_hwframe
  [unknown]
    86
Detaching...
```
### vfsstat

**vfsstat** provides statistics for common VFS calls.

`root #``vfsstat`
TIME         READ/s  WRITE/s  FSYNC/s   OPEN/s CREATE/s UNLINK/s  MKDIR/s  RMDIR/s
23:40:36:      6730     2806        5      161        0        2        0        0
23:40:37:      6798     2725        0      124        0        2        0        0
23:40:38:      5692     2304        0       83        0        2        0        0
23:40:39:      5839     2556        0      159        0        0        0        0
23:40:40:      6036     2361        0       82        0        0        0        0
23:40:41:      5507     2520        2       94        0        0        0        0
23:40:42:      4119     1875        0      340        0        0        0        0
23:40:42:       616      320        0       37        0        0        0        0

### xfsslower

**xfsslower** is a tool to trace common XFS operations that are slower than N milliseconds.

`root #``xfsslower`
Tracing XFS operations slower than 3 ms
TIME     COMM           PID    T BYTES   OFF\_KB   LAT(ms) FILENAME
23:42:28 Permission     26739  S 0       0          14.38 permissions.sqlite-journal
23:42:28 Permission     26739  S 0       0           6.43 permissions.sqlite
23:42:30 CFileWriterThr 28723  S 0       0          16.68 localconfig.vdf.async28723.tmp
