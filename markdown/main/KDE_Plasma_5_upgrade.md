<!-- source: https://wiki.gentoo.org/wiki/KDE/Plasma_5_upgrade | group: Gentoo Wiki (Main) | wiki-title: KDE/Plasma 5 upgrade -->
---
title: KDE/Plasma 5 upgrade
url: https://wiki.gentoo.org/wiki/KDE/Plasma_5_upgrade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-05"
fingerprint: "5a59534eecfdbda6"
license: CC BY-SA 4.0
---

# KDE/Plasma 5 upgrade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


Plasma 5 is the current generation of KDE's desktop environment, based on Qt 5 and KDE Frameworks 5.

## Preparations

### Profile

It is highly recommended to use the Plasma desktop profile as it sets a number of important options to ensure things run smoothly.

In order to choose the most suitable profile, first list what's available:

`root #``eselect profile list`
...
  \[12\]  default/linux/amd64/17.0 (stable)
  \[13\]  default/linux/amd64/17.0/selinux (stable)
  \[14\]  default/linux/amd64/17.0/hardened (stable)
  \[15\]  default/linux/amd64/17.0/hardened/selinux (stable)
  \[16\]  default/linux/amd64/17.0/desktop (stable)
  \[17\]  default/linux/amd64/17.0/desktop/gnome (stable)
  \[18\]  default/linux/amd64/17.0/desktop/gnome/systemd (stable)
  \[19\]  default/linux/amd64/17.0/desktop/plasma (stable)
  \[20\]  default/linux/amd64/17.0/desktop/plasma/systemd (stable)
  ...

Then, select the right profile, substituting the appropriate profile number (which may vary between configurations):

`root #``eselect profile set 8`
### make.conf

The plasma profile now takes care of the right settings for a full-blown KDE Plasma/Applications installation. In the foreseeable future, this entails both Qt 4 as well as Qt 5 based packages. It also handles possible USE flag conflicts on a per-package basis. If there are any existing definitions of `qt4`, `kde` or `plasma` in /etc/portage/make.conf, make sure to remove them before proceeding, else conflicts may arise. Also, **never set** `-minimal` in  /etc/portage/make.conf. In case of USE `handbook`, the recommendation is to either remove it as well, or disable it if the KDE documentation is not needed.

Check for these flags and if necessary alter the file using your favourite editor:

`root #``grep -e "qt[[:digit:]]" -e "handbook" -e "kde" -e "plasma" /etc/portage/make.conf`
You should now also revise your entries in /etc/portage/package.use:

`root #````
grep -e "qt[[:digit:]]$" -e "qt[[:digit:]] " -e "handbook$" -e "handbook " -R /etc/portage/package.use
```
`root #````
grep -e "kde$" -e "kde " -e "plasma$" -e "plasma " -R /etc/portage/package.use
```
Of course you can adjust these flags afterwards again.

### Clean up @world

Now is a good time to clean up [@world](<https://wiki.gentoo.org/wiki/World_set_(Portage)>). If it contains packages either directly from or depending on KDE Workspaces 4, it will lead to unsolvable emerge conflicts later on. The following command will probably take some time, but is not going to remove anything yet - examine the output.

`root #``for x in amarok libkscreen kde-gtk-config ksshaskpass freespacenotifier kcheckpass kcminit kde4-l10n kdebase-cursors kdebase-startkde kdm kephal khotkeys kinfocenter klipper kmenuedit krunner kscreensaver ksmserver ksnapshot ksplash kstartupconfig kstyles ksysguard ksystraycmd kwin kwrited libkgreeter libkworkspace liboxygenstyle libplasmaclock libplasmagenericshell libtaskmanager plasma-workspace powerdevil qguiplatformplugin_kde solid-actions-kcm systemsettings; do equery -q d ${x} | sed -e "s/-[0-9].*//"; done | sed -e "/kdebase-meta/g" -e "/kde-meta/g" | xargs emerge --deselect --pretend`
If you are sure to proceed, run the above command without `--pretend` again. Only the @world file is going to be changed - while the packages stay installed, they will not block a subsequent installation of Plasma 5.

### Apply the changes

Any packages affected by the profile change need to be rebuilt:

`root #``emerge --ask --changed-use --newrepo --deep world`
## Installation

It is now time to head back to [KDE](https://wiki.gentoo.org/wiki/KDE) to proceed with the regular installation of [KDE#Plasma](https://wiki.gentoo.org/wiki/KDE#Plasma) and [KDE#Applications](https://wiki.gentoo.org/wiki/KDE#Applications).

## Migration

### SSH/GPG Agent startup/shutdown scripts

Configuration moved from /etc/kde/{startup,shutdown} in KDE SC 4 to /etc/plasma/{startup,shutdown} for Plasma 5, you will need to edit the scripts there (or copy your scripts from the old location).

## Troubleshooting

The most common issue with upgrading is running into blockers. Unfortunately, some packages just can't be co-installed so they'll need to be removed. Feel free to drop by [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) or [#gentoo-kde](ircs://irc.libera.chat/#gentoo-kde) ([webchat](https://web.libera.chat/#gentoo-kde)) for assistance with resolving any blockers.

If previously [x11-misc/appmenu-qt](https://packages.gentoo.org/packages/x11-misc/appmenu-qt) was installed, application menus (file, edit, view, help, etc) in kdelibs4-based packages might be missing. There is no way to fix this via a GUI, kde rc files will need to be edited to restore the application menu in Plasma 5. Find out which files were modified using the following command:

`user $``grep -iR menubar ~/.kde4`
Change any lines containing `MenuBar`=Disabled`MenuBar`=true`MenuBar` in the rc files are changed - some have more than one, like kmail2.
