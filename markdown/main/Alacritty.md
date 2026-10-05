<!-- source: https://wiki.gentoo.org/wiki/Alacritty | group: Gentoo Wiki (Main) | wiki-title: Alacritty -->
---
title: Alacritty
url: https://wiki.gentoo.org/wiki/Alacritty
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-11"
fingerprint: "9f42db5f82a7a3cb"
license: CC BY-SA 4.0
---

# Alacritty

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Alacritty is a [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) focused on simplicity and performance. The performance goal means it *should*<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> be faster than any other terminal emulators available. The simplicity goal means it does not have features such as tabs or splits (which can be provided by some [window managers](https://wiki.gentoo.org/wiki/Window_manager), or  [terminal multiplexers](https://wiki.gentoo.org/wiki/Recommended_tools#Terminal_multiplexers))<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

Alacritty is written in [Rust](<https://en.wikipedia.org/wiki/Rust_(programming_language)>) and GPU-accelerated using [OpenGL](https://en.wikipedia.org/wiki/OpenGL).

## USE flags


## Installation

### Emerge

Install [x11-terms/alacritty](https://packages.gentoo.org/packages/x11-terms/alacritty) package:

`root #``emerge --ask x11-terms/alacritty`
## Configuration

### Files

alacritty does not automatically create or install a configuration file, but it will search for one in the following locations:

- $XDG\_CONFIG\_HOME/alacritty/alacritty.yml
- $XDG\_CONFIG\_HOME/alacritty.yml
- $HOME/.config/alacritty/alacritty.yml
- $HOME/.alacritty.yml

Configuration files with currently supported values are provided with [each upstream release](https://github.com/alacritty/alacritty/releases). On Gentoo, depending on the version installed, the file can be found in the following location. Be sure to adjust the `PV` ( **p**ackage **v**ersion) value to align with whatever version is currently installed on the system.

The default configuration can be created in a users' home directory with the following commands:

`user $````
mkdir --parents ~/alacritty
```
`user $````
bzcat /usr/share/doc/alacritty-${PV}/alacritty.yml.bz2 > ~/alacritty/alacritty.yml
```
By default alacritty will reload the configuration automatically when changes have been written into the file. This behavior can be disabled with the following invocation:

`user $``alacritty --no-live-config-reload`
This can also be disabled via the configuration file:

**`~/.config/alacritty/alacritty.yml`**

**disable live reload**

```
# Live config reload (changes require restart)
live_config_reload: false
```
The configuration file should be downloaded and edited from the [repository's release page](https://github.com/alacritty/alacritty/releases). Explanations are provided in the configuration file.

### Migration from YAML to TOML

Since since version 0.13.0, the [TOML](https://en.wikipedia.org/wiki/TOML) configuration format is used by alacritty. Users with an existing YAML configuration can use the **migrate** subcommand to convert their current configuration:

`user $``alacritty migrate`
### Font configuration

One can run the following command and copy the desired font name:

`user $``fc-list -f '%{family}\n' | awk '!x[$0]++'`
Changing the default font by editing the config file.

**`~/.config/alacritty/alacritty.yml`**

**font configure**

```
# Font configuration (changes require restart)
font:
  # The normal (roman) font face to use.
  normal:
    family: Hack
    # Style can be specified to pick a specific face.
    style: Regular
  # The bold font face
  bold:
    family: Hack
    # Style can be specified to pick a specific face.
    # style: Bold
  # The italic font face
  italic:
    family: Hack
    # Style can be specified to pick a specific face.
    # style: Italic
  size: 11.0
```
This will change the font to one provided by [media-fonts/hack](https://packages.gentoo.org/packages/media-fonts/hack), given that the package is installed.

### Colors configuration

The easiest way is to code Alacritty theme directory and include the relevant theme :

`user $``mkdir -p ~/.config/alacritty/themes` For example, with Gruvbox Light:

**`~/.config/alacritty/alacritty.yml`**

**color schemes**

```
import:
    ~/.config/alacritty/themes/gruvbox_light.yaml
```
More schemes can be found from this page: [alacritty Wiki of Color schemes](https://github.com/alacritty/alacritty/wiki/Color-schemes).

### Transparent background

**`~/.config/alacritty/alacritty.yml`**

**Set background opacity to 0.8**

```
window:
   opacity: 0.8
```
### Configuration with tabbed

Since Alacritty does not support tabs intentionally<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>, one can use [x11-misc/tabbed](https://packages.gentoo.org/packages/x11-misc/tabbed):

`user $``tabbed -r 2 alacritty --embed ""`
See the man page for more information;

`user $``man 1 tabbed`
## Troubleshooting

### Using fcitx

This needs [app-i18n/fcitx](https://packages.gentoo.org/packages/app-i18n/fcitx) and [x11-wm/i3](https://packages.gentoo.org/packages/x11-wm/i3) installed.  The initialization file should look like this:

**`~/.xprofile or ~/.xinitrc`**

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
eval "$(dbus-launch --sh-syntax --exit-with-session)"
exec i3
```
The most important thing is to start [i3](https://wiki.gentoo.org/wiki/I3) last.

### Colorful LS

To modify `ls` to have colorful output, add the following in `/etc/DIR_COLORS`

**`/etc/DIR_COLORS`**

See [related GitHub issue](https://github.com/alacritty/alacritty/issues/2210).

### Window title

The default title is: `Alacritty`. This can be changed via the shell.

#### Bash

In [Bash](https://wiki.gentoo.org/wiki/Bash), one can set the window title by manipulating the environment variable `PROMPT_COMMAND`.

The following will set the window title to: `username@hostname:cwd`.

**`~/.bashrc`**

**window title bar**

```
# set PROMPT_COMMAND
PROMPT_COMMAND=${PROMPT_COMMAND:+$PROMPT_COMMAND; }'printf "\033]0;%s@%s:%s\007" "${LOGNAME}" "${HOSTNAME%%.*}" "${PWD/#$HOME/\~}"'
```
#### Zsh

In [Zsh](https://wiki.gentoo.org/wiki/Zsh), one can set the window title by using the functions `precmd()` and `preexec()`:

The following will set the window title to: `username@hostname: zsh[shell_level] cwd_or_current_command`

For example, when user `larry` is in the `/etc/conf.d` directory, this will be the title bar:

`larry@gentoo.local: zsh[4] /etc/conf.d`

While executing `tail -f /var/log/*`:

`larry@gentoo.local: zsh[4] tail -f /var/log/*`

**`~/.zshrc`**

**window title bar**

```
if [[ "${TERM}" != "" && "${TERM}" == "alacritty" ]]
then
    precmd()
    {
        # output on which level (%L) this shell is running on.
        # append the current directory (%~), substitute home directories with a tilde.
        # "\a" bell (man 1 echo)
        # "print" must be used here; echo cannot handle prompt expansions (%L)
        print -Pn "\e]0;$(id --user --name)@$(hostname): zsh[%L] %~\a"
    }
    preexec()
    {
        # output current executed command with parameters
        echo -en "\e]0;$(id --user --name)@$(hostname): ${1}\a"
    }
fi
```
## See also

- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).

## External resources

- [Rust Meetup January 2017](https://air.mozilla.org/rust-meetup-january-2017/) - A short talk about Alacritty at the Rust Meetup January 2017 (starts at 57:00).

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [https://github.com/kovidgoyal/kitty/issues/2701#issuecomment-636497270](https://github.com/kovidgoyal/kitty/issues/2701#issuecomment-636497270) [https://lwn.net/Articles/751763/](https://lwn.net/Articles/751763/)
2. [↑](https://wiki.gentoo.org#cite_ref-2) Joe Wilm, [Announcing Alacritty, a GPU-accelerated terminal emulator](https://jwilm.io/blog/announcing-alacritty/), jwilm.io. Retrieved on December 6, 2018
3. [↑](https://wiki.gentoo.org#cite_ref-3) [https://github.com/alacritty/alacritty/tree/v0.5.0#faq](https://github.com/alacritty/alacritty/tree/v0.5.0#faq)
