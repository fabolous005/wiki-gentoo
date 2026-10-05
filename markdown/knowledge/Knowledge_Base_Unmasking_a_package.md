<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Unmasking_a_package | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Unmasking_a_package -->
---
title: Knowledge Base:Unmasking a package
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Unmasking_a_package
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-29"
fingerprint: fa93f4c08aab9ca
license: CC BY-SA 4.0
---

# Knowledge Base:Unmasking a package

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

When an ebuild maintainer determines that a package is broken, or may break another package, the ebuild associated to package will be masked. Masked ebuilds must be explicitly *unmasked* in order to be installed to the system. The process of unmasking a package causes the system administrator to *willingly accept the risk* associated with installing the masked package.

Packages are masked for good reason: to benefit the stability and/or safety of the operating system. For this reason, not all of the packages present in the Gentoo [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) will be immediately available to install with the [emerge](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Installing_software) command.

## Trying to emerge a masked package

On trying to install a masked package from the Gentoo repository, emerge will quit after issuing a warning that will look something like this:

`root #``emerge --ask =dev-java/eclipse-ecj-4.15````
These are the packages that would be merged, in order:
Calculating dependencies... done!
!!! All ebuilds that could satisfy "=dev-java/eclipse-ecj-4.15" have been masked.
!!! One of the following masked packages is required to complete your request:
- dev-java/eclipse-ecj-4.15::gentoo (masked by: package.mask)
/var/db/repos/gentoo/profiles/package.mask:
# Miroslav Šulc <fordfrog@gentoo.org> (2020-02-27)
# >=dev-java/ant-eclipse-ecj-4.10 depends on masked >=virtual/{jdk,jre}-11
# www-servers/tomcat >= 9 depends on masked dev-java/eclipse-ecj
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.
```
## Unmasking a package

Unmasking a package will enable installation of packages that have been marked against use.

### Unmask automatically with emerge --autounmask or --autounmask-write

The `--autounmask` flag will let emerge spit out a list of all packages that require unmasking. When paired with `--autounmask-write` and/or `--ask`, emerge prompts the user to write (or not) to the "last semantically-named file" in the /etc/portage/package.accept\_keywords/ location (unless `--pretend` is specified, then no prompt is shown).

Running the emerge command with `--autounmask-write` automatically queues the required [text file alteration](https://wiki.gentoo.org/wiki/Knowledge_Base:Autounmask-write); as with all configuration files, the final update is made by running [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf).

To keep some order in the files, it is recommended to do something like:

`root #````
touch /etc/portage/package.accept_keywords/zzz_autounmask
```
`root #````
emerge mypackage --autounmask-write --autounmask
```
`root #``dispatch-conf`
Examine the diff output of the configuration files, and then press `u` for use-new to accept the changes or press `z` to zap (disregard) the changes.

`root #``mv /etc/portage/package.accept_keywords/zzz_autounmask /etc/portage/package.accept_keywords/mypackage`
Finally, re-run emerge one last time:

`root #``emerge mypackage`
### Unmask manually

#### Flat file

Create the /etc/portage directory if it does not yet exist:

`root #``mkdir -p /etc/portage`
Add the required line to a (new) package.unmask *file*:

`root #``echo "=x11-base/xorg-server-1.11.99.2" >> /etc/portage/package.unmask`
#### Using directories

Create the /etc/portage/package.unmask *directory* if it does not yet exist:

`root #``mkdir -p /etc/portage/package.unmask`
Create a file (or files) for the unmask operations to perform. For instance, to unmask the `xorg-server-1.11.99.2` package as mentioned earlier, run:

`root #``echo "=x11-base/xorg-server-1.11.99.2" > /etc/portage/package.unmask/xorg-server`
Most masked packages will not accept generic keywords, so specific keywords may need to be granted for the masked package.

## How packages are masked

Developers explicitly mask out a package by mentioning it in the [/var/db/repos/gentoo/profiles/package.mask](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/package.mask) file in each profile. The masked file can be coupled with an explanation as to why the package has been masked.

Packages can also be masked by system administrators using [/etc/portage/package.mask](https://wiki.gentoo.org/wiki//etc/portage/package.mask).

See [Knowledge Base:Masking a package](https://wiki.gentoo.org/wiki/Knowledge_Base:Masking_a_package) for more information.

## See also

- [Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package) in the Knowledge Base.
- [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords)
