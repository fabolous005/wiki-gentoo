<!-- source: https://wiki.gentoo.org/wiki/Toybox | group: Gentoo Wiki (Main) | wiki-title: Toybox -->
---
title: Toybox
url: https://wiki.gentoo.org/wiki/Toybox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-16"
fingerprint: "90a33de335029a91"
license: CC BY-SA 4.0
---

# Toybox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**toybox** is a utility that combines tiny versions of many common UNIX utilities into a *single, small executable*.

## Installation

### USE flags


### Emerge

`root #``emerge --ask sys-apps/toybox`
## Usage

### Invocation

To view usage information use

`user $``toybox --help````
Toybox 0.8.6 multicall binary: https://landley.net/toybox (see toybox --help)
usage: toybox [--long | --help | --version | [COMMAND] [ARGUMENTS...]]
With no arguments, "toybox" shows available COMMAND names. Add --long
to include suggested install path for each command, see
https://landley.net/toybox/faq.html#install for details.
First argument is name of a COMMAND to run, followed by any ARGUMENTS
to that command. Most toybox commands also understand:
--help          Show command help (only)
--version       Show toybox version (only)
The filename "-" means stdin/stdout, and "--" stops argument parsing.
Numerical arguments accept a single letter suffix for
kilo, mega, giga, tera, peta, and exabytes, plus an additional
"d" to indicate decimal 1000's instead of 1024.
Durations can be decimal fractions and accept minute ("m"), hour ("h"),
or day ("d") suffixes (so 0.1m = 6s).
```
Available functions:

`user $``toybox` \[ acpi arch ascii base32 base64 basename blkdiscard blkid blockdev bunzip2 bzcat cal cat catv
chattr chgrp chmod chown chroot chrt chvt cksum clear cmp comm count cp cpio crc32 cut date
devmem df dirname dmesg dnsdomainname dos2unix du echo egrep eject env expand factor fallocate
false fgrep file find flock fmt free freeramdisk fsfreeze fstype fsync ftpget ftpput getconf
grep groups gunzip halt head help hexedit hostname hwclock i2cdetect i2cdump i2cget i2cset
iconv id ifconfig inotifyd insmod install ionice iorenice iotop kill killall killall5 link
linux32 ln logger login logname losetup ls lsattr lsmod lspci lsusb makedevs mcookie md5sum
microcom mix mkdir mkfifo mknod mkpasswd mkswap mktemp modinfo mount mountpoint mv nbd-client
nc netcat netstat nice nl nohup nproc nsenter od oneit partprobe passwd paste patch pgrep pidof
ping ping6 pivot\_root pkill pmap poweroff printenv printf prlimit ps pwd pwdx pwgen readahead
readelf readlink realpath reboot renice reset rev rfkill rm rmdir rmmod rtcwake sed seq setfattr
setsid sha1sum sha224sum sha256sum sha384sum sha3sum sha512sum shred sleep sntp sort split
stat strings su swapoff swapon switch\_root sync sysctl tac tail tar taskset tee test time timeout
top touch true truncate tty tunctl ulimit umount uname unicode uniq unix2dos unlink unshare
uptime usleep uudecode uuencode uuidgen vconfig vmstat w watch watchdog wc which who whoami
xargs xxd yes zcat

To create one html file with usage informations for all commands run:

`user $``toybox help -a -h > /tmp/toybox.html`
Open toybox.html in a web browser.

### Using toybox functions

To use a tool specifically from toybox, execute toybox, followed by the utility's (function's) name. For example, the following command will run toybox's uname utility:

`user $``toybox uname -a`
Linux pi400 5.10.63-v8 #1 SMP PREEMPT Sun Dec 5 15:48:04 CET 2021 aarch64 Toybox

## See also

- [Busybox](https://wiki.gentoo.org/wiki/Busybox) — a utility that combines tiny versions of many common UNIX utilities into a *single, small executable*.
