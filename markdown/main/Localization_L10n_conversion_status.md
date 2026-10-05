<!-- source: https://wiki.gentoo.org/wiki/Localization/L10n_conversion_status | group: Gentoo Wiki (Main) | wiki-title: Localization/L10n conversion status -->
---
title: Localization/L10n conversion status
url: https://wiki.gentoo.org/wiki/Localization/L10n_conversion_status
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: cfb03968d2b24203
license: CC BY-SA 4.0
---

# Localization/L10n conversion status

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Legend

l10n\_\* cand -- candidate for using l10n\* flags, i.e. used in SRC\_URI, \*DEPEND, or directly by ebuild (but not to set LINGUAS)

LINGUAS cand -- uses linguas\* to set LINGUAS for the build system

Statuses:

1. No|TODO -- need to figure out what to do with the package,
2. Partial|In progress -- bug reported, waiting for commit,
3. Yes|DONE -- necessary l10n\* changes committed / no changes needed.

## Workflow

1. If package uses linguas\*, add it with TODO state.
2. Figure out why package uses linguas\*:
  - if it uses it purely to set LINGUAS for the ebuild system, mark *LINGUAS cand* and *DONE*,
  - if it uses it purely for other purposes, mark *l10n\* cand* and see below,
  - if it uses it both for other purposes and to set LINGUAS, mark both and see below.
3. If you filed a bug, mark *In progress* and add the bug reference to notes.
4. If you committed the change to all versions, mark *DONE*.

## How to update ebuilds

If ebuild is l10n\* candidate only, i.e. uses linguas\* to control downloading stuff, deps, installing stuff outside of LINGUAS build system etc., just replace linguas\* flags with l10n\* flags.

If ebuild uses linguas\* only to control value of LINGUAS, leave the flags as-is and update the status. We'll be dropping explicit linguas\* later in a single commit.

If ebuild does both, copy appropriate linguas\* to l10n\*, and update appropriate parts of ebuild. Leave linguas\* in place to control LINGUAS. We'll drop the extra linguas\* part in one commit later.

## Progress table

