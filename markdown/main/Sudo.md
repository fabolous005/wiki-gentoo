<!-- source: https://wiki.gentoo.org/wiki/Sudo | group: Gentoo Wiki (Main) | wiki-title: Sudo -->
---
title: sudo
url: https://wiki.gentoo.org/wiki/Sudo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-19"
fingerprint: f7119d5865433ba5
license: CC BY-SA 4.0
---

# sudo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The sudo command provides a simple and secure way to configure privilege escalation — i.e., letting normal users execute certain (or even all) commands as root or as another user, either with or without giving a password.

To allow some users to perform certain administrative steps on a system without granting them total [root access](https://en.wikipedia.org/wiki/Superuser), using sudo is the best option. Using sudo allows control over who can do what.

This article is meant as a quick introduction - the [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo) package is a lot more powerful than what is described here. It has special features for editing files as a different user (sudoedit), running from within a script (so it can background, read the password from standard input instead of the keyboard, etc.), etc.

Please read the [sudo(8)](https://man.archlinux.org/man/sudo.8.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [sudoers(5)](https://man.archlinux.org/man/sudoers.5.en) [manual pages for more information.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## Installation

### USE flags


| [+secure-path](https://packages.gentoo.org/useflags/+secure-path) | Replace PATH variable with compile time secure paths | 
| [+sendmail](https://packages.gentoo.org/useflags/+sendmail) | Allow sudo to send emails with sendmail | 
| [gcrypt](https://packages.gentoo.org/useflags/gcrypt) | Use message digest functions from dev-libs/libgcrypt instead of sudo's | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [offensive](https://packages.gentoo.org/useflags/offensive) | Let sudo print insults when the user types the wrong password | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [skey](https://packages.gentoo.org/useflags/skey) | Enable S/Key (Single use password) authentication support | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [sssd](https://packages.gentoo.org/useflags/sssd) | Add System Security Services Daemon support | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask app-admin/sudo`
## Configuration

### Logging activity

One additional advantage of sudo is that it can [log](https://wiki.gentoo.org/wiki/Logging) any attempt (successful or not) to run an application. This is very useful when tracking who made that one fatal mistake that took 10 hours to fix :)

### Granting permissions

The [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo) package allows the system administrator to grant permission to other users to execute one or more applications they would normally have no right to. Unlike using the `setuid` bit on these applications sudo gives a more fine-grained control on *who* can execute a certain command and *when*.

With sudo a clear list can be made of *who* can execute a certain application. If the [setuid](https://en.wikipedia.org/wiki/setuid) bit is set on an executable, any user would be able to run the application (or any user of a certain group, depending on the permissions used). With sudo the user can (and probably should) be required to provide a password in order to execute the application.

The sudo configuration is managed by the /etc/sudoers file.

### Basic syntax

The most difficult part of sudo is the /etc/sudoers syntax. The basic syntax is as follows:

This line tells sudo that the user, identified by `user` and logged in on the system `host`, can execute the command `command` (which can also be a comma-separated list of allowed commands).

A more real-life example might make this more clear: To allow the user larry to execute emerge when they are logged in on localhost:

The user name can also be substituted with a group name, in which case the name is prefaced by a `%` sign. For instance, to allow any one in the wheel group to execute emerge:

To enable more than one command for a given user on a given machine, multiple commands can be listed on the same line. For instance, to allow larry to not only run emerge but also ebuild and emerge-webrsync as root:

The precise command line can also be specified (including parameters and arguments) not just the name of the executable. This is useful to restrict the use of a certain tool to a specified set of command options. The sudo tool allows [shell](https://wiki.gentoo.org/wiki/Shell)-style [wildcards](https://en.wikipedia.org/wiki/Wildcard_character) (AKA meta or glob characters) to be used in path names as well as command-line arguments in the sudoers file. Note that these are *not* regular expressions.

Here is an example of sudo from the perspective of a first-time user of the tool who has been granted access to the full power of emerge:

`user $``sudo emerge -uDN world````
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:
 
    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.
 
Password: ## (Enter the user password, not root!)
```
The password that sudo requires is the user's own password. This is to make sure that no terminal that is accidentally left open to others is abused for malicious purposes.

### Basic syntax with LDAP

The [ldap](https://packages.gentoo.org/useflags/ldap) [and](https://wiki.gentoo.org/wiki/USE_flag) [pam](https://packages.gentoo.org/useflags/pam) [USE flags are needed for LDAP support.](https://wiki.gentoo.org/wiki/USE_flag)

When using sudo with LDAP, sudo will read configuration from the LDAP server as well. So two configuration files need to edited.

Firstly, /etc/ldap.conf.sudo:

**`/etc/ldap.conf.sudo`**

When done, use [chmod(1)](https://man.archlinux.org/man/chmod.1.en) [to set the file's permissions to 400.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Secondly, add the following line to /etc/nsswitch.conf:

**`/etc/nsswitch.conf`**

The following LDAP entry will need to be added for sudo.

For further information about using sudo with LDAP, refer to the [Sudoers LDAP Manual](https://www.sudo.ws/man/sudoers.ldap.man.html).

### Using aliases

In larger environments having to enter all users over and over again (or hosts, or commands) can be a daunting task. To ease the administration of /etc/sudoers *aliases* can be defined. The format to declare aliases is quite simple:

One alias that always works, for any position, is the `ALL` alias (to make a good distinction between aliases and non-aliases it is recommended to use capital letters for aliases). The `ALL` alias is an alias to all possible settings.

A sample use of the `ALL` alias to allow *any* user to execute the shutdown command if they are logged on locally is:

Another example is to allow the user larry to execute the emerge command as root, regardless of where they are logged in from:

More interesting is to define a set of users who can run software administrative applications (such as emerge and ebuild) on the system and a group of administrators who can change the password of any user, except root!

### Non-root execution

It is also possible to have a user run an application as a different, non-root user. This can be very interesting when running applications as a different user (for instance apache for the web server) and want to allow certain users to perform administrative steps as that user (like killing zombie processes).

Inside /etc/sudoers list the user(s) in between `(` and `)` before the command listing:

For instance, to allow larry to run the kill tool as the apache or gorg user:

With this set, the user can run sudo -u to select the user they want to run the application as:

`user $``sudo -u apache pkill apache`
An alias can be set for the user to run an application as using the `Runas_Alias` directive. Its use is identical to the other `_Alias` directives we have seen before.

### Passwords and default settings

By default, sudo asks the user to identify themselves using their own password. Once a password is entered, sudo remembers it for 5 minutes, allowing the user to focus on their tasks and not repeatedly re-entering their password.

Of course, this behavior can be changed: set the `Defaults:` directive in /etc/sudoers to change the default behavior for a user.

For instance, to change the default 5 minutes to 0 (never remember):

A setting of `-1` would remember the password indefinitely (until the system reboots).

A different setting would be to require the password of the user that the command should be run as and not the users' personal password. This is accomplished using `runaspw`. In the following example we also set the number of retries (how many times the user can re-enter a password before sudo fails) to `2` instead of the default 3:

Another interesting feature is to keep the `DISPLAY` variable set so that graphical tools can be executed:

Dozens of default settings can be changed using the `Defaults:` directive. Fire up the sudoers manual page and search for `Defaults`.

To allow a user to run a certain set of commands without providing any password whatsoever, start the commands with `NOPASSWD:`, like so:

### Bash completion

Users that want bash completion with sudo should ensure that the [app-shells/bash-completion](https://packages.gentoo.org/packages/app-shells/bash-completion) package is installed.

`root #``emerge --ask app-shells/bash-completion`
### zsh completion

Users that want zsh completion with sudo should install the [app-shells/zsh-completions](https://packages.gentoo.org/packages/app-shells/zsh-completions) package.

`root #``emerge --ask app-shells/zsh-completions`
## Usage

### Listing privileges

To list the current user's capabilities, run sudo -l :

`user $``sudo -l````
User larry may run the following commands on this host:
    (root)   /usr/libexec/xfsm-shutdown-helper
    (root)   /usr/bin/emerge
    (root)   /usr/bin/passwd [a-zA-Z0-9_-]*
    (root)   !/usr/bin/passwd root
    (apache) /usr/bin/pkill
    (apache) /bin/kill
```
Any command in /etc/sudoers that does not require a password to be entered, a password will not be required to list the entries either. Otherwise sudo will ask for a password if it isn't remembered.

### Prolonging password timeout

By default, if a user has entered their password to authenticate themselves to sudo, it is remembered for 5 minutes. If the user wants to prolong this period, they can run sudo -v to reset the time stamp so that it will take another 5 minutes before sudo asks for the password again.

`user $``sudo -v`
The inverse is to kill the time stamp using sudo -k.

## See also

- [doas](https://wiki.gentoo.org/wiki/Doas) — provides a way to perform commands as another user.
- [su](https://wiki.gentoo.org/wiki/Su) — used to adopt the privileges of other users from the system
