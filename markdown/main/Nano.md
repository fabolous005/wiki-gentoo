<!-- source: https://wiki.gentoo.org/wiki/Nano | group: Gentoo Wiki (Main) | wiki-title: Nano -->
---
title: nano
url: https://wiki.gentoo.org/wiki/Nano
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-14"
fingerprint: "1c83c73ac3a6bb6a"
license: CC BY-SA 4.0
---

# nano

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**GNU nano** is an easy to use [terminal](https://wiki.gentoo.org/wiki/Terminal_emulator)-based [text editor](https://wiki.gentoo.org/wiki/Text_editor) that also provides some more advanced functionality. nano allows just anyone to jump into editing text files, but still provides useful productivity features that don't get in the way of wanting to just open a file and edit away.

By default, at the bottom of the screen, nano displays some useful pointers on basic functionality and how to find more help, so that first-time users can start to find their way around (this is configurable). It of course comes with classic basic features, such as copy/paste, undo/redo, syntax highlighting, interactive search-and-replace, auto-indentation, macros, etc.

nano's small size, portability, and ease of use, have lead to it being the editor that gets included in Gentoo's official [stage3](https://wiki.gentoo.org/wiki/Stage_file#Stage_3) installation seeds (a text editor is an absolute requirement in these stage3 files, and nano is as good as any, if not arguably better). This makes it a staple of Gentoo installations, and though of course it can be easily [replaced with another text editor](https://wiki.gentoo.org/wiki/Text_editor#Default.2C_fallback.2C_and_virtual_packages), a working text editor is such an essential part of the Gentoo operating system (particularly in the case of system issues) that keeping nano installed as a backup is usually for the best.

## Installation

### USE flags


| [+spell](https://packages.gentoo.org/useflags/+spell) | Add dictionary support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable debug messages and assert warnings. Note that these will all be sent straight to stderr rather than some logging facility. | 
| [justify](https://packages.gentoo.org/useflags/justify) | Enable justify/unjustify functions for text formatting. | 
| [magic](https://packages.gentoo.org/useflags/magic) | Add magic file support (sys-apps/file) to automatically detect appropriate syntax highlighting | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Disable all fancy features, including ones that otherwise have a dedicated USE flag (such as spelling). | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [unicode](https://packages.gentoo.org/useflags/unicode) | Add support for Unicode | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask app-editors/nano`
## Usage

### First steps

Start nano by typing nano in a terminal followed by options or a file. Passing a file name is the most common use:

`user $``nano filename`
Nano now shows the content of the text file and which can be modified as desired. Navigate through the text with the arrow keys.

At the bottom nano shows shortcuts for common actions, e.g. save or exit. The shortcut to save is shown as `^O`. Prefix the shortcut with the `Ctrl` key. So to save a document (after editing it) press `Ctrl`+`O`. To exit press `Ctrl`+`X`.

To see an overview over all options run nano --help

### Cut, copy, and paste

Lines can be cut with the shortcut `Ctrl`+`K` (copied with `Alt`+`^`) and paste with `Ctrl`+`U`. To cut or copy multiple lines press the shortcut multiple times.

### Search

Search the text with `Ctrl`+`W`. Continue the search with `Alt`+`W`.

### More shortcuts

| Action | Shortcut | Other shortcut | 
|---|---|---|
| Show the help | `Ctrl`+`G` | `F1` | 
| Close file | `Ctrl`+`X` | `F2` | 
| Save file | `Ctrl`+`O` | `F3` | 
| Search text | `Ctrl`+`W` | `F6` | 
| Continue search | `Alt`+`W` | `F16` | 
| Copy line to clipboard | `Alt`+`^` | `Alt`+`6` | 
| Cut line to clipboard | `Ctrl`+`K` | `F9` | 
| Paste line from clipboard | `Ctrl`+`U` | `F10` | 
| Go to the first line of the file | `Alt`+`\` | `Ctrl`+`Home` | 
| Go to the end line of the file | `Alt`+`/` | `Ctrl`+`End` | 

## Configuration

Set options permanently in the /etc/nanorc configuration file. This configuration applies system wide to all users. To change options only for one user, set the option in the user's \~/.nanorc file. As a general rule, files present in a user's home directory override system wide settings.

### Syntax highlighting

Support for syntax highlight is achieved through plugins and include statements in nano's configuration file (\~/.nanorc for individual users).

`user $``mkdir ~/.nano`
Copy the plugin into the \~/.nano directory, then reference it with a include statement. For example:

**`~/.nanorc`**

**Apply ebulid specific syntax highlighting for nano**

## See also

- [Knowledge Base:Edit a configuration file](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file)
- [Nano/Guide](https://wiki.gentoo.org/wiki/Nano/Guide) — covers basic operations in nano, and is meant to be very concise.
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
