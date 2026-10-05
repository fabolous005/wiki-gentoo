<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Software_builds_fail_with_perl_module_missing_even_though_it_is_installed | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Software_builds_fail_with_perl_module_missing_even_though_it_is_installed -->
---
title: Software builds fail with Perl module missing even though it is installed
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Software_builds_fail_with_perl_module_missing_even_though_it_is_installed
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-01"
fingerprint: ba1d2e4ecdc9a5e5
license: CC BY-SA 4.0
---

# Software builds fail with Perl module missing even though it is installed

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Synopsis

When building software, it fails in the configure phase with a message similar to the following:

`root #``emerge x11-misc/shared-mime-info-1.2-r1`
...
checking for perl >= 5.8.1... 5.16.3
checking for XML::Parser... configure: error: XML::Parser perl module is required for intltool

However, after checking it can be verified that that XML::Parser is installed on the system:

`root #``emerge -p dev-perl/XML-Parser`
\[ebuild   R      \] dev-perl/XML-Parser-2.410.0

## Environment

This error can come up on any system with Perl.

## Analysis

Perl modules are built for a particular Perl target. When Perl upgrades to a higher version, these modules are not automatically rebuilt. As a result, Perl itself might be a higher version although the module is only available for a lower version:

`root #``perl --version`
This is perl 5, version 38, subversion 2 (v5.38.2) built for x86\_64-linux
...

`root #``qlist XML-Parser | sed -e "s:XML/Parser.*::g" | sort | uniq`
/usr/lib64/perl5/vendor\_perl/5.38/x86\_64-linux

In the above examples, Perl is at 5.38 whereas the XML::Parser module is for 5.12.4.

The solution is to rebuild the XML-Parser package, or even better, rebuild all Perl-depending modules.

## Resolution

Use perl-cleaner to rebuild all Perl modules currently built for older Perl versions.

`root #``perl-cleaner modules`
### If perl-cleaner is not working

If perl-cleaner does not work, please file a bug. But as an interim solution, the following tools can help address this issue manually:

`root #``equery belongs --name-only /usr/lib64/perl5/vendor_perl/5.38/`
This will give a list of all Gentoo packages that are still installed for version `5.38`, which need reinstalling.

You may want to install them by hand, or this command may do the trick:

`root #``emerge --verbose --ask --tree --oneshot $( equery belongs --name-only /usr/lib64/perl5/vendor_perl/5.38/ | sed 's/ .*//' )`
### If the local kernel version contains a *user@host* appendix

This could be misinterpreted by Perl in a file like /usr/lib64/perl5/5.38/x86\_64-linux-thread-multi/Errno.pm *(see above)*.

```
"$Config{'archname'}-$Config{'osvers'}" eq
"x86_64-linux-thread-multi-3.17.3-gentoo_5.enihprom@balaenoptera.hyperprime.gaia" or
   die "Errno architecture (x86_64-linux-thread-multi-3.17.3-gentoo_5.enihprom@balaenoptera.hyperprime.gaia) does not match executable architecture ($Config{'archname'}-$Config{'osvers'})";
```
Since Perl interprets `@`-prefixed variables inside double quoted strings as array, changing these lines that contain `@` symbols to be preceded by `\` (backslashes) should help temporarily resolve the problem:

**`/usr/lib64/perl5/5.18.2/x86_64-linux-thread-multi/Errno.pm`**

```
"$Config{'archname'}-$Config{'osvers'}" eq
'x86_64-linux-thread-multi-3.17.3-gentoo_5.enihprom@balaenoptera.hyperprime.gaia' or
   die "Errno architecture (x86_64-linux-thread-multi-3.17.3-gentoo_5.enihprom\@balaenoptera.hyperprime.gaia) does not match executable architecture ($Config{'archname'}-$Config{'osvers'})";
```
Unfortunately this specific file contains a comment saying:

\# This file is auto-generated. \*\*\*ANY\*\*\* changes here will be lost

This bug should be fixed inside the escaping policy of Perls standard library build scripts and this section removed.
