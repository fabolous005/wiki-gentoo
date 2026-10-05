<!-- source: https://wiki.gentoo.org/wiki/Dispatch-conf | group: Gentoo Wiki (Main) | wiki-title: Dispatch-conf -->
---
title: dispatch-conf
url: https://wiki.gentoo.org/wiki/Dispatch-conf
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-26"
fingerprint: a614de74a6b7bf9b
license: CC BY-SA 4.0
---

# dispatch-conf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

- how to revert a change
- how backups are made
- is archive-dir still an option ? it isn't in the config file by default

**dispatch-conf** is a utility included with [Portage](https://wiki.gentoo.org/wiki/Portage), used to safely and conveniently manage configuration files after package updates. It is an important tool to manage the issue of transitioning configuration across package updates, without the user needing to manually migrate changes.

dispatch-conf allows system administrators to review configuration file changes made by Portage, to gracefully manage changes to the structure of the configuration of an updated package, in a standard way. Portage will indicate when config files need updating, and running dispatch-conf will propose to either use the new configuration as-is, reject changes, or selectively merge modifications. dispatch-conf will automatically update config files that the user has never modified, or that differ from the current version only in CVS cruft, comments, or white space.

dispatch-conf keeps a history of changes and permits rollback to previous versions, so any mistakes can be reverted. Changes made to config files will be saved in the archive directory. Alternatively it can integrate with rcs to allow for version-controlled configuration file management.

dispatch-conf will check all directories in the `CONFIG_PROTECT` environment variable for changes. Any config files found in paths in the `CONFIG_PROTECT_MASK` environment variable will not be protected and will **automatically be overwritten**.

## The configuration management problem with updates

Configuration migration is a general problem with any software update process: some version changes will modify behavior, such that the previous configuration parameters become no longer pertinent to the new version. New configuration options may be added, removed, their meaning may be changed or the names or the format of each directive may change.

Each software project manages this in its own way. Some projects have robust configuration version tracking and can automatically transfer configuration from one version to another; some projects may have issues between major versions, or if updating from much older versions; for some projects, updating while trying to preserve previous configurations may cause issues or even be impossible.

There is a long-standing Unix convention of using plain text files, often with a semi-standard syntax, for storing configuration. Major updates could require manual migration of these files, or risk them becoming outdated. This may seem cumbersome today, but it remains a very robust way to manage configuration, and avoids ambiguity of how the configuration directives may change between versions, which is important for many - sometimes critical - services such as Internet servers.

## Configuration

### Files

The dispatch-conf configuration file is /etc/dispatch-conf.conf. The directory referenced by the `archive-dir` variable in this file may need to be created.

### RCS (revision control system) integration

When dispatch-conf is configured to integrate with rcs, it will store all changes in /etc/config-archive.

Install revision control system:

`root #``emerge --ask dev-vcs/rcs`
This process of configuration is as simple as editing the configuration file to include the following:

**`/etc/dispatch-conf.conf`**

```
=yes
```
Administrators can then view the differences using the rcs utilities like rlog as well as roll-back changes using co. The rcs utilities work with file locking itself, so the moment it is needed for administrative tasks, understand that:

- dispatch-conf only stores the changes made when the package suggests to alter the file. Changes made afterwards are *not* registered yet
- when checking out a file, rcs will want to write the file to the file system, so make sure to backup existing files first, or work with standard output (see later)
- to check in a file, a lock must first be taken on said file. Also, make sure not to remove the working file

To view the commit history on /etc/conf.d/udev:

`user $``rlog /etc/config-archive/etc/conf.d/udev,v`
RCS file: /etc/config-archive/etc/conf.d/udev,v
Working file: udev
head: 1.1
branch:
locks: strict
access list:
symbolic names:
keyword substitution: kv
total revisions: 2;     selected revisions: 2
description:
Archived config file.
----------------------------
revision 1.1
date: 2011/06/15 18:14:59;  author: root;  state: Exp;
branches:  1.1.1;
dispatch-conf update.
----------------------------
revision 1.1.1.1
date: 2011/06/15 18:14:59;  author: root;  state: Exp;  lines: +3 -2
dispatch-conf update.
=============================================================================

To roll back to a particular version, a simple way to do so is to check out a previous version:

`root #````
cp udev udev.orig
```
`root #````
co -p -r1.1.1.1 /etc/config-archive/etc/conf.d/udev,v > udev
```
etc/config-archive/etc/conf.d/udev,v  -->  standard output
revision 1.1.1.

After making final changes (it is possible to use the backup udev.orig to merge any changes made later), check in the file again:

`root #``co -p -l /etc/config-archive/etc/conf.d/udev,v`
Edit the file, and finally check in the changes:

`root #``ci -l /etc/config-archive/etc/conf.d/udev,v`
/etc/config-archive/etc/conf.d/udev,v  \<--  udev
new revision: 1.2; previous revision: 1.1
enter log message, terminated with single '.' or end of file:
>> Merged changes for persistant rules
>> .
done

### Changing diff or merge tools

#### Color diff output

Reading changes in all grey text can be a bit annoying. Fortunately, modern versions of [sys-apps/diffutils](https://packages.gentoo.org/packages/sys-apps/diffutils) have a `--color` switch, which displays the different types of changes in different colors. Configuration is simple - change the diff line in the config file to:

**`/etc/dispatch-conf.conf`**

```
diff="diff --color=always -Nu '%s' '%s'"
```
Alternatively, the package [app-misc/colordiff](https://packages.gentoo.org/packages/app-misc/colordiff) can be used. Install colordiff with:

`root #``emerge --ask app-misc/colordiff`
Then change the config to use it:

**`/etc/dispatch-conf.conf`**

```
diff="colordiff -Nu '%s' '%s'"
```
#### Use (g)vimdiff to merge changes

It is possible to use vimdiff/gvimdiff instead of the default method of merging files. To do this, modify the /etc/dispatch-conf.conf configuration file by changing the merge line:

**`/etc/dispatch-conf.conf`**

```
merge="vimdiff -c'saveas %s' -c next -c'setlocal noma readonly' -c prev %s %s"
```
vimdiff (for gvimdiff use `-f` option moreover) can also be used to merge changes.


And for **neovim**:

**`/etc/dispatch-conf.conf`**

```
merge="nvim -d -c'saveas %s' -c next -c'setlocal noma readonly' -c prev %s %s"
```
Some useful commands related to merge with vimdiff:

"\]c" : jump to next change
"\[c" : jump to previous change
"CTRL-W \<Right>" or "CTRL-W \<Left>" : go to the other window
"do" (diff obtain): get the text of the highlighted block from the other window
"dp" (diff put) : put the text of the highlighted block to the other window
"zo" : open fold under the cursor
"zc" : close fold under the cursor
"zr" : open all folds
":wqa" : write and exit

See the [Vim](https://wiki.gentoo.org/wiki/Vim) documentation for further help.

#### Using imediff to merge changes

Another merge alternative is [dev-util/imediff](https://packages.gentoo.org/packages/dev-util/imediff). This simple tool allows using just a few keys to toggle between options.  Primarily, this is done through the `a` or `b` keys.  Also, the `e` key will open the user's favorite editor set by the `EDITOR` shell variable for manual merging.

To use with dispatch-conf, first install imediff:

`root #``emerge --ask dev-util/imediff`
Then, configure /etc/dispatch-conf.conf:

**`/etc/dispatch-conf.conf`**

```
# imediff-1.X
merge="imediff2 -c -N --output='%s' '%s' '%s'"
# imediff-3.X
merge="imediff --output='%s' '%s' '%s'"
```
## Usage

### Invocation

dispatch-conf should be run as root, as configuration files are usually owned by root:

`root #``dispatch-conf`
The [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Tools#dispatch-conf) and the [dispatch-conf(1)](https://dev.gentoo.org/~zmedico/portage/doc/man/dispatch-conf.1.html) [man page contain further information on usage:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``man 1 dispatch-conf`
## Tips

### Keep configuration changes in a git repository

On installation, [etckeeper](https://wiki.gentoo.org/wiki/Etckeeper) will hook into dispatch-conf to make backups of configuration files upon each invocation.

## See also

- [cfg-update](https://wiki.gentoo.org/wiki/Cfg-update) — a utility used on Gentoo to manage configuration file updates.
