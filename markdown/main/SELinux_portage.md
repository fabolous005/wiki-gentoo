<!-- source: https://wiki.gentoo.org/wiki/SELinux/portage | group: Gentoo Wiki (Main) | wiki-title: SELinux/portage -->
---
title: SELinux/portage
url: https://wiki.gentoo.org/wiki/SELinux/portage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-14"
fingerprint: b730ba3a8ee21384
license: CC BY-SA 4.0
---

# SELinux/portage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Structure

### Domains

The **portage** module provides the following domains:

| Domain | Process(es) | Description | 
|---|---|---|
| portage\_t | emerge, ebuild, quickpkg, ebuild.sh, regenworld, sandbox | Gentoo's package manager domain. | 
| portage\_sandbox\_t | sandbox | Portage compile sandbox domain. | 
| portage\_fetch\_t | rsync, emerge-webrsync, layman | Domain responsible for fetching ebuilds and sources as well as managing overlays. | 
| gcc\_config\_t | gcc-config | Domain for the gcc-config wrapper. | 

### File types/labels

The following table lists the file type/labels defined in the **portage** module:

| Type | Description | 
|---|---|
| portage\_exec\_t | Entrypoints for the portage and portage-related domains. Used for binaries or scripts such as sandbox, emerge, ... | 
| gcc\_config\_exec\_t | Entrypoints for the gcc-config wrapper domain. | 
| portage\_ebuild\_t | Type assigned to the ebuild files and directories. | 
| portage\_srcrepo\_t | Type assigned to the live repository pulls (git, svn, cvs, ...) used by live ebuilds. | 
| portage\_fetch\_tmp\_t | Type used by the portage\_fetch\_t domain when storing files in a temporary location. | 
| portage\_db\_t | Type used by Portage' data files. | 
| portage\_conf\_t | Type used by Portage' configuration files. | 
| portage\_cache\_t | Type used for the Portage cache. | 
| portage\_log\_t | Type used by Portage for its log files. | 
| portage\_tmp\_t, portage\_tmpfs\_t | Type used by Portage for temporary files . | 

### Other types

Besides the files and domains, the following types are also defined in the **portage** module:

| Type | Description | 
|---|---|
| portage\_devpts\_t | Type used for the terminal output device/location | 

## Using the portage module

### File locations

The policy offered only contains the right file context rules for the default locations. If you deviate from these locations, you'll need to update the contexts accordingly.

The following table provides an overview of the Portage settings (variables in make.conf) that are commonly changed by end users, and the file context that it should have.

| Variable in make.conf | Default Location | Old location | File context(s) | 
|---|---|---|---|
| `${PORTDIR}` | /var/db/repos/gentoo | /usr/portage | system\_u:object\_r:portage\_ebuild\_t | 
| `${DISTDIR}` | /var/cache/distfiles | /usr/portage/distfiles | system\_u:object\_r:portage\_ebuild\_t | 
| `${DISTDIR}`/{git,svc,cvs}-src | /var/cache/distfiles/{git,svc,cvs}-src | /usr/portage/distfiles/{git,svc,cvs}-src | system\_u:object\_r:portage\_srcrepo\_t | 
| `${PKGDIR}` | /var/cache/binpkgs | /usr/portage/packages | system\_u:object\_r:portage\_ebuild\_t | 
| `${PORT_LOGDIR}` | /var/log/portage | /var/log/portage | system\_u:object\_r:portage\_log\_t | 
| `${PORTAGE_TMPDIR}` | /var/tmp/portage | /var/tmp/portage | system\_u:object\_r:portage\_tmp\_t | 

If you use different locations, use the following commands to update the file contexts accordingly (example to use /var/repo/portage for the Portage tree instead of /usr/portage):

`root #````
semanage fcontext -a -t portage_ebuild_t /var/repo/portage
```
`root #````
restorecon -R /var/repo/portage
```
Don't forget that Portage uses subdirectories with different labels (think distfiles or the repositories for the live ebuilds) so take care when relabeling locations!

If you are using different mounts, you might need to use the `rootcontext=` mount option to set the initial context. If the file system does not support SELinux contexts (like NFS), you can use the context= mount option to force the context of all files on the mounted location.

### SELinux booleans

The Portage module within Gentoo defines one boolean, called portage\_use\_nfs.

- When *portage\_use\_nfs* is enabled, then the Portage-related domains will be able to manage the nfs\_t and as such, allow for the Portage tree and other locations to be NFS-mounted without correcting their label (which is still supported when using the context= mount option).

To switch booleans, use setsebool or togglesebool.

`root #``setsebool -P portage_use_nfs on`
