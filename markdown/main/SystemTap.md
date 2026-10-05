<!-- source: https://wiki.gentoo.org/wiki/SystemTap | group: Gentoo Wiki (Main) | wiki-title: SystemTap -->
---
title: SystemTap
url: https://wiki.gentoo.org/wiki/SystemTap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-19"
fingerprint: b4d3f90b0da2afe4
license: CC BY-SA 4.0
---

# SystemTap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**SystemTap** (stap) is a powerful tool that provides an infrastructure to simplify the gathering of information about the running Linux kernel or userspace programs<sup>[\[1\]](https://wiki.gentoo.org#cite_note-SystemTap_-_Debian_Wiki-1)</sup>. It allows users to write and reuse simple scripts to deeply examine the activities of a running Linux system. These scripts can be designed to extract data, filter it, and summarize it quickly (and safely), enabling the diagnosis of complex performance (or even functional) problems.[\[2\]](https://wiki.gentoo.org#cite_note-SystemTap_Beginners_Guide-2)

## How it Works

SystemTap scripts are written in the SystemTap scripting language, are then compiled to C-code kernel modules and inserted into the kernel. This allows the scripts to instrument the execution of functions or statements in the kernel or user-space.

## Usage

SystemTap provides a command line interface and a scripting language to examine the activities of a running Linux system, particularly the kernel, in fine detail.

### Kernel

As SystemTap taps into the kernel at a low level, it requires that debug symbols be enabled (DWARF5, specifically) - for Gentoo this means reconfiguring the kernel<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

For [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources):

**debug symbol generation**

**`FILE /etc/kernel/config.d/99-debugsyms.config`**

Additional options that are probably already enabled: `CONFIG_KPROBES`, `CONFIG_RELAY`, `CONFIG_DEBUG_FS`, `CONFIG_MODULES`, `CONFIG_MODULE_UNLOAD`, `CONFIG_UPROBES`.

Users should try to reduce the number of modules / enabled options for an instrumented kernel where possible—`CONFIG_DEBUG_INFO` can multiply disk space usage. Be sure to leave `CONFIG_DEBUG_INFO_SPLIT` disabled; SystemTap doesn't handle split debuginfo yet.

### Installation

`root #``emerge --ask dev-util/systemtap`
### Basic Usage

After installation, a basic probe to read the VFS can be performed to validate SystemTap functionality:

`root #````
stap -v -e 'probe vfs.read {printf("read performed\n"); exit()}'
```
Pass 1: parsed user script and 45 library script(s) in 340usr/0sys/358real ms.
Pass 2: analyzed script: 1 probe(s), 1 function(s), 0 embed(s), 0 global(s) in 290usr/260sys/568real ms.
Pass 3: translated to C into "/tmp/stapiArgLX/stap\_e5886fa50499994e6a87aacdc43cd392\_399.c" in 490usr/430sys/938real ms.
Pass 4: compiled C into "stap\_e5886fa50499994e6a87aacdc43cd392\_399.ko" in 3310usr/430sys/3714real ms.
Pass 5: starting run.
read performed
Pass 5: run completed in 10usr/40sys/73real ms.

This command instructs SystemTap to print read performed and then exit properly once a virtual file system read is detected. If the SystemTap deployment was successful, it prints output similar to the above; the last three lines of the output (beginning with Pass 5) indicate that SystemTap was able to successfully create the instrumentation to probe the kernel, run the instrumentation, detect the event being probed (in this case, a virtual file system read), and execute a valid handler (print text then close it with no errors)<sup>[\[4\]](https://wiki.gentoo.org#cite_note-stap-validation-4)</sup>.

## Viewing Kernel Information

SystemTap can be used to view information about the kernel in various ways. For example, it can be used to identify the top system calls used by the system. It can also be used to determine which processes are performing the highest volume of system calls, providing more data in investigating systems for polling processes and other resource hogs.

### Real-world Usage

This example describes using SystemTap to view the \`inet\_getname\` function which was identified as the source of the following nfsd errors: `nfsd: peername failed (err 107)!`

**`nfsd_peername.stp`**

When run, the above script will log calls made to `inet_gentame` from binaries named nfsd, as well as the return value:

`root #``vim nfsd_peername.stp``root #``stap -v nfsd_peername.stp`
Pass 1: parsed user script and 114 library scripts using 57340virt/40276res/5700shr/35220data kb, in 120usr/10sys/130real ms.
Pass 2: analyzed script: 6 probes, 24 functions, 9 embeds, 3 globals using 228712virt/213276res/7376shr/206592data kb, in 2270usr/570sys/2737real ms.
Pass 3: translated to C into "/tmp/stapkilbvn/stap\_aaaa6994e39808fec232416d081ab400\_33413\_src.c" using 228712virt/213468res/7568shr/206592data kb, in 10usr/20sys/32real ms.
Pass 4: compiled C into "stap\_aaaa6994e39808fec232416d081ab400\_33413.ko" in 7670usr/1440sys/8936real ms.
Pass 5: starting run.
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.84 port: 750 state: TCP\_CLOSE\_WAIT
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: 0
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.76 port: 940 state: TCP\_CLOSE
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: -107
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.72 port: 671 state: TCP\_CLOSE
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: -107
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.82 port: 742 state: TCP\_CLOSE
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: -107
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.79 port: 749 state: TCP\_CLOSE
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: -107
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.62 port: 886 state: TCP\_ESTABLISHED
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: 0
Thu Dec  7 15:19:49 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.93 port: 861 state: TCP\_ESTABLISHED
Thu Dec  7 15:19:49 2023 AEST nfsd \<- inet\_getname ret: 0
Thu Dec  7 15:19:50 2023 AEST nfsd -> inet\_getname addr: 10.xxx.xxx.76 port: 940 state: TCP\_ESTABLISHED
Thu Dec  7 15:19:50 2023 AEST nfsd \<- inet\_getname ret: 0

## See Also

- [DTrace](https://wiki.gentoo.org/wiki/DTrace) — dynamic tracing tool for analysing or debugging the whole system