| Package | eclass | l10n\_\* cand | LINGUAS cand | status | notes | 
|---|---|---|---|---|---|
| app-accessibility/edbrowse |  |  |  |  |  | 
| app-accessibility/mbrola |  |  |  |  | [bug #586838](https://bugs.gentoo.org/show_bug.cgi?id=586838) | 
| app-admin/packagekit-base |  |  |  |  | old version uses -n $LINGUAS to determine --enable-nls | 
| app-admin/system-config-printer |  |  |  |  | uses -n $LINGUAS to determine --enable-nls | 
| app-backup/kbackup | kde4-base kde4-functions |  |  |  |  | 
| app-backup/luckybackup | l10n |  |  |  | seds qmake project to control installing localizations in /usr/share/${PN}/translations | 
| app-backup/pdumpfs |  |  |  |  |  | 
| app-backup/tsm |  |  |  |  |  | 
| app-cdr/cdemu | l10n |  |  |  | removes installed gettext locales post-install (no build system support) | 
| app-cdr/dvdisaster |  |  |  |  |  | 
| app-cdr/gcdemu | l10n |  |  |  | removes installed gettext locales post-install (no build system support) | 
| app-cdr/k3b | kde4-base kde4-functions |  |  |  |  | 
| app-cdr/k9copy | kde4-base kde4-functions |  |  |  |  | 
| app-cdr/kcdemu | kde4-base kde4-functions |  |  |  |  | 
| app-cdr/recorder |  |  |  |  | seds enabled locales in the Makefile | 
| app-crypt/gpa |  |  |  |  |  | 
| app-crypt/tinyca |  |  |  |  | installs locales (.mo) manually | 
| app-doc/csound-manual |  |  |  |  |  | 
| app-doc/doxygen |  |  |  |  | old version uses LINGUAS to pass custom lang code list to cmake, removed in 1.8.11 downstream | 
| app-doc/gimp-help |  |  |  |  |  | 
| app-doc/kicad-doc |  |  |  |  |  | 
| app-doc/php-docs |  |  |  |  |  | 
| app-editors/focuswriter | l10n |  |  |  |  | 
| app-editors/gummi |  |  |  |  |  | 
| app-editors/mp |  |  |  |  |  | 
| app-editors/qwriter |  |  |  |  |  | 
| app-editors/retext | l10n |  |  |  |  | 
| app-editors/tea | l10n |  |  |  |  | 
| app-editors/wxhexeditor | l10n |  |  |  |  | 
| app-emacs/auto-complete |  |  |  |  |  | 
| app-emacs/cmail |  |  |  |  |  | 
| app-emacs/emacs-w3m |  |  |  |  |  | 
| app-emacs/emacs-wget |  |  |  |  |  | 
| app-emacs/lyskom-elisp-client |  |  |  |  |  | 
| app-emacs/mew |  |  |  |  |  | 
| app-emacs/riece |  |  |  |  |  | 
| app-emacs/semi |  |  |  |  |  | 
| app-emacs/wanderlust |  |  |  |  |  | 
| app-emacs/yatex |  |  |  |  |  | 
| app-emulation/lxd | l10n |  |  |  |  | 
| app-emulation/q4wine | l10n |  |  |  |  | 
| app-emulation/qemu | l10n |  |  |  |  | 
| app-emulation/vboxgtk | l10n |  |  |  |  | 
| app-emulation/virt-manager |  |  |  |  |  | 
| app-emulation/wine | l10n |  |  |  |  | 
| app-i18n/mozc | l10n |  |  |  |  | 
| app-i18n/nkf |  |  |  |  |  | 
| app-i18n/poedit | l10n |  |  |  |  | 
| app-i18n/scim-tables |  |  |  |  |  | 
| app-i18n/tagainijisho |  |  |  |  |  | 
| app-i18n/uim |  |  |  |  | uses l10n\* to pull in fonts | 
| app-misc/brewtarget | l10n |  |  |  |  | 
| app-misc/emelfm2 |  |  |  |  |  | 
| app-misc/sl |  |  |  |  |  | 
| app-misc/specto |  |  |  |  |  | 
| app-misc/subsurface | l10n |  |  |  |  | 
| app-misc/tasque |  |  |  |  |  | 
| app-mobilephone/gammu |  |  |  |  |  | 
| app-mobilephone/gnokii |  |  |  |  |  | 
| app-mobilephone/wammu |  |  |  |  |  | 
| app-office/calligra-l10n | kde4-base kde4-functions |  |  |  |  | 
| app-office/kmymoney | kde4-base kde4-functions |  |  |  |  | 
| app-office/kraft | kde4-base kde4-functions |  |  |  |  | 
| app-office/libreoffice-l10n |  |  |  |  | [bug #587114](https://bugs.gentoo.org/show_bug.cgi?id=587114) | 
| app-office/lyx |  |  |  |  |  | 
| app-office/openoffice-bin |  |  |  |  |  | 
| app-office/scribus |  |  |  |  |  | 
| app-portage/eix | l10n |  |  |  |  | 
| app-portage/elogv |  |  |  |  |  | 
| app-portage/esearch |  |  |  |  |  | 
| app-portage/porthole |  |  |  |  |  | 
| app-text/a2ps |  |  |  |  |  | 
| app-text/acroread |  |  |  |  | uses l10n\* to pull fonts in | 
| app-text/aspell |  |  |  |  | uses l10n\* to pull dicts in [bug #586780](https://bugs.gentoo.org/show_bug.cgi?id=586780) | 
| app-text/cherrytree | l10n |  |  |  |  | 
| app-text/dos2unix | l10n |  |  |  |  | 
| app-text/ghostscript-gpl |  |  |  |  | uses l10n\* to pull fonts in | 
| app-text/goldendict | l10n |  |  |  |  | 
| app-text/hunspell |  |  |  |  | uses l10n\* to pull dicts in [bug #586778](https://bugs.gentoo.org/show_bug.cgi?id=586778) | 
| app-text/iso-codes | l10n |  |  |  | selects \*.po based on LINGUAS | 
| app-text/kding | kde4-base kde4-functions |  |  |  |  | 
| app-text/namazu |  |  |  |  |  | 
| app-text/po4a | l10n |  |  |  |  | 
| app-text/qpdfview | l10n |  |  |  |  | 
| app-text/sdcv | l10n |  |  |  |  | 
| app-text/tesseract |  |  |  |  |  | 
| app-text/texlive |  |  |  |  | metapackage [bug #586774](https://bugs.gentoo.org/show_bug.cgi?id=586774) | 
| app-text/yagf | l10n |  |  |  |  | 
| app-vim/cream |  |  |  |  |  | 
| dev-db/postgresql |  |  |  |  |  | 
| dev-games/mygui |  |  |  |  |  | 
| dev-java/zemberek |  |  |  |  |  | 
| dev-lang/esco |  |  |  |  |  | 
| dev-lang/icc |  |  |  |  |  | 
| dev-lang/ifc |  |  |  |  |  | 
| dev-libs/guiloader |  |  |  |  |  | 
| dev-libs/guiloader-c++ |  |  |  |  |  | 
| dev-libs/pslib |  |  |  |  |  | 
| dev-perl/MIME-Charset |  |  |  |  |  | 
| dev-qt/qt-creator | l10n |  |  |  |  | 
| dev-tex/serienbrief |  |  |  |  |  | 
| dev-util/cppi |  |  |  |  |  | 
| dev-util/crow-designer |  |  |  |  |  | 
| dev-util/debhelper |  |  |  |  |  | 
| dev-util/electron |  |  |  |  |  | 
| dev-util/eric |  |  |  |  |  | 
| dev-util/indent |  |  |  |  |  | 
| dev-util/kdbg | kde4-base kde4-functions |  |  |  |  | 
| dev-util/kdevelop | kde4-base kde4-functions |  |  |  |  | 
| dev-util/kdevelop-php | kde4-base kde4-functions |  |  |  |  | 
| dev-util/kdevelop-php-docs | kde4-base kde4-functions |  |  |  |  | 
| dev-util/kdevelop-python | kde4-base kde4-functions |  |  |  |  | 
| dev-util/kdevelop-qmljs | kde4-base kde4-functions |  |  |  |  | 
| dev-util/kdevplatform | kde4-base kde4-functions |  |  |  |  | 
| dev-util/monkeystudio |  |  |  |  |  | 
| dev-util/netbeans |  |  |  |  | DANGER: do not look into the ebuild, it's crazy | 
| dev-util/universalindentgui |  |  |  |  |  | 
| dev-vcs/bzr | l10n |  |  |  |  | 
| dev-vcs/git | l10n |  |  |  |  | 
| dev-vcs/kdesvn | kde4-base kde4-functions |  |  |  |  | 
| games-action/d1x-rebirth |  |  |  |  |  | 
| games-action/d2x-rebirth |  |  |  |  |  | 
| games-board/knights | kde4-base kde4-functions |  |  |  |  | 
| games-board/xscrabble |  |  |  |  |  | 
| games-emulation/dolphin | l10n |  |  |  |  | 
| games-emulation/pcsx2 | l10n |  |  |  |  | 
| games-fps/quake4-bin |  |  |  |  |  | 
| games-misc/fortune-mod-all |  |  |  |  | metapackage [bug #586786](https://bugs.gentoo.org/show_bug.cgi?id=586786) | 
| games-mud/kmuddy | kde4-base kde4-functions |  |  |  |  | 
| games-puzzle/gottet |  |  |  |  |  | 
| games-puzzle/hexalate |  |  |  |  |  | 
| games-roguelike/hengband |  |  |  |  |  | 
| games-rpg/draci-historie |  |  |  |  |  | 
| games-rpg/drascula |  |  |  |  |  | 
| games-rpg/dreamweb |  |  |  |  |  | 
| games-rpg/lure |  |  |  |  |  | 
| games-rpg/nwn |  |  |  |  |  | 
| games-rpg/nwn-data |  |  |  |  |  | 
| games-rpg/queen |  |  |  |  |  | 
| games-rpg/soltys |  |  |  |  |  | 
| games-rpg/sumwars |  |  |  |  |  | 
| games-server/nwn-ded |  |  |  |  |  | 
| games-strategy/ja2-stracciatella |  |  |  |  |  | 
| gnome-extra/cinnamon-translations | l10n |  |  |  |  | 
| kde-apps/kde-l10n |  |  |  |  |  | 
| kde-apps/kde4-l10n | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/kdepim-l10n | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-accounts-kcm | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-approver | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-auth-handler | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-call-ui | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-common-internals | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-contact-list | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-contact-runner | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-desktop-applets | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-filetransfer-handler | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-kded-module | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-l10n |  |  |  |  |  | 
| kde-apps/ktp-send-file | kde4-base kde4-functions |  |  |  |  | 
| kde-apps/ktp-text-ui | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/about-distro | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/adjustableclock | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/colibri | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/customizable-weather | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/eventlist | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/fancytasks | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/homerun | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/katelatexplugin | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kbiff | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kcm-grub2 | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kcm-ufw | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kde-gtk-config | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kdeconnect | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kdesudo | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kdiff3 | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kfilebox |  |  |  |  |  | 
| kde-misc/kgtk | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kimtoy | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kio-locate | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kio\_gopher | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kover | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/krecipes | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/krename | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/krusader | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kscreen | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/kshutdown | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/milou | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/miniplayer | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/networkmanagement | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/plasma-applet-daisy | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/plasma-mpd-nowplaying | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/plasma-nm | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/plasmoid-workflow | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/quadkonsole | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/quickaccess | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/redshift-plasmoid | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/serverstatuswidget | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/skanlite | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/smooth-tasks | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/steamcompanion | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/takeoff | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/tellico | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/wacomtablet | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/yakuake | kde4-base kde4-functions |  |  |  |  | 
| kde-misc/yawp | kde4-base kde4-functions |  |  |  |  | 
| mail-client/mail-notification |  |  |  |  |  | 
| mail-client/thunderbird | mozlinguas |  |  |  | [bug #587334](https://bugs.gentoo.org/show_bug.cgi?id=587334) | 
| mail-client/thunderbird-bin | mozlinguas |  |  |  | [bug #587334](https://bugs.gentoo.org/show_bug.cgi?id=587334) | 
| mail-client/trojita |  |  |  |  |  | 
| media-fonts/acroread-asianfonts |  |  |  |  |  | 
| media-fonts/infinality-ultimate-meta |  |  |  |  | metapackage [bug #586824](https://bugs.gentoo.org/show_bug.cgi?id=586824) | 
| media-fonts/source-han-sans |  |  |  |  |  | 
| media-gfx/birdfont | l10n |  |  |  |  | 
| media-gfx/comix | l10n |  |  |  |  | 
| media-gfx/converseen |  |  |  |  |  | 
| media-gfx/darktable |  |  |  |  |  | 
| media-gfx/dcraw |  |  |  |  |  | 
| media-gfx/digikam | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/exiv2 |  |  |  |  |  | 
| media-gfx/gimp |  |  |  |  |  | 
| media-gfx/hugin |  |  |  |  |  | 
| media-gfx/iscan |  |  |  |  |  | 
| media-gfx/iscan-plugin-gt-f720 |  |  |  |  |  | 
| media-gfx/kcoloredit | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kfax | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kgrab | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kgraphviewer | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kiconedit | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kphotoalbum | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kpovmodeler | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kuickshow | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/kxstitch | kde4-base kde4-functions |  |  |  |  | 
| media-gfx/llgal |  |  |  |  |  | 
| media-gfx/luminance-hdr |  |  |  |  |  | 
| media-gfx/mcomix | l10n |  |  |  |  | 
| media-gfx/mypaint |  |  |  |  |  | 
| media-gfx/qiviewer | l10n |  |  |  |  | 
| media-gfx/shotwell |  |  |  |  |  | 
| media-gfx/smile |  |  |  |  |  | 
| media-gfx/valentina |  |  |  |  |  | 
| media-libs/stimg |  |  |  |  |  | 
| media-plugins/kipi-plugins | kde4-base kde4-functions |  |  |  |  | 
| media-plugins/vdr-epgsearch |  |  |  |  |  | 
| media-radio/radiotray |  |  |  |  |  | 
| media-sound/amarok | kde4-base kde4-functions |  |  |  |  | 
| media-sound/audex | kde4-base kde4-functions |  |  |  |  | 
| media-sound/bempc |  |  |  |  |  | 
| media-sound/cantata | l10n |  |  |  |  | 
| media-sound/clementine |  |  |  |  |  | 
| media-sound/csound |  |  |  |  |  | 
| media-sound/flacon | l10n |  |  |  |  | 
| media-sound/gbsplay | l10n |  |  |  |  | 
| media-sound/gnac |  |  |  |  |  | 
| media-sound/guayadeque |  |  |  |  |  | 
| media-sound/herrie |  |  |  |  |  | 
| media-sound/kaudiocreator | kde4-base kde4-functions |  |  |  |  | 
| media-sound/kid3 | kde4-base kde4-functions |  |  |  |  | 
| media-sound/kmid | kde4-base kde4-functions |  |  |  |  | 
| media-sound/kmidimon | kde4-base kde4-functions |  |  |  |  | 
| media-sound/kradio | kde4-base kde4-functions |  |  |  |  | 
| media-sound/kwave | kde4-base kde4-functions |  |  |  |  | 
| media-sound/lilypond |  |  |  |  |  | 
| media-sound/mp3c |  |  |  |  |  | 
| media-sound/qastools | l10n |  |  |  |  | 
| media-sound/qsynth |  |  |  |  |  | 
| media-sound/retrovol | l10n |  |  |  |  | 
| media-sound/soundkonverter | kde4-base kde4-functions |  |  |  |  | 
| media-video/2mandvd |  |  |  |  |  | 
| media-video/aegisub | l10n |  |  |  |  | 
| media-video/arista | l10n |  |  |  |  | 
| media-video/avidemux | l10n |  |  |  |  | 
| media-video/bino |  |  |  |  |  | 
| media-video/gxine |  |  |  |  |  | 
| media-video/imagination |  |  |  |  |  | 
| media-video/kaffeine | kde4-base kde4-functions |  |  |  |  | 
| media-video/kamerka | kde4-base kde4-functions |  |  |  |  | 
| media-video/kmplayer | kde4-base kde4-functions |  |  |  |  | 
| media-video/kplayer | kde4-base kde4-functions |  |  |  |  | 
| media-video/loopy | kde4-base kde4-functions |  |  |  |  | 
| media-video/makemkv | l10n |  |  |  |  | 
| media-video/minitube | l10n |  |  |  |  | 
| media-video/plasma-mediacenter | kde4-base kde4-functions |  |  |  |  | 
| media-video/smplayer | l10n |  |  |  |  | 
| media-video/smtube | l10n |  |  |  |  | 
| media-video/subtitlecomposer | kde4-base kde4-functions |  |  |  |  | 
| media-video/tsmuxer |  |  |  |  |  | 
| media-video/xvideoservicethief |  |  |  |  |  | 
| net-analyzer/httping |  |  |  |  |  | 
| net-analyzer/nmap |  |  |  |  |  | 
| net-analyzer/nmapsi | l10n |  |  |  |  | 
| net-dialup/gtkterm |  |  |  |  |  | 
| net-dialup/minicom |  |  |  |  |  | 
| net-dns/dnsmasq |  |  |  |  |  | 
| net-dns/namecoin-qt | kde4-functions |  |  |  |  | 
| net-ftp/lftp |  |  |  |  |  | 
| net-ftp/proftpd |  |  |  |  |  | 
| net-im/choqok | kde4-base kde4-functions |  |  |  |  | 
| net-im/licq |  |  |  |  |  | 
| net-im/mcabber |  |  |  |  |  | 
| net-im/psi | l10n |  |  |  |  | 
| net-im/qutim |  |  |  |  |  | 
| net-im/vacuum |  |  |  |  |  | 
| net-irc/iroffer-dinoex | l10n |  |  |  |  | 
| net-irc/rbot |  |  |  |  |  | 
| net-irc/weechat |  |  |  |  |  | 
| net-libs/gnutls |  |  |  |  |  | 
| net-libs/libkfbapi | kde4-base kde4-functions |  |  |  |  | 
| net-libs/libkpeople | kde4-base kde4-functions |  |  |  |  | 
| net-libs/libktorrent | kde4-base kde4-functions |  |  |  |  | 
| net-libs/libkvkontakte | kde4-base kde4-functions |  |  |  |  | 
| net-libs/neon |  |  |  |  |  | 
| net-misc/asterisk-core-sounds |  |  |  |  |  | 
| net-misc/asterisk-extra-sounds |  |  |  |  |  | 
| net-misc/electrum |  |  |  |  |  | 
| net-misc/icaclient |  |  |  |  |  | 
| net-misc/knemo | kde4-base kde4-functions |  |  |  |  | 
| net-misc/knutclient | kde4-base kde4-functions |  |  |  |  | 
| net-misc/kvpnc | kde4-base kde4-functions |  |  |  |  | 
| net-misc/nut-monitor |  |  |  |  |  | 
| net-misc/openconnect |  |  |  |  |  | 
| net-misc/smb4k | kde4-base kde4-functions |  |  |  |  | 
| net-news/quiterss | l10n |  |  |  |  | 
| net-nntp/kwooty | kde4-base kde4-functions |  |  |  |  | 
| net-p2p/bitcoin-qt | kde4-functions |  |  |  |  | 
| net-p2p/bitcoinxt-qt | kde4-functions |  |  |  |  | 
| net-p2p/classified-ads | l10n |  |  |  |  | 
| net-p2p/deluge | l10n |  |  |  |  | 
| net-p2p/dogecoin-qt | kde4-functions |  |  |  |  | 
| net-p2p/eiskaltdcpp | l10n |  |  |  |  | 
| net-p2p/ktorrent | kde4-base kde4-functions |  |  |  |  | 
| net-p2p/litecoin-qt | kde4-functions |  |  |  |  | 
| net-p2p/ppcoin-qt | kde4-functions |  |  |  |  | 
| net-p2p/primecoin-qt | kde4-functions |  |  |  |  | 
| net-print/adobeps |  |  |  |  |  | 
| net-print/cups |  |  |  |  |  | 
| net-print/kyocera-mita-ppds |  |  |  |  |  | 
| net-print/pnm2ppa |  |  |  |  |  | 
| net-voip/gnugk |  |  |  |  |  | 
| net-voip/linphone |  |  |  |  |  | 
| net-wireless/bluedevil | kde4-base kde4-functions |  |  |  |  | 
| net-wireless/wireless-tools |  |  |  |  |  | 
| sci-astronomy/stellarium |  |  |  |  |  | 
| sci-calculators/keurocalc | kde4-base kde4-functions |  |  |  |  | 
| sci-calculators/speedcrunch | l10n |  |  |  |  | 
| sci-chemistry/gperiodic |  |  |  |  |  | 
| sci-chemistry/massxpert |  |  |  |  |  | 
| sci-electronics/eagle |  |  |  |  |  | 
| sci-electronics/kicad |  |  |  |  |  | 
| sci-electronics/splat |  |  |  |  |  | 
| sci-geosciences/merkaartor | l10n |  |  |  |  | 
| sci-libs/mathgl |  |  |  |  |  | 
| sci-mathematics/maxima |  |  |  |  |  | 
| sci-mathematics/rkward | kde4-base kde4-functions |  |  |  |  | 
| sci-physics/lightspeed |  |  |  |  |  | 
| sci-visualization/qtiplot |  |  |  |  |  | 
| sci-visualization/xyscan |  |  |  |  |  | 
| sci-visualization/zhu3d |  |  |  |  |  | 
| sys-apps/bleachbit | l10n |  |  |  |  | 
| sys-apps/groff |  |  |  |  | old version only [bug #586848](https://bugs.gentoo.org/show_bug.cgi?id=586848) | 
| sys-apps/lshw | l10n |  |  |  |  | 
| sys-apps/man-pages |  |  |  |  | [bug #586748](https://bugs.gentoo.org/show_bug.cgi?id=586748) | 
| sys-apps/portage |  |  |  |  |  | 
| sys-apps/pv |  |  |  |  |  | 
| sys-apps/shadow |  |  |  |  |  | 
| sys-auth/polkit-kde-agent | kde4-base kde4-functions |  |  |  |  | 
| sys-block/ms-sys |  |  |  |  |  | 
| sys-boot/unetbootin |  |  |  |  |  | 
| sys-fs/mhddfs |  |  |  |  |  | 
| sys-process/fcron |  |  |  |  |  | 
| www-apps/liquid\_feedback\_frontend |  |  |  |  | old version only | 
| www-apps/mod\_survey |  |  |  |  |  | 
| www-client/chromium |  |  |  |  |  | 
| www-client/firefox | mozlinguas |  |  |  | [bug #587334](https://bugs.gentoo.org/show_bug.cgi?id=587334) | 
| www-client/firefox-bin | mozlinguas |  |  |  | [bug #587334](https://bugs.gentoo.org/show_bug.cgi?id=587334) | 
| www-client/google-chrome |  |  |  |  |  | 
| www-client/google-chrome-beta |  |  |  |  |  | 
| www-client/google-chrome-unstable |  |  |  |  |  | 
| www-client/opera |  |  |  |  |  | 
| www-client/opera-beta |  |  |  |  |  | 
| www-client/opera-developer |  |  |  |  |  | 
| www-client/qupzilla | l10n |  |  |  |  | 
| www-client/rekonq | kde4-base kde4-functions |  |  |  |  | 
| www-client/seamonkey | mozlinguas |  |  |  | [bug #587334](https://bugs.gentoo.org/show_bug.cgi?id=587334) | 
| www-client/seamonkey-bin | mozlinguas |  |  |  | [bug #587334](https://bugs.gentoo.org/show_bug.cgi?id=587334) | 
| www-client/uget |  |  |  |  |  | 
| www-client/vivaldi |  |  |  |  |  | 
| www-client/w3m |  |  |  |  |  | 
| www-plugins/google-talkplugin |  |  |  |  |  | 
| x11-libs/gtk+ |  |  |  |  |  | 
| x11-misc/devilspie2 | l10n |  |  |  |  | 
| x11-misc/kdocker |  |  |  |  |  | 
| x11-misc/lightdm-kde | kde4-base kde4-functions |  |  |  |  | 
| x11-misc/pcmanfm | l10n |  |  |  |  | 
| x11-misc/qcomicbook | l10n |  |  |  |  | 
| x11-misc/qlipper | l10n |  |  |  |  | 
| x11-misc/tinymount | l10n |  |  |  |  | 
| x11-misc/treeline |  |  |  |  | old version only | 
| x11-misc/vym |  |  |  |  |  | 
| x11-misc/xfe | l10n |  |  |  |  | 
| x11-plugins/pidgin-privacy-please |  |  |  |  |  | 
| x11-terms/mrxvt |  |  |  |  |  | 
| x11-themes/nitrogen | kde4-base kde4-functions |  |  |  |  |
