<!-- source: https://wiki.gentoo.org/wiki/Rr | group: Gentoo Wiki (Main) | wiki-title: Rr -->
---
title: rr
url: https://wiki.gentoo.org/wiki/Rr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-16"
fingerprint: e088405341390186
license: CC BY-SA 4.0
---

# rr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**rr**, **R**ecord and **R**eplay Framework, is a C/C++ debugging tool for Linux that extends [GDB](https://wiki.gentoo.org/wiki/GDB) with 'time-travel debugging'. It provides an efficient reverse execution under GDB, allowing for the replaying of recorded instructions.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-debug/rr`
## Usage

The important thing to understand is that *rr* allows hitting a crash, then going backwards to inspect state *before the crash happens*. Suppose a function *foo* gets called many (at least 20) times and it crashes the last time: setting a breapkoint on *foo* will hit many useless instances. To debug it, one needs the last one.

Using rr record ... to record the crash, then rr replay to open an rr-infused gdb session, then *b foo* to set a breakpoint on *foo*, finally by using *reverse-continue*, the session will land on the penultimate *foo* call which crashed.

### Debugging a segmentation fault

Take the following C file for example:

Compile the file with the `-g` flag:

`user $``gcc -ggdb3 main.c`
Upon running this file after compilation, the following is output:

`user $``./a.out`
Hello world!
Upgrading cookie to a new number...
Time to crash!
Aborted                    (core dumped) ./a.out

To investigate the problem with rr, use `rr record`:

`user $``rr record ./a.out`
rr: Saving execution to trace directory \`/home/larry/.local/share/rr/a.out-1'.
On Zen CPUs, rr will not work reliably unless you disable the hardware SpecLockMap optimization.
For instructions on how to do this, see https://github.com/rr-debugger/rr/wiki/Zen
Hello world!
Upgrading cookie to a new number...
Time to crash!
Aborted                    rr record ./a.out

Now replay the file using `rr replay`:

`user $``rr replay`
On Zen CPUs, rr will not work reliably unless you disable the hardware SpecLockMap optimization.
For instructions on how to do this, see https://github.com/rr-debugger/rr/wiki/Zen
Reading symbols from /home/larry/.local/share/rr/a.out-1/mmap\_copy\_4\_a.out...
Remote debugging using 127.0.0.1:35159
Reading symbols from /lib64/ld-linux-x86-64.so.2...
warning: BFD: warning: system-supplied DSO at 0x6fffd000 has a section extending past end of file
warning: Discarding section .replay.text which has an invalid size (27) \[in module system-supplied DSO at 0x6fffd000\]
0x00007f5269447d40 in \_start () from /lib64/ld-linux-x86-64.so.2
(rr)

Let it crash:

`(rr)``continue`
Continuing.
Hello world!
Upgrading cookie to a new number...
Time to crash!
Program received signal SIGABRT, Aborted.
\_\_pthread\_kill\_implementation (threadid=\<optimized out>, signo=6, no\_tid=0) at pthread\_kill.c:44
44            return INTERNAL\_SYSCALL\_ERROR\_P (ret) ? INTERNAL\_SYSCALL\_ERRNO (ret) : 0;

Look at the backtrace:

`(rr)``bt`
#0  \_\_pthread\_kill\_implementation (threadid=\<optimized out>, signo=6, no\_tid=0) at pthread\_kill.c:44
#1  \_\_pthread\_kill\_internal (threadid=\<optimized out>, signo=6) at pthread\_kill.c:89
#2  \_\_GI\_\_\_pthread\_kill (threadid=\<optimized out>, signo=signo@entry=6) at pthread\_kill.c:100
#3  0x00007f5269021042 in \_\_GI\_raise (sig=sig@entry=6) at ../sysdeps/posix/raise.c:26
#4  0x00007f52690013a1 in \_\_GI\_abort () at abort.c:73
#5  0x000055cea0a1e509 in main () at main.c:14

Look at the code around the failure, by first entering the right frame:

`(rr)``frame 5`
#5  0x000055cea0a1e509 in main () at main.c:14
14              \_\_builtin\_abort();

Then list surrounding code:

`(rr)``list````
9               printf("Upgrading cookie to a new number...\n");
10              cookie = 3;
11
12              printf("Time to crash!\n");
13              cookie = 0;
14              __builtin_abort();
15      }
```
Set a watchpoint on `cookie`:

`(rr)``watch cookie`
Hardware watchpoint 1: cookie

Reverse continue (rewind) until the breakpoint is hit:

`(rr)``reverse-continue`
Continuing.
Program received signal SIGABRT, Aborted.
\_\_pthread\_kill\_implementation (threadid=\<optimized out>, signo=6, no\_tid=0) at pthread\_kill.c:44
44            return INTERNAL\_SYSCALL\_ERROR\_P (ret) ? INTERNAL\_SYSCALL\_ERRNO (ret) : 0;

Do it again:

`(rr)``reverse-continue`
Continuing.
Hardware watchpoint 1: cookie
Old value = 0
New value = 3
main () at main.c:13
13              cookie = 0;

The value of *cookie* can now be seen just before the "Time to crash" message was '3'.

## Troubleshooting

### AMD Zen CPUs

Owners of AMD Zen CPUs may need to activate a [workaround](https://github.com/rr-debugger/rr/wiki/Zen).

systemd users can run:

`root #``systemctl start rr-zen_workaround.service`
There isn't an OpenRC service available at present, but /usr/bin/rr-zen\_workaround.py can be invoked directly.

## See also

- [GDB](https://wiki.gentoo.org/wiki/GDB) — used to investigate runtime errors that normally involve memory corruption
