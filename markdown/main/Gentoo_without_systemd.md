<!-- source: https://wiki.gentoo.org/wiki/Gentoo_without_systemd | group: Gentoo Wiki (Main) | wiki-title: Gentoo without systemd -->
---
title: Gentoo without systemd
url: https://wiki.gentoo.org/wiki/Gentoo_without_systemd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-13"
fingerprint: c23b32fe2f63975b
license: CC BY-SA 4.0
---

# Gentoo without systemd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

One of Gentoo's distinctive characteristics is the ability to support [systemd](https://wiki.gentoo.org/wiki/Systemd) as an *option*, instead of it being the single available [init system](https://wiki.gentoo.org/wiki/Init_system) implementation. The distribution offers the choice of having a working Linux, non systemd-based operating system, and this article provides some tips on how to avoid unwanted installation of [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd).

While it may have been easier in the past to have systemd installed by accident, in modern Gentoo installations Portage is effectively able prevent this when a non-systemd profile is selected.

## Gentoo's init system and service manager setup

Several [Gentoo profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) that select the Linux kernel and the [GNU C library](https://wiki.gentoo.org/wiki/GNU_C_library) (`KERNEL` [USE\_EXPAND](https://wiki.gentoo.org/wiki/USE_EXPAND) variable set to `linux` and `ELIBC` USE\_EXPAND variable set to `glibc`) have [virtual/service-manager](https://packages.gentoo.org/packages/virtual/service-manager) and [virtual/dev-manager](https://packages.gentoo.org/packages/virtual/dev-manager) packages in [the system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), i.e. it is guaranteed to be present in every Gentoo machine. They are a **virtual packages**, that is, it installs no files but has an *any-of* dependency, `|| ( ... )`, on other packages. For these profiles, [virtual/service-manager](https://packages.gentoo.org/packages/virtual/service-manager) can be satisfied by [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) or [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd), and [virtual/dev-manager](https://packages.gentoo.org/packages/virtual/dev-manager) can be satisfied by [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils), and [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) itself.

A Gentoo system installed from a [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit) + [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) stage3 tarball (i.e. a stage3-\*.tar.xz or stage3-\*.tar.bz2 file that does not contain "systemd" in the name) will have [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) and [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils) already installed, so systemd shouldn't get installed unless some other package pulls it as a dependency for some reason.

For a refresher on stage3 tarballs and profiles, please review [the Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page).

### So, why is systemd pulled in?

Users of a non-systemd Gentoo system might be occasionally surprised by [Portage](https://wiki.gentoo.org/wiki/Portage) trying to install systemd in response to an emerge command. Systemd is a large software package that implements an init system, but also provides a number of other executables (systemd-udevd, systemd-logind, systemd-resolved, systemd-networkd, systemd-tmpfiles, systemd-localed, systemd-machined, systemd-nspawn, etc.), libraries (libsystemd, libudev), a [PAM module](https://wiki.gentoo.org/wiki/PAM) (pam\_systemd.so) and a UEFI boot manager ([systemd-boot](https://wiki.gentoo.org/wiki/Systemd/systemd-boot)), among other components. So any other package that needs any of these components, even if it is just one, would pull the whole systemd package as a dependency.

Most these packages, however, only have a "soft dependency" on systemd, that is, optional functionality that uses a systemd component, and that can be turned on or off at build time. Gentoo, being a source-based distribution, is able to easily take advantage of such package features, and corresponding ebuilds usually expose a [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) that can be unset to avoid installing systemd as a dependency. Profiles that install OpenRC (those that **do not** have "systemd" in their names) have the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag unset by default.](https://wiki.gentoo.org/wiki/USE_flag)

A small number of packages have a ["hard dependency"](https://wiki.gentoo.org/wiki/Gentoo_without_systemd#harddep) on systemd, that is, their ebuild unconditionally lists [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) as a dependency, with no regard to the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag. The most well known example of this used to be the](https://wiki.gentoo.org/wiki/USE_flag) [GNOME desktop environment](https://wiki.gentoo.org/wiki/GNOME), which, for versions 3.28 and earlier, contained components that required systemd-logind (among them, notably, its display manager, [GDM](https://wiki.gentoo.org/wiki/GNOME/gdm)). As of version 3.30, though, Gentoo's packaging of GNOME allows it to work once again with sysvinit + OpenRC as the init system<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. This is achieved through [elogind](https://wiki.gentoo.org/wiki/Elogind); non-systemd GNOME profiles (i.e. those with "gnome", but not "systemd", in their name) set the [elogind](https://packages.gentoo.org/useflags/elogind) [USE flag so that packages pull](https://wiki.gentoo.org/wiki/USE_flag) [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) as a dependency instead of sys-apps/systemd.

It might also be necessary to remove lines that set the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag from files in /etc/portage/package.use/ that could have been added automatically.](https://wiki.gentoo.org/wiki/USE_flag)

## Taking explicit measures to avoid installation of systemd

Current profile settings are usually enough to avoid systemd on a Gentoo system installed from a sysvinit + OpenRC stage3 tarball, provided a non-systemd profile has been selected. However, the following explict measures can be easily taken by concerned administrators.

### Globally disabling and masking the systemd USE flag

This is set via the profile so no changes are need.

The best assurance that systemd will not be installed is [masking the package](https://wiki.gentoo.org/wiki//etc/portage/package.mask) altogether:

**`/etc/portage/package.mask/systemd`**

**package.mask directory example**

If an emerge command for any reason (including odd USE flag combinations) tries to pull sys-apps/systemd as a dependency, the package mask will cause a blocker that Portage cannot resolve (i.e. the output of emerge will show `[blocks B      ]`), and make the emerge command fail. The administrator can then look at the blocker messages and figure out what to do, which in some cases might mean to just give up and not install [certain packages](https://wiki.gentoo.org/wiki/Gentoo_without_systemd#harddep).

## systemd unit files

Some upstream packages provide systemd unit files, to make them easier to install on systemd-based distributions and try make them work mostly out of the box, but don't otherwise have any heavier integration with systemd, or require any systemd-specific functionality. This sort of packages are not considered to have an actual dependency on systemd (neither "soft" or "hard"), and, according to the [official ebuild policy for systemd](https://wiki.gentoo.org/wiki/Project:Systemd/Ebuild_policy), unit files follow the usual guidelines against small text files ([Bash completion](https://wiki.gentoo.org/index.php?title=Bash_completion&action=edit&redlink=1), [logrotate](https://wiki.gentoo.org/wiki/Logrotate) etc.) and ebuilds must not prevent their installation based on the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag.](https://wiki.gentoo.org/wiki/USE_flag)

Unit files are harmless and do nothing if systemd is not installed, just like OpenRC service scripts do nothing if [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) is not installed. However, users that absolutely do not want systemd unit files on their machines, can add systemd's unit file paths to the `INSTALL_MASK` variable in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

```
# Assuming INSTALL_MASK contains more items represented by ellipsis:
INSTALL_MASK="... /lib/systemd/*/*.service /usr/lib/systemd/*/*.service ..."
```
Or alternatively, write a Portage postsync hook in /etc/portage/postsync.d:

**`/etc/portage/postsync.d/10systemd`**

```
 -rf /lib/systemd/*/*.service /usr/lib/systemd/*/*.service
```
For information about `INSTALL_MASK` or postsync hooks, please consult man make.conf and man portage, respectively.

## Troubleshooting

As long as systemd is [package masked](https://wiki.gentoo.org/wiki/Gentoo_without_systemd#pkgmask), it's impossible to install packages with a "hard" dependency on it. Cases like this almost always happen by upstream's choice, so there is little Gentoo can do in a packager and distributor role to avoid that, short of developing a Gentoo-specific patch set and committing to its long-term maintenance across upstream's releases. So the most advisable course of action for a solution is to try contacting the upstream developer team directly: filing a bug report, contributing a patch, etc. Interested people must be aware that upstream's openness to accept such kinds of patches or bug reports may vary widely from project to project.

Sometimes alternative packages with similar functionality and no hard dependency on systemd can be found.

### Checking if systemd is installed or not

The equery program from [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) can be used to check if systemd is installed:

`user $``equery list sys-apps/systemd`
\* Searching for systemd in sys-apps ...
!!! No installed packages matching 'sys-apps/systemd'

Nowadays, getting systemd accidentally installed if the selected profile is a non-systemd one is hard, because of blockers that Portage cannot resolve (i.e. the output of emerge will show `[blocks B      ]`). For example, [sys-apps/gentoo-systemd-integration](https://packages.gentoo.org/packages/sys-apps/gentoo-systemd-integration) and [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils) (which includes udev) cannot be installed at the same time, and the same is the case for [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) and [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind), which also cannot both be used at once.

Further, [sys-apps/sysvinit](https://packages.gentoo.org/packages/sys-apps/sysvinit) blocks [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) because of the on-by-default state of the [sysv-utils](https://packages.gentoo.org/useflags/sysv-utils) [USE flag in recent versions of](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. There are further cases where these packages are blocked, other than the ones listed here. The result is, effectively, that systemd will not be installed if openrc is installed and a non-systemd profile is selected.

Turning a Gentoo system installed from a sysvinit + OpenRC stage3 tarball into a systemd-based one requires an `emerge --newuse --deep @world` step with a particular USE flag setup as per [the installation instructions](https://wiki.gentoo.org/wiki/Systemd#Profile), which is facilitated by systemd profiles. And if the package is somehow installed, actually running systemd as the init system requires a machine reboot with a suitable setup to either execute the systemd program as process 1 (e.g. an `init=` kernel parameter or a suitable [initramfs](https://wiki.gentoo.org/wiki/Initramfs) setup), or have /sbin/init be a symlink to systemd.

Nevertheless, if systemd is accidentally installed, it is advised to get in touch with [Gentoo's support community](https://www.gentoo.org/support) for help with its removal, because it might be a nontrivial task depending on why package got installed in the first place, and whether it is actually running as the init system, or just installed but inactive.

## See also

- [Hard dependencies on systemd](https://wiki.gentoo.org/wiki/Hard_dependencies_on_systemd) — a (possibly partial) list of packages in Gentoo's repository that unconditionally require [systemd](https://wiki.gentoo.org/wiki/Systemd)

## External resources

- [without-systemd](https://github.com/KenjiBrown/without-systemd) overlay, containing replacements for [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)
- [A thread in the Gentoo Forums](https://forums.gentoo.org/viewtopic-t-1074786.html) about using the `INSTALL_MASK` variable to prevent installation of systemd unit files.
- [Funtoo Linux Optimization Proposal: No-systemd system](https://www.funtoo.org/FLOP:No-systemd_system)
