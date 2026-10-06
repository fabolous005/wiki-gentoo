<!-- source: https://wiki.gentoo.org/wiki/GNU_Emacs | group: Gentoo Wiki (Main) | wiki-title: GNU Emacs -->
---
title: GNU Emacs
url: https://wiki.gentoo.org/wiki/GNU_Emacs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-02"
fingerprint: b225897da891a2ce
license: CC BY-SA 4.0
---

# GNU Emacs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**GNU Emacs** is a powerful, extensible, self-documenting text editor, that has use way-beyond simple text editing.

Some examples of built-in Emacs features are a file browser, email client, terminal emulator, text-based web browser, man and info page viewers, IRC client, calendar/planner, and several games, though there are many more.

Emacs' renowned flexibility comes from being built around [Emacs Lisp (Elisp)](https://en.wikipedia.org/wiki/Emacs_Lisp), which enables much of the application to be easily extended, configured, or changed. Elisp code can be distributed as packages, and many such packages are available for Emacs, ranging from simple configurations to full-blown applications.

A great number of third-party packages are available for installation from diverse sources, including from a choice of online package repositories, and there are multiple built-in and third party package managers to install and manage packages. Some emacs packages are available from the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

GNU Emacs was first released by the [Free Software Foundation](https://www.fsf.org/), and has been under development since 1985. It is now the most widely-used editor of the [Emacs](https://wiki.gentoo.org/wiki/Emacs) family, which started in 1976 as a set of “**E**ditor **Mac**ro**s**” for the [TECO](<https://en.wikipedia.org/wiki/TECO_(text_editor)>) editor.

In Gentoo, GNU Emacs is maintained by the team of the same name, which can be reached through [gnu-emacs@gentoo.org](mailto:gnu-emacs@gentoo.org). Detailed developer information can be found on the [project page](https://wiki.gentoo.org/wiki/Project:Emacs).

## Installation

### USE flags


### USE flags for
            [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs)
            
            The advanced, extensible, customizable, self-documenting editor

| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+gmp](https://packages.gentoo.org/useflags/+gmp) | Use the GNU multiple precision arithmetic library (dev-libs/gmp) instead of the bundled mini-gmp subset | 
| [+inotify](https://packages.gentoo.org/useflags/+inotify) | Enable inotify filesystem monitoring support | 
| [+threads](https://packages.gentoo.org/useflags/+threads) | Add elisp threading support | 
| [+xpm](https://packages.gentoo.org/useflags/+xpm) | Add support for XPM graphics format | 
| [Xaw3d](https://packages.gentoo.org/useflags/Xaw3d) | Add support for the 3d athena widget set | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [aqua](https://packages.gentoo.org/useflags/aqua) | Include support for the Mac OS X Aqua (Carbon/Cocoa) GUI | 
| [athena](https://packages.gentoo.org/useflags/athena) | Enable the MIT Athena widget set (x11-libs/libXaw) | 
| [cairo](https://packages.gentoo.org/useflags/cairo) | Enable support for the cairo graphics library | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [dynamic-loading](https://packages.gentoo.org/useflags/dynamic-loading) | Enable loading of dynamic libraries (modules) at runtime | 
| [games](https://packages.gentoo.org/useflags/games) | Support shared score files for games | 
| [gfile](https://packages.gentoo.org/useflags/gfile) | Use gfile (dev-libs/glib) for file notification | 
| [gif](https://packages.gentoo.org/useflags/gif) | Add GIF image support | 
| [gpm](https://packages.gentoo.org/useflags/gpm) | Add support for sys-libs/gpm (Console-based mouse driver) | 
| [gsettings](https://packages.gentoo.org/useflags/gsettings) | Use gsettings (dev-libs/glib) to read the system font name | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [gzip-el](https://packages.gentoo.org/useflags/gzip-el) | Compress bundled Emacs Lisp source | 
| [harfbuzz](https://packages.gentoo.org/useflags/harfbuzz) | Use media-libs/harfbuzz as text shaping engine | 
| [imagemagick](https://packages.gentoo.org/useflags/imagemagick) | Use media-gfx/imagemagick for image processing | 
| [jit](https://packages.gentoo.org/useflags/jit) | Compile with Emacs Lisp native compiler support via libgccjit | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [json](https://packages.gentoo.org/useflags/json) | Compile with native JSON support using dev-libs/jansson | 
| [lcms](https://packages.gentoo.org/useflags/lcms) | Add lcms support (color management engine) | 
| [libxml2](https://packages.gentoo.org/useflags/libxml2) | Use dev-libs/libxml2 to parse XML instead of the internal Lisp implementations | 
| [livecd](https://packages.gentoo.org/useflags/livecd) | !!internal use only!! DO NOT SET THIS FLAG YOURSELF!, used during livecd building | 
| [m17n-lib](https://packages.gentoo.org/useflags/m17n-lib) | Enable m17n-lib support | 
| [mailutils](https://packages.gentoo.org/useflags/mailutils) | Retrieve e-mail using net-mail/mailutils instead of the internal movemail substitute | 
| [motif](https://packages.gentoo.org/useflags/motif) | Add support for the Motif toolkit | 
| [png](https://packages.gentoo.org/useflags/png) | Add support for libpng (PNG images) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sound](https://packages.gentoo.org/useflags/sound) | Enable sound support | 
| [source](https://packages.gentoo.org/useflags/source) | Install C source files and make them available for find-function | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [svg](https://packages.gentoo.org/useflags/svg) | Add support for SVG (Scalable Vector Graphics) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [tiff](https://packages.gentoo.org/useflags/tiff) | Add support for the TIFF image format | 
| [toolkit-scroll-bars](https://packages.gentoo.org/useflags/toolkit-scroll-bars) | Use the selected toolkit's scrollbars in preference to Emacs' own scrollbars | 
| [tree-sitter](https://packages.gentoo.org/useflags/tree-sitter) | Support the dev-libs/tree-sitter parsing library | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 
| [wide-int](https://packages.gentoo.org/useflags/wide-int) | Prefer wide Emacs integers (typically 62-bit). This option has an effect only on 32-bit systems, where it increases the maximum buffer size from 0.5 to 2 GiB, at the cost of 10% to 30% Lisp slowdown. | 
| [xattr](https://packages.gentoo.org/useflags/xattr) | Add support for extended attributes (filesystem-stored metadata) | 
| [xft](https://packages.gentoo.org/useflags/xft) | Build with support for XFT font renderer (x11-libs/libXft) | 
| [xwidgets](https://packages.gentoo.org/useflags/xwidgets) | Enable use of GTK widgets in Emacs buffers (requires GTK3) | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

#### GUI

For Xorg or Wayland, at least `USE="gui"` is required. After Emacs 29 on Wayland, [bug #831154](https://bugs.gentoo.org/show_bug.cgi?id=831154) recommends `USE="-X gtk"` enabled "pure GTK mode".

Toolkit USE flags are mutually exclusive, so enable only one of: `gtk`, `athena`, `motif`, or `aqua`.

- `USE="aqua"` only applies to [macOS](https://wiki.gentoo.org/wiki/Prefix/Darwin).
- `USE="gtk"` is generally good for systems with one display.
- Multiple displays may `USE="athena Xaw3d"` which resembles gtk very well.
- An alternative for multiple displays is `USE="motif"`.
- To use Emacs as a daemon, [bug #292471](https://bugs.gentoo.org/show_bug.cgi?id=292471) recommends `USE="athena Xaw3d -motif -gtk"` or `USE="motif -athena -Xaw3d -gtk"`.



#### JIT

Emacs can use [gcc](https://wiki.gentoo.org/wiki/C) to compile Emacs Lisp code to native binaries (they have `.eln` suffix) which gives a nice performance boost.

To use it, activate the [jit](https://packages.gentoo.org/useflags/jit) [USE flag for](https://wiki.gentoo.org/wiki/USE_flag) [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs) and [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc):

**`/etc/portage/package.use`**

```
app-editors/emacs jit
sys-devel/gcc jit
```
Then complete a world upgrade for these changes to take effect:

`root #``emerge --ask --update --changed-use @world`


#### PGTK

Emacs can be built to the use "pgtk" (Pure [GTK](https://wiki.gentoo.org/wiki/GTK)) frontend: Emacs will no longer use [X11](https://wiki.gentoo.org/wiki/X11) APIs directly, instead it only uses [GTK](https://wiki.gentoo.org/wiki/GTK) (and [Cairo](https://wiki.gentoo.org/index.php?title=Cairo&action=edit&redlink=1)), hence (in theory) any platform supported by [GTK](https://wiki.gentoo.org/wiki/GTK), such as [Wayland](https://wiki.gentoo.org/wiki/Wayland) (without the need for XWayland).

To activate pgtk, disable the [X](https://packages.gentoo.org/useflags/X) [USE flag and enable](https://wiki.gentoo.org/wiki/USE_flag) [gui](https://packages.gentoo.org/useflags/gui) [and](https://wiki.gentoo.org/wiki/USE_flag) [gtk](https://packages.gentoo.org/useflags/gtk) [for](https://wiki.gentoo.org/wiki/USE_flag) [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs):

**`/etc/portage/package.use`**

```
app-editors/emacs gui gtk -X
```
Then run:

`root #``emerge --ask --update --changed-use @world`
This will also get rid of the warning:

Warning: due to a long standing Gtk+ bug
http://bugzilla.gnome.org/show\_bug.cgi?id=85715
Emacs might crash when run in daemon mode and the X11 connection is unexpectedly lost.
Using an Emacs configured with --with-x-toolkit=lucid does not have this problem.

Of course, the underlying Gtk+ problem still exists, and multiple displays cannot be used with pgtk.

#### SSL

To use Emacs for ERC or when desiring to download packages from ELPA/MELPA via Emacs, it is wise to compile with the `ssl` USE flag. Emacs will use the program gnutls-cli (provided by [net-libs/gnutls](https://packages.gentoo.org/packages/net-libs/gnutls)), but only when it is also compiled with the `tools` USE flag. The `tools` USE flag is not enabled by default.

### Emerge

To install Emacs, run:

`root #``emerge --ask app-editors/emacs`
### Several versions side-by-side

In Gentoo, several Emacs versions can be installed on a system simultaneously. The upstream version already installs elisp and data files into versioned subdirectories. To avoid file collisions between slots, in Gentoo binaries and man pages are suffixed with their corresponding version number, too.

The eselect module from [app-eselect/eselect-emacs](https://packages.gentoo.org/packages/app-eselect/eselect-emacs) can be used to link /usr/bin/emacs and its auxiliary programs to the ones belonging to the desired Emacs version. Consult the [eselect user guide](https://wiki.gentoo.org/wiki/Project:Eselect/User_guide) for details on eselect.

## Updating (major versions)

If upgrading from a previous major version of Emacs, it is strongly recommended to use [app-admin/emacs-updater](https://packages.gentoo.org/packages/app-admin/emacs-updater) to rebuild all byte-compiled elisp files of the installed Emacs packages.

## Configuration

Emacs can be customized by clicking through the GUI (use `M-x` `customize-group` `RET`).

Emacs can also be configured by using the \~/.emacs configuration file, which is written in Emacs Lisp, Emacs' own Lisp dialect. The \~/.emacs file is not automatically created on installation or on first invocation, though the directory \~/.emacs.d can be.

### Gentoo packages

Gentoo provide many Emacs Lisp packages available for installation:

`root #``emerge --search 'app-emacs/*'`
Package are installed to `/usr/share/emacs/site-lisp`.
After installation, package's paths are added to Emacs `load-path` global variable and *autoloaded*.
This behavior may be controlled in `/etc/emacs/site-start.el` Emacs initialization file.

## Daemon

GNU Emacs version 23 or later supports running as a daemon, to which a user can connect through the `emacsclient` program. This can be done either in the current terminal with `emacsclient -t`, or by creating a new graphical client frame (i.e. opening a new X window) with `emacsclient -c`.

Every user who wants to connect to an Emacs server must have their own instance of the daemonized GNU Emacs.

To get all benefits of the daemon setup, you should set the `EDITOR` environment variable to `emacsclient`, so that programs calling an external editor will use the existing Emacs process. Also file associations in your desktop system should call emacsclient. When closing a session or frame, no content is lost; you may simply reconnect to the Emacs server.

### OpenRC (user service)

[OpenRC](https://wiki.gentoo.org/wiki/OpenRC) version 0.60 or later supports *user services* and user init scripts in the /etc/user/init.d/ directory. In order to automatically start Emacs as a daemon in your user session, log in as a normal user and execute the command:

`user $``rc-update --user add emacs default`
This will add the /etc/user/init.d/emacs init script to the default runlevel in \~/.config/rc/runlevels/.

Further customization can be done by placing an emacs configuration file into /etc/user/conf.d/, or users can add individual configuration files in their \~/.config/rc/conf.d/ directory. The following variables can be configured:

- `EMACS`
- Absolute path to the emacs binary; /usr/bin/emacs by default
- `EMACS_OPTS`
- Options to pass to Emacs (in addition to `--fg-daemon` which is always passed); empty by default
- `EMACS_START`
- Wrapper script for starting Emacs. This executes a login shell, in order to read the user's profile ([bug #246460](https://bugs.gentoo.org/show_bug.cgi?id=246460)); /usr/libexec/emacs/emacs-wrapper.sh by default.
- `EMACS_SIGNAL_TIMEOUT`
- Retry specification for stopping the daemon; `TERM/30/KILL/5` by default. See [supervise-daemon(8)](https://manpages.org/supervise-daemon/8)- Other variables
- The configuration file can also be used to set and export other environment variables that may be useful in Emacs. Common examples include `DBUS_SESSION_BUS_ADDRESS` and `SSH_AUTH_SOCK`. Alternatively, you can define these variables in your login shell's startup files.

#### Launching the Emacs daemon at system startup

If you want to launch your user's Emacs daemon at system startup and have it persist across login sessions, you must multiplex OpenRC's user system service: As the superuser, create a symbolic link (do not copy the script, or you will miss eventual updates!) in the /etc/init.d/ directory:

`root #``ln -s user /etc/init.d/user.`*username*
where *username* is your user's login name. The service can then be started manually using:

`root #``rc-service user.`*username* start
or it may be added to the boot sequence (and will run with the user's privileges):

`root #``rc-update add user.`*username* default
See the [OpenRC user guide](https://github.com/OpenRC/openrc/blob/master/user-guide.md) for a more complete guide on how to configure user services at the OpenRC side.

### OpenRC (system service)

Gentoo provides OpenRC init scripts for Emacs in the package [app-emacs/emacs-daemon](https://packages.gentoo.org/packages/app-emacs/emacs-daemon):

`root #``emerge --ask app-emacs/emacs-daemon`
First, create a symlink for at least one user in /etc/init.d:

`root #``ln -s /etc/init.d/emacs /etc/init.d/emacs.myuser`
Then add it to the appropriate runlevel:

`root #``rc-update add emacs.myuser default`
Finally, start up the new Emacs service:

`root #``/etc/init.d/emacs.myuser start`
Usually, an Emacs server socket will be created for emacsclient to connect to. The socket will be created at /tmp/emacs\<user\_id>/server, e.g. /tmp/emacs1000/server. To use emacsclient with this socket, use the `-s` / `--socket-name` option, e.g.:

`user $``emacsclient --socket-name='/tmp/emacs1000/server'`
### systemd

With systemd, simply enable the upstream user unit:

`user $``systemctl --user enable --now emacs`
### Builtin method

The user can also start the daemon inside an already running instance of emacs. For that, type `M-x server-start RET` (that is, `Meta`, or `Alt`, followed by `x server-start`, and finally `Enter`).

### Connect to emacs daemon

`user $``emacsclient --socket-name=/tmp/emacs$(id -u)/server file`
## Usage

### Exiting Emacs

For beginners who don't know the key combinations, it may be difficult to exit Emacs. To close Emacs, type `C-x C-c` (`Ctrl`+`x` followed by `Ctrl`+`c`).

### Documentation

For a quick-start documentation, type in Emacs: `C-h t` (`Ctrl`+`h` followed by `t`). For further help on how to use Emacs, start `emacs` and type `C-h r` (`Ctrl`+`h` followed by `r`).

### Starter Kits

Since Emacs supports so many features and additional packages, configuration and customization can become cumbersome, an [*Emacs distribution*](https://www.emacswiki.org/emacs/StarterKits) may help starting out. They aim to provide a "batteries included" approach and plug together many features and packages automatically:

### Vim controls

Contrary to popular belief, many Emacs users prefer to get the best out of ["both worlds"](https://en.wikipedia.org/wiki/Editor_war).
To do so, [Vim](https://wiki.gentoo.org/wiki/Vim) and Emacs features can be combined (and used alongside), for example, with [Evil](https://github.com/emacs-evil/evil).

[Spacemacs](https://wiki.gentoo.org/wiki/Spacemacs) and [Doom](https://github.com/doomemacs/doomemacs) support this out of the box.

## Additional elisp packages

Emacs has lots of additional packages written in elisp.

Users have two choices:

1. If installing packages per-user, [package.el](https://www.emacswiki.org/emacs/ELPA) is recommended. Other methods exist too such as [straight.el](https://github.com/radian-software/straight.el) and [Elpaca](https://github.com/progfolio/elpaca).
2. If installing packages through the system's package manager is preferred, then only emerge is needed.

## See also

- [Emacs](https://wiki.gentoo.org/wiki/Emacs) — a class of powerful, extensible, self-documenting text editors.
- [Knowledge Base:Edit a configuration file](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file)
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
- [Xft support for GNU Emacs](https://wiki.gentoo.org/wiki/Xft_support_for_GNU_Emacs) — describes how to enable font anti-aliasing in Emacs using the Xft library.
