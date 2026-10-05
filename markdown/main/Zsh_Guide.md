<!-- source: https://wiki.gentoo.org/wiki/Zsh/Guide | group: Gentoo Wiki (Main) | wiki-title: Zsh/Guide -->
---
title: Zsh/Guide
url: https://wiki.gentoo.org/wiki/Zsh/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-13"
fingerprint: b9beba5fcda57b9d
license: CC BY-SA 4.0
---

# Zsh/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide details installation, configuration, and light usage functionality for zsh.

## Installation

### Emerge

Emerging zsh under Gentoo ([app-shells/zsh](https://packages.gentoo.org/packages/app-shells/zsh)) will install it:

`root #``emerge --ask app-shells/zsh`
### Additional software

It is also a good idea to install the zsh auto-completion scripts ([app-shells/zsh-completions](https://packages.gentoo.org/packages/app-shells/zsh-completions)):

`root #``emerge --ask app-shells/zsh-completions`
For Gentoo systems there is also a set of special Gentoo completion scripts:

`root #``emerge --ask app-shells/gentoo-zsh-completions`
## Configuration

### Initialization

When starting zsh for the first time, the following message will appear. This menu can be skipped (by pressing `q` then `Enter`) as an init file will be created later in this guide.

`user $``zsh````
This is the Z Shell configuration function for new users, zsh-newuser-install.
You are seeing this message because you have no zsh startup files
(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory
~).  This function can help you with a few settings that should
make your use of the shell easier.
You can:
(q)  Quit and do nothing.  The function will be run again next time.
(0)  Exit, creating the file ~/.zshrc containing just a comment.
     That will prevent this function being run again.
(1)  Continue to the main menu.
--- Type one of the keys in parentheses --- q
```
### Advanced tab-completion

To enable the famous zsh tab-completion system, the following commands must be run:

`user $````
autoload -U compinit
```
`user $````
compinit
```
Default completion style is quite plain and ugly. To improve its appearance, enter the following commands:

`user $````
zstyle ':completion:*:descriptions' format '%U%B%d%b%u'
```
`user $````
zstyle ':completion:*:warnings' format '%BSorry, no matches for: %d%b'
```
For an introduction to creating your own completion functions, refer to [The zsh completions HOWTO](https://github.com/vapniks/zsh-completions/blob/master/zsh-completions-howto.org).

### Command correction

It is also a good idea to enable the auto-correction of the commands typed:

`user $``setopt correctall`
### Prompts

The prompt may seem quite boring however, but we can fix it easily in several steps. First, we need to initialize advanced prompt support:

`user $````
autoload -U promptinit
```
`user $````
promptinit
```
Available prompts samples may be printed via the prompt -p command. Now we can activate our favorite one:

`user $``prompt gentoo``gentoo>``ls`
Another option is to create a custom prompt. To do this the `PS1` environment variable must be modified. For example:

`user $``export PS1="[Test Prompt] > "``[Test Prompt] >``ls`
While it is good to be able to create a custom text prompt, zsh also provides many escape sequences that allow system information to be inserted into the prompt. Some of the escape sequences available are:

| Sequence | Printed | 
|---|---|
| `%T` | System time (HH:MM). | 
| `%*` | System time (HH:MM:SS). | 
| `%D` | System date (YY-MM-DD). | 
| `%n` | Current username. | 
| `%B` - `%b` | Begin - end bold print. | 
| `%U` - `%u` | Begin - end underlining. | 
| `%d` | The current working directory. | 
| `%~` | The current working directory, relative to the home directory. | 
| `%M` | The computer's hostname. | 
| `%m` | The computer's hostname (truncated before the first period). | 
| `%l` | The current tty. | 

These escape sequences may simply be inserted into the environment variable, `PS1`, and zsh will parse them automatically.

`user $``export PS1="[%* - %D] %d %% "``[08:44:23 - 06-02-18] /home/username %``ls`
### History

Unfortunately, the default zsh configuration in Gentoo does not include command history support. As working with a shell without history is very frustrating, we should enter the following commands:

`user $````
export HISTSIZE=2000
```
`user $````
export HISTFILE="$HOME/.history"
```
History won't be saved without the following command:

`user $``export SAVEHIST=$HISTSIZE`
To prevent history from recording duplicated entries (such as ls -l entered many times during single shell session), the `hist_ignore_all_dups` option can be set:

`user $``setopt hist_ignore_all_dups`
A useful trick to prevent particular entries from being recorded into a history by preceding them with at least one space.

`user $``setopt hist_ignore_space`
The following command won't be recorded. Note the extra space before it:

`user $` `cat /proc/cpuinfo`
### Miscellaneous settings

The `autocd` option can be set to avoid tedious typing of cd command while changing current directory (for example /etc instead of cd /etc).

`user $``setopt autocd`
If standard bash-like globbing does not satisfy, `extendedglob` option may be set to enable extended globbing (one similar to regular expressions).

`user $``setopt extendedglob`
When option above is set, extended globbing queries such as cp ^\*.(tar are available for use.

### Saving settings

#### Saving zsh settings

Once we have customized zsh the way we like it, it is a good idea to save these options as the zsh defaults for the system. One possible way to achieve this is to write our settings in the /etc/zsh/zshrc script. Alternatively, we could make them the defaults for our account only be editing \~/.zshrc.

**`~/.zshrc`**

**An example \~/.zshrc file**

```
#!/bin/zsh
# Completion
autoload -U compinit
compinit
# Correction
setopt correctall
# Prompt
autoload -U promptinit
promptinit
prompt gentoo
```
### Making zsh default shell

#### The regular way

The shell for the current user can be changed using the chsh command:

`user $``chsh -s /bin/zsh`
This will set zsh as default shell for the current account. In order to set it as a shell for another account chsh must be ran with as root (super user) privileges.

#### Alternative (without chsh)

If a system administrator (despite gentle requests) refuses to set the default login shell to zsh and the chsh command does not work, bash can be used to execute zsh on startup. Only a slight modification to the \~/.bashrc file is required:

**`~/.bashrc`**

```
exec zsh
```
## Usage

### Invocation

To start zsh for a single session, invoke zsh from a terminal emulator.

For a more permanent solution, please refer to the above [Saving settings](https://wiki.gentoo.org#Saving_settings) section. After the installation, zsh seems to be very simple and rather limited in the area of functionality. To change this, several configuration changes need to be performed.

## Themes

You can make zsh more powerful and visually interesting by installing oh-my-zsh.

Using curl:

Using wget:

NOTE: the installer will rename the existing `.zshrc` file to `.zshrc.pre-oh-my-zsh`.

You can now add a theme you like (See also section):

`user $``nano .zshrc`
Then change ZSH\_THEME="" to ZSH\_THEME="afowler".

## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Bash](https://wiki.gentoo.org/wiki/Bash) — the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.
- [Dash](https://wiki.gentoo.org/wiki/Dash) — a small, fast, and [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compliant [shell](https://wiki.gentoo.org/wiki/Shell).
- [oh-my-zsh themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)--- a collection of themes.
- [oh-my-zsh plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)--- a collection of plugins.
