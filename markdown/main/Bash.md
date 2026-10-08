<!-- source: https://wiki.gentoo.org/wiki/Bash | group: Gentoo Wiki (Main) | wiki-title: Bash -->
---
title: Bash
url: https://wiki.gentoo.org/wiki/Bash
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-07"
fingerprint: b0a18c5845b5bb9d
license: CC BY-SA 4.0
---

# Bash

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GNU Bash** (**B**ourne-**a**gain **sh**ell) is the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.

See the [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator#General_usage) article for some general usage pointers.

## Installation

bash is part of the [*@system* set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) and comes installed on every Gentoo system. It is used internally by [Portage](https://wiki.gentoo.org/wiki/Portage), Gentoo's default package manager, and other Gentoo system components. It is therefore *highly recommended to not uninstall bash* (which is usual for a package in the @system set), this would undoubtedly break the system. Do not uninstall bash just because another shell is installed, used, or set as a [login shell](https://wiki.gentoo.org/wiki/Login_shell).

### USE flags

It is possible to [change USE flags](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE#Declaring_USE_flags_for_individual_packages) for the bash package:


### USE flags for
            [app-shells/bash](https://packages.gentoo.org/packages/app-shells/bash)
            
            The standard GNU Bourne again shell

| [+net](https://packages.gentoo.org/useflags/+net) | Enable /dev/tcp/host/port redirection | 
| [+readline](https://packages.gentoo.org/useflags/+readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [afs](https://packages.gentoo.org/useflags/afs) | Add OpenAFS support (distributed file system) | 
| [bashlogger](https://packages.gentoo.org/useflags/bashlogger) | Log ALL commands typed into bash; should ONLY be used in restricted environments such as honeypots | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [mem-scramble](https://packages.gentoo.org/useflags/mem-scramble) | Build with custom malloc/free overwriting allocated/freed memory | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pgo](https://packages.gentoo.org/useflags/pgo) | Optimize the build using Profile Guided Optimization (PGO) | 
| [plugins](https://packages.gentoo.org/useflags/plugins) | Add support for loading builtins at runtime via 'enable' | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

After making USE modifications, ask Portage to update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
### Shell completion

Programmable completion, which Bash checks first when completion is attempted with `Tab`, is available to many programs and their parameters on Gentoo.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-bash-readme-2)</sup> By default, Bash completes variable names, user names, host names, command names and file names when `Tab` is pressed, and its programmable completion facilities are enabled.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> For better suggestions of options and arguments for many commands, install the [app-shells/bash-completion](https://packages.gentoo.org/packages/app-shells/bash-completion) package, a collection of completion recipes for bash.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-bash-completion-readme-4)</sup> No special USE flags for packages, which support completion, are required on individual packages. Post-installation, completion functionality can be managed through [eselect](https://wiki.gentoo.org/wiki/Eselect).

`root #``emerge --ask app-shells/bash-completion`
The [app-shells/gentoo-bashcomp](https://packages.gentoo.org/packages/app-shells/gentoo-bashcomp) package, which is installed by [app-shells/bash-completion](https://packages.gentoo.org/packages/app-shells/bash-completion), adds Gentoo-specific completions for e.g. emerge.

Shell completion is enabled by default for all supported programs<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>.

## Configuration

### Login shell

Bash is the default shell on Gentoo, after installation. The default login shell for a user is defined in the /etc/passwd file.

The login shell can be changed using the chsh utility, to use another POSIX compatible shell (chsh is from [sys-apps/shadow](https://packages.gentoo.org/packages/sys-apps/shadow), which is part of the base profile). Note that many systems require the full pathname to a shell to appear in /etc/shells before it can be made a login shell.<sup>[\[6\]](https://wiki.gentoo.org#cite_note-bash-faq-6)</sup> See the articles on [login](https://wiki.gentoo.org/wiki/Login) and on [shell configuration](https://wiki.gentoo.org/wiki/Shell#Configuration) for more info.

### Files

There are many settings to consider when determining how to modify the shell's behavior. Configuration changes can be defined via variables, functions, or shell built-ins. These settings are defined in several different configuration files, where the settings in the last file parsed will overwrite entries in previously defined files.

- /etc/profile - Initial (global) settings for all users; executed for login shells.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup>
- /etc/bash/bashrc.d - Initial (global) settings for all users; here multiple files may be stored. Not a standard directory, but a few Linux distributions support it.
- \~/.bash\_profile - Local bash profile settings for the current user; executed for login shells.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup>
- \~/.bash\_login - Local settings for the current user, if \~/.bash\_profile does not exist or is not readable.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup>
- \~/.profile - Settings for the current user, if neither \~/.bash\_profile nor \~/.bash\_login exists and is readable. When bash is invoked as an interactive login shell, or as a non-interactive shell with the --login option, it reads and executes commands from the first of \~/.bash\_profile, \~/.bash\_login and \~/.profile that exists and is readable.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup><sup>[\[6\]](https://wiki.gentoo.org#cite_note-bash-faq-6)</sup>
- \~/.bash\_logout - Individual login shell cleanup file. If it exists, it is read and executed when an interactive login shell exits, or when a non-interactive login shell executes the exit builtin.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup>

If an interactive shell is started that is not a login shell (e.g. in a terminal on a desktop), the following files are used:[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

- /etc/bash/bashrc - Initial (global) settings for all users. (set via *SYS\_BASHRC* compile flag)
- \~/.bashrc - Local settings on a per-user basis.

#### .bashrc

In Gentoo, and many other Linux distributions, /etc/bash/bashrc is parsed from /etc/profile. Bash reads /etc/profile when it is invoked as an interactive login shell, or as a non-interactive shell with the --login option; when an interactive shell that is not a login shell is started, bash reads \~/.bashrc.[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Local existing \~/.bashrc file settings always override global /etc/bash/bashrc settings. The Bash Reference Manual notes that, typically, \~/.bash\_profile contains the line `if [ -f ~/.bashrc ]; then . ~/.bashrc; fi` after (or before) any login-specific initializations.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

**`~/.bashrc`**

```
# This file contains suggested settings for a user's ~/.bashrc file.
# Modify the PS1 variable to adjust command prompt
# \u the username of the current user
# \h the hostname up to the first `.'
# \w the current working directory, with $HOME abbreviated with a tilde (uses the value of the PROMPT_DIRTRIM variable)
# \$ if the effective UID is 0, a #, otherwise a $
# For more PS1 options see the PROMPTING section of `man 1 bash`
PS1='\u@\h \w \$ '
# No double entries in the shell history; lines beginning with a space are not saved either.
export HISTCONTROL="$HISTCONTROL:erasedups:ignoreboth"
# Do not overwrite files when redirecting output by default.
set -o noclobber
# Wrap the following commands for interactive use to avoid accidental file overwrites.
rm() { command rm -i "${@}"; }
cp() { command cp -i "${@}"; }
mv() { command mv -i "${@}"; }
```
### Shell completion integrations

List available completions via:

`user $``eselect bashcomp list`
Disable specific completions using

`user $``eselect bashcomp disable <command-name>`
To use bash's default completion instead of a bash-completion one for a specific command for a single user, the upstream bash-completion README suggests defining `complete -o default -o bashdefault <command-name>` in the user file (\~/.bash\_completion, or the file named by `BASH_COMPLETION_USER_FILE` if that variable is set and not empty) or in a user startup script.[\[4\]](https://wiki.gentoo.org#cite_note-bash-completion-readme-4)

To disable *all* bash shell completion integration for a particular user, create an .inputrc file with the following content in the user's home directory, then open a new bash shell:

**`~/.inputrc`**

```
# Include any system-wide bindings and variable assignments
$include /etc/inputrc
# Disable bash completion
set disable-completion on
```
It's considered good practice to store configs under [Git](https://wiki.gentoo.org/wiki/Git).

## Usage

### Environment variables

See all variables for the current shell process which have the export attribute set:

`user $``export`
Of course, users can export their own variables, which are available to the current process and inherited by child processes:

`user $``export MYSTUFF=Hello`
Environment variables can also be localized to an individual child process by prepending an assignment list to a simple command. The resulting environment passed to `execve()` will be the union of the assignment list with the environment of the calling shell process:

`user $``USE=kde emerge -pv libreoffice`
To check the value of a variable:

`user $``typeset -p MYSTUFF`
#### PS1

The special shell variable named `PS1` defines what the terminal prompt looks like:

This prompt would be the following value for the `PS1` variable:

The following table lists some of the backslash-escaped special characters that can be used in the `PS1` variable (they can also be used in `PS0`, `PS2` and `PS4`).<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> The complete list is in the *Controlling the Prompt* section of the Bash Reference Manual; see also the PROMPTING section of the bash man page:[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)

| Code | Effect | 
|---|---|
| `\u` | Username. | 
| `\h` | Hostname, up to the first `.`.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> | 
| `\H` | Hostname. <sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> | 
| `\w` | Current directory. | 
| `\d` | Current date. | 
| `\t` | Current time. | 
| `\$` | `#` if the effective UID is 0, otherwise `$`.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> | 
| `\j` | Number of currently running tasks (jobs). | 
| `\e` | An ASCII escape character (033). <sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> | 
| `\[` | Begin a sequence of non-printing characters, which could be used to embed a terminal control sequence into the prompt. <sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> | 
| `\]` | End a sequence of non-printing characters. <sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> | 

Complete commands can be put into the prompt using a command substitution. The following will execute the `cut -d\  -f1 /proc/loadavg` command to show the one-minute load average at the beginning of the prompt:

Looks like:

Having colors in the prompt:

The `\[\e[0;32m\]` changes the color for every next output, put `\[\e[0m\]` at the end of the variable to reset the color, otherwise the whole prompt will be in green. `\[` begins and `\]` ends a sequence of non-printing characters; this tells Readline that the characters between them take up no space on the screen.[\[6\]](https://wiki.gentoo.org#cite_note-bash-faq-6)[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)

Color codes:

| Code | Color | 
|---|---|
| `\[\e[0;30m\]` | Black | 
| `\[\e[0;31m\]` | Red | 
| `\[\e[0;32m\]` | Green | 
| `\[\e[0;33m\]` | Yellow | 
| `\[\e[0;34m\]` | Blue | 
| `\[\e[0;35m\]` | Magenta | 
| `\[\e[0;36m\]` | Cyan | 
| `\[\e[0;37m\]` | White | 
| `\[\e[0m\]` | Reset to standard colors | 

The `0;` in `\[\e[0;31m\]` means foreground. If desired, other values like `1;` for foreground bold and `4;` for foreground underlined can be defined. Omit this number to refer to the background, e.g. `\[\e[31m\]`.

This script prints out all 256 color codes available in the Bash terminal that can be used in PS1 prompts.
It prints color swatches in groups of 8 and demonstrates how to set text color using escape sequences.
To reset the color back to the default after using an escape sequence, end the printout with `\e[0m`.

**`colors.sh`**

**Bash Terminal Color Script**

```
#!/bin/bash
for color in {0..255}; do
  printf "\\e[38;5;%sm%3s\\e[0m " $color "\\e[38;5;${color}m"
  if ! ((($color + 1) % 8)); then
    echo
  fi
done
```
The *tput* command in the terminal provides an abstraction to control the appearance of the shell prompt. Instead of hard-coding escape sequences, *tput* uses terminal capabilities from the *terminfo* database. Here's an example that can be used for *tput* to change the color properties of your PS1 prompt:

This sets the text color to green for the prompt. The `tput setaf 2` command sets the foreground color using ANSI color codes, with *2* representing green. The `tput sgr0` command then resets the text formatting back to the terminal's default.

The prompt can be made visually informative about the Git repository status by coloring the branch name according to the repository state. Below is a *PS1* definition that color-codes the current branch as red for uncommitted changes, green for a clean directory, and yellow for stashed changes:

This prompt function checks the current Git branch and repository status, setting colors accordingly. Insert the function *git\_branch* in your \~/.bashrc or \~/.bash\_profile file, followed by the *PS1* assignment for this functionality to take effect. Bash executes \~/.bash\_profile for login shells and reads \~/.bashrc when an interactive shell that is not a login shell is started; a typical \~/.bash\_profile contains the line `if [ -f ~/.bashrc ]; then . ~/.bashrc; fi`.[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

### Built-ins

#### set

The set command is a shell built-in used to display and change settings in the bash shell.

Show the current settings of the shell options controlled by set:[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``set -o`
Additional shell options are controlled with the shopt builtin; run it without arguments to show their current settings:[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``shopt`
Disable the shell history:

`user $``set +o history`
Enable the shell history:

`user $``set -o history`
#### alias

The alias builtin can be used to define a new command or redefine an existing command:

`user $``alias ll='ls -l'`
Now when ll (two lowercase Ls) is read unquoted in a position where it can be the first word of a simple command, the shell replaces it with ls -l.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Aliases are not expanded when the shell is not interactive (for example, when bash runs a shell script), unless the expand\_aliases shell option is set using shopt.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

To remove an alias:

`user $``unalias ll`
To temporarily bypass an alias escape the first letter of the command with a backslash character:

`user $``\ls`
##### Listing

Run alias without any arguments to display a list of currently defined aliases:

`user $``alias`
#### history

The history of used commands in a session is written, when the shell exits, to the file named by the `HISTFILE` variable (default \~/.bash\_history).<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> The easiest way to access the commands in the history is using the `Up` and `Down` keys. The bash manual lists `ctrl`+`p` (`previous-history`) and `ctrl`+`n` (`next-history`) as the default key sequences for moving back and forward through the history list, and notes that these commands may also be bound to the up and down arrow keys on some keyboards.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)

To show all commands in the current history:

`user $``history`
To search for commands in the history, by piping the output through grep and filtering for words:

`user $``history | grep echo`
The commands are numbered and can be executed using their index:

`user $``!2`
To execute the last command used:

`user $``!!`
Delete every command in the history:

`user $``history -c`
Show the current value of `HISTCONTROL`, a colon-separated list of values controlling how commands are saved on the history list:[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``echo $HISTCONTROL`
Other history settings described in the bash manual include the variables `HISTFILE`, `HISTFILESIZE`, `HISTIGNORE`, `HISTSIZE` and `HISTTIMEFORMAT`, and the histappend, cmdhist and lithist shell options.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

### Keyboard shortcuts

bash includes two different keyboard shortcut modes to make editing input on the command-line easier: emacs mode and vi mode. bash defaults to emacs mode.

#### vi mode

When a line is entered in vi mode, editing starts in insertion mode, as if `i` had been typed.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Pressing `Esc` switches to command mode, where the line can be edited with the standard vi movement keys, `k` moves to previous history lines, and `j` moves to subsequent lines.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> It can be a bit awkward to learn this mode. To change the mode to vi mode, execute the following command:

`user $``set -o vi`
Review this bash [vi editing mode cheat sheet](https://www.catonmat.net/download/bash-vi-editing-mode-cheat-sheet.pdf) document by Peteris Krumins for more details on key bindings in vi mode.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

#### emacs mode

To switch to emacs mode (which is the default mode):

`user $``set -o emacs`
The following sections contain useful command-line navigation shortcuts and bash built-ins. The bash man page, which can be read locally with man 1 bash, lists the readline commands together with the default key sequences to which they are bound.[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)

Search for `Commands for Moving` for the beginning of the section.

The key bindings currently in effect can be listed with bind -P, or with bind -p in a terser format suitable for an inputrc file.[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

**Line movement:**

- `ctrl`+`a`
- Move cursor to the beginning of the line (home).
- `ctrl`+`e`
- Move cursor to the **e**nd of the line (end).
- `ctrl`+`f`
- Move the cursor forward one character.
- `ctrl`+`b`
- Move the cursor back one character.
- `alt`+`f`
- Move cursor **f**orward one word.
- `alt`+`b`
- Move cursor **b**ackward one word.
- `ctrl`+`x ctrl`+`x`
- Swap the cursor position with the mark (a cursor position saved with `ctrl`+`@`); the old cursor position becomes the new mark.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup>
- `ctrl`+`]`-`<char>`
- Move cursor to the first occurrence of the entered character to the right.
- `ctrl`+`alt`-`]`-`<char>`
- Move cursor to the first occurrence of the entered character to the left.

**Directory movement:**

- cd /path
- Change to /path directory.
- cd -
- Change to previous directory.
- cd
- Change to home directory.

##### Screen control

- `ctrl`+`s`
- Stop (pause) output on the screen.
- `ctrl`+`q`
- Resume output on the screen (after stopping it with the previous command).
- `ctrl`+`l`
- Clears the screen preserving the current command (very similar to the clear command).

##### Text manipulation

**Deletion:**

- `ctrl`+`u`
- Remove text relative to the cursor's current position to the beginning of the line.
- `ctrl`+`k`
- Remove text relative to the cursor's current position to the end of the line.
- `alt`+`d`
- Remove one word moving forward from the cursor's current position.
- `alt`+`Backspace`
- Remove one word moving backward from the cursor's current position.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup><sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup>
- `ctrl`+`w`
- Remove one word moving backward from the cursor's current position using whitespace as a word boundary.
- `ctrl`+`y`
- Paste deleted text.

##### Command history

- `ctrl`+`p`
- Scroll backward through **p**revious entries in the command history list, which on startup is initialized from the history file (\~/.bash\_history by default).<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup>
- `ctrl`+`n`
- Scroll forward through **n**ext entry in the command history list.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup>
- `ctrl`+`r`
- Reverse search through history (type parts of the command to initiate query).

## Scripts

Shell scripts are text files which contain programs written in a certain shell scripting language. Which shell is used to interpret the commands in a script is defined by the first line of the script. This consists of two special characters `#!` (called the shebang), followed by the interpreter (for example a direct path to the shell) and an optional argument for it.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> For example:

**`myscript`**

```
#!/bin/bash
echo 'Hello World!'
```
A common idiom is `#!/usr/bin/env bash`, which uses env to find the first occurrence of bash in `$PATH`.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

If no shell is defined and the script is started from Bash, Bash assumes the file is a shell script and creates a new instance of itself to execute it.[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Often /bin/sh is used, which is the father of all shells and has very limited functionalities. Nearly all shells available understand commands used when running /bin/sh, so those scripts are highly portable.

### Script execution

To run scripts directly from the command-line, they need to be executable. To make a shell script executable:

`user $``chmod +x myscript`
Now the script can be executed by using the ./ prefix, where the shell defined by the shebang in the script is used (if no shell is defined and the script is started from Bash, a new instance of Bash executes it):[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``./myscript`
### Pipelines and redirection

In Bash it is possible to connect the output of one program to the input of another program using a pipe, indicated by the `|` symbol; such a sequence of commands is called a *pipeline*.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> This enables users to create command chains. Here is an example to pass the output of ls -l to the program /usr/bin/less:[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``ls -l | less`
To redirect output into a file:

`user $``ls -l > ls_l.txt`
Unless the noclobber option is enabled, the `>` operator will erase any previous content (the file is truncated to zero size) before adding new one.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> If noclobber has been enabled (`set -o noclobber` or `set -C`, as in the \~/.bashrc example above), the redirection fails if the file exists and is a regular file; the `>|` operator overrides this.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> If overwriting is not desired, use the `>>` (append) operator instead.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

### AND and OR lists

The control operators `&&` and `||` (which form AND and OR lists) are very useful to chain commands together.[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> This is helpful when checking if the previous command finished successfully or not.

`&&` (AND) - The following command prints 'Success' only if our test script is successful:

`user $``./myscript && echo 'Success'`
`||` (OR) - The following command prints 'Failure' only if our test script is unsuccessful:

`user $``./myscript || echo 'Failure'`
### Jobs

Usually when a script or command has been executed shell input is blocked until after execution has completed. To start a program directly in the background, append the ampersand character (&) to the end of the command:

`user $``./myscript &`
This will execute the script as **job** number 1 and the prompt expects the next input.

When a program is already running, but the shell is needed for another task, it is possible to move programs from *foreground* to *background* and vice versa. To get a command prompt when a command is running on the shell, stop (suspend) it using `Ctrl`+`z`, then move it to the background:[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``bg %1`
`%1` is a job specification (jobspec) that refers to job number 1.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Without a jobspec, bg and fg use the current job; a job that is stopped while in the foreground or started in the background becomes the current job.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

To list the status of all jobs, including stopped jobs (jobs -r displays only running jobs, jobs -s only stopped jobs):[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

`user $``jobs`
To move a job back to foreground:

`user $``fg %1`
### Command substitution

Using a command substitution, it is possible to run programs as parameters of other commands like here:

`user $``emerge --ask --oneshot $(qlist -CI x11-drivers)`
This will first execute the command in the parentheses and replace the command substitution with the standard output of the command, with any trailing newlines deleted.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Because the substitution is not within double quotes, the result is scanned for word splitting, and each resulting word becomes a separate argument to emerge.[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)

More substitutions can be performed in one command like this:

`user $``emerge --ask --oneshot $(qlist -CI x11-drivers) $(qlist -CI modules)`
## Troubleshooting

### Garbled display

The output of a shell can, in some conditions, become corrupt. See the [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator#Garbled_display) article for instructions to help fix this.

If a prompt string such as `PS1` contains terminal escape sequences, use `\[` to begin and `\]` to end each sequence of non-printing characters.[\[6\]](https://wiki.gentoo.org#cite_note-bash-faq-6)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> Without them, Readline assumes that each character in the prompt takes up one character position on the screen, and bash can wrap lines at the wrong column.[\[6\]](https://wiki.gentoo.org#cite_note-bash-faq-6)

### Reporting bugs and getting help

Before reporting a bug upstream, make sure that it really is a bug and that it appears in the latest version of Bash.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup> Upstream asks for bug reports to be submitted with the bashbug command or the form at the [Bash project page](https://savannah.gnu.org/projects/bash/).[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup> Questions and requests for help with Bash and Bash programming may be sent to the help-bash@gnu.org mailing list.[\[2\]](https://wiki.gentoo.org#cite_note-bash-readme-2)

## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Dash](https://wiki.gentoo.org/wiki/Dash) — a small, fast, and [POSIX](https://wiki.gentoo.org/wiki/POSIX)-compliant [shell](https://wiki.gentoo.org/wiki/Shell).
- [Zsh](https://wiki.gentoo.org/wiki/Zsh) — an interactive login shell that can also be used as a powerful scripting language interpreter.
- [Fish](https://wiki.gentoo.org/wiki/Fish) — a smart and user-friendly command line [shell](https://wiki.gentoo.org/wiki/Shell) for OS X, Linux, and the rest of the family.
- [Nushell](https://wiki.gentoo.org/wiki/Nushell) — a new kind of [shell](https://wiki.gentoo.org/wiki/Shell) for OS X, Linux, and Windows.
- [bc](https://wiki.gentoo.org/wiki/Bc) — arbitrary-precision fixed-point mathematical scripting language
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.

## External resources

### Learning Modern Bash

- [Wikibooks Bash Scripting Guide](https://en.wikibooks.org/wiki/Bash_Shell_Scripting) — an easy introduction to Bash scripting.
- [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/) — a slightly older but still useful Bash scripting guide.
- [New Bash Features](https://mywiki.wooledge.org/BashFAQ/061) — a list of features added to Bash over time.
- The `NEWS` file in the Bash distribution — a terse description of the new features added in each Bash release from bash-2.0 to bash-5.3 (the manual page is the place to look for complete descriptions).<sup>[\[7\]](https://wiki.gentoo.org#cite_note-bash-news-7)</sup><sup>[\[2\]](https://wiki.gentoo.org#cite_note-bash-readme-2)</sup>
- [Bash Traps](https://web.archive.org/web/20230330234404/https://wiki.bash-hackers.org/scripting/newbie_traps) — a collection of common mistakes and how to avoid them.
- [Pure Bash Bible](https://github.com/dylanaraps/pure-bash-bible) — a collection of pure Bash alternatives to external processes.
- [Awesome Bash](https://github.com/awesome-lists/awesome-bash) — a curated list of Bash scripts and resources.
- [Exercism: Bash Track](https://exercism.org/tracks/bash) — an interactive Bash tutorial.

#### Debugging and Testing

- [Defensive Bash Programming](https://web.archive.org/web/20180917174959/http://www.kfirlavi.com/blog/2012/11/14/defensive-bash-programming) — how not to shoot yourself in the foot with Bash.
- [Unofficial Strict Mode](https://gist.github.com/robin-a-meade/58d60124b88b60816e8349d1e3938615) — a "strict mode" inspired by the `use strict` pragma in [Perl](https://wiki.gentoo.org/wiki/Perl) and a [detailed explanation](https://gist.github.com/mohanpedala/1e2ff5661761d3abd0385e8223e16425) of how this works.
- [Unit Testing in Bash](https://github.com/dodie/testing-in-bash) — a collection of unit testing tools.
- [shellcheck](https://github.com/koalaman/shellcheck) — a static analysis tool for shell scripts.

#### Code Style and Documentation

- [Shell Script Style Guide](https://google.github.io/styleguide/shellguide.html) — Google's style guide for shell scripts.
- [bash-doxygen](https://github.com/Anvil/bash-doxygen) — A doxygen filter for bash scripts; great for developer documentation.
- [Plain Old Documentation](https://charlotte-ngs.github.io/2015/01/BashScriptPOD.html) — Perl-style POD documentation in Bash scripts; great for `man` page generation with `pod2man`.

#### Cheat Sheets

### Bash Reference Guides

- The Bash manual page (man 1 bash), which the GNU Bash Reference Manual names as the definitive reference on shell behavior, and the [GNU Bash home page](https://www.gnu.org/software/bash/).<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bash-ref-1)</sup><sup>[\[2\]](https://wiki.gentoo.org#cite_note-bash-readme-2)</sup>
- [Bash reference](https://devmanual.gentoo.org/tools-reference/bash/index.html) from the Gentoo Developer's Handbook.
- [Chet's Bash page](https://tiswww.case.edu/php/chet/bash/bashtop.html).
- The [Bash FAQ](https://mywiki.wooledge.org/BashFAQ) and [Bash guide](https://mywiki.wooledge.org/BashGuide) on Greg Wooledge's wiki.
- [POSIX Shell and Utilities](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) volume of IEEE Std 1003.1-2024, and a [description of Bash's posix mode](http://tiswww.case.edu/~chet/bash/POSIX).<sup>[\[8\]](https://wiki.gentoo.org#cite_note-bash-posix-8)</sup><sup>[\[3\]](https://wiki.gentoo.org#cite_note-bash-man-3)</sup>
- [mksh](https://www.mirbsd.org/man/mksh.1), [ksh93](http://www2.research.att.com/sw/download/man/man1/ksh.html), and [ksh88](http://www2.research.att.com/sw/download/man/man1/ksh88.html) manuals for cross-reference.

## References

1. ↑ <sup>[1.00](https://wiki.gentoo.org#cite_ref-bash-ref_1-0)</sup> <sup>[1.01](https://wiki.gentoo.org#cite_ref-bash-ref_1-1)</sup> <sup>[1.02](https://wiki.gentoo.org#cite_ref-bash-ref_1-2)</sup> <sup>[1.03](https://wiki.gentoo.org#cite_ref-bash-ref_1-3)</sup> <sup>[1.04](https://wiki.gentoo.org#cite_ref-bash-ref_1-4)</sup> <sup>[1.05](https://wiki.gentoo.org#cite_ref-bash-ref_1-5)</sup> <sup>[1.06](https://wiki.gentoo.org#cite_ref-bash-ref_1-6)</sup> <sup>[1.07](https://wiki.gentoo.org#cite_ref-bash-ref_1-7)</sup> <sup>[1.08](https://wiki.gentoo.org#cite_ref-bash-ref_1-8)</sup> <sup>[1.09](https://wiki.gentoo.org#cite_ref-bash-ref_1-9)</sup> <sup>[1.10](https://wiki.gentoo.org#cite_ref-bash-ref_1-10)</sup> <sup>[1.11](https://wiki.gentoo.org#cite_ref-bash-ref_1-11)</sup> <sup>[1.12](https://wiki.gentoo.org#cite_ref-bash-ref_1-12)</sup> <sup>[1.13](https://wiki.gentoo.org#cite_ref-bash-ref_1-13)</sup> <sup>[1.14](https://wiki.gentoo.org#cite_ref-bash-ref_1-14)</sup> <sup>[1.15](https://wiki.gentoo.org#cite_ref-bash-ref_1-15)</sup> <sup>[1.16](https://wiki.gentoo.org#cite_ref-bash-ref_1-16)</sup> <sup>[1.17](https://wiki.gentoo.org#cite_ref-bash-ref_1-17)</sup> <sup>[1.18](https://wiki.gentoo.org#cite_ref-bash-ref_1-18)</sup> <sup>[1.19](https://wiki.gentoo.org#cite_ref-bash-ref_1-19)</sup> <sup>[1.20](https://wiki.gentoo.org#cite_ref-bash-ref_1-20)</sup> <sup>[1.21](https://wiki.gentoo.org#cite_ref-bash-ref_1-21)</sup> <sup>[1.22](https://wiki.gentoo.org#cite_ref-bash-ref_1-22)</sup> <sup>[1.23](https://wiki.gentoo.org#cite_ref-bash-ref_1-23)</sup> <sup>[1.24](https://wiki.gentoo.org#cite_ref-bash-ref_1-24)</sup> <sup>[1.25](https://wiki.gentoo.org#cite_ref-bash-ref_1-25)</sup> <sup>[1.26](https://wiki.gentoo.org#cite_ref-bash-ref_1-26)</sup> <sup>[1.27](https://wiki.gentoo.org#cite_ref-bash-ref_1-27)</sup> <sup>[1.28](https://wiki.gentoo.org#cite_ref-bash-ref_1-28)</sup> <sup>[1.29](https://wiki.gentoo.org#cite_ref-bash-ref_1-29)</sup> <sup>[1.30](https://wiki.gentoo.org#cite_ref-bash-ref_1-30)</sup> <sup>[1.31](https://wiki.gentoo.org#cite_ref-bash-ref_1-31)</sup> <sup>[1.32](https://wiki.gentoo.org#cite_ref-bash-ref_1-32)</sup> <sup>[1.33](https://wiki.gentoo.org#cite_ref-bash-ref_1-33)</sup> <sup>[1.34](https://wiki.gentoo.org#cite_ref-bash-ref_1-34)</sup> <sup>[1.35](https://wiki.gentoo.org#cite_ref-bash-ref_1-35)</sup> <sup>[1.36](https://wiki.gentoo.org#cite_ref-bash-ref_1-36)</sup> <sup>[1.37](https://wiki.gentoo.org#cite_ref-bash-ref_1-37)</sup> <sup>[1.38](https://wiki.gentoo.org#cite_ref-bash-ref_1-38)</sup> <sup>[1.39](https://wiki.gentoo.org#cite_ref-bash-ref_1-39)</sup> <sup>[1.40](https://wiki.gentoo.org#cite_ref-bash-ref_1-40)</sup> <sup>[1.41](https://wiki.gentoo.org#cite_ref-bash-ref_1-41)</sup> <sup>[1.42](https://wiki.gentoo.org#cite_ref-bash-ref_1-42)</sup> <sup>[1.43](https://wiki.gentoo.org#cite_ref-bash-ref_1-43)</sup> <sup>[1.44](https://wiki.gentoo.org#cite_ref-bash-ref_1-44)</sup> <sup>[1.45](https://wiki.gentoo.org#cite_ref-bash-ref_1-45)</sup> <sup>[1.46](https://wiki.gentoo.org#cite_ref-bash-ref_1-46)</sup> <sup>[1.47](https://wiki.gentoo.org#cite_ref-bash-ref_1-47)</sup> <sup>[1.48](https://wiki.gentoo.org#cite_ref-bash-ref_1-48)</sup> <sup>[1.49](https://wiki.gentoo.org#cite_ref-bash-ref_1-49)</sup> <sup>[1.50](https://wiki.gentoo.org#cite_ref-bash-ref_1-50)</sup> <sup>[1.51](https://wiki.gentoo.org#cite_ref-bash-ref_1-51)</sup> <sup>[1.52](https://wiki.gentoo.org#cite_ref-bash-ref_1-52)</sup> <sup>[1.53](https://wiki.gentoo.org#cite_ref-bash-ref_1-53)</sup> <sup>[1.54](https://wiki.gentoo.org#cite_ref-bash-ref_1-54)</sup> <sup>[1.55](https://wiki.gentoo.org#cite_ref-bash-ref_1-55)</sup> <sup>[1.56](https://wiki.gentoo.org#cite_ref-bash-ref_1-56)</sup> <sup>[1.57](https://wiki.gentoo.org#cite_ref-bash-ref_1-57)</sup> Chet Ramey, Brian Fox. [GNU Bash Reference Manual](https://tiswww.case.edu/php/chet/bash/bashref.html), Edition 5.3, 2025-05-18.
2. ↑ <sup>[2.0](https://wiki.gentoo.org#cite_ref-bash-readme_2-0)</sup> <sup>[2.1](https://wiki.gentoo.org#cite_ref-bash-readme_2-1)</sup> <sup>[2.2](https://wiki.gentoo.org#cite_ref-bash-readme_2-2)</sup> <sup>[2.3](https://wiki.gentoo.org#cite_ref-bash-readme_2-3)</sup> [Bash README](https://tiswww.case.edu/php/chet/bash/README), GNU Bash 5.3 distribution.
3. ↑ <sup>[3.00](https://wiki.gentoo.org#cite_ref-bash-man_3-0)</sup> <sup>[3.01](https://wiki.gentoo.org#cite_ref-bash-man_3-1)</sup> <sup>[3.02](https://wiki.gentoo.org#cite_ref-bash-man_3-2)</sup> <sup>[3.03](https://wiki.gentoo.org#cite_ref-bash-man_3-3)</sup> <sup>[3.04](https://wiki.gentoo.org#cite_ref-bash-man_3-4)</sup> <sup>[3.05](https://wiki.gentoo.org#cite_ref-bash-man_3-5)</sup> <sup>[3.06](https://wiki.gentoo.org#cite_ref-bash-man_3-6)</sup> <sup>[3.07](https://wiki.gentoo.org#cite_ref-bash-man_3-7)</sup> <sup>[3.08](https://wiki.gentoo.org#cite_ref-bash-man_3-8)</sup> <sup>[3.09](https://wiki.gentoo.org#cite_ref-bash-man_3-9)</sup> <sup>[3.10](https://wiki.gentoo.org#cite_ref-bash-man_3-10)</sup> <sup>[3.11](https://wiki.gentoo.org#cite_ref-bash-man_3-11)</sup> <sup>[3.12](https://wiki.gentoo.org#cite_ref-bash-man_3-12)</sup> <sup>[3.13](https://wiki.gentoo.org#cite_ref-bash-man_3-13)</sup> <sup>[3.14](https://wiki.gentoo.org#cite_ref-bash-man_3-14)</sup> <sup>[3.15](https://wiki.gentoo.org#cite_ref-bash-man_3-15)</sup> <sup>[3.16](https://wiki.gentoo.org#cite_ref-bash-man_3-16)</sup> <sup>[3.17](https://wiki.gentoo.org#cite_ref-bash-man_3-17)</sup> <sup>[3.18](https://wiki.gentoo.org#cite_ref-bash-man_3-18)</sup> <sup>[3.19](https://wiki.gentoo.org#cite_ref-bash-man_3-19)</sup> <sup>[3.20](https://wiki.gentoo.org#cite_ref-bash-man_3-20)</sup> <sup>[3.21](https://wiki.gentoo.org#cite_ref-bash-man_3-21)</sup> <sup>[3.22](https://wiki.gentoo.org#cite_ref-bash-man_3-22)</sup> <sup>[3.23](https://wiki.gentoo.org#cite_ref-bash-man_3-23)</sup> <sup>[3.24](https://wiki.gentoo.org#cite_ref-bash-man_3-24)</sup> <sup>[3.25](https://wiki.gentoo.org#cite_ref-bash-man_3-25)</sup> <sup>[3.26](https://wiki.gentoo.org#cite_ref-bash-man_3-26)</sup> <sup>[3.27](https://wiki.gentoo.org#cite_ref-bash-man_3-27)</sup> <sup>[3.28](https://wiki.gentoo.org#cite_ref-bash-man_3-28)</sup> <sup>[3.29](https://wiki.gentoo.org#cite_ref-bash-man_3-29)</sup> <sup>[3.30](https://wiki.gentoo.org#cite_ref-bash-man_3-30)</sup> <sup>[3.31](https://wiki.gentoo.org#cite_ref-bash-man_3-31)</sup> <sup>[3.32](https://wiki.gentoo.org#cite_ref-bash-man_3-32)</sup> <sup>[3.33](https://wiki.gentoo.org#cite_ref-bash-man_3-33)</sup> <sup>[3.34](https://wiki.gentoo.org#cite_ref-bash-man_3-34)</sup> Chet Ramey, Brian Fox. [bash(1) manual page](https://tiswww.case.edu/php/chet/bash/bash.html), GNU Bash 5.3, 2025-04-07.
4. ↑ <sup>[4.0](https://wiki.gentoo.org#cite_ref-bash-completion-readme_4-0)</sup> <sup>[4.1](https://wiki.gentoo.org#cite_ref-bash-completion-readme_4-1)</sup> [bash-completion README](https://github.com/scop/bash-completion), version 2.18.0.
5. [↑](https://wiki.gentoo.org#cite_ref-5) [News Items - bash-completion-2.1-r90](https://www.gentoo.org/support/news-items/2014-11-25-bash-completion-2_1-r90.html), November 25th, 2014. Retrieved on May 13th, 2017.
6. ↑ <sup>[6.0](https://wiki.gentoo.org#cite_ref-bash-faq_6-0)</sup> <sup>[6.1](https://wiki.gentoo.org#cite_ref-bash-faq_6-1)</sup> <sup>[6.2](https://wiki.gentoo.org#cite_ref-bash-faq_6-2)</sup> <sup>[6.3](https://wiki.gentoo.org#cite_ref-bash-faq_6-3)</sup> <sup>[6.4](https://wiki.gentoo.org#cite_ref-bash-faq_6-4)</sup> <sup>[6.5](https://wiki.gentoo.org#cite_ref-bash-faq_6-5)</sup> <sup>[6.6](https://wiki.gentoo.org#cite_ref-bash-faq_6-6)</sup> Chet Ramey. [Bash FAQ](https://tiswww.case.edu/php/chet/bash/FAQ), version 4.15 (no longer maintained).
7. [↑](https://wiki.gentoo.org#cite_ref-bash-news_7-0) [Bash NEWS](https://tiswww.case.edu/php/chet/bash/NEWS), GNU Bash 5.3 distribution.
8. [↑](https://wiki.gentoo.org#cite_ref-bash-posix_8-0) [Bash and POSIX](https://tiswww.case.edu/php/chet/bash/POSIX), GNU Bash 5.3 distribution.
