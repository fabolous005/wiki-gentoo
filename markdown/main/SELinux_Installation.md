<!-- source: https://wiki.gentoo.org/wiki/SELinux/Installation | group: Gentoo Wiki (Main) | wiki-title: SELinux/Installation -->
---
title: SELinux/Installation
url: https://wiki.gentoo.org/wiki/SELinux/Installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: "6ebc8d2f86c30b00"
license: CC BY-SA 4.0
---

# SELinux/Installation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Getting an SELinux-powered Gentoo installation doesn't require weird actions. What is needed is to install Gentoo Linux with the correct profile, correct kernel configuration and some file system relabeling. It is strongly recommended to use SELinux together with other hardening improvements.

## Installing Gentoo (Hardened)

This document assumes the reader starts with an existing Gentoo Linux system which needs to be converted to Gentoo with SELinux. It is possible to make the right decisions during a Gentoo installation to immediately start with an SELinux system. However, this article is focusing on a *conversion* of an existing system as that is the most common approach.

SELinux stage3 tarballs are also available and supported - this is significantly easier than performing the steps below. The tarballs can be simply unpacked onto a target system, relabel the entire system, add the initial user to the administration SELinux user and reboot.

### Performing a standard installation

Install Gentoo Linux according to the Gentoo Handbook installation instructions. It is recommended to use the hardened stage 3 tarballs. Perform a full installation to the point that the system is booted into a (primitive) Gentoo base installation.

### Choosing an SELinux policy type

Gentoo supports four policy types within SELinux: *strict*, *targeted*, *mcs*, and *mls*.

#### Targeted

The differentiation between *strict* and *targeted* is based upon the *unconfined* domain. When *targeted* is loaded, the processes on the system that are not specifically confined within a particular policy module will be part of the unconfined domains whose purpose is to allow most activities by default (rather than deny by default). As a result, processes that run inside unconfined domains have no restrictions apart from those already enforced by standard Linux security.

Although running without the unconfined domains is considered more secure when paired with proper policy, it will also be more challenging for the administrator to make sure the system still functions properly as there are no policy modules for each and every application "out there".

However, unconfined domains can be a useful part of a security policy. It is important to make SELinux a part of a system's security policy, not to replace it, and balancing the security policy with convenience dependent on the threat model.

#### MCS

Next to *targeted* and *strict*, administrators can opt for *mcs* to allow categorization of resources. This is useful on multi-tenant systems such as web servers, virtualization hosts, ... where multiple processes will be running, most of them in the same security domain, but in different categories.

Note though that to take advantage of the additional category support, either the applications themselves (such as web server or hypervisor tools) need to configure the SELinux categories (so they need to support SELinux) or the administrator will need to script around to start the individual instances with separate categories. Otherwise, *mcs* is just the same as *targeted* or *strict*.

#### MLS

Finally, Gentoo also provides *mls* to support a true multi-level security system. However, MLS is currently still considered experimental in Gentoo and as such not recommended.

In case of *mcs* or *mls*, the unconfined USE flag needs to be used to enable or disable unconfined domains in these policy types. The *strict* policy store does not use the USE flag at all (as it does not support unconfined domains at all) and the *targeted* policy store (which uses unconfined domains) requires the USE flag set.

#### Overview

Generally speaking, *mcs* is recommended for most users. It allows for the greatest amount of flexibility and is usually expected by programs which can take advantage of the aforementioned category separation, namely virtualization and container technologies. Without *mcs* support, such programs may need additional configuration on other policy types.

Save the choice for the policy store in /etc/portage/make.conf. That way, Portage will only install the policy modules for that SELinux policy store. By default, the SELinux profiles enable *mcs*.

**`/etc/portage/make.conf`**

**Setting the supported policy stores**

Multiple stores can be defined, although only one store can be active at any point in time. This is handled through the SELinux configuration itself, which is covered later in the installation.

### Setting file system contexts

If the /tmp location is a tmpfs-mounted file system, then it is necessary to tell the kernel that the root context of this location is `tmp_t` instead of `tmpfs_t`, which is the default context assigned to tmpfs-mounted file systems.

To configure the /tmp mount, edit the /etc/fstab file:

**`/etc/fstab`**

