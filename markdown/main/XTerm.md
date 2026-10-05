<!-- source: https://wiki.gentoo.org/wiki/XTerm | group: Gentoo Wiki (Main) | wiki-title: XTerm -->
---
title: XTerm
url: https://wiki.gentoo.org/wiki/XTerm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-04"
fingerprint: "6603f3355383b3e0"
license: CC BY-SA 4.0
---

# XTerm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**XTerm** is a fast and featureful graphical [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) for [X](https://wiki.gentoo.org/wiki/Xorg). It predates X11, but is still actively developed.


| [+openpty](https://packages.gentoo.org/useflags/+openpty) | Use openpty() in preference to posix\_openpt() | 
| [Xaw3d](https://packages.gentoo.org/useflags/Xaw3d) | Add support for the 3d athena widget set | 
| [sixel](https://packages.gentoo.org/useflags/sixel) | Enable sixel graphics support | 
| [toolbar](https://packages.gentoo.org/useflags/toolbar) | Enable the xterm toolbar to be built | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [unicode](https://packages.gentoo.org/useflags/unicode) | Add support for Unicode | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

XTerm has a variety of compile-time options available. These options can be passed to [emerge](https://wiki.gentoo.org/wiki/Emerge) via the `EXTRA_ECONF` flag.

`user $``./configure --help````
`configure' configures this package to adapt to many kinds of systems.
Usage: ./configure [OPTION]... [VAR=VALUE]...
To assign environment variables (e.g., CC, CFLAGS...), specify them as
VAR=VALUE.  See below for descriptions of some of the useful variables.
Defaults for the options are specified in brackets.
Configuration:
  -h, --help              display this help and exit
      --help=short        display options specific to this package
      --help=recursive    display the short help of all the included packages
  -V, --version           display version information and exit
  -q, --quiet, --silent   do not print `checking...' messages
      --cache-file=FILE   cache test results in FILE [disabled]
  -C, --config-cache      alias for `--cache-file=config.cache'
  -n, --no-create         do not create output files
      --srcdir=DIR        find the sources in DIR [configure dir or `..']
Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
  --exec-prefix=EPREFIX   install architecture-dependent files in EPREFIX
                          [PREFIX]
By default, `make install' will install all the files in
`/usr/local/bin', `/usr/local/lib' etc.  You can specify
an installation prefix other than `/usr/local' using `--prefix',
for instance `--prefix=$HOME'.
For better control, use the options below.
Fine tuning of the installation directories:
  --bindir=DIR            user executables [EPREFIX/bin]
  --sbindir=DIR           system admin executables [EPREFIX/sbin]
  --libexecdir=DIR        program executables [EPREFIX/libexec]
  --datarootdir=DIR       read-only architecture-independent data [PREFIX/share]
  --datadir=DIR           read-only architecture-independent data [DATAROOTDIR]
  --sysconfdir=DIR        read-only single-machine data [PREFIX/etc]
  --sharedstatedir=DIR    modifiable architecture-independent data [PREFIX/com]
  --localstatedir=DIR     modifiable single-machine data [PREFIX/var]
  --runstatedir=DIR       extra definition of runtime data [LOCALSTATEDIR/run]
  --libdir=DIR            object code libraries [EPREFIX/lib]
  --includedir=DIR        C header files [PREFIX/include]
  --oldincludedir=DIR     C header files for non-gcc [/usr/include]
  --infodir=DIR           info documentation [DATAROOTDIR/info]
  --mandir=DIR            man documentation [DATAROOTDIR/man]
Program names:
  --program-prefix=PREFIX            prepend PREFIX to installed program names
  --program-suffix=SUFFIX            append SUFFIX to installed program names
  --program-transform-name=PROGRAM   run sed PROGRAM on installed program names
X features:
  --x-includes=DIR    X include files are in DIR
  --x-libraries=DIR   X library files are in DIR
System types:
  --build=BUILD           configure for building on BUILD [guessed]
  --host=HOST       build programs to run on HOST [BUILD]
  --target=TARGET   configure for building compilers for TARGET [HOST]
Optional Packages:
  --with-PACKAGE[=ARG]    use PACKAGE [ARG=yes]
  --without-PACKAGE       do not use PACKAGE (same as --with-PACKAGE=no)
Optional Features:
  --disable-FEATURE       do not include FEATURE (same as --enable-FEATURE=no)
  --enable-FEATURE[=ARG]  include FEATURE [ARG=yes]
  --with-system-type=XXX  test: override derived host system-type
Compile/Install Options:
  --disable-full-tgetent  disable check for full tgetent function
  --with-app-class=XXX    override X applications class (default XTerm)
  --with-app-defaults=DIR directory in which to install resource files (EPREFIX/lib/X11/app-defaults)
  --with-icon-name[=XXX]  override icon name (default: mini.xterm)
  --with-icon-symlink[=XXX] make symbolic link for icon name (default: xterm)
  --with-pixmapdir=DIR    directory in which to install pixmaps (DATADIR/pixmaps)
  --with-icondir=DIR      directory in which to install icons for desktop
  --with-icon-theme[=XXX] install icons into desktop theme (hicolor)
  --disable-desktop       disable install of xterm desktop files
  --with-desktop-category=XXX  one or more desktop categories or auto
  --with-reference=XXX    program to use as permissions-reference
  --with-xterm-symlink[=XXX] make symbolic link to installed xterm
  --disable-openpty       disable openpty, prefer other interfaces
  --disable-setuid        disable setuid in xterm, do not install setuid/setgid
  --disable-setgid        disable setgid in xterm, do not install setuid/setgid
  --with-setuid[=XXX]     use the given setuid user
  --with-utmp-setgid[=XXX] use setgid to match utmp/utmpx file
  --with-utempter         use utempter library for access to utmp
  --with-utmp-path=XXX    use XXX rather than auto for utmp path
  --with-wtmp-path=XXX    use XXX rather than auto for wtmp path
  --with-tty-group[=XXX]  use XXX for the tty-group
  --with-x                use the X Window System
  --with-pkg-config[=CMD] enable/disable use of pkg-config and its name CMD
  --with-xpm[=DIR]        use Xpm library for colored icon, may specify path
  --without-xinerama      do not use Xinerama extension for multiscreen support
  --with-Xaw3d            link with Xaw 3d library
  --with-Xaw3dxft         link with Xaw 3d xft library
  --with-neXtaw           link with neXT Athena library
  --with-XawPlus          link with Athena-Plus library
  --disable-xcursor       disable cursorTheme resource
  --enable-narrowproto    enable narrow prototypes for X libraries
  --enable-imake          enable use of imake for definitions
  --with-man2html[=XXX]   use XXX rather than groff
Terminal Configuration:
  --with-terminal-id=V    set default decTerminalID (default: vt420)
  --with-terminal-type=T  set default $TERM (default: xterm)
  --with-xterm-kbs[=XXX]  specify if xterm backspace-key sends BS or DEL
  --enable-backarrow-key  set default backarrowKey resource (default: true)
  --enable-backarrow-is-erase set default backarrowKeyIsErase resource (default: false)
  --enable-delete-is-del  set default deleteIsDEL resource (default: maybe)
  --enable-pty-erase      set default ptyInitialErase resource (default: maybe)
  --enable-alt-sends-esc  set default altSendsEscape resource (default: no)
  --enable-meta-sends-esc set default metaSendsEscape resource (default: no)
  --with-own-terminfo[=P] set default $TERMINFO (default: from environment)
  --enable-env-terminfo   setenv $TERMINFO if --with-own-terminfo gives value
Optional Features:
  --disable-active-icon   disable X11R6.3 active-icon feature
  --disable-ansi-color    disable ANSI color
  --disable-16-color      disable 16-color support
  --disable-256-color     disable 256-color support
  --disable-direct-color  disable direct-color support
  --disable-88-color      disable 88-color support
  --disable-blink-cursor  disable support for blinking cursor
  --enable-block-select   meta-button1 begins block-selection
  --enable-broken-osc     allow broken Linux OSC-strings
  --disable-broken-st     disallow broken string-terminators
  --enable-builtin-xpms   compile-in icon data
  --disable-c1-print      disallow -k8 option for printable 128-159
  --disable-bold-color    disable PC-style mapping of bold colors
  --disable-color-class   disable separate color class resources
  --disable-color-mode    disable default colorMode resource
  --disable-highlighting  disable support for color highlighting
  --disable-doublechars   disable support for double-size chars
  --disable-boxchars      disable fallback-support for box chars
  --disable-exec-selection disable "exec-formatted" and "exec-selection" actions
  --enable-exec-xterm     enable "spawn-new-terminal" action
  --enable-double-buffer  enable double-buffering in default resources
  --disable-freetype      disable freetype library-support
  --with-freetype-config  configure script to use for FreeType
  --with-freetype-cflags  -D/-I options for compiling with FreeType
  --with-freetype-libs    -L/-l options to link FreeType
  --enable-hp-fkeys       enable support for HP-style function keys
  --enable-sco-fkeys      enable support for SCO-style function keys
  --disable-sun-fkeys     disable support for Sun-style function keys
  --disable-fifo-lines    disable FIFO-storage for saved-lines
  --disable-i18n          disable internationalization
  --disable-initial-erase disable setup for stty erase
  --disable-input-method  disable input-method
  --enable-load-vt-fonts  enable load-vt-fonts() action
  --enable-logging        enable logging
  --enable-logfile-exec   enable exec'd logfile filter
  --disable-maximize      disable actions for iconify/deiconify/maximize/restore
  --disable-num-lock      disable NumLock keypad support
  --disable-paste64       disable get/set base64 selection data
  --disable-pty-handshake disable pty-handshake support
  --disable-readline-mouse disable support for mouse in readline applications
  --disable-regex         disable regular-expression selections
  --with-pcre2=[XXX]    use PCRE2 package XXX for regular-expressions
  --with-pcre             use PCRE for regular-expressions
  --disable-rightbar      disable right-scrollbar support
  --disable-samename      disable check for redundant name-change
  --disable-selection-ops disable selection-action operations
  --disable-session-mgt   disable support for session management
  --enable-status-line    enable support for status-line
  --disable-tcap-fkeys    disable termcap function-keys support
  --disable-tcap-query    disable compiled-in termcap-query support
  --disable-tek4014       disable tek4014 emulation
  --enable-toolbar        compile-in toolbar for pulldown menus
  --disable-vt52          disable VT52 emulation
  --disable-wide-attrs    disable wide-attribute support
  --disable-wide-chars    disable wide-character support
  --enable-16bit-chars    enable 16-bit character support
  --enable-mini-luit      enable mini-luit (built-in Latin9 support)
  --disable-luit          enable luit filter (Unicode translation)
  --enable-dabbrev        enable dynamic-abbreviation support
  --enable-dec-locator    enable DECterm Locator support
  --disable-screen-dumps  disable XHTML and SVG screen dumps
  --enable-regis-graphics enable ReGIS graphics support
  --disable-sixel-graphics disable sixel graphics support
  --disable-print-graphics disable screen dump to sixel support
  --disable-rectangles    disable VT420 rectangle support
  --disable-resize-adjust disable resize/cursor-adjust feature
  --disable-ziconbeep     disable -ziconbeep option
Testing/development Options:
  --enable-trace          test: set to enable debugging traces
  --with-dmalloc          test: use Gray Watson's dmalloc library
  --with-dbmalloc         test: use Conor Cahill's dbmalloc library
  --with-valgrind         test: use valgrind
  --disable-leaks         test: free permanent memory, analyze leaks
  --disable-echo          do not display "compiling" commands
  --enable-xmc-glitch     test: enable xmc magic-cookie emulation
  --enable-warnings       test: turn on gcc compiler warnings
  --enable-stdnoreturn    enable C11 _Noreturn feature for diagnostics
  --disable-rpath-hack    don't add rpath options for additional libraries
Some influential environment variables:
  CC          C compiler command
  CFLAGS      C compiler flags
  LDFLAGS     linker flags, e.g. -L<lib dir> if you have libraries in a
              nonstandard directory <lib dir>
  CPPFLAGS    C/C++ preprocessor flags, e.g. -I<include dir> if you have
              headers in a nonstandard directory <include dir>
  CPP         C preprocessor
Use these variables to override the choices made by `configure' or to help
it to find libraries and programs with nonstandard names/locations.
```
For example, to disable the Tektronix 4014 and DEC VT52 device emulation:

`root #``EXTRA_ECONF="--disable-tek4014 --disable-vt52" emerge --ask x11-terms/xterm`
Additionally, [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) can be edited to append `EXTRA_ECONF` every time emerge is invoked:

**`/etc/portage/env/xterm-minimal.conf`**

**`/etc/portage/package.env`**

`root #``emerge --ask x11-terms/xterm`
XTerm understands all of the [X Toolkit (Xt)](https://en.wikipedia.org/wiki/X_Toolkit_Intrinsics) resource names and classes. Normally, these options are set in an [X resources](https://wiki.gentoo.org/wiki/X_resources) file.

For example, the following sets JetBrainsMono as the default font:

**`~/.Xresources`**

**Font setup for XTerm**

The font name and size can also be specified on the command line, via the `-fa` and `-fs` options; this will override other resource settings:

`user $``xterm -fa JetBrainsMono -fs 14`
XTerm will generally be launched from an on-screen menu or keyboard shortcut in a user's graphical environment. However, it can also be started directly from a [shell](https://wiki.gentoo.org/wiki/Shell):

`user $``xterm -help`  ```
XTerm(367) usage:
    xterm [-options ...] [-e command args]
where options include:
    -/+132                       turn on/off 80/132 column switching
    -C                           intercept console messages
    -Sccn                        slave mode on "ttycc", file descriptor "n"
    -T string                    title name for window
    -/+ah                        turn on/off always highlight
    -/+ai                        turn off/on active icon
    -/+aw                        turn on/off auto wraparound
    -b number                    internal border in pixels
    -baudrate rate               set line-speed (default 38400)
    -/+bc                        turn on/off text cursor blinking
    -bcf milliseconds            time text cursor is off when blinking
    -bcn milliseconds            time text cursor is on when blinking
    -bd color                    border color
    -/+bdc                       turn off/on display of bold as color
    -bg color                    background color
    -bw number                   border width in pixels
    -/+cb                        turn on/off cut-to-beginning-of-line inhibit
    -cc classrange               specify additional character classes
    -/+cjk_width                 turn on/off legacy CJK width convention
    -class string                class string (XTerm)
    -/+cm                        turn off/on ANSI color mode
    -/+cn                        turn on/off cut newline inhibit
    -cr color                    text cursor color
    -/+cu                        turn on/off curses emulation
    -/+dc                        turn off/on dynamic color selection
    -display displayname         X server to contact
    -e command args ...          command to execute
    -fa pattern                  FreeType font-selection pattern
    -fb fontname                 bold text font
    -/+fbb                       turn on/off normal/bold font comparison inhibit
    -/+fbx                       turn off/on linedrawing characters
    -fc fontmenu                 start with named fontmenu choice
    -fd pattern                  FreeType Doublesize font-selection pattern
    -fg color                    foreground color
    -fi fontname                 icon font for active icon
    -fn fontname                 normal text font
    -fs size                     FreeType font-size
    -/+fullscreen                turn on/off fullscreen on startup
    -fw fontname                 doublewidth text font
    -fwb fontname                doublewidth bold text font
    -fx fontname                 XIM fontset
    %geom                        Tek window geometry
    #geom                        icon window geometry
    -geometry geom               size (in characters) and position
    -help                        print out this message
    -/+hm                        turn on/off selection-color override
    -/+hold                      turn on/off logic that retains window after exit
    -iconic                      start iconic
    -/+ie                        turn on/off initialization of 'erase' from pty
    -/+im                        use insert mode for TERMCAP
    -into windowId               use the window id given to -into as the parent window rather than the default root window
    -/+itc                       turn off/on display of italic as color
    -/+j                         turn on/off jump scroll
    -/+k8                        turn on/off C1-printable classification
    -kt keyboardtype             set keyboard type: tcap sun vt220
    -/+l                         turn on/off logging
    -/+lc                        turn on/off locale mode using luit
    -lcc path                    filename of locale converter (/usr/bin/luit)
    -leftbar                     force scrollbar left
    -lf filename                 logging filename (use '-' for standard out)
    -/+ls                        turn on/off login shell
    -/+maximized                 turn on/off maximize on startup
    -/+mb                        turn on/off margin bell
    -mc milliseconds             multiclick time in milliseconds
    -/+mesg                      forbid/allow messages
    -/+mk_width                  turn on/off simple width convention
    -ms color                    pointer color
    -n string                    icon name for window
    -name string                 client instance, icon, and title strings
    -nb number                   margin bell in characters from right end
    -/+nul                       turn off/on display of underlining
    -/+pc                        turn on/off PC-style bold colors
    -pf fontname                 cursor font for text area pointer
    -/+pob                       turn on/off pop on bell
    -report-charclass            report "charClass" after initialization
    -report-colors               report colors as they are allocated
    -report-fonts                report fonts as loaded to stdout
    -report-icons                report title/icon updates
    -report-xres                 report X resources for VT100 widget
    -rightbar                    force scrollbar right (default left)
    -/+rv                        turn on/off reverse video
    -/+rvc                       turn off/on display of reverse as color
    -/+rw                        turn on/off reverse wraparound
    -/+s                         turn on/off multiscroll
    -/+samename                  turn on/off the no-flicker option for title and icon name
    -/+sb                        turn on/off scrollbar
    -selbg color                 selection background color
    -selfg color                 selection foreground color
    -/+sf                        turn on/off Sun Function Key escape codes
    -sh number                   scale line-height values by the given number
    -/+si                        turn on/off scroll-on-tty-output inhibit
    -/+sk                        turn on/off scroll-on-keypress
    -sl number                   number of scrolled lines to save
    -/+sm                        turn on/off the session-management support
    -/+sp                        turn on/off Sun/PC Function/Keypad mapping
    -/+t                         turn on/off Tek emulation window
    -ti termid                   terminal identifier
    -title string                title string
    -tm string                   terminal mode keywords and characters
    -tn name                     TERM environment variable name
    -/+u8                        turn on/off UTF-8 mode (implies wide-characters)
    -/+uc                        turn on/off underline cursor
    -/+ulc                       turn off/on display of underline as color
    -/+ulit                      turn off/on display of underline as italics
    -/+ut                        turn on/off utmp support
    -/+vb                        turn on/off visual bell
    -version                     print the version number
    -/+wc                        turn on/off wide-character mode
    -/+wf                        turn on/off wait for map before command exec
    -xrm resourcestring          additional resource specifications
    -ziconbeep percent           beep and flag icon of window having hidden output
Fonts should be fixed width and, if both normal and bold are specified, should
have the same size.  If only a normal font is specified, it will be used for
both normal and bold text (by doing overstriking).  The -e option, if given,
must appear at the end of the command line, otherwise the user's default shell
will be started.  Options that start with a plus sign (+) restore the default.
```
