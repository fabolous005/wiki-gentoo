<!-- source: https://wiki.gentoo.org/wiki/Zsh | group: Gentoo Wiki (Main) | wiki-title: Zsh -->
---
title: zsh
url: https://wiki.gentoo.org/wiki/Zsh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-25"
fingerprint: "33b61b1fdd9ffbcc"
license: CC BY-SA 4.0
---

# zsh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

zsh (**Z sh**ell) is an interactive login shell that can also be used as a powerful scripting language interpreter. It is similar to [bash](https://wiki.gentoo.org/wiki/Bash) and the Korn shell, but offers extensive configurability, powerful command-line completion, file globbing, and spelling correction.

See the [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator#General_usage) article for some general usage pointers.

## Installation

### USE flags


| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Add support for sys-libs/gdbm (GNU database libraries) | 
| [maildir](https://packages.gentoo.org/useflags/maildir) | Add support for maildir (\~/.maildir) style mail spools | 
| [pcre](https://packages.gentoo.org/useflags/pcre) | Add support for Perl Compatible Regular Expressions | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Install [app-shells/zsh](https://packages.gentoo.org/packages/app-shells/zsh):

`root #``emerge --ask app-shells/zsh`
### Add-ons

#### app-shells/zsh-completions

Emerging [app-shells/zsh-completions](https://packages.gentoo.org/packages/app-shells/zsh-completions) enables auto-completion for arguments of commands, which is one of the advantages zsh has over other shells:

`root #``emerge --ask app-shells/zsh-completions`
#### app-shells/gentoo-zsh-completions

Emerging [app-shells/gentoo-zsh-completions](https://packages.gentoo.org/packages/app-shells/gentoo-zsh-completions) enables Gentoo specific auto-completion for arguments of Portage and other Gentoo commands:

`root #``emerge --ask app-shells/gentoo-zsh-completions`
When installing this package be sure to add the following to the respective \~/.zshrc files:

**`~/.zshrc`**

**Enabling Portage completions and Gentoo prompt for zsh**

To enable a cache for the completions add:

**`~/.zshrc`**

**Enabling cache for the completions for zsh**

## Configuration

### Invocation

`user $``zsh`
Upon running zsh for the first time as a new user, you will be greeted by a basic configuration dialog. The setup process can be skipped by pressing `q`. If the setup process is skipped zsh can be setup manually.

### Setting zsh as the default shell

To make zsh the default shell for a user, run:

`user $``chsh -s /bin/zsh`
Note that this method is fine, but trying to use zsh as /bin/sh is strongly discouraged as it has issues in POSIX emulation mode. For example, glibc may fail to build: [bug #804645](https://bugs.gentoo.org/show_bug.cgi?id=804645). If trying to minimise use of Bash on the system, consider [Dash](https://wiki.gentoo.org/wiki/Dash) for /bin/sh.

### File

zsh's main configuration file is located in each user's home directory at \~/.zshrc. Reload this file in running shells for the changes to take effect:

`user $``source ~/.zshrc`
### Scripting

Launch scripts can be made in each user's home directory at \~/.zprofile. This file is executed when Zsh starts as a login shell. The \~/.zprofile file may need to be created:

`user $``touch ~/.zprofile`
### Included prompt customizations

Zsh includes some prompt customization options. These can be viewed by running:

`user $``prompt -p`
For example, some of these appear as:


There are many more available than are included here.

A prompt can be selected using:

`user $``prompt -s [theme]`
And with arguments, for example:

`user $``prompt -s fire red magenta blue white white white`
This example will set the fire theme with the selected colors.

This can be set permanently in \~/.zshrc:

**`~/.zshrc`**

This example includes the same colors, but these are not required. If the prompt was set to `gentoo` using the configuration provided above, then that line may be changed instead. More information can be found in the [Zsh documentation](https://zsh.sourceforge.io/Doc/Release/User-Contributions.html#Prompt-Themes).

### oh-my-zsh

The zsh community created numerous tweaks, the easiest way to acquire them is to install [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) framework. It contains handy plugins and eye [candy themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes), and makes their configuration very easy. However you should always consider the security risk involving running code outside of Gentoo developers jurisdiction.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-shells/zsh`
## Troubleshooting

### Inactive keys

If the `Home` or `End` or `Del` key does not work, try entering keybindings into the user's \`\~/.zshrc\` such as examples in [ArchWiki](https://wiki.archlinux.org/index.php/Zsh#Key_bindings).

### Garbled display

The output of a shell can, in some conditions, become corrupt. See the [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator#Garbled_display) article for instructions to help fix this.

## See also

- [Zsh/Guide](https://wiki.gentoo.org/wiki/Zsh/Guide) — details installation, configuration, and light usage functionality for zsh.
- [Bash](https://wiki.gentoo.org/wiki/Bash) — the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.
- [Dash](https://wiki.gentoo.org/wiki/Dash) — a small, fast, and [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compliant [shell](https://wiki.gentoo.org/wiki/Shell).
- [Fish](https://wiki.gentoo.org/wiki/Fish) — a smart and user-friendly command line [shell](https://wiki.gentoo.org/wiki/Shell) for OS X, Linux, and the rest of the family.
- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Nushell](https://wiki.gentoo.org/wiki/Nushell) — a new kind of [shell](https://wiki.gentoo.org/wiki/Shell) for OS X, Linux, and Windows.