**Setting the rootcontext for the /tmp mount**

Next, set the next line in /etc/fstab to configure the context for the /run location:

**`/etc/fstab`**

**Setting the rootcontext for the /run mount**

### Change the Gentoo profile

Switch the Gentoo profile to the right SELinux profile (for instance, default/linux/amd64/23.0/hardened/selinux).

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]  default/linux/amd64/23.0/systemd (stable)
  \[2\]  default/linux/amd64/23.0/desktop (stable)
  \[3\]  default/linux/amd64/23.0/desktop/systemd (stable)
  \[4\]  default/linux/amd64/23.0/desktop/gnome (stable)
  \[5\]  default/linux/amd64/23.0/desktop/gnome/systemd (stable)
  \[6\]  default/linux/amd64/23.0/desktop/plasma (stable)
  \[7\]  default/linux/amd64/23.0/desktop/plasma/systemd (stable)
  \[8\]  default/linux/amd64/23.0/no-multilib (stable)
  \[9\]  default/linux/amd64/23.0/no-multilib/systemd (stable)
  \[10\]  default/linux/amd64/23.0/no-multilib/hardened (stable)
  \[11\]  default/linux/amd64/23.0/no-multilib/hardened/systemd (stable)
  \[12\]  default/linux/amd64/23.0/no-multilib/hardened/selinux (stable)
  \[13\]  default/linux/amd64/23.0/no-multilib/hardened/selinux/systemd (stable)
  \[14\]  default/linux/amd64/23.0/no-multilib/prefix (exp)
  \[15\]  default/linux/amd64/23.0/no-multilib/prefix/kernel-2.6.32+ (exp)
  \[16\]  default/linux/amd64/23.0/no-multilib/prefix/kernel-2.6.16+ (exp)
  \[17\]  default/linux/amd64/23.0/no-multilib/prefix/kernel-3.2+ (exp)
  \[18\]  default/linux/amd64/23.0/llvm (stable)
  \[19\]  default/linux/amd64/23.0/llvm/systemd (stable)
  \[20\]  default/linux/amd64/23.0/hardened (stable)
  \[21\]  default/linux/amd64/23.0/hardened/systemd (stable)
  \[22\]  default/linux/amd64/23.0/hardened/selinux (stable)
  \[23\]  default/linux/amd64/23.0/hardened/selinux/systemd (stable)
  \[24\]  default/linux/amd64/23.0/split-usr (stable)
  \[25\]  default/linux/amd64/23.0/split-usr/desktop (stable)
  \[26\]  default/linux/amd64/23.0/split-usr/desktop/gnome (stable)
  \[27\]  default/linux/amd64/23.0/split-usr/desktop/plasma (stable)
  \[28\]  default/linux/amd64/23.0/split-usr/no-multilib (stable)
  \[29\]  default/linux/amd64/23.0/split-usr/no-multilib/selinux (stable)
  \[30\]  default/linux/amd64/23.0/split-usr/no-multilib/hardened (stable)
  \[31\]  default/linux/amd64/23.0/split-usr/no-multilib/hardened/selinux (stable)
  \[32\]  default/linux/amd64/23.0/split-usr/no-multilib/prefix (exp)
  \[33\]  default/linux/amd64/23.0/split-usr/no-multilib/prefix/kernel-2.6.32+ (exp)
  \[34\]  default/linux/amd64/23.0/split-usr/no-multilib/prefix/kernel-2.6.16+ (exp)
  \[35\]  default/linux/amd64/23.0/split-usr/no-multilib/prefix/kernel-3.2+ (exp)
  \[36\]  default/linux/amd64/23.0/split-usr/llvm (stable)
  \[37\]  default/linux/amd64/23.0/split-usr/hardened (stable)
  \[38\]  default/linux/amd64/23.0/split-usr/hardened/selinux (stable)
  \[39\]  default/linux/amd64/23.0/x32 (dev)
  \[40\]  default/linux/amd64/23.0/x32/systemd (exp)
  \[41\]  default/linux/amd64/23.0/split-usr/x32 (exp)
  \[42\]  default/linux/amd64/23.0/musl (dev)
  \[43\]  default/linux/amd64/23.0/musl/llvm (exp)
  \[44\]  default/linux/amd64/23.0/musl/hardened (exp)
  \[45\]  default/linux/amd64/23.0/musl/hardened/selinux (exp)
  \[46\]  default/linux/amd64/23.0/split-usr/musl (dev)
  \[47\]  default/linux/amd64/23.0/split-usr/musl/llvm (exp)
  \[48\]  default/linux/amd64/23.0/split-usr/musl/hardened (exp)
  \[49\]  default/linux/amd64/23.0/split-usr/musl/hardened/selinux (exp)

