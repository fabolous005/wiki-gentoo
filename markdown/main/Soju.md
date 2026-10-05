<!-- source: https://wiki.gentoo.org/wiki/Soju | group: Gentoo Wiki (Main) | wiki-title: Soju -->
---
title: Soju
url: https://wiki.gentoo.org/wiki/Soju
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-18"
fingerprint: "92f91d583107bbd7"
license: CC BY-SA 4.0
---

# Soju

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

soju is a user-friendly IRC bouncer:

soju connects to upstream IRC servers on behalf of the user to provide extra functionality. soju supports many features such as multiple users, numerous IRCv3 extensions, chat history playback and detached channels.


## Installation

### USE flags


| [+sqlite](https://packages.gentoo.org/useflags/+sqlite) | Add support for sqlite - embedded sql database | 
| [moderncsqlite](https://packages.gentoo.org/useflags/moderncsqlite) | Use moderncsqlite, a cgo-free port of SQLite | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask net-irc/soju`
## Configuration

An example configuration file is provided below; refer to the [soju(1)](https://man.archlinux.org/man/soju.1.en) [man page for a detailed list of configuration options.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

**`/etc/soju/config`**

**Example configuration**

### Service

#### OpenRC

To start the soju OpenRC service:

`root #``rc-service soju start`
To start the soju service on system boot, add it to the default runlevel:

`root #``rc-update add soju default`
#### systemd

To start the soju systemd service:

`root #``systemctl start soju.service`
To start the soju systemd service at boot, enable it:

`root #``systemctl enable soju.service`
### Adding networks

Once the soju service is running, connect to it via an [IRC client](https://wiki.gentoo.org/wiki/IRC#Available_software), and send a "help" message to the BouncerServ service to get a list of available commands:

/msg BouncerServ help

The BouncerServ channel can be used to ask for help on specific commands:

```
<user> help network create
<BouncerServ> network create -addr <addr> [-name name] [-username username]
              [-pass pass] [-realname realname] [-certfp fingerprint] [-nick
              nick] [-auto-away auto-away] [-enabled enabled]
              [-connect-command command]...: add a new network
```
and to run those commands.

For example, to add the Libera.chat network:

\<user> network create -addr [ircs://irc.libera.chat](ircs://irc.libera.chat) -name libera -nick \<nick> -enabled true -connect-command "PRIVMSG NickServ :IDENTIFY \<nick-password>" -connect-command "PRIVMSG NickServ :RELEASE \<nick>" -connect-command "PRIVMSG NickServ :REGAIN \<nick>"

Note that the `-name` and `-username` options are for server access, not nick management; however, the default value for `-nick` is the value of `-username`.

## External resources

- [soju(1)](https://man.archlinux.org/man/soju.1.en)- [soju client list](https://codeberg.org/emersion/soju/src/branch/master/contrib/clients.md) – Instructions for setting up various IRC clients to connect to soju
