<!-- source: https://wiki.gentoo.org/wiki/Qt | group: Gentoo Wiki (Main) | wiki-title: Qt -->
---
title: Qt
url: https://wiki.gentoo.org/wiki/Qt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-05"
fingerprint: afba1fdb0ae733da
license: CC BY-SA 4.0
---

# Qt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Qt** (pronounced as /ˈkjuːt/ "cute") is a cross-platform application framework that is widely used for developing application software with a graphical user interface (GUI) (in which cases Qt is classified as a widget toolkit), and also used for developing non-GUI programs such as command-line tools and consoles for servers<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

The current major release of Qt is Qt6<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

In Gentoo, the ebuilds for Qt and related packages are maintained by [a dedicated team](https://wiki.gentoo.org/wiki/Project:Qt). This team also maintains [the official qt overlay](https://gitweb.gentoo.org/proj/qt.git). They are very open to user contributions and volunteers who want to become new developers.

Most users will not need to install the Qt libraries intentionally, but can just let them be pulled in as dependencies of the applications they want to use. For those who want the complete set (e.g. for software development purposes), there are [sets](https://gitweb.gentoo.org/proj/qt.git/tree/sets) in the overlay for Qt6.

There are currently two Qt-based desktop environments available in Gentoo. The most well-known and best developed one is [KDE Plasma](https://wiki.gentoo.org/wiki/KDE#Plasma). There is also a younger and more "light-weight" DE: [LXQt](https://wiki.gentoo.org/wiki/LXQt) (a merge of Razor-Qt and LXDE). These can be complemented with [KDE Applications](https://wiki.gentoo.org/wiki/KDE#Applications) and packages from the [Qt Desktop applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) list.

Programs that need Qt6 will need its version of qmake. It is located in /usr/lib/qt6/bin and needs to be executed with its full path (i.e., /usr/lib/qt6/bin/qmake). After it's run, compilation will just work.

In the context of Qt, a theme is a particular combination of 'style', 'icon theme', and 'color theme'.

The default *style* of KDE Plasma is Breeze, provided by the [kde-plasma/breeze](https://packages.gentoo.org/packages/kde-plasma/breeze) package; the Breeze icon theme is provided by [kde-frameworks/breeze-icons](https://packages.gentoo.org/packages/kde-frameworks/breeze-icons). A 'Breeze' theme is also available for [GRUB](https://wiki.gentoo.org/wiki/GRUB) ([kde-plasma/breeze-grub](https://packages.gentoo.org/packages/kde-plasma/breeze-grub)) and [Plymouth](https://wiki.gentoo.org/wiki/Plymouth) ([kde-plasma/breeze-plymouth](https://packages.gentoo.org/packages/kde-plasma/breeze-plymouth)).

There doesn't appear to be any [XDG directory](https://wiki.gentoo.org/wiki/XDG_directories) specification for themes in general (although there is a specification for [icon themes](https://specifications.freedesktop.org/icon-theme-spec/icon-theme-spec-latest.html) in particular).  However, common locations for themes include $XDG\_DATA\_HOME/themes/ and \~/.themes/.

The primary way to theme Qt applications in a non-KDE environment is qt6ct, provided by the [gui-apps/qt6ct](https://packages.gentoo.org/packages/gui-apps/qt6ct) package. To use it, ensure that the `QT_QPA_PLATFORMTHEME` is set to `qt6ct` in your environment, e.g. via \~/.bash\_profile:

**`~/.bash_profile`**

```
export QT_QPA_PLATFORMTHEME=qt6ct
```
Themes, as well as things such as preferred fonts, can then be selected by running qt6ct.

By default, chosen settings are saved to the $XDG\_CONFIG\_HOME/qt6ct/ directory (e.g. /home/larry/.config/qt6ct/, which is referenced by Qt applications during startup.

However, it is also possible to directly specify a theme without using qt6ct by setting the `QT_STYLE_OVERRIDE` variable in your environment, e.g.

**`~/.bash_profile`**

```
export QT_STYLE_OVERRIDE=adwaita
```
Additionally, it is possible to pass a style name directly to a Qt application by using the `-style` option. For example, to use the adwaita-dark style ([x11-themes/adwaita-qt](https://packages.gentoo.org/packages/x11-themes/adwaita-qt)) with [Wireshark](https://wiki.gentoo.org/wiki/Wireshark):

`user $``wireshark -style adwaita-dark`
Kvantum ([x11-themes/kvantum](https://packages.gentoo.org/packages/x11-themes/kvantum)) is an SVG-based system that provides a 'kvantum' Qt style instead of a platform theme, and then allowing the selection of specific Kvantum styles via the Kvantum system. It can be used by selecting "kvantum" as the style in qt6ct, or by setting the `QT_STYLE_OVERRIDE` environment variable (described above) to `kvantum`.

Kvantum styles can be previewed and selected by running the kvantummanager program (or simply previewed by running the kvantumpreview program). As of Kvantum 1.1.2, these programs are only built and installed if the `kde` USE flag is enabled on the [x11-themes/kvantum](https://packages.gentoo.org/packages/x11-themes/kvantum) package.

The following table lists some of the Qt-specific environment variables available.

| Variable | Notes | 
|---|---|
| `QT_LOGGING_RULES` | Can be used to help debug issues with Qt applications; refer to [this page on the KDE Community Wiki](https://community.kde.org/Guidelines_and_HOWTOs/Debugging/Using_Error_Messages) for details. | 
| `QT_QPA_PLATFORMTHEME` | Can be used to specify a theme directly, or set to `qt5ct` to utilize qt{5,6}ct to specify a theme. Refer to the "[Qt theming outside of KDE](https://wiki.gentoo.org#Qt_theming_outside_of_KDE)" section, above, for more information. | 
| `QT_SCALE_FACTOR` | Specifies a scaling factor, including for fonts, on High DPI displays. | 
| `QT_SCALE_FACTOR_ROUNDING_POLICY` | Specifies policy for scaling on High DPI displays.  Note also [this comment on how it can affect font rendering](https://bugs.kde.org/show_bug.cgi?id=479891#c27C). | 
| `QT_STYLE_OVERRIDE` | Can be used to directly specify a theme without using qt{5,6}ct. Refer to the " [Qt theming outside of KDE](https://wiki.gentoo.org#Qt_theming_outside_of_KDE)" and "[Kvantum](https://wiki.gentoo.org#Kvantum)" sections, above, for more information. | 

Refer to [Qt/troubleshooting](https://wiki.gentoo.org/wiki/Qt/troubleshooting).