`root #``eselect profile set default/linux/amd64/23.0/hardened/selinux`
Starting from the profile change, Portage will warn after every installation that it was "Unable to set SELinux security labels". This is to be expected, because the tools and capabilities that Portage requires to set the [security labels](https://wiki.gentoo.org/wiki/SELinux/Labels) aren't available yet. This warning will vanish the moment the SELinux installation is completed.

Don't update the system yet - a couple of packages need to be installed first in a particular order that Portage isn't aware of.

### Updating make.conf

Take a look at the following SELinux related USE flags and decide which ones to enable or disable.


| [+ubac](https://packages.gentoo.org/useflags/+ubac) | Enable User Based Access Control (UBAC) in the SELinux policy | 
| [+unconfined](https://packages.gentoo.org/useflags/+unconfined) | Enable support for the unconfined SELinux module | 
| [+unknown-perms](https://packages.gentoo.org/useflags/+unknown-perms) | Default allow unknown classes in kernels newer than the policy (SELinux policy capability). | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

Update the USE variable in /etc/portage/make.conf or in an appropriate /etc/portage/package.use location for the [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base) package.

### Installing an SELinux kernel

#### Option 1: re-use the default gentoo-kernel-bin (or unmodified gentoo-kernel)

The regular pre-built Gentoo dist-kernel [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin) already contains support for SELinux but it comes pre-disabled (as of 5.4.x) via the option called "SELinux runtime disable" (`CONFIG_SECURITY_SELINUX_DISABLE=y`).

If you use gentoo-kernel-bin, it is **not** necessary to recompile in order to enable it. Of course, [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) uses the same configuration by default, so this advice applies there too.

However, a kernel command-line parameter is **required** at boot time (to undo the runtime disable, which can be set in the bootloader.

- `lsm=selinux`: New parameter, and can contain a list of other modules. Theoretically, you could append more modules to the `lsm=` in a comma separated list, like `"selinux,yama"`.
- `security=selinux`: Old and deprecated, and can only contain 1 entry, but still works.

Another parameter `selinux=1` is often mentioned instead, but doesn't actually take effect by itself, because the LSM parameter now takes precedence.

These parameters just enable kernel support, after which, the [/etc/selinux/config](https://packages.gentoo.org/packages//etc/selinux/config) file will then take over and choose the mode of "enforcing/permissive/disabled" and type of "targeted/strict/mls/mcs". Enforcing mode can be set as a boot parameter with `enforcing=1` or permissive mode set with `enforcing=0`

Those enforcing options will be relevant later in the guide, and dealt with: do not set them now.

#### Option 2: recompiling kernel from gentoo-sources

The default Linux kernel sources also offer SELinux support, but need to be explicitly configured during compilation:

`root #``emerge --ask sys-kernel/gentoo-sources`
Next, reconfigure the kernel with the appropriate security settings. This includes, but is not limited to

- Support for extended attributes in the various file systems
- Support system-call auditing
- Support for SELinux

Below, a quick overview of the recommended settings to configure in `make menuconfig`:

**Enabling SELinux**

Do not forget to prepend selinux to *Ordered list of enabled LSMs*. Otherwise, the `security=selinux` boot parameter has to be set in the bootloader configuration in order to enable SELinux.

If you must be able to enable/disable SELinux, the following settings allow to boot the kernel with option `selinux=0` to disable it:

**Enabling SELinux**

Build and install the new Linux kernel and its modules.

## Configure SELinux

In the second part of the SELinux installation, we cover the installation of the proper utilities, relabel the entire file system and configure the policy.

### Installing policies and utilities, part one

First, install the base SELinux policy package [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base). This package provides the SELinux configuration file which needs to be adjusted prior to building all other SELinux packages. As soon as [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils) is installed (it's a dependency of [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base)), Portage will attempt to enable its SELinux support. However, as SELinux is currently not properly configured, it is necessary to disable this through `FEATURES="-selinux"`:

`root #``FEATURES="-selinux" emerge --ask --verbose --oneshot selinux-base`
### Configuring the SELinux policy

The main SELinux configuration file is now at /etc/selinux/config. It needs to be edited to set two important values: `SELINUX` and `SELINUXTYPE`.

**`/etc/selinux/config`**

The `SELINUX` variable defines how [SELinux should behave](https://wiki.gentoo.org/wiki/SELinux/States):

- *enforcing* will enable and enforce policies. This is the target at the end, but we need to start in permissive mode first to make sure everything would work as expected.
- *permissive* will enable policies, but not enforce them. Any violation is reported but not denied. This is where most users should start from as it will not impact the system, yet allow users to get acquainted with SELinux - and validate the warnings to see if the system can be switched towards enforcing or not later.
- *disabled* will completely disable the policies. This completely disables the userspace loading of SELinux and the system policy. As a result, SELinux will not label new files created, resulting them being incorrectly labeled, and will not log any possible denials. This means that should SELinux be reenabled, either in permissive or enforcing, in the future, the system is no longer in an adequate state, and a relabel of the full system is required. It is not recommend in almost **any** situation to completely disable the loading of SELinux. If SELinux is preventing an action and it is needed to "turn off" enforcement, the *permissive* option should be set instead.

The `SELINUXTYPE` variable selects the SELinux policy store to load. For most users, the *mcs* policy store should be set.

Make sure that the `SELINUX` variable is set to permissive right now. We will switch to enforcing later.

### Installing policies and utilities, part two

Now continue with the installation of the SELinux policies:

1. Rebuild the [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base) package if `SELINUXTYPE` is changed to something other than *mcs*;
2. Install the core SELinux policies through the [sec-policy/selinux-base-policy](https://packages.gentoo.org/packages/sec-policy/selinux-base-policy) package. This package contains the core SELinux policies needed to get the system up and running using SELinux.

As Portage will try to [label](https://wiki.gentoo.org/wiki/SELinux/Labels) and reload policies, we need to temporarily disable SELinux support again (as Portage wouldn't be able to label anything as it doesn't understand it yet).

`root #``FEATURES="-selinux -sesandbox" emerge --ask --verbose --oneshot selinux-base``root #``FEATURES="-selinux -sesandbox" emerge --ask --verbose --oneshot selinux-base-policy`
Install [sec-policy/selinux-policykit](https://packages.gentoo.org/packages/sec-policy/selinux-policykit) and [sec-policy/selinux-dbus](https://packages.gentoo.org/packages/sec-policy/selinux-dbus), otherwise /etc/selinux/$SELINUXTYPE/contexts/files/file\_contexts will not be present in the system and relabeling will be impossible (see [bug #891963](https://bugs.gentoo.org/show_bug.cgi?id=891963)):

`root #``FEATURES="-selinux -sesandbox" emerge --ask --verbose --oneshot selinux-policykit``root #``FEATURES="-selinux -sesandbox" emerge --ask --verbose --oneshot selinux-dbus`
Now it is finally time to rebuild all packages affected by the profile change. Don't forget to use `dispatch-conf` afterwards as some changes to configuration files will need to be made. This operation will also pull in all SELinux policy packages needed for the various components already installed on the system.

`root #``emerge --ask --verbose --update --deep --newuse @world`
Before proceeding to the next section, perform a reboot.

### Relabel

Next [relabel](https://wiki.gentoo.org/wiki/SELinux/Labels) all devices and openrc related files. This will apply the correct security contexts (labels) onto the necessary files. We start by bind-mounting / onto /mnt/gentoo. This will allow us to relabel the mount points themselves rather than the mounted file systems that are already mounted on the main file system.

`root #````
mkdir /mnt/gentoo
```
`root #``mount -o bind / /mnt/gentoo`
In the following command, substitute *$SELINUXTYPE* in the next command with the policy type set in the /etc/selinux/config file. If your system has more (or fewer) active mountpoints than the usual set of /dev,/efi,/proc,/run,/sys,/tmp, list them too.

`root #````
setfiles -T$(nproc) -v -r /mnt/gentoo /etc/selinux/$SELINUXTYPE/contexts/files/file_contexts /mnt/gentoo/{dev,efi,proc,run,sys,tmp}
```
`root #``umount /mnt/gentoo`
If the system uses a swapfile rather than a swap partition, label it accordingly:

`root #````
semanage fcontext --add --seuser system_u --type swapfile_t --ftype f "/swapfile"
```
`root #``restorecon /swapfile`
If system uses a swap partition, you would do something like this:

`root #````
semanage fcontext --add --seuser system_u --type swapfile_t --ftype f "/dev/path/to/partition"
```
`root #``restorecon /dev/path/to/partition`
Now relabel the entire file system. The next command will apply the correct security context onto the files on the entire file system, based on the security context information provided by the SELinux policy modules installed.

`root #``restorecon -RFv -T$(nproc) /`
If an SELinux policy module for a package is installed after that particular package, then `rlpkg` needs to be run for that package to make sure that the security contexts for its files are set correctly. For instance, if [sec-policy/selinux-screen](https://packages.gentoo.org/packages/sec-policy/selinux-screen) would be installed manually (due to a missing dependency) after installing [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen):

`root #``rlpkg screen`
### Define the administrator accounts

If the `SELINUXTYPE` variable is set to *strict*, then it is necessary to map the account(s) used to manage the system (those that need access to Portage) to the *staff\_u* [SELinux user](https://wiki.gentoo.org/wiki/SELinux/Users_and_logins). If not, none of the accounts will be able to successfully manage the system (except for *root*, but then the administrator will need to login as root directly and not through `sudo` or `su`.)

By default, users are mapped to the *user\_u* SELinux user who doesn't have the appropriate rights (nor access to the appropriate roles) to manage a system. Accounts that are mapped to *staff\_u* can, but might need to switch roles from *staff\_r* to *sysadm\_r* before they are granted the appropriate privileges.

In the following example, we map the *john* Linux account to the *staff\_u* SELinux user:

`root #````
semanage login -a -s staff_u john
```
`root #``restorecon -R -F /home/john`
When system administration tasks need to be executed, the user will need to switch the role to *sysadm\_r*. For this, the `newrole` command can be used.

`user $``id -Z`
staff\_u:staff\_r:staff\_t

`user $``newrole -r sysadm_r`
Password: (Enter your password)

`user $``id -Z`
staff\_u:sysadm\_r:sysadm\_t

In a *targeted* policy, the users will be of type *unconfined\_t* and will already have the necessary privileges to perform system administrative tasks.

#### Sudo

`sudo` can also be configured to transition from *staff\_u* (or other) users to configured SELinux types/roles, for example:

**`/etc/sudoers.d/wheel`**

### Supporting service administration

By default, the Gentoo Hardened SELinux policies will allow the *sysadm\_t* domain access to all services. However, some of these services have policies that allow them to be assigned to individual, non-root users. This requires the user to be granted the *system\_r* role (meaning the user can, under certain circumstances, have his role change towards the system role).

It is therefore recommended to grant the *system\_r* role to the administrative SELinux users that are going to administer the system. These are most likely the *root* and *staff\_u* SELinux user.

`root #````
semanage user -m -R "staff_r sysadm_r system_r" root
```
`root #``semanage user -m -R "staff_r sysadm_r system_r" staff_u`
## Summary

That's it! SELinux is now fully configured on the system.

Although it will boot in permissive mode (most likely) right now, a fair amount of work is most likely still needed to successfully boot in enforcing. The installed Gentoo refpolicy can be seen as a base policy framework for you to build out your own system policy on top of.

Running in *mcs* mode, rather than *targeted*, means more on your system is protected, at the cost of needing policy for everything you run. You can always change the mode being used rather than stop using SELinux entirely.

Make sure to read the Gentoo [SELinux tutorials](https://wiki.gentoo.org/wiki/SELinux/Tutorials) as well as the other [documentation](https://wiki.gentoo.org/wiki/SELinux). To report problems, review the [reporting policy bugs](https://wiki.gentoo.org/wiki/Project:SELinux/Reporting_policy_bugs) page.
