<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Linux_Security_Modules/AppArmor | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Linux Security Modules/AppArmor -->
---
title: Security Handbook/Linux Security Modules/AppArmor
url: https://wiki.gentoo.org/wiki/Security_Handbook/Linux_Security_Modules/AppArmor
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-17"
fingerprint: "4093a8760735a746"
license: CC BY-SA 4.0
---

# Security Handbook/Linux Security Modules/AppArmor

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**AppArmor** is a MAC (Mandatory Access Control) system, implemented upon LSM (Linux Security Modules).

## Installation

### Kernel

Activate the following kernel options:

Below the "First legacy 'major LSM' to be initialized (AppArmor)  --->" 

Is an "Ordered List of enabled LSMs" (CONFIG\_LSM=) containing a list of strings that needs to be modified to include AppArmor 

Yama is the only one Gentoo sets for now, AppArmor needs to be added to the list. So the new string should become: "yama, apparmor" 

<u>Please make sure CONFIG\_LSM="yama, apparmor" is set in your kernel .config</u> 


### Emerge

Install the userspace tools. It contains the profile parser and init script:

`root #``emerge --ask sys-apps/apparmor`
Emerging [sys-apps/apparmor-utils](https://packages.gentoo.org/packages/sys-apps/apparmor-utils) is optional but recommended. It contains additional userspace utilities to assist with profile management:

`root #``emerge --ask sys-apps/apparmor-utils`
Portage describes [sec-policy/apparmor-profiles](https://packages.gentoo.org/packages/sec-policy/apparmor-profiles) as "A collection of pre-built profiles contributed by the AppArmor community".  However, besides profiles, this package also contains:

- /etc/apparmor.d/abi/
- /etc/apparmor.d/abi/3.0
- /etc/apparmor.d/abi/4.0
- /etc/apparmor.d/abi/kernel-5.4-outoftree-network
- /etc/apparmor.d/abi/kernel-5.4-vanilla
- /etc/apparmor.d/abstractions/
- /etc/apparmor.d/local/
- /etc/apparmor.d/tunables/

`root #``emerge --ask sec-policy/apparmor-profiles`
### Other software

- [sys-libs/libapparmor](https://packages.gentoo.org/packages/sys-libs/libapparmor) -- The core library to support the userspace utilities.  Dependency of sys-apps/apparmor.

## Configuration

### Enabling AppArmor

If AppArmor was not selected as the default security module, and the boot parameter was not set to the default value in the kernel configuration, AppArmor must be manually enabled at boot time.

#### GRUB

**`/boot/grub/grub.conf`**

**Example GRUB config for AppArmor with simple kernel**

```
title=Gentoo with AppArmor
root (hd0,0)
kernel /vmlinuz root=/dev/sda2 apparmor=1 security=apparmor
```
#### GRUB 2

**`/etc/default/grub`**

**Enabling AppArmor with GRUB 2**

```
GRUB_CMDLINE_LINUX_DEFAULT="apparmor=1 security=apparmor"
```
Apply changes by running:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
### securityfs

securityfs is the filesystem used by Linux kernel security modules. The init script mounts it automatically if it is not already, but some may prefer to do it manually:

**`/etc/fstab`**

**securityfs entry for fstab**

```
     /sys/kernel/security securityfs defaults            0      0
```
### Services

#### OpenRC

Adding AppArmor to boot runlevel:

`root #``rc-update add apparmor boot`
#### systemd

Enabling the service will load all profiles on startup:

`root #``systemctl enable apparmor.service`
### Working with profiles

Profiles are stored as simple text files in /etc/apparmor.d. They may take any name, and may be stored in subdirectories - they may be organized as desired.

`root #``ls /etc/apparmor.d`
abstractions  program-chunks  usr.lib.apache2.mpm-prefork.apache2  usr.lib.dovecot.managesieve-login  usr.sbin.dovecot  usr.sbin.nscd
apache2.d     sbin.klogd      usr.lib.dovecot.deliver              usr.lib.dovecot.pop3               usr.sbin.identd   usr.sbin.ntpd
bin.ping      sbin.syslog-ng  usr.lib.dovecot.dovecot-auth         usr.lib.dovecot.pop3-login         usr.sbin.lspci    usr.sbin.smbd
disable       sbin.syslogd    usr.lib.dovecot.imap                 usr.sbin.avahi-daemon              usr.sbin.mdnsd    usr.sbin.smbldap-useradd
local         tunables        usr.lib.dovecot.imap-login           usr.sbin.dnsmasq                   usr.sbin.nmbd     usr.sbin.traceroute

Profiles are referred to by name, including any parent subdirectories if present.

#### Automatic control

The init script will automatically load all profiles located in the profile directory. Unless specified otherwise, each profile will be loaded in enforce mode.

#### Manual control

To activate a profile, simply set it to enforce mode:

`root #``aa-enforce usr.sbin.dnsmasq`
Setting /etc/apparmor.d/usr.sbin.dnsmasq to enforce mode.

Similarly, to deactivate a profile, simply set it to complain mode:

`root #``aa-complain usr.sbin.dnsmasq`
Setting /etc/apparmor.d/usr.sbin.dnsmasq to complain mode.

The current status profiles may be viewed using aa-status:

`root #``aa-status`
apparmor module is loaded.
6 profiles are loaded.
5 profiles are in enforce mode.
   /bin/ping
   /sbin/klogd
   /sbin/syslog-ng
   /usr/sbin/dnsmasq
   /usr/sbin/identd
1 profiles are in complain mode.
   /usr/sbin/lspci
1 processes have profiles defined.
1 processes are in enforce mode.
   /usr/sbin/dnsmasq (12905)
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.

### Generating profiles

AppArmor can grab kernel audit logs from the userspace auditd daemon, allowing profile generation. To generate new profiles, the Audit framework should be installed and running:

`root #``emerge --ask --verbose sys-process/audit`
#### OpenRC

`root #``rc-update add auditd default``root #``rc-service auditd start`
#### Systemd

`root #``systemctl enable auditd.service``root #``systemctl start auditd.service`
New AppArmor profiles can be generated by utilizing aa-genprof:

`root #``aa-genprof /path/to/executable`
Run the executable in a different terminal window and exercise its full functionality, then go back and \[S\]can, and either \[A\]llow or \[D\]eny. Press \[F\]inish to save the profile.

The aa-logprof utility can be used to scan log files for AppArmor audit messages, review them and update the profiles if the program is misbehaving after generating an initial profile. From a terminal:

`root #``aa-logprof`
### Cache profiles to improve boot times

AppArmor needs to parse every profile to be loaded at boot time. When having a big number of profiles in the system, this can have a noticeable impact in boot time. To improve it, it is possible to cache the parsed profiles with the following configuration:

## External resources

- [AppArmor Documentation](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation) - Official AppArmor documentation
- [apparmor.d](https://github.com/roddhjav/apparmor.d) - A project independent from AppArmor with +1500 maintained profiles (systemd only)
- [Profiling with tools](https://gitlab.com/apparmor/apparmor/-/wikis/Profiling_with_tools) - Official AppArmor tutorial
- [How to create an AppArmor Profile](https://ubuntu.com/tutorials/beginning-apparmor-profile-development) - Official Ubuntu Tutorial
- [apparmor-profile-torbrowser](https://github.com/Kicksecure/apparmor-profile-torbrowser/blob/master/etc/apparmor.d/home.tor-browser.firefox) - Example profile for Tor Browser by Kicksecure
