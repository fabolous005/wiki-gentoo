<!-- source: https://wiki.gentoo.org/wiki/Libvirtd/Files | group: Gentoo Wiki (Main) | wiki-title: Libvirtd/Files -->
---
title: Libvirtd/Files
url: https://wiki.gentoo.org/wiki/Libvirtd/Files
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-30"
fingerprint: "8f1bc86a5ce25a07"
license: CC BY-SA 4.0
---

# Libvirtd/Files

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Files and directories

The libvirtd daemon and its linked utility code reference the following files, directories, and kernel interfaces.

### Configuration and system identity

| Path | Description | 
|---|---|
| /etc/libvirt/libvirtd.conf | Monolithic daemon configuration. | 
| /etc/libvirt/util.conf | Libvirt utility configuration. | 
| /etc/machine-id | Host machine identifier. | 
| /etc/pki/CA/cacert.pem | Certificate authority certificate for TLS connections. | 
| /etc/pki/libvirt/servercert.pem | Libvirt TLS server certificate. | 
| /etc/pki/libvirt/private/serverkey.pem | Private key for the libvirt TLS server certificate. | 

### Runtime sockets and process files

| Path | Description | 
|---|---|
| \[/var\]/run/libvirt/libvirt-admin-sock | Local libvirt administration API socket. | 
| \[/var\]/run/libvirt/libvirt-sock | Local read-write libvirt API socket. | 
| \[/var\]/run/libvirt/libvirt-sock-ro | Local read-only libvirt API socket. | 
| \[/var\]/run/pid/libvirt.pid | Daemon process ID file. | 
| /var/run/utmp | System login-session records. | 

### Kernel interfaces

| Path | Description | 
|---|---|
| /proc/devices | Registered character and block device types. | 
| /proc/filesystems | Filesystem types supported by the kernel. | 
| /proc/mounts | Active filesystem mount table. | 
| /proc/self/cgroup | Cgroup membership of the current process. | 
| /proc/%lld/cgroup | Cgroup membership of a process identified by its PID. | 
| /proc/self/status | Current process status and resource information. | 
| /proc/sys/kernel/cap\_last\_cap | Highest capability number supported by the kernel. | 
| /sys/devices/system/cpu/possible | Set of possible CPU IDs. | 
| /sys/devices/system/node/node0/meminfo | Memory statistics for NUMA node 0. | 
| /sys/devices/system/node | NUMA node device hierarchy. | 

### Per-user files and directories

| Path | Description | 
|---|---|
| $HOME/.cache | Default per-user cache directory. | 
| $HOME/.local/share | Default per-user application data directory. | 
| $HOME/.pki/libvirt/cacert.pem | Per-user certificate authority certificate. | 
| $HOME/.pki/libvirt/servercert.pem | Per-user TLS server certificate. | 
| $HOME/.pki/libvirt/serverkey.pem | Per-user TLS server private key. | 
| $XDG\_DATA\_HOME | Per-user application data directory; defaults to $HOME/.local/share. | 
| $XDG\_CACHE\_HOME | Per-user cache directory; defaults to $HOME/.cache. | 

### Locale

| Path | Description | 
|---|---|
| /usr/lib/locale/locale-archive | Compiled system locale database. | 

The following path notation is used:

- \[/var\] - Optional /var prefix; for example, /run/ or /var/run/.
- %lld - Decimal process ID substituted into the path.
- $HOME, $XDG\_DATA\_HOME, $XDG\_CACHE\_HOME - Environment-variable expansions.

Some paths are conditional on the build, runtime environment, or enabled functionality. A source-code reference does not necessarily mean that the path is opened on every daemon invocation.

### Domain definitions

The QEMU driver reads persistent domain XML definitions from the following paths:

| Operating mode | Path | URI | 
|---|---|---|
| System | /etc/libvirt/qemu/ | qemu:///system | 
| User session | $HOME/.config/libvirt/qemu/ | qemu:///session | 

For files used by the virsh client, see [Virsh/Files](https://wiki.gentoo.org/wiki/Virsh/Files). For additional libvirt driver files and directories, see [Libvirt/Files](https://wiki.gentoo.org/wiki/Libvirt/Files).
