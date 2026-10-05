<!-- source: https://wiki.gentoo.org/wiki/Su | group: Gentoo Wiki (Main) | wiki-title: Su -->
---
title: su
url: https://wiki.gentoo.org/wiki/Su
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-08"
fingerprint: "92e9ddc000a59bb5"
license: CC BY-SA 4.0
---

# su

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The **su** (**s**ubstitute **u**ser) command can be used to adopt the privileges of other users from the system.

The command is provided by the [util-linux](https://wiki.gentoo.org/wiki/Util-linux) package, that has the [su](https://packages.gentoo.org/useflags/su) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) enabled by default. The su command is also available with [sys-apps/shadow](https://packages.gentoo.org/packages/sys-apps/shadow), that also has a [su](https://packages.gentoo.org/useflags/su) [USE flag. Avoid installing both these commands simultaneously.](https://wiki.gentoo.org/wiki/USE_flag)

## Caveats

The use of su to access the user root is only permitted when the calling user is a member of the user group wheel.

In the next example the user *john* is added to the user group wheel.

`root #``usermod -aG wheel john`
## Usage

`user $``su --help````
Usage:
 su [options] [-] [<user> [<argument>...]]
 
Change the effective user ID and group ID to that of <user>.
A mere - implies -l.  If <user> is not given, root is assumed.
 
Options:
 -m, -p, --preserve-environment      do not reset environment variables
 -w, --whitelist-environment <list>  don't reset specified variables
 
 -g, --group <group>             specify the primary group
 -G, --supp-group <group>        specify a supplemental group
 
 -, -l, --login                  make the shell a login shell
 -c, --command <command>         pass a single command to the shell with -c
 --session-command <command>     pass a single command to the shell with -c
                                   and do not create a new session
 -f, --fast                      pass -f to the shell (for csh or tcsh)
 -s, --shell <shell>             run <shell> if /etc/shells allows it
 -P, --pty                       create a new pseudo-terminal
 
 -h, --help                      display this help
 -V, --version                   display version
 
For more details see su(1).
```
### Adopt root privileges

su will run commands as root by default. Since not specifying a username will cause su to ask for root privileges, the following command will run as root and halt the system:

`user $``su -c 'shutdown -h now'`
### Adopt another user's privileges

It is also possible to specify a user other than root to substitute commands. The following example will run the command echo as the user larry:

`user $``su -c 'echo "Moo to the Gentoo Wiki reader out there!"' larry`
