<!-- source: https://wiki.gentoo.org/wiki/Etckeeper | group: Gentoo Wiki (Main) | wiki-title: Etckeeper -->
---
title: etckeeper
url: https://wiki.gentoo.org/wiki/Etckeeper
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: "9600746b8aa3fb93"
license: CC BY-SA 4.0
---

# etckeeper

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**etckeeper** is a collection of tools to store /etc in a VCS (version control) repository, to keep a backup of changes to system configuration files. This allows to review or revert changes to /etc, or to push the repository elsewhere for backups, or cherry-picking configuration changes.

On installation, *etckeeper* places a hook in /etc/portage/conf-update.d/ so that it will be called to commit changes when [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) is executed.

*etckeeper* can hook into [Portage](https://wiki.gentoo.org/wiki/Portage), to automatically commit changes made to /etc during package upgrades - see [Portage integration](https://wiki.gentoo.org#Portage_integration) section.

*etckeeper* is modular and configurable, but simple to use for those who know the underlying VCS. It tracks file metadata that git does not normally support, but that is important for /etc, such as the permissions of /etc/shadow.

## Installation

### USE flags


### USE flags for
            [sys-apps/etckeeper](https://packages.gentoo.org/packages/sys-apps/etckeeper)
            
            A collection of tools to let /etc be stored in a repository

### Emerge

Install [sys-apps/etckeeper](https://packages.gentoo.org/packages/sys-apps/etckeeper):

`root #``emerge --ask sys-apps/etckeeper`
### Additional software

etckeeper supports the following version control systems:

- [git](https://wiki.gentoo.org/wiki/Git)
- [dev-vcs/mercurial](https://packages.gentoo.org/packages/dev-vcs/mercurial)
- [darcs](https://wiki.gentoo.org/wiki/Darcs)
- bzr (Removed from Gentoo repo)

## Configuration

### Portage integration

**`/etc/portage/bashrc`**

**Integrate etckeeper with Portage hooks**

```
# From /usr/share/doc/etckeeper*/bashrc.example
case "${EBUILD_PHASE}" in
        setup|prerm) etckeeper pre-install ;;
        postinst|postrm) etckeeper post-install ;;
esac
```
### Files

#### .gitignore

After initialization, the /etc/.gitignore file can be use to avoid committing certain files into etckeeper. Patterns should be added near the end of the file, after the "# end section managed by etckeeper" section.

**`/etc/.gitignore`**

**Excluding patterns from etckeeper**

```
# begin section managed by etckeeper (do not edit this section by hand)
# new and old versions of conffiles, stored by emerge
._cfg*
# old versions of files
*.old
# mount(8) records system state here, no need to store these
blkid.tab
blkid.tab.old
# some other files in /etc that typically do not need to be tracked
nologin
ld.so.cache
prelink.cache
mtab
mtab.fuselock
.pwd.lock
*.LOCK
network/run
adjtime
udev/hwdb.bin
lvm/cache
lvm/archive
X11/xdm/authdir/authfiles/*
ntp.conf.dhcp
.initctl
webmin/fsdump/*.status
webmin/webmin/oscache
apparmor.d/cache/*
service/*/supervise/*
service/*/log/supervise/*
sv/*/supervise/*
sv/*/log/supervise/*
*.elc
*.pyc
*.pyo
init.d/.depend.*
openvpn/openvpn-status.log
cups/subscriptions.conf
cups/subscriptions.conf.O
fake-hwclock.data
check_mk/logwatch.state
NetworkManager/system-connections
# editor temp files
*~
.*.sw?
.sw?
\#*\#
DEADJOE
# end section managed by etckeeper
shadow
shadow-
*.lock*
*.keep*
```
#### etckeeper.conf

The main configuration file is /etc/etckeeper/etckeeper.conf. Following configuration options in the main configuration file need specific settings:

**`/etc/etckeeper/etckeeper.conf`**

```
# The VCS to use.
VCS="git"
...
# Options passed to git commit when run by etckeeper.
GIT_COMMIT_OPTIONS=""
...
# Etckeeper includes both a cron job and a systemd timer, which each
# can commit exiting changes to /etc automatically once per day.
# To enable the systemd timer, run: systemctl enable etckeeper.timer
# The cron job is enabled by default; to disable it set:
# 0 to not avoid daily autocommits, commit daily automatically
# 1 is avoid daily autocommits
# AVOID_DAILY_AUTOCOMMITS=1
...
# Uncomment to avoid etckeeper committing existing changes to
# /etc before installation. It will cancel the installation,
# so you can commit the changes by hand.
AVOID_COMMIT_BEFORE_INSTALL=1
...
# Gentoo specific:                                                                                                                                                                                          
# For portage this is emerge
HIGHLEVEL_PACKAGE_MANAGER=emerge
# Gentoo specific:
# For portage this is qlist
LOWLEVEL_PACKAGE_MANAGER=qlist
...
# To push each commit to a remote, put the name of the remote here.
# (eg, "origin" for git). Space-separated lists of multiple remotes
# also work (eg, "origin gitlab github" for git).
PUSH_REMOTE=""
```


### Service

#### Cron

When installed with the [cron](https://packages.gentoo.org/useflags/cron) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) enabled, a script will be installed in /etc/cron.daily/etckeeper to auto commit changes to /etc once a day.

The cron job is enabled by **default** in the configuration. To disable the daily auto commits using [cron](https://wiki.gentoo.org/wiki/Cron), configure like in the following example:

**`/etc/etckeeper/etckeeper.conf`**

```
...
AVOID_DAILY_AUTOCOMMITS=1
..
```
#### systemd

To enable the systemd timer, run:

`root #``systemctl enable etckeeper.timer`
## Usage

First, it must be considered if sensitive files (such as private keys) shall be included in the repository. If there are concerns, it is suggested to create a .gitignore file before running the etckeeper initialization.

Initialization of the repository is done by running:

`root #``cd /etc``root #``etckeeper init`
When initialization is done through the sudo or the doas command, the author of the commit will be set to the corresponding user:

`user $``sudo etckeeper init`
The command creates a .gitignore file in the /etc directory. If it already exist, then a "managed by etckeeper" comment block is added. It also sets up pre-commit hooks. This command does not yet commit files, but runs git add to ensure all interesting files are included in the initial commit later.

When specific files shall be excluded,  effectiveness of the .gitignore file can be verified by running git status. In the following example it is tested whether shadow files will be checked in. If any files that shall be excluded show up as a *new file*, then the .gitignore is not applied properly and needs fixing.

`root #````
cd /etc
```
`root #````
git status | grep shadow
```
```
        new file:   gshadow
        new file:   gshadow-
        new file:   pam.d/shadow
        new file:   shadow
        new file:   shadow-
```
If not interesting files show up, it is suggested to undo the etckeeper initialization (as described later in the Removal section), fix the .gitignore and start again.

Finally commit all changes in /etc to the repository. A commit message can be specified. It is possible to use the underlying VCS to commit manually. Note that etckeeper commit will notice if a user has used sudo or su to become root, and record the original username in the commit. At this time it is recommended to use the git commit command.

`root #````
cd /etc
```
`root #````
git commit -a -m  'Initial /etc commit.'
```
If the initial version has been committed, from this time forward it is safe to use following command below to commit diffs:

`user $``sudo etckeeper commit new-changes`
or:

`root #``etckeeper commit new-changes`
Optionally, pack the git repository to save disk space using following command:

`root #````
cd /etc
```
`root #````
git gc
```
## Removal

The following command deletes the /etc/.git directory:

`root #````
etckeeper uninit
```
\*\* Warning: This will DESTROY all recorded history for /etc,
\*\* including the git repository.
Are you sure you want to do this? \[yN\]

Uninstall etckeeper:

`root #``emerge --ask --depclean --verbose sys-apps/etckeeper`
## See also

- [Dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) — a utility included with [Portage](https://wiki.gentoo.org/wiki/Portage), used to safely and conveniently manage configuration files after package updates.
