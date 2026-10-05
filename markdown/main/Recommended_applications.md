<!-- source: https://wiki.gentoo.org/wiki/Recommended_applications | group: Gentoo Wiki (Main) | wiki-title: Recommended applications -->
---
title: Recommended applications
url: https://wiki.gentoo.org/wiki/Recommended_applications
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: "125d6f7a099728bd"
license: CC BY-SA 4.0
---

# Recommended applications

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland)), with suggestions for reliable and  [easy to install](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Installing_software) software for common Gentoo Linux needs.

Most of these packages are in the [stable branch](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Stable), but some useful and otherwise high quality software is still in the [testing branch](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Testing). Testing branch packages may be made available for installation by [accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package), however packages from the testing branch should only be used after [taking note of any risks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Testing).

In most cases, software designed for one [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) will work just as well with another desktop environment, though sometimes with slightly less integration. Installing software from a different desktop environment may pull in more dependencies than an equivalent native application; there is usually no issue at all with this.

To reference a new piece of software here, please read the [adding to this page](https://wiki.gentoo.org/wiki/Recommended_applications#Adding_to_this_page) section.

This is a "best of kind" list, **much more software is available on Gentoo**. Use [eix](https://wiki.gentoo.org/wiki/Eix) or browse [packages.gentoo.org](https://packages.gentoo.org/categories) to find **all** applications available on Gentoo.

| Name | Package | Description | 
|---|---|---|
|  [dmenu](https://wiki.gentoo.org/wiki/Dwm#dmenu) | [x11-misc/dmenu](https://packages.gentoo.org/packages/x11-misc/dmenu) | Generic, highly customizable, and efficient menu for [X11](https://wiki.gentoo.org/wiki/Xorg). | 
| Rofi | [x11-misc/rofi](https://packages.gentoo.org/packages/x11-misc/rofi) | Window switcher, run dialog, and dmenu replacement. | 
| [Wofi](https://wiki.gentoo.org/wiki/Wofi) | [gui-apps/wofi](https://packages.gentoo.org/packages/gui-apps/wofi) | Launcher/menu program for [wlroots](https://wiki.gentoo.org/wiki/Wlroots)-based [Wayland](https://wiki.gentoo.org/wiki/Wayland) compositors such as [Sway](https://wiki.gentoo.org/wiki/Sway). | 

The X11 clipboard supports multiple selections which may be filled by simply selecting text on the screen or by specifically using a copy function. [X11](https://wiki.gentoo.org/wiki/Xorg) selections have a coherent operation and applications implement their use by convention, though the conventions are not always fully respected. Clipboard managers generally manage a history of the X11 selections and sometimes modify their behavior.

| Name | Package | Description | 
|---|---|---|
| Clipman | [xfce-extra/xfce4-clipman-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-clipman-plugin) | Clipboard manager for [Xfce](https://wiki.gentoo.org/wiki/Xfce) called Clipman. | 
| CopyQ | [x11-misc/copyq](https://packages.gentoo.org/packages/x11-misc/copyq) | Advanced clipboard manager with editing and scripting features. | 
| Parcellite | [x11-misc/parcellite](https://packages.gentoo.org/packages/x11-misc/parcellite) | Lightweight [GTK](https://wiki.gentoo.org/wiki/GTK)-based clipboard manager. | 
| Qlipper | [x11-misc/qlipper](https://packages.gentoo.org/packages/x11-misc/qlipper) | Lightweight and cross-platform clipboard history applet. | 
| wl-clipboard | [gui-apps/wl-clipboard](https://packages.gentoo.org/packages/gui-apps/wl-clipboard) | Copy data from standard input to [Wayland](https://wiki.gentoo.org/wiki/Wayland) clipboard. | 
| xclip | [x11-misc/xclip](https://packages.gentoo.org/packages/x11-misc/xclip) | Copy data from standard input to [X](https://wiki.gentoo.org/wiki/Xorg) clipboard. | 

| Name | Package | Description | 
|---|---|---|
| MenuLibre | [x11-misc/menulibre](https://packages.gentoo.org/packages/x11-misc/menulibre) | Application menu editor. | 
| QtCurve | [x11-themes/qtcurve](https://packages.gentoo.org/packages/x11-themes/qtcurve) | Toolkit theme to provide a common style to [GNOME](https://wiki.gentoo.org/wiki/GNOME)/[GTK](https://wiki.gentoo.org/wiki/GTK) and [KDE](https://wiki.gentoo.org/wiki/KDE)/[Qt](https://wiki.gentoo.org/wiki/Qt) applications. See [GTK themes in Qt applications](https://wiki.gentoo.org/wiki/GTK_themes_in_Qt_applications) for more information. | 

A [taskbar](https://en.wikipedia.org/wiki/Taskbar) is generally a strip on one edge of the screen with a menu, a representation of open windows, shortcuts to launch apps, widgets to display information etc. [Desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) usually provide a taskbar but the default can often be replaced or complemented according to user preference. A taskbar associated with a [window manager](https://wiki.gentoo.org/wiki/Window_manager) can form the basis of a custom desktop environment.

| Name | Package | Description | 
|---|---|---|
| fbpanel | [x11-misc/fbpanel](https://packages.gentoo.org/packages/x11-misc/fbpanel) | Lightweight panel that works with any NETWM compliant window manager (e.g. [xfwm4](https://wiki.gentoo.org/wiki/Xfce), [sawfish](https://wiki.gentoo.org/wiki/Sawfish), [openbox](https://wiki.gentoo.org/wiki/Openbox), [KDE](https://wiki.gentoo.org/wiki/KDE) window manager). | 
| LXPanel | [lxde-base/lxpanel](https://packages.gentoo.org/packages/lxde-base/lxpanel) | Lightweight [X11](https://wiki.gentoo.org/wiki/Xorg) desktop panel for [LXDE](https://wiki.gentoo.org/wiki/LXDE), also works with other window managers. | 
| [Polybar](https://wiki.gentoo.org/wiki/Polybar) | [x11-misc/polybar](https://packages.gentoo.org/packages/x11-misc/polybar) | Tool for creating highly customizable status bars for the desktop environment. | 
| [Tint2](https://wiki.gentoo.org/wiki/Tint2) | [x11-misc/tint2](https://packages.gentoo.org/packages/x11-misc/tint2) | Lightweight panel/taskbar specifically made for [Openbox](https://wiki.gentoo.org/wiki/Openbox), but it can also work with other window managers. | 
| wbar | [x11-misc/wbar](https://packages.gentoo.org/packages/x11-misc/wbar) | Fast, lightweight, quick-launch bar. | 
| [Waybar](https://wiki.gentoo.org/wiki/Waybar) | [gui-apps/waybar](https://packages.gentoo.org/packages/gui-apps/waybar) | Highly customizable [Wayland](https://wiki.gentoo.org/wiki/Wayland) bar for [Sway](https://wiki.gentoo.org/wiki/Sway) and [wlroots](https://wiki.gentoo.org/wiki/Wlroots)-based compositors. | 

Theses graphical [file archivers](https://en.wikipedia.org/wiki/File_archiver) support multiple archive formats and should work with any desktop environment. See [data compression](https://wiki.gentoo.org/wiki/Data_compression) for more information on archivers in general.

| Name | Package | Description | 
|---|---|---|
| Ark | [kde-apps/ark](https://packages.gentoo.org/packages/kde-apps/ark) | Archive manager by KDE. | 
| Engrampa | [app-arch/engrampa](https://packages.gentoo.org/packages/app-arch/engrampa) | Official  archive  manager for  the [MATE](https://wiki.gentoo.org/wiki/MATE) Desktop Environment. | 
| File Roller | [app-arch/file-roller](https://packages.gentoo.org/packages/app-arch/file-roller) | Archive manager for the [GNOME](https://wiki.gentoo.org/wiki/GNOME) desktop environment. | 
| LXQt Archiver | [app-arch/lxqt-archiver](https://packages.gentoo.org/packages/app-arch/lxqt-archiver) | Simple & lightweight [Qt](https://wiki.gentoo.org/wiki/Qt)-based but desktop-agnostic file archiver. | 
| Xarchiver | [app-arch/xarchiver](https://packages.gentoo.org/packages/app-arch/xarchiver) | [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment)-independent archiver front end based on [GTK](https://wiki.gentoo.org/wiki/GTK). | 

| Name | Package | Description | 
|---|---|---|
| Brasero | [app-cdr/brasero](https://packages.gentoo.org/packages/app-cdr/brasero) | CD/DVD burning application for the [GNOME](https://wiki.gentoo.org/wiki/GNOME) desktop. | 
| K3b | [kde-apps/k3b](https://packages.gentoo.org/packages/kde-apps/k3b) | Fully-featured CD/DVD burner for [KDE](https://wiki.gentoo.org/wiki/KDE). | 
| Xfburn | [app-cdr/xfburn](https://packages.gentoo.org/packages/app-cdr/xfburn) | [GTK](https://wiki.gentoo.org/wiki/GTK) disc burner from [Xfce](https://wiki.gentoo.org/wiki/Xfce). | 

See the [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) article for information on writing iso files to USB devices. See [CD/DVD/BD writing](https://wiki.gentoo.org/wiki/CD/DVD/BD_writing) on burning disks from the [command line](https://wiki.gentoo.org/wiki/Terminal_emulator). See [FAQ - How do I burn an ISO file?](https://wiki.gentoo.org/wiki/FAQ#How_do_I_burn_an_ISO_file.3F) about burning iso to CD on various OSs.

| Name | Package | Description | 
|---|---|---|
| [Calibre](https://wiki.gentoo.org/wiki/Calibre) | [app-text/calibre](https://packages.gentoo.org/packages/app-text/calibre) | Electronic book management tool. | 
| crqt-ng | [app-text/crqt-ng](https://packages.gentoo.org/packages/app-text/crqt-ng) | Cross-platform e-book reader. | 
| [Foliate](https://wiki.gentoo.org/wiki/Foliate) | [app-text/foliate](https://packages.gentoo.org/packages/app-text/foliate) | Simple, modern, open-source [GTK](https://wiki.gentoo.org/wiki/GTK) eBook viewer. Supports EPUB, Mobipocket, Kindle, FictionBook, and comic book archive formats. | 
| [Zathura](https://wiki.gentoo.org/wiki/Zathura) | [app-text/zathura-cb](https://packages.gentoo.org/packages/app-text/zathura-cb) | Extension for Zathura document viewer. No [GTK](https://wiki.gentoo.org/wiki/GTK) dependency. | 

| Name | Package | Description | 
|---|---|---|
| apvlv | [app-text/apvlv](https://packages.gentoo.org/packages/app-text/apvlv) | PDF/EPUB viewer with vim like bindings. | 
| Evince | [app-text/evince](https://packages.gentoo.org/packages/app-text/evince) | Simple document viewer for [GNOME](https://wiki.gentoo.org/wiki/GNOME). | 
| llpp | [app-text/llpp](https://packages.gentoo.org/packages/app-text/llpp) | Graphical PDF viewer which aims to superficially resemble [less(1)](https://man.archlinux.org/man/less.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [MuPDF](https://wiki.gentoo.org/wiki/MuPDF) | [app-text/mupdf](https://packages.gentoo.org/packages/app-text/mupdf) | Free and open-source software framework written in C that implements a PDF, XPS, and EPUB parsing and rendering engine. | 
| Okular | [kde-apps/okular](https://packages.gentoo.org/packages/kde-apps/okular) | [KDE](https://wiki.gentoo.org/wiki/KDE) universal document viewer. | 
| qpdfview | [app-text/qpdfview](https://packages.gentoo.org/packages/app-text/qpdfview) | Lightweight tabbed PDF viewer with a [Qt](https://wiki.gentoo.org/wiki/Qt) interface. | 
| [Zathura](https://wiki.gentoo.org/wiki/Zathura) | [app-text/zathura](https://packages.gentoo.org/packages/app-text/zathura) [app-text/zathura-meta](https://packages.gentoo.org/packages/app-text/zathura-meta) | Free, plugin-based document viewer. | 

| Name | Package | Description | 
|---|---|---|
| GNote | [app-misc/gnote](https://packages.gentoo.org/packages/app-misc/gnote) | Note-taking application for [GNOME](https://wiki.gentoo.org/wiki/GNOME). (Not to be confused with "GNotes".) | 
| Marknote | [app-text/marknote](https://packages.gentoo.org/packages/app-text/marknote) | Note-taking application with markdown formatting - from [KDE](https://wiki.gentoo.org/wiki/KDE) community. | 
| Notes | [xfce-extra/xfce4-notes-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-notes-plugin) | [Xfce](https://wiki.gentoo.org/wiki/Xfce) panel sticky notes plugin. | 
| Xournal | [app-text/xournal](https://packages.gentoo.org/packages/app-text/xournal) | Handwritten note-taking, sketching, and keeping a journal using a stylus. | 
| Xournal++ | [app-text/xournalpp](https://packages.gentoo.org/packages/app-text/xournalpp) | Handwriting note-taking software with PDF annotation support. | 
| Xpad | [x11-misc/xpad](https://packages.gentoo.org/packages/x11-misc/xpad) | A sticky note application for [GTK](https://wiki.gentoo.org/wiki/GTK). | 
| Zim | [x11-misc/zim](https://packages.gentoo.org/packages/x11-misc/zim) | Desktop wiki as a graphical text editor used to maintain a collection of wiki pages. | 

| Name | Package | Description | 
|---|---|---|
| Abiword | [app-office/abiword](https://packages.gentoo.org/packages/app-office/abiword) | Light and fast cross-platform word processor. | 
| Calligra | [app-office/calligra](https://packages.gentoo.org/packages/app-office/calligra) | The [KDE](https://wiki.gentoo.org/wiki/KDE) office suite. | 
| [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) | [app-office/libreoffice](https://packages.gentoo.org/packages/app-office/libreoffice) [app-office/libreoffice-bin](https://packages.gentoo.org/packages/app-office/libreoffice-bin) | Full office productivity suite, a successor of the OpenOffice.org that strives to be a better and less bloated office suite. | 
| LyX | [app-office/lyx](https://packages.gentoo.org/packages/app-office/lyx) | WYSIWYM frontend for LaTeX, DocBook, etc. | 
| [Pdfbox](https://wiki.gentoo.org/wiki/Pdfbox) | [dev-java/pdfbox](https://packages.gentoo.org/packages/dev-java/pdfbox) | [pdfbox](https://wiki.gentoo.org/wiki/Pdfbox) — an open source Java tool for working with PDF documents. | 
| Scribus | [app-office/scribus](https://packages.gentoo.org/packages/app-office/scribus) | Desktop publishing (DTP) and layout program. | 
| Sigil | [app-text/sigil](https://packages.gentoo.org/packages/app-text/sigil) | Multi-platform WYSIWYG ebook editor for ePub format. | 

| Name | Package | Description | 
|---|---|---|
| [HexChat](https://wiki.gentoo.org/wiki/HexChat) | [net-irc/hexchat](https://packages.gentoo.org/packages/net-irc/hexchat) | Graphical IRC client based on XChat. | 
| Konversation | [net-irc/konversation](https://packages.gentoo.org/packages/net-irc/konversation) | IRC client by [KDE](https://wiki.gentoo.org/wiki/KDE). | 
| [Pidgin](https://wiki.gentoo.org/wiki/Pidgin) | [net-im/pidgin](https://packages.gentoo.org/packages/net-im/pidgin) | Easy to use and free chat client that supports AIM, Google Talk, ICQ, IRC, XMPP, and more chat networks all at once. | 
| Psi | [net-im/psi](https://packages.gentoo.org/packages/net-im/psi) | [Qt](https://wiki.gentoo.org/wiki/Qt) Jabber client. | 
| qTox | [net-im/qtox](https://packages.gentoo.org/packages/net-im/qtox) | [Qt](https://wiki.gentoo.org/wiki/Qt)-based Tox client. | 
| [Quassel](https://wiki.gentoo.org/wiki/Quassel) | [net-irc/quassel](https://packages.gentoo.org/packages/net-irc/quassel) | Daemon/headless IRC client, part of Quassel, that supports 24/7 connectivity. | 
| [Signal Desktop](https://wiki.gentoo.org/wiki/Signal_Desktop) | [net-im/signal-desktop-bin](https://packages.gentoo.org/packages/net-im/signal-desktop-bin) | Messaging application geared towards privacy. | 
| WebRTC <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup><sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) or [www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin) | Implemented in [Firefox](https://wiki.gentoo.org/wiki/Firefox). | 

Some popular proprietary messaging services can be found in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository) if needed, such as [Discord](https://wiki.gentoo.org/wiki/Discord) or [Telegram](https://wiki.gentoo.org/wiki/Telegram) (open source desktop client with closed source server backend). Some other services can be accessed on Gentoo using their web interface, such as WhatsApp.

See the [IRC](https://wiki.gentoo.org/wiki/IRC) article for some more IRC clients. See the [instant messaging](https://wiki.gentoo.org/wiki/Category:Instant_messaging) category for more articles on this subject.

| Name | Package | Description | 
|---|---|---|
| [Claws Mail](https://wiki.gentoo.org/wiki/Claws_Mail) | [mail-client/claws-mail](https://packages.gentoo.org/packages/mail-client/claws-mail) | Mail client forked from Sylpheed. | 
| Evolution | [mail-client/evolution](https://packages.gentoo.org/packages/mail-client/evolution) | Integrated e-mail and calendar client, particularly useful for connecting to Microsoft Office365 and Exchange servers. | 
| Geary | [mail-client/geary](https://packages.gentoo.org/packages/mail-client/geary) | Geary is an email application built around conversations, for the [GNOME](https://wiki.gentoo.org/wiki/GNOME) desktop. | 
| [Thunderbird](https://wiki.gentoo.org/wiki/Thunderbird) | [mail-client/thunderbird](https://packages.gentoo.org/packages/mail-client/thunderbird) [mail-client/thunderbird-bin](https://packages.gentoo.org/packages/mail-client/thunderbird-bin) | Mozilla's solution to the e-mail client. | 

| Name | Package | Description | 
|---|---|---|
| [Deluge](https://wiki.gentoo.org/wiki/Deluge) | [net-p2p/deluge](https://packages.gentoo.org/packages/net-p2p/deluge) | Open-source, cross platform [BitTorrent](https://wiki.gentoo.org/wiki/BitTorrent) client. | 
| Freenet | [net-p2p/freenet](https://packages.gentoo.org/packages/net-p2p/freenet) | An encrypted network without censorship | 
| [QBittorrent](https://wiki.gentoo.org/wiki/QBittorrent) | [net-p2p/qbittorrent](https://packages.gentoo.org/packages/net-p2p/qbittorrent) | [Qt](https://wiki.gentoo.org/wiki/Qt) BitTorrent client. | 
| Transmission | [net-p2p/transmission](https://packages.gentoo.org/packages/net-p2p/transmission) | BitTorrent client with [GTK](https://wiki.gentoo.org/wiki/GTK), [Qt](https://wiki.gentoo.org/wiki/Qt), CLI, and Web front-ends. | 

| Name | Package | Description | 
|---|---|---|
| [Brave](https://wiki.gentoo.org/wiki/Brave) | - | Focused on privacy, blocking trackers, and advertisements. | 
| [Chromium](https://wiki.gentoo.org/wiki/Chromium) | [www-client/chromium](https://packages.gentoo.org/packages/www-client/chromium) | Open source version of Google's [Chrome](https://wiki.gentoo.org/wiki/Chrome) web browser. | 
| [Epiphany](https://wiki.gentoo.org/wiki/Epiphany) | [www-client/epiphany](https://packages.gentoo.org/packages/www-client/epiphany) | Simple web browser tightly integrated with [GNOME](https://wiki.gentoo.org/wiki/GNOME). Based on WebKit and [GTK](https://wiki.gentoo.org/wiki/GTK)4. | 
| [Falkon](https://wiki.gentoo.org/wiki/Falkon) | [www-client/falkon](https://packages.gentoo.org/packages/www-client/falkon) | Lightweight web browser based on QtWebEngine. | 
| [Firefox](https://wiki.gentoo.org/wiki/Firefox) | [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) [www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin) | Mozilla's web browser. | 
| [Qutebrowser](https://wiki.gentoo.org/wiki/Qutebrowser) | [www-client/qutebrowser](https://packages.gentoo.org/packages/www-client/qutebrowser) | Web browser with vim-style key bindings based on QtWebEngine. | 
| [Vivaldi](https://wiki.gentoo.org/wiki/Vivaldi) | [www-client/vivaldi](https://packages.gentoo.org/packages/www-client/vivaldi) | A browser for our friends | 

See also [Category:Web browser](https://wiki.gentoo.org/wiki/Category:Web_browser).

| Name | Package | Description | 
|---|---|---|
| [Audacity](https://wiki.gentoo.org/wiki/Audacity) | [media-sound/audacity](https://packages.gentoo.org/packages/media-sound/audacity) | Free cross-platform audio recorder and editor. | 
| SoundRecorder | [media-sound/gnome-sound-recorder](https://packages.gentoo.org/packages/media-sound/gnome-sound-recorder) | Simple sound recorder. | 

See also [music production](https://wiki.gentoo.org/wiki/Music_production).

| Name | Package | Description | 
|---|---|---|
| Document Scanner | [media-gfx/simple-scan](https://packages.gentoo.org/packages/media-gfx/simple-scan) | Simple document scanning utility for GNOME. | 
| gscan2pdf | [media-gfx/gscan2pdf](https://packages.gentoo.org/packages/media-gfx/gscan2pdf) | Scan documents, perform OCR, produce PDFs and DjVus, gtk based. | 
| Paperwork | [app-text/paperwork](https://packages.gentoo.org/packages/app-text/paperwork) | Personal document manager: scan or import documents and find them back in a snap with full text search! | 
| Skanlite | [kde-misc/skanlite](https://packages.gentoo.org/packages/kde-misc/skanlite) | Simple image scanning application based on libksane and [KDE](https://wiki.gentoo.org/wiki/KDE) Frameworks. | 
| XSane | [media-gfx/xsane](https://packages.gentoo.org/packages/media-gfx/xsane) | Tried and tested graphical scanning frontend. | 

See the wiki page on [SANE](https://wiki.gentoo.org/wiki/SANE) for more information on scanning in Gentoo.

| Name | Package | Description | 
|---|---|---|
| Cheese | [media-video/cheese](https://packages.gentoo.org/packages/media-video/cheese) | [GTK](https://wiki.gentoo.org/wiki/GTK) program to take pictures and videos from your webcam, GNOME oriented. | 
| Guvcview | [media-video/guvcview](https://packages.gentoo.org/packages/media-video/guvcview) | Simple [Qt](https://wiki.gentoo.org/wiki/Qt) or [GTK](https://wiki.gentoo.org/wiki/GTK)3 interface for capturing and viewing video from v4l2 devices. | 
| [OBS Studio](https://wiki.gentoo.org/wiki/OBS_Studio) | [media-video/obs-studio](https://packages.gentoo.org/packages/media-video/obs-studio) | Free software for video recording and live streaming. | 
| SimpleScreenRecorder | [media-video/simplescreenrecorder](https://packages.gentoo.org/packages/media-video/simplescreenrecorder) | Just as the name implies. | 

| Name | Package | Description | 
|---|---|---|
| [Audacious](https://wiki.gentoo.org/wiki/Audacious) | [media-sound/audacious](https://packages.gentoo.org/packages/media-sound/audacious) [media-plugins/audacious-plugins](https://packages.gentoo.org/packages/media-plugins/audacious-plugins) | Media player similar to XMMS, and Winamp. | 
| Elisa | [media-sound/elisa](https://packages.gentoo.org/packages/media-sound/elisa) | Simple music player for the [KDE](https://wiki.gentoo.org/wiki/KDE) Framework. | 
| gmusicbrowser | [media-sound/gmusicbrowser](https://packages.gentoo.org/packages/media-sound/gmusicbrowser) | Open-source jukebox for large collections of MP3/Ogg/FLAC files. | 
| MOC (music on console) | [media-sound/moc](https://packages.gentoo.org/packages/media-sound/moc) | ncurses interface for playing audio files, Music On Console. | 
| [MPD](https://wiki.gentoo.org/wiki/MPD) | [media-sound/mpd](https://packages.gentoo.org/packages/media-sound/mpd) | Flexible, server-side application for playing music with X11 clients. | 
| Qmmp | [media-sound/qmmp](https://packages.gentoo.org/packages/media-sound/qmmp) | [Qt](https://wiki.gentoo.org/wiki/Qt)-based audio player with Winamp/XMMS skins support. | 
| Rhythmbox | [media-sound/rhythmbox](https://packages.gentoo.org/packages/media-sound/rhythmbox) | Music playing application for [GNOME](https://wiki.gentoo.org/wiki/GNOME) framework. | 
| Strawberry | [media-sound/strawberry](https://packages.gentoo.org/packages/media-sound/strawberry) | Music player and library organizer based on Amarok and [Qt](https://wiki.gentoo.org/wiki/Qt). | 

See also [Category:Audio player](https://wiki.gentoo.org/wiki/Category:Audio_player).

| Name | Package | Description | 
|---|---|---|
| ImageMagick display command | [media-gfx/imagemagick](https://packages.gentoo.org/packages/media-gfx/imagemagick) | Collection of tools and libraries for many image formats, including an image viewer. | 
| Eye of GNOME (eog) | [media-gfx/eog](https://packages.gentoo.org/packages/media-gfx/eog) | Image viewer and browser for [GNOME](https://wiki.gentoo.org/wiki/GNOME). | 
| fbida | [media-gfx/fbida](https://packages.gentoo.org/packages/media-gfx/fbida) | Image viewers for the framebuffer console (fbi) and [X11](https://wiki.gentoo.org/wiki/Xorg) (ida). | 
| [Feh](https://wiki.gentoo.org/wiki/Feh) | [media-gfx/feh](https://packages.gentoo.org/packages/media-gfx/feh) | Open-source image viewer mainly aimed at command-line users but works as a very lightweight minimal viewer for [X11](https://wiki.gentoo.org/wiki/Xorg). | 
| Geeqie | [media-gfx/geeqie](https://packages.gentoo.org/packages/media-gfx/geeqie) | Lightweight [GTK](https://wiki.gentoo.org/wiki/GTK) image viewer. | 
| GPicView | [media-gfx/gpicview](https://packages.gentoo.org/packages/media-gfx/gpicview) | [GTK](https://wiki.gentoo.org/wiki/GTK) [LXDE](https://wiki.gentoo.org/wiki/LXDE) lightweight image viewer. | 
| gThumb | [media-gfx/gthumb](https://packages.gentoo.org/packages/media-gfx/gthumb) | Image viewer and browser for [GNOME](https://wiki.gentoo.org/wiki/GNOME). | 
| Gwenview | [kde-apps/gwenview](https://packages.gentoo.org/packages/kde-apps/gwenview) | Fast and easy to use image viewer for browsing and displaying a collection of images in [KDE](https://wiki.gentoo.org/wiki/KDE). | 
| [imv](https://wiki.gentoo.org/wiki/Imv) | [media-gfx/imv](https://packages.gentoo.org/packages/media-gfx/imv) | Image viewer for [X11](https://wiki.gentoo.org/wiki/Xorg) and [Wayland](https://wiki.gentoo.org/wiki/Wayland). | 
| nomacs | [media-gfx/nomacs](https://packages.gentoo.org/packages/media-gfx/nomacs) | Lightweight [Qt](https://wiki.gentoo.org/wiki/Qt) image viewer. | 
| Sxiv | [media-gfx/sxiv](https://packages.gentoo.org/packages/media-gfx/sxiv) | Lightweight, simple image viewer with multiple features. Written in C. | 
| Viewnior | [media-gfx/viewnior](https://packages.gentoo.org/packages/media-gfx/viewnior) | Fast, simple, elegant, minimalist image viewer for [X11](https://wiki.gentoo.org/wiki/Xorg). Written in C with [GTK](https://wiki.gentoo.org/wiki/GTK). | 

| Name | Package | Description | 
|---|---|---|
| [Batik](https://wiki.gentoo.org/wiki/Batik) | [dev-java/batik](https://packages.gentoo.org/packages/dev-java/batik) | [Batik](https://wiki.gentoo.org/wiki/Batik) — Java-based toolkit for applications or applets that want to use images in the Scalable Vector Graphics (SVG) format | 
| Converseen | [media-gfx/converseen](https://packages.gentoo.org/packages/media-gfx/converseen) | Batch image converter and resizer based on ImageMagick. | 
| [Flameshot](https://wiki.gentoo.org/wiki/Flameshot) | [media-gfx/flameshot](https://packages.gentoo.org/packages/media-gfx/flameshot) | Powerful and simple to use screenshot software, with seamless in-place editing capabilities. | 
| [GIMP](https://wiki.gentoo.org/wiki/GIMP) | [media-gfx/gimp](https://packages.gentoo.org/packages/media-gfx/gimp) | The GNU Image Manipulation Program. It can be used as a simple paint tool, photo retouching and general image manipulation. | 
| G'MIC | [media-gfx/gmic](https://packages.gentoo.org/packages/media-gfx/gmic) | Framework and script language that allows the creation of complex macros, with GIMP and Krita plugins (enable appropriate use flags). | 
| [Inkscape](https://wiki.gentoo.org/wiki/Inkscape) | [media-gfx/inkscape](https://packages.gentoo.org/packages/media-gfx/inkscape) | Powerful, free vector graphics design workshop. | 
| KolourPaint | [kde-apps/kolourpaint](https://packages.gentoo.org/packages/kde-apps/kolourpaint) | Simple [KDE](https://wiki.gentoo.org/wiki/KDE)-based painting program, like the traditional MS Paint. | 
| Krita | [media-gfx/krita](https://packages.gentoo.org/packages/media-gfx/krita) | Professional open source painting program made by artists. | 
| MyPaint | [media-gfx/mypaint](https://packages.gentoo.org/packages/media-gfx/mypaint) | Fast and easy graphics application for digital painters. | 
| XPaint | [media-gfx/xpaint](https://packages.gentoo.org/packages/media-gfx/xpaint) | Ultra light, bare [X11](https://wiki.gentoo.org/wiki/Xorg) image editing tool. | 

| Name | Package | Description | 
|---|---|---|
| [Darktable](https://wiki.gentoo.org/wiki/Darktable) | [media-gfx/darktable](https://packages.gentoo.org/packages/media-gfx/darktable) | Photography workflow application and RAW developer. | 
| digiKam | [media-gfx/digikam](https://packages.gentoo.org/packages/media-gfx/digikam) | Powerful, feature-rich [KDE](https://wiki.gentoo.org/wiki/KDE) digital photo management application. | 
| Hugin | [media-gfx/hugin](https://packages.gentoo.org/packages/media-gfx/hugin) | Graphical user interface for the creation & processing of panoramic images. | 
| Luminance HDR | [media-gfx/luminance-hdr](https://packages.gentoo.org/packages/media-gfx/luminance-hdr) | Graphical user interface for high dynamic range image composition and processing. | 
| RawTherapee | [media-gfx/rawtherapee](https://packages.gentoo.org/packages/media-gfx/rawtherapee) | Powerful, free, cross-platform raw photo processing system. | 
| Shotwell | [media-gfx/shotwell](https://packages.gentoo.org/packages/media-gfx/shotwell) | Photo manager for [GNOME](https://wiki.gentoo.org/wiki/GNOME). | 

| Name | Package | Description | 
|---|---|---|
| [Mpv](https://wiki.gentoo.org/wiki/Mpv) | [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv) | Open source media player with a minimal interface. Supports a wide variety of media formats, has powerful scripting capabilities, and high quality video output. | 
| Celluloid | [media-video/celluloid](https://packages.gentoo.org/packages/media-video/celluloid) | A simple [GTK+](https://wiki.gentoo.org/wiki/GTK) frontend for [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv). | 
| SMPlayer | [media-video/smplayer](https://packages.gentoo.org/packages/media-video/smplayer) | Fully-featured [Qt](https://wiki.gentoo.org/wiki/Qt) front-end for [media-video/mplayer](https://packages.gentoo.org/packages/media-video/mplayer). | 
| [VLC](https://wiki.gentoo.org/wiki/VLC) | [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc) | Wildly popular, cross platform video player and streamer. | 

See also [Category:Video player](https://wiki.gentoo.org/wiki/Category:Video_player).

| Name | Package | Description | 
|---|---|---|
| Avidemux | [media-video/avidemux](https://packages.gentoo.org/packages/media-video/avidemux) | Free video editor designed for simple cutting, filtering and encoding tasks. | 
| Kdenlive | [kde-apps/kdenlive](https://packages.gentoo.org/packages/kde-apps/kdenlive) | Non-linear video editing suite by [KDE](https://wiki.gentoo.org/wiki/KDE). | 
| Shotcut | [media-video/shotcut](https://packages.gentoo.org/packages/media-video/shotcut) | A free, open source, cross-platform video editor (testing branch, as of 2022-11). | 
| Pitivi | [media-video/pitivi](https://packages.gentoo.org/packages/media-video/pitivi) | Stable and effective non-linear video editor for low-end systems. | 

| Name | Package | Description | 
|---|---|---|
| [Blender](https://wiki.gentoo.org/wiki/Blender) | [media-gfx/blender](https://packages.gentoo.org/packages/media-gfx/blender) | Free and open-source 3D creation suite. | 

| Name | Package | Description | 
|---|---|---|
| [Nfs-utils](https://wiki.gentoo.org/wiki/Nfs-utils) | [net-fs/nfs-utils](https://packages.gentoo.org/packages/net-fs/nfs-utils) | File system protocol that allows client machines to access network attached filesystems (called exports) from a host system. | 
| [Samba](https://wiki.gentoo.org/wiki/Samba) | [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba) | Re-implementation of the [SMB/CIFS](https://wiki.gentoo.org/wiki/Smbnetfs) networking protocol, a Microsoft Windows alternative to Network File System ([NFS](https://wiki.gentoo.org/wiki/NFS)). | 

| Name | Package | Description | 
|---|---|---|
| [Dhcpcd-ui](https://wiki.gentoo.org/wiki/Dhcpcd-ui) | [net-misc/dhcpcd-ui](https://packages.gentoo.org/packages/net-misc/dhcpcd-ui) | [Qt](https://wiki.gentoo.org/wiki/Qt) and [GTK](https://wiki.gentoo.org/wiki/GTK) monitor and configuration graphical user interface for [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd). | 
| [Iwd](https://wiki.gentoo.org/wiki/Iwd) | [net-wireless/iwd](https://packages.gentoo.org/packages/net-wireless/iwd) | Up-and-coming wireless daemon for Linux. | 
| [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) | [net-misc/netifrc](https://packages.gentoo.org/packages/net-misc/netifrc) | Gentoo's default framework for configuring and [managing network](https://wiki.gentoo.org/wiki/Network_management) interfaces on systems running [OpenRC](https://wiki.gentoo.org/wiki/OpenRC). | 
| [systemd-networkd](https://wiki.gentoo.org/wiki/Systemd#systemd-networkd) | [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) | Part of systemd useful for simple configuration of wired network interfaces. Follow article for installation method. | 
| [Wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) | [net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant) | Wifi supplicant to handle network authentication. | 

| Name | Package | Description |  | 
|---|---|---|---|
| Celestia | [sci-astronomy/celestia](https://packages.gentoo.org/packages/sci-astronomy/celestia) | Space simulation that lets you explore our universe in three dimensions with a real-time 3D visualization of space. |  | 
| Stellarium | [sci-astronomy/stellarium](https://packages.gentoo.org/packages/sci-astronomy/stellarium) | Open source planetarium for your computer with a realistic sky in real-time 3D, just like what you see with the naked eye, binoculars or a telescope. |  | 

| Name | Package | Description | 
|---|---|---|
| Gwyddion | [sci-visualization/gwyddion](https://packages.gentoo.org/packages/sci-visualization/gwyddion) | View, convert, and analyze scanning microscopy data (e.g. atomic force microscopy). | 
| KLayout | [sci-electronics/klayout](https://packages.gentoo.org/packages/sci-electronics/klayout) | View, edit, and convert electron beam lithography files. | 

| Name | Package | Description | 
|---|---|---|
| KDiff3 | [kde-misc/kdiff3](https://packages.gentoo.org/packages/kde-misc/kdiff3) | [Qt](https://wiki.gentoo.org/wiki/Qt) frontend to [diff3(1)](https://man.archlinux.org/man/diff3.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| Kompare | [kde-apps/kompare](https://packages.gentoo.org/packages/kde-apps/kompare) | [KDE](https://wiki.gentoo.org/wiki/KDE) tool for viewing the differences between files. | 
| Meld | [dev-util/meld](https://packages.gentoo.org/packages/dev-util/meld) | Visual diff and merge tool targeted at developers, under [GNOME](https://wiki.gentoo.org/wiki/GNOME). | 
| Quilt | [dev-util/quilt](https://packages.gentoo.org/packages/dev-util/quilt) | Patch manager to easily manage large numbers of patches by keeping track of the changes each patch makes. | 
| Difftastic | [dev-util/difftastic](https://packages.gentoo.org/packages/dev-util/difftastic) | A structural diff that understands syntax. | 

| Name | Package | Description | 
|---|---|---|
| Bluefish | [app-editors/bluefish](https://packages.gentoo.org/packages/app-editors/bluefish) | [GTK](https://wiki.gentoo.org/wiki/GTK) HTML editor for the experienced web designer or programmer. | 
| Code::Blocks | [dev-util/codeblocks](https://packages.gentoo.org/packages/dev-util/codeblocks) | Open source, cross platform, free C++ IDE. | 
| [Eclipse](https://wiki.gentoo.org/wiki/Eclipse) | *dev-util/eclipse-sdk-bin* from eclipse repository | Java based IDE that can be used for other languages via plugins. Not in the Gentoo repository but can be installed from an overlay according to the the instructions. | 
| Geany | [dev-util/geany](https://packages.gentoo.org/packages/dev-util/geany) | [GTK](https://wiki.gentoo.org/wiki/GTK) based fast and lightweight IDE supporting many different languages. | 
| KDevelop | [dev-util/kdevelop](https://packages.gentoo.org/packages/dev-util/kdevelop) | Supporting KF5/ [Qt](https://wiki.gentoo.org/wiki/Qt), C/C++ and much more. | 
| Qt Creator | [dev-qt/qt-creator](https://packages.gentoo.org/packages/dev-qt/qt-creator) | Lightweight C++/ [Qt](https://wiki.gentoo.org/wiki/Qt) IDE from the Qt Project. | 
| [Vscode](https://wiki.gentoo.org/wiki/Vscode) | [app-editors/vscode](https://packages.gentoo.org/packages/app-editors/vscode) | Visual Studio Code is a lightweight but powerful source code editor. Requires accepting the [Microsoft-vscode software license](https://gitweb.gentoo.org/repo/gentoo.git/tree/licenses/Microsoft-vscode) before installation. | 
| [Vscodium](https://wiki.gentoo.org/wiki/Vscode) | [app-editors/vscodium](https://packages.gentoo.org/packages/app-editors/vscodium) | Free/Libre Open Source Software Binaries of Microsoft's VSCode. | 

| Name | Package | Description | 
|---|---|---|
| Solaar | [app-misc/solaar](https://packages.gentoo.org/packages/app-misc/solaar) | Device Manager for Logitech Unifying devices. | 
| USBView | [app-admin/usbview](https://packages.gentoo.org/packages/app-admin/usbview) | Display the topology of devices on the USB bus. | 

See also the article on [hardware detection](https://wiki.gentoo.org/wiki/Hardware_detection) software.

| Name | Package | Description | 
|---|---|---|
| Baobab | [sys-apps/baobab](https://packages.gentoo.org/packages/sys-apps/baobab) | [GTK](https://wiki.gentoo.org/wiki/GTK) disk usage analyzer. | 
| GParted | [sys-block/gparted](https://packages.gentoo.org/packages/sys-block/gparted) | [GTK](https://wiki.gentoo.org/wiki/GTK) disk partition editor GUI. | 

The applications listed here should be widely useful, and of sufficient quality, to merit inclusion.

If you regularly use a desktop software package from the Gentoo repository and can confirm it is of *excellent quality*, *stable* and of ***broad appeal for common tasks***, please [add it](https://wiki.gentoo.org/wiki/Help:Editing_pages) to the list ! The software should at least be *maintained* (i.e. relatively recent commits to the source; have periodic releases; not have too many reported bugs; most bugs should be getting fixed rather than accumulating, etc.), and preferably be *well documented* and from the stable branch. Please don't use this page just to promote a package because you like it, are an author or have other interest etc.

For software titles that are of *more interest* to users of a specific [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment), please use or create the [meta page](https://wiki.gentoo.org/wiki/Category:Meta) ([blueprint](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Article_Blueprints/Meta)) for that desktop environment (e.g. [Xfce](https://wiki.gentoo.org/wiki/Xfce)).

It is good practice to create a [stub article](https://wiki.gentoo.org/wiki/Help:Starting_a_new_page#Stub_articles) for any package added here that does not have a page already, as anything notable enough to be listed here will also be notable enough to have a dedicated page.

- [Games](https://wiki.gentoo.org/wiki/Games) — a landing page for many of the games (especially open source variants) available in Gentoo's main ebuild repository.
- [Qt Desktop applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) — a list of recommendations for a light-weight, non-[KDE](https://wiki.gentoo.org/wiki/KDE), [Qt](https://wiki.gentoo.org/wiki/Qt)-only desktop environment.
- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator))
- [Terminal productivity software](https://wiki.gentoo.org/wiki/Terminal_productivity_software) — applications designed to run within the constraints of a text-based terminal window that are typically associated with GUI-based office productivity software
- [Wayland Desktop Landscape](https://wiki.gentoo.org/wiki/Wayland_Desktop_Landscape) — various desktop related packages for Wayland

- [linuxlinks software directory](https://www.linuxlinks.com/best-free-open-source-software/) - large, well organized catalogue of software with good descriptions. Software that is not in Portage can sometimes be installed by other means, such as being downloaded and complied after installing the appropriate dependencies.
