<!-- source: https://wiki.gentoo.org/wiki/Vim | group: Gentoo Wiki (Main) | wiki-title: Vim -->
---
title: Vim
url: https://wiki.gentoo.org/wiki/Vim
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-14"
categories: ['app-vim']
fingerprint: "7620ce5e01acbbe9"
license: CC BY-SA 4.0
---

# Vim

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Vim** (**v**i **im**proved) is a [vi](https://wiki.gentoo.org/wiki/Vi)-like [text editor](https://wiki.gentoo.org/wiki/Text_editor), originally descended from the [Stevie](<https://en.wikipedia.org/wiki/Stevie_(text_editor)>) vi clone. It can be used from the command-line or as a standalone application with a [graphical user interface](https://wiki.gentoo.org#Gvim).

Vim should not be confused with [Neovim](https://wiki.gentoo.org/wiki/Neovim), which is based on Vim, but has slightly different goals.


| [X](https://packages.gentoo.org/useflags/X) | Link console vim against X11 libraries to enable title and clipboard features in xterm | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [crypt](https://packages.gentoo.org/useflags/crypt) | Use dev-libs/libsodium for crypto support | 
| [cscope](https://packages.gentoo.org/useflags/cscope) | Enable cscope interface | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [gpm](https://packages.gentoo.org/useflags/gpm) | Add support for sys-libs/gpm (Console-based mouse driver) | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Install a very minimal build (disables, for example, plugins, fonts, most drivers, non-critical features) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pango](https://packages.gentoo.org/useflags/pango) | Render \|:hardcopy\| output with x11-libs/pango and x11-libs/cairo rather than Vim's own PostScript generator | 
| [perl](https://packages.gentoo.org/useflags/perl) | Add optional support/bindings for the Perl language | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [racket](https://packages.gentoo.org/useflags/racket) | Enable support for Scheme using dev-scheme/racket | 
| [ruby](https://packages.gentoo.org/useflags/ruby) | Add support/bindings for the Ruby language | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sound](https://packages.gentoo.org/useflags/sound) | Enable sound support | 
| [tcl](https://packages.gentoo.org/useflags/tcl) | Add support the Tcl language | 
| [terminal](https://packages.gentoo.org/useflags/terminal) | Enable terminal emulation support | 
| [vim-pager](https://packages.gentoo.org/useflags/vim-pager) | Install vimpager and vimmanpager links | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

If X Window System support is not needed, install [app-editors/vim](https://packages.gentoo.org/packages/app-editors/vim):

`root #``emerge --ask app-editors/vim`
To install Vim with both the [ncurses](https://en.wikipedia.org/wiki/Ncurses)-based interface (/usr/bin/vim) as well as the graphical interface (for the X Window System - /usr/bin/gvim), install the [app-editors/gvim](https://packages.gentoo.org/packages/app-editors/gvim) package:

`root #``emerge --ask app-editors/gvim`
Vim has support for packages, which provide a native mechanism to extend functionality. Read the built-in packages documentation for more information: :help packages

The category [app-vim](https://packages.gentoo.org/categories/app-vim) provides a lot of additional syntax definitions, plugins and other Vim related stuff.

Use emerge or eix to get an overview of available packages in the [app-vim](https://packages.gentoo.org/categories/app-vim) category:

`user $````
emerge --search "%@^app-vim"
```
`user $``eix -cC app-vim`
Not all Vim plugins will be available in the Gentoo repository. Vim now includes [native packages](https://vimhelp.org/repeat.txt.html#packages), as a way to install plugins, and there are also several [plugin managers](https://vim.fandom.com/wiki/Plugin-manager) for Vim.

Vim can be configured on a per-user basis or through a system-wide configuration file:

- /etc/vim/vimrc - The system wide (global) settings file.
- \~/.vimrc - The user-specific (local) configuration file. The tilde (\~) means it is in the user's home directory.
- \~/.vim/pack/foo - Location where plugins are installed (vim 8 and higher).<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> Substitute foo for the name of each plugin.


From version 9.1.0327 Vim adopts [freedesktop XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/latest/index.html): that means, you can now place your configuration files under \~/.config/vim/ so Vim will stop littering your home directory.

Want to share the same config for root or other users? Softlink it:

`root #``ln -s /home/someUserName/.config/vim /root/.config/vim`
About a dozen color schemes are shipped with the core Vim package. They can be listed in last line mode by typing colorscheme (followed by a space), then pressing either `Ctrl`+`d` or pressing the `Tab` key twice:

`:``colorscheme` blue       darkblue   default    delek      desert     elflord    evening    industry   koehler    morning    murphy     pablo      peachpuff  ron        shine      slate      torte      zellner

They can be changed in Vim by using the colorscheme (alternatively use colo) command while in last line mode:

`:``colorscheme peachpuff`
Color schemes can be permanently applied in the .vimrc file. Note that the `syntax on` line is also needed for activation:

**`~/.vimrc`**

```
colorscheme peachpuff
syntax on
```
The first line sets the default color scheme while the last line activates the color scheme.

If Vim - and only Vim - is installed, the vi command should launch Vim. If other [vi-like](https://en.wikipedia.org/wiki/vi) editors are installed, [eselect](https://wiki.gentoo.org/wiki/Eselect) may be used to [choose which editor the vi command launches](https://wiki.gentoo.org/wiki/Vi#.2Fusr.2Fbin.2Fvi_symlink).

The [text editor](https://wiki.gentoo.org/wiki/Text_editor) article may also be of interest for [setting a system default editor](https://wiki.gentoo.org/wiki/Text_editor#Setting_system_default), if Vim is to be set as the default.

To enable support for the Gentoo syntax plugin ([app-vim/gentoo-syntax](https://packages.gentoo.org/packages/app-vim/gentoo-syntax)) within vim, add the following lines to the user's \~/.vimrc file:

**`~/.vimrc`**

```
filetype plugin on
filetype indent on
```
From the command line:

`user $``vim --help`
vim --help
VIM - Vi IMproved 8.2 (2019 Dec 12, compiled Nov 26 2021 11:56:27)
 
Usage: vim \[arguments\] \[file ..\]       edit specified file(s)
   or: vim \[arguments\] -               read text from stdin
   or: vim \[arguments\] -t tag          edit file where tag is defined
   or: vim \[arguments\] -q \[errorfile\]  edit file with first error
 
Arguments:
   --			Only file names after this
   -v			Vi mode (like "vi")
   -e			Ex mode (like "ex")
   -E			Improved Ex mode
   -s			Silent (batch) mode (only for "ex")
   -d			Diff mode (like "vimdiff")
   -y			Easy mode (like "evim", modeless)
   -R			Readonly mode (like "view")
   -Z			Restricted mode (like "rvim")
   -m			Modifications (writing files) not allowed
   -M			Modifications in text not allowed
   -b			Binary mode
   -l			Lisp mode
   -C			Compatible with Vi: 'compatible'
   -N			Not fully Vi compatible: 'nocompatible'
   -V\[N\]\[fname\]		Be verbose \[level N\] \[log messages to fname\]
   -D			Debugging mode
   -n			No swap file, use memory only
   -r			List swap files and exit
   -r (with file name)	Recover crashed session
   -L			Same as -r
   -A			Start in Arabic mode
   -H			Start in Hebrew mode
   -T \<terminal>	Set terminal type to \<terminal>
   --not-a-term		Skip warning for input/output not being a terminal
   --ttyfail		Exit if input or output is not a terminal
   -u \<vimrc>		Use \<vimrc> instead of any .vimrc
   --noplugin		Don't load plugin scripts
   -p\[N\]		Open N tab pages (default: one for each file)
   -o\[N\]		Open N windows (default: one for each file)
   -O\[N\]		Like -o but split vertically
   +			Start at end of file
   +\<lnum>		Start at line \<lnum>
   --cmd \<command>	Execute \<command> before loading any vimrc file
   -c \<command>		Execute \<command> after loading the first file
   -S \<session>		Source file \<session> after loading the first file
   -s \<scriptin>	Read Normal mode commands from file \<scriptin>
   -w \<scriptout>	Append all typed commands to file \<scriptout>
   -W \<scriptout>	Write all typed commands to file \<scriptout>
   -x			Edit encrypted files
   --startuptime \<file>	Write startup timing messages to \<file>
   -i \<viminfo>		Use \<viminfo> instead of .viminfo
   --clean		'nocompatible', Vim defaults, no plugins, no viminfo
   -h  or  --help	Print Help (this message) and exit
   --version		Print version information and exit

The vi command [may also be used](https://wiki.gentoo.org/wiki/Vim#Selecting_vi_editor_and_system_default_editor) to launch Vim, if so configured.

Specify a name, to open an existing file, or to create a new one:

`user $``vim <filename>`
Vim has a built-in tutorial which should require around 30 minutes to go through. Start it using the vimtutor command:

`user $``vimtutor`
It is possible to use Vim for one-liners — commands that can be used in scripts or on the command-line to make changes in an unattended manner.

For instance, the following command adds `#` to the beginning of each line in the file.txt file:

`user $``vim -c ":%s/^/#/g" -c ":x" file.txt`
What happens is that Vim interprets the passed on commands (through the `-c` option). The first command is Vim's substitution command (which is very similar to sed's), the second one is Vim's instruction to save and exit the editor.

To change the file encoding of a file to [UTF-8](https://wiki.gentoo.org/wiki/UTF-8), use the following command (in last line mode):

`:``e ++enc=utf8`
As shown in the previous trick, it is possible to do this from the command line as well:

`user $``vim -c ":wq! ++enc=utf8" file.txt`
For someone entering vim without first having learned how to use it, it may not be obvious how to quit.

Press `esc` several times, then `:`, `q`, `enter`. To quit without saving, press `esc` several times, then `:`, `q`, `!`, `enter`.

If several files are open, try pressing `esc` several times, then `:`, `q`, `a`, `enter`. To quit without saving, press `esc` several times, then `:`, `q`, `a`, `!`, `enter`.

If this doesn't help, and desperate measures are needed, something like killall vim may be a last ditch solution (from another [terminal](https://wiki.gentoo.org/wiki/Terminal_emulator#Virtual_consoles_and_switching)). Beware that this will terminate **all** the vim sessions for a user, without saving. If run as root, such a command will terminate all vim sessions **for all users** on the system.

Say the following text is trying to be pasted into a Vim buffer:

Vim may interpret this with autotabbing, breaking the paste:

To correct this, run the following command: :set paste to enable Vim's paste mode, once the paste is completed, :set nopaste can be used to go back to the previous settings.

`user $``vim foobar`
E1187: Failed to source defaults.vim
Press ENTER or type command to continue

Seems solved by [destabilize 8.2.4328](https://gitweb.gentoo.org/repo/gentoo.git/commit/app-editors/vim?id=1a80136202f15e6e08d915630119ccba76778fe8).

- [Knowledge Base:Edit a configuration file](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file)
- [Neovim](https://wiki.gentoo.org/wiki/Neovim) — a [vi](https://wiki.gentoo.org/wiki/Vi)-like [text editor](https://wiki.gentoo.org/wiki/Text_editor) designed around a relatively small but extendable core that aims to be a versatile and powerful text editor
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
- [Useful plugin](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds#How_to_create_an_ebuild): [app-vim/gentoo-syntax](https://packages.gentoo.org/packages/app-vim/gentoo-syntax)
- [Vi](https://wiki.gentoo.org/wiki/Vi) — a powerful *modal* [text-based editor](https://wiki.gentoo.org/wiki/Text_editor) with a long history in Unix and Unix-like operating systems.
- [Vim/Guide](https://wiki.gentoo.org/wiki/Vim/Guide) — explain basic usage for users new to [vi-like](https://en.wikipedia.org/wiki/vi) text editors in general, and vim in particular.
- [Emacs](https://wiki.gentoo.org/wiki/Emacs) — a class of powerful, extensible, self-documenting text editors.

- [Vim Documentation](http://vimdoc.sourceforge.net/) Includes Manuals (aka ":help" and Free VIM OPL Book), FAQS, HOWTO's, Tutorials, in HTML PDF, and PS formats.
- [A vim Tutorial and Primer](https://danielmiessler.com/study/vim) - An excellent vim tutorial/primer. Read this first.
- [VIM Scripts/Plugins](http://www.vim.org/scripts/index.php)
- [Vim for Humans](https://github.com/vjousse/vim-for-humans-book) (free ebook) - Clone and cd into the sources directory, mkdir dist, change `sphinx2-build` to `sphinx-build` for `SPHINXBUILD` in rst/en/Makefile. Run ./makedist.sh. Read PDF in ./dist/vimpourleshumains/.
- [Learning the vi and Vim Editors, 7th Edition](http://shop.oreilly.com/product/9780596529833.do) O'Reilly  Print ISBN: 978-0-596-52983-3, Ebook ISBN: 978-0-596-15935-1
- [Vim anti-patterns](https://sanctum.geek.nz/arabesque/vim-anti-patterns/) - A blog entry on maintaining flow with Vim.
- [Vim Tips Wiki](https://vim.fandom.com/) - Previously known as Wikia, Vim Tips Wiki is now on Fandom.com.
- [Vim: Seven habits of effective text editing](https://www.moolenaar.net/habits.html) - A guide written in the year 2000 that still is relevant today!
- [vimcolorschemes.com](https://vimcolorschemes.com/) - A website that offers many more options for color schemes.
