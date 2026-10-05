<!-- source: https://wiki.gentoo.org/wiki/Libvirt/libvirtd | group: Gentoo Wiki (Main) | wiki-title: Libvirt/libvirtd -->
---
title: libvirt/libvirtd
url: https://wiki.gentoo.org/wiki/Libvirt/libvirtd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-24"
fingerprint: ae0fdc4a6fa7d98e
license: CC BY-SA 4.0
---

# libvirt/libvirtd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

.

libvirtd is a daemon for [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) management of virtual machines.

The daemon is used to ensure consistent handling of virtual machines, containers, and domains.

[Libvirt](https://wiki.gentoo.org/wiki/Libvirt) (and libvirtd) currently supports [QEMU](https://wiki.gentoo.org/wiki/QEMU), [LXC](https://wiki.gentoo.org/wiki/LXC), [Xen](https://wiki.gentoo.org/wiki/Xen), [KVM](https://wiki.gentoo.org/wiki/KVM), [OpenVZ](https://wiki.gentoo.org/wiki/OpenVZ), [VirtualBox](https://wiki.gentoo.org/wiki/VirtualBox) and [VMware](https://wiki.gentoo.org/wiki/VMware) ESX.



## Installation

See [libvirt](https://wiki.gentoo.org/wiki/Libvirt) for installation of [libvirtd].



## Configuration

### Environment variables

A list of optional environment variables that are read and checked by the virsh command:

| Optional environment variable name | Description | Type | 
|---|---|---|
| `LIBVIRT_AUTH_FILE` | authentication, when libvirt clients (virsh, virt-manager) connecting to libvirtd daemon. contains authentication credentials for connecting to a libvirt daemon (like libvirtd or virtqemud) when using certain authentication mechanisms, particularly Polkit or SASL.  Its file format is a simple INI containing a **\[credentials\]** section line followed by a **username=** and a **password=** keyvalue lines.   If `LIBVIRT_AUTH_FILE` is explicitly set, libvirt uses that.  If not set, libvirt checks $XDG\_CONFIG\_HOME/libvirt/auth.conf;  If `XDG_CONFIG_HOME` is unset, then $HOME/.config/libvirt/auth.conf.  SECURITY: The file should be readable only by the user. | file path | 
| `LIBVIRT_DEFAULT_URI` | specify the default connection URI to a hypervisor or virtualization backend (like QEMU/KVM, Xen, LXC, etc.).  It tells libvirt-based tools (like virsh, virt-manager, or libvirt API clients) which hypervisor and transport to connect to by default, when a URI is not explicitly given.  Default for regular users is qemu:///session, for root is qemu:///system. See **virt-admin(1)** manpage. | URI | 
| `LIBVIRT_LOG_FILTERS` | Filter list in which to select a category of debug outputs.  A **component:level** colon-pair, each separated by commas. | The **level**s are **error**, **warning**, **info**, **debug**, **trace**.; Some **component**s are **qemu**, **network**, **storage**, **cpu**, **driver**, **interface**, **xml**, **domain**, **migration**, **security**, **host**, **api**, **config**, **event**, **util**. | 
| `QEMU_AUDIO_DRV` | type of audio driver to select | **pa**, **none**, **alsa**, **coreaudio**, **jack**, **oss**, **pipewire**, **sdl**, **spice** or **wav**. | 
| `SDL_AUDIODRIVER` | SDL (Simple DirectMedia Layer) to control which audio backend it uses on your system. | **pipewire**, **pulseaudio**, **esd**, **alsa**, **jack**, **dsp**, or **arts**. | 
| `VIRTD_PATH` | find the libvirtd executable.  If not found or defined, fallback to `LIBVIRTD_PATH`. | colon-separated directory path(s) | 
| `LIBVIRTD_PATH` | find the libvirtd executable; colon-separated directory spec(s).  If `VIRTD_PATH` not found nor defined, this variable is tried. | colon-separated directory path(s) | 


For debug-related envvar:

| Debug environment variable names | Description | Type | 
|---|---|---|
| `LIBVIRT_DEBUG` | sets the verbosity level of libvirt logging. It works in conjunction with `LIBVIRT_LOG_OUTPUTS` to define how much detail is included in logs.  It tells libvirt how detailed the log messages should be. | verbosity level: 0 (fewer messages) to 4 (most verbose) | 
| `LIBVIRT_LOG_OUTPUTS` | control where log messages go (stdout, stderr, file, etc.) and what gets logged. File must be created manually. | filepath | 
| `LIBVIRT_GNUTLS_DEBUG` | enable GNUTLS debug, if defined | (none) | 
| `LIBVIRT_LIBSSH_DEBUG` | enable LIBSSH debug, if defined; use `TRACE_LIBSSH=<level>` to define debug level. | (none) | 


For [systemd](https://wiki.gentoo.org/wiki/Systemd) support:

| systemd environment variable name | Description | Type | 
|---|---|---|
| `LISTEN_FDS` | informs a service how many file descriptors have been passed to it by socket activation. For systemd only. | file descriptor ID | 
| `LISTEN_PID` | the PID of the process the file descriptors are for (should match your PID). For systemd only. | Process ID | 
| `NOTIFY_SOCKET` | inform systemd of its actual UNIX socket path for the client. for systemd-only. | Unix domain socket path | 


For libvirt\_leasehelper during network setup:

| libvirt\_leasehelper environment variable name | Description | Type | 
|---|---|---|
| `DNSMASQ_CLIENT_ID` | a custom DHCP client ID set by the VM's DHCP client, or a MAC address, depending on how the VM was configured. Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | 14-hex, 14-hex, colon-separated, or user-defined string | 
| `DNSMASQ_IAID` | the 32-bit IAID provided by the DHCP IPv6 client. Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | 32-bit Decimal, or Hex (prefix, no-prefix, colon-separated). | 
| `DNSMASQ_INTERFACE` | The bridge network interface name on which the lease event occurred. Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | network interface name | 
| `DNSMASQ_LEASE_EXPIRES` | Unix timestamp when the lease will expire (or 0 if expired). Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | UNIX timestamp (i.e., seconds since the epoch: Jan 1, 1970). | 
| `DNSMASQ_OLD_HOSTNAME` | Old hostname before the current lease change (as sent by client via DHCP option 12). Set by dnsmasq when a DHCP lease update causes a client's hostname to change. Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. May be empty if previous not set. | hostname/DNS label (RFC1036/RFC1123) | 
| `DNSMASQ_SERVER_DUID` | uniquely identifies a DHCPv6 server. Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | hostname (RFC3315) | 
| `DNSMASQ_SUPPLIED_HOSTNAME` | Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | hostname/DNS label (RFC1036/RFC1123) | 
| `VIR_BRIDGE_NAME` | contains the hostname explicitly supplied by the client during its DHCP request --typically via DHCPv4 option 12 (Host Name) or DHCPv6 option 39 (FQDN or Client FQDN).  It's different from `DNSMASQ_HOSTNAME`, which may include DNS-derived or static-host config values.  Used by libvirt\_leaseshelper with dnsmasqd during bridge driver. | simple Linux bridge name | 


Part of the XDG Base Directory Specification, which standardizes where user-specific files for GUI support should be stored:

| XDG environment variable name | Description | Type | 
|---|---|---|
| `XDG_RUNTIME_DIR` | Used to find the $XDG\_RUNTIME\_DIR/libvirt/ directory. | directory path | 
| `XDG_CONFIG_HOME` | It specifies the base directory where user-specific configuration files should be stored.  If `XDG_CONFIG_HOME` is not set, applications default to: $HOME/.config. | file path | 
| `XDG_DATA_HOME` | store non-essential, user-specific data—things like icons, application state (like a game save), caches of downloaded data, plugin data, fonts installed only for the user.  If `XDG_DATA_HOME` is not set, it defaults to $HOME/.local/share. | directory path | 
| `XDG_CACHE_HOME` | location for user-specific non-essential cached data files. It's used by applications to store cached data that can be safely deleted without losing important information, it will be regenerated by the app if needed. If not explicitly set, it defaults to $HOME/.cache. | file path | 

For VirtualBox support:

| VirtualBox environment variable name | Description | Type | 
|---|---|---|
| `DISPLAY` | the display X server and screen to which graphical applications should be sent. For virtualbox-only | X Display Offset | 
| `VBOX_APP_HOME` | for virtualbox-only | directory path | 


Deprecated goes here:

| Deprecated environment variable name | Description | Type | 
|---|---|---|
| `LIBVIRT_ADMIN_DEFAULT_URI` | administration, see **virt-admin(1)** manpage. (Deprecated; use `LIBVIRT_DEFAULT_URI`) | URI | 
| `VIRSH_DEFAULT_CONNECT_URI` | administration, see **virsh(1)** manpage. (Deprecated; used `LIBVIRT_DEFAULT_URI`). | URI | 

A list of optional environment variables that are read and checked by the libvirt.so library linked by libvirtd:

| libvirt environment variable name | Description | Type | 
|---|---|---|
| `LC_ALL` | to force all locale settings (like language, number formatting, collation, etc.) for programs running in your environment. It overrides all other locale-related variables such as: LANG    LC\_CTYPE, LC\_TIME, LC\_NUMERIC, LC\_MESSAGES, etc. | locale name | 
| `LD_PRELOAD` | Overrides library search paths; useful for debugging alternative GTK or QEMU builds. Useful for debugging by swapping out `malloc()` function.  Colon-separated for 2 or more directories. | Directory Path | 
| `LD_LIBRARY_PATH` | Overrides library search paths; useful for debugging alternative GTK or QEMU builds. | colon-separated directory paths | 
| `DYLD_INSERT_LIBRARIES` | works similarly to `LD_PRELOAD` but for macOS platforms. SIP blocks this for macOS system binaries. Only works with `DYLD_FORCE_FLAT_NAMESPACE=1` setting. | colon-separated directory path(s) | 
| `DYLD_FORCE_FLAT_NAMESPACE` | macOS has 2-level deep namespace.  Set to `1` to flatten. May cause symbol collisions. | digit | 
| `USER` | login username of the current user. Often used by shell scripts. | username | 
| `LOGNAME` | Use by system (e.g., login, at). | username | 
| `TMPDIR` | temporary directory where programs should write temporary files. | directory path | 

### Files

Files that are read by the host-side OS libvirtd daemon command:

- /etc/libvirt/libvirtd.conf
- /etc/libvirt/util.conf
- /etc/machine-id
- /etc/pki/CA/cacert.pem
- /etc/pki/libvirt/servercert.pem
- /etc/pki/libvirt/private/serverkey.pem
- /proc/devices
- /proc/filesystems
- /proc/mounts
- /proc/self/cgroup
- /proc/%lld/cgroup
- /proc/self/status
- /proc/sys/kernel/cap\_last\_cap
- \[/var\]/run/libvirt/libvirt-admin-sock
- \[/var\]/run/libvirt/libvirt-sock
- \[/var\]/run/libvirt/libvirt-sock-ro
- \[/var\]/run/pid/libvirt.pid
- /sys/devices/system/cpu/possible
- /sys/devices/system/node/node0/meminfo
- /sys/devices/system/node
- /usr/lib/locale/locale-archive
- /var/run/utmp
- $HOME/.cache
- $HOME/.local/share
- $HOME/.pki/libvirt/cacert.pem
- $HOME/.pki/libvirt/servercert.pem
- $HOME/.pki/libvirt/serverkey.pem
- $XDG\_DATA\_HOME
- $XDG\_CACHE\_HOME



### User permissions

libvirtd runs in either root (for system mode) or as a non-root user (for session mode).

See [libvirt configuration](https://wiki.gentoo.org/wiki/Libvirt#Configuration) for more setup on enabling a non-root user to use the libvirtd command.



### Service

Typically, libvirtd daemon is run in system mode by the system startup script (like ATT SysV, rc.local, systemd).  See [libvirt service](https://wiki.gentoo.org/wiki/Libvirt#Service) for more commands.

A quick test to see if the libvirtd daemon is up and running from the command line in one of three ways:



#### libvirtd service - status by CLI

`host-root#``virsh list --all`
virsh list --all
 Id   Name       State
---------------------------
 16   gentoo     running
 -    gentoo2    shut off



#### libvirtd service - status by explicit channel

`host-root#``virsh -c qemu:///system list --all`
virsh -c qemu:///system list --all
 Id   Name       State
---------------------------
 16   gentoo     running
 -    gentoo2    shut off



#### libvirtd service - status by environment variable

By environment variable, run:

then run:

`host-root#``virsh list --all`
virsh  list --all
 Id   Name       State
---------------------------
 16   gentoo     running
 -    gentoo2    shut off



#### libvirtd service - status by config file

The libvirt configuration file is in the following directory order:

1\. /etc/libvirt/libvirt.conf

Use **@uri\_default** if using this configuration file is copied as-is across multiple hosts; no probe for hypervisor driver done here, but if it does exist, use them.



### Invocation

`host$``libvirtd --help````
Usage:
  libvirtd [options]
Options:
  -h | --help            Display program help
  -v | --verbose         Verbose messages
  -d | --daemon          Run as a daemon & write PID file
  -l | --listen          Listen for TCP/IP connections
  -t | --timeout <secs>  Exit after timeout period
  -f | --config <file>   Configuration file
  -V | --version         Display version information
  -p | --pid-file <file> Change name of PID file
libvirt management daemon:
  Default paths:
    Configuration file (unless overridden by -f):
      /etc/libvirt/libvirtd.conf
    Sockets:
      /run/libvirt/libvirt-sock
      /run/libvirt/libvirt-sock-ro
    TLS:
      CA certificate: /etc/pki/CA/cacert.pem
      Server certificate: /etc/pki/libvirt/servercert.pem
      Server private key: /etc/pki/libvirt/private/serverkey.pem
    PID file (unless overridden by -p):
      /run/libvirtd.pid
```


## Removal

Removal of [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) package (toolkit, library, and utilities) can be done by executing:

`root #``emerge --ask --depclean --verbose app-emulation/libvirt`
## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.

- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit

- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
