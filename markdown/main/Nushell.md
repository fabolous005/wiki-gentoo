<!-- source: https://wiki.gentoo.org/wiki/Nushell | group: Gentoo Wiki (Main) | wiki-title: Nushell -->
---
title: nushell
url: https://wiki.gentoo.org/wiki/Nushell
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: b40b58584da5e2d4
license: CC BY-SA 4.0
---

# nushell

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Nushell** is a new kind of [shell](https://wiki.gentoo.org/wiki/Shell) for OS X, Linux, and Windows. Nushell uses structured data allowing for powerful but simple pipelines. It offers great error messages, completions and works with existing datatypes.

## Installation

### USE flags


| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [mcp](https://packages.gentoo.org/useflags/mcp) | Build MCP server | 
| [plugins](https://packages.gentoo.org/useflags/plugins) | Build official plugins | 
| [system-clipboard](https://packages.gentoo.org/useflags/system-clipboard) | System clipboard support in \`reedline\` | 

### Emerge

Install [app-shells/nushell](https://packages.gentoo.org/packages/app-shells/nushell):

`root #``emerge --ask app-shells/nushell`
## Caveats

Nushell is not [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compatible, thus it is *strongly advised not to set Nushell as the login shell for any user*. Refer to [the discussion for the fish shell](https://wiki.gentoo.org/wiki/Fish#Caveats) for more details.

A simple working solution is to start nushell from the terminal, as advised in [the official documentation](https://www.nushell.sh/book/default_shell.html#setting-nu-as-default-shell-on-your-terminal).

To use nushell as the default login shell, \~/.bashrc can be used to have nushell inherit the environment from the login shell, which is left as Bash. Add the following to the user's \~/.bashrc, making sure it's placed below the test for an interactive shell, e.g. at the end of the file:

**`~/.bashrc`**

```
[...]
# Use nushell in place of bash
# keep this line at the bottom of ~/.bashrc
[ -x /usr/bin/nu ] && SHELL=/usr/bin/nu exec nu
```
When bash is started as an interactive shell, this will automatically launch nushell for the user, once bash has fully initialized the correct system environment. It will also set the `SHELL` environment variable to `/usr/bin/nu` in nushell.

## Features

In Nushell, data is structured as tables. This allows one to query, filter and sort easily. For example, a search for all Emacs processes can be done with:

`user $``ps | name =~ emacs`
List files with older first:

`user $``ls | sort-by modified`
Getting the basename of several files:

`user $``ls *.png  | get name | path parse | get stem`
Applying a command to each line in the table can done using a closure with each. Converting all Org-mode files in a directory to Markdown is a good illustration of a closure and string substitution:

`user $``ls *.org  | each {|e| pandoc $e.name -o $"($e.name | path parse | get stem).md"}` Thanks to string substitution, it allows for complex but natural commands that would have been rather awkward in Bash.

## Configuration

Nushell creates a default configuration file in \~/.config/nushell/config.nu. Alternatively, one could access said file with

`user $``config nu`
However, one needs to note that the default editor (`config.buffer_editor`) is needed to be set first in order to use this method. One could simply invoke

`user $``$env.config.buffer_editor = "vi"`
to initialize it within the current session to be able to run `config nu`.

Disabling the welcome banner is done with:

**`~/.config/nushell/config.nu`**

Changing the theme requires defining and enabling a theme with:

**`~/.config/nushell/config.nu`**

Due to Nushell development, updates may break the configuration file.

Also please note that by defining `config` variable under the config file, you are actually defining the key-value pair (akin to dictionary) of the said config. Hence, welcome banner example is a preferable way if you are intended to fix just a few key-value pairs rather than the entire environment like changing the theme.

### Environment variables

Environment variables can be set up in the current session with:

`user $``$env.FOO = 'BAR'`
`PATH` is a list of strings so appending a new location can be done with:

`user $``$env.PATH = ($env.PATH | prepend "/home/USER/.juliaup/bin")`
Or, alternatively

`user $``$env.PATH ++= ["/home/USER/.juliaup/bin"]`
as Nushell store `PATH` as a list, of which one could append it directly.

To define environment variables in all sessions, put them in \~/.config/nushell/env.nu.

## Tips

### SSH agent

Since eval is not available in nushell, [several workarounds exist](https://www.nushell.sh/cookbook/ssh_agent.html#workarounds). One option is to run ssh-agent as a user service.

For example, with systemd create:

**`~/.config/systemd/user/ssh-agent.service`**

```
[Unit]
Description=SSH key agent
 
[Service]
Type=simple
Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
ExecStart=/usr/bin/ssh-agent -D -a $SSH_AUTH_SOCK
 
[Install]
WantedBy=default.target
```
Then add the following to \~/.bash\_profile:

**`~/.bash_profile`**

```
export SSH_AUTH_SOCK=/run/user/1000/ssh-agent.socket
ssh-add "$HOME/.ssh/id_ed25519"
```
### Using Nushell with Nix

The modifications to \~/.bashrc described above prevent Nushell from applying its environment variable configuration. [Nix](https://nixos.org/) users can use the following variant to work around this problem by disabling Nushell execution within a nix-shell:

**`~/.bashrc`**

```
[...]
# Use nushell in place of bash
# keep this line at the bottom of ~/.bashrc
[ -x /usr/bin/nu ] && [ -z "$IN_NIX_SHELL" ] && SHELL=/usr/bin/nu exec nu
```
With this configuration in place, running nix-shell will result in a Bash shell. To start a nix-shell with Nushell, use nix-shell --command nushell, which will correctly apply the Nix environment before launching Nushell.

### Sudo inside Doom Emacs

Accessing files with sudo inside Emacs fails as it cannot run the /bin/sh -i command. Setting Emacs' `shell-file-name` variable to `/bin/bash` does not work. A simple way to get around it is to reconfigure the `SHELL` environment variable for Doom Emacs with:

`user $``SHELL="/bin/bash" ~/.emacs.d/bin/doom env`
### Signing Git commit messages with GPG

If it fails with gpg: signing failed: Inappropriate ioctl for device, set `GPG_TTY` to the output of tty with:

`user $``$env.GPG_TTY = (tty)`
### Interactive news item reading

To read [eselect news](https://wiki.gentoo.org/wiki/Eselect#News) in a more interactive manner, one could take advantage of Nushell's features such as parsing and fuzzy input:

This could be further customized to only read news from a certain period by adding an additional pipe like | where Date > (date now) - 2wk to only read items for the past two weeks.

### Python virtual environment

While default `venv` does not provide the `activate` script to initialize the python environment, [dev-python/virtualenv](https://packages.gentoo.org/packages/dev-python/virtualenv) does provide said script.

First, start by creating the environment

`user $``python -m virtualenv testenv`
This will create the ./testenv directory similar to how `venv` create the virtual environment. From here, one can simply invoke

`user $``overlay use ./testenv/bin/activate.nu`
to activate said environment.



## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Bash](https://wiki.gentoo.org/wiki/Bash) — the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.
- [Dash](https://wiki.gentoo.org/wiki/Dash) — a small, fast, and [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compliant [shell](https://wiki.gentoo.org/wiki/Shell).
- [Zsh](https://wiki.gentoo.org/wiki/Zsh) — an interactive login shell that can also be used as a powerful scripting language interpreter.
- [Fish](https://wiki.gentoo.org/wiki/Fish) — a smart and user-friendly command line [shell](https://wiki.gentoo.org/wiki/Shell) for OS X, Linux, and the rest of the family.
