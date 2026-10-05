<!-- source: https://wiki.gentoo.org/wiki/KDE/Troubleshooting | group: Gentoo Wiki (Main) | wiki-title: KDE/Troubleshooting -->
---
title: KDE/Troubleshooting
url: https://wiki.gentoo.org/wiki/KDE/Troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-03"
fingerprint: "8b4e975a0e2739dc"
license: CC BY-SA 4.0
---

# KDE/Troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article contains various sections to help users of KDE software troubleshoot their systems.

## Rebuilding the application database

If the KMenu lacks any application or the whole application list, the KDE application database probably needs to be rebuilt. This is also a possible fix for any KMenu related issues, like missing icons.

`user $``kbuildsycoca6 --noincremental`
## Akonadi complains about the MySQL config

Start by checking the permissions in /usr/share/config. If they're 700, update them to 755 recursively.

`root #````
chmod -R 755 /usr/share/config
```
If that doesn't solve the error, open the akonadi configuration in \~/.config/akonadi/akonadiserverrc and change the default MySQL config. To use a MySQL server and not the local mysqld executable, make sure that MySQL is running.

## Black screen after login

Make sure \~/.bash\_profile does not have any interactive components like [keychain](https://wiki.gentoo.org/wiki/Keychain). Check \~/.xsession-errors for the prompt for input.

## Screen tearing or flickering when using Radeon graphics drivers

If there is severe flickering or "tearing" when using Radeon based graphics cards, it may be necessary to change the compositor sync settings to something other than the default "Automatic":

## Delayed response of KMenu, krunner, etc.

Packages from the `dev-qt` category provide a `gles2-only` USE flag which in the past has caused this effect. It is not advised to enable it. If for no good reason this flag is found to be enabled for `dev-qt`, [kde-frameworks/plasma](https://packages.gentoo.org/packages/kde-frameworks/plasma) or [kde-plasma/kwin](https://packages.gentoo.org/packages/kde-plasma/kwin), then remove all occurrences of this flag and rebuild affected packages.

Make sure that [kde-plasma/powerdevil](https://packages.gentoo.org/packages/kde-plasma/powerdevil) and [sys-power/upower](https://packages.gentoo.org/packages/sys-power/upower) are installed. Also check that the user is in the users group.

## KDE Plasma high CPU usage

If you are noticing relatively high CPU usage (normally the dbus-daemon or kwin\_x11 processes) when running KDE Plasma make sure to check the syslog for errors that look like the following. Normally just tailing the log will enable you to see this right away since the error is thrown at such a high rate.

**`/var/log/syslog`**

```
 17 00:30:26 localhost obexd[32399]: obex_server_init failed 
Oct 17 00:30:26 localhost obexd[32401]: OBEX daemon 5.39 
Oct 17 00:30:26 localhost obexd[32401]: obex_server_init failed 
Oct 17 00:30:26 localhost obexd[32403]: OBEX daemon 5.39
```

This occurs due to being unable to connect to the bluetooth service, you can ensure this is started by running /etc/init.d/bluetooth start on OpenRC systems. To ensure this does not happen on any other start run the following.

`root #``rc-update add bluetooth`
Alternatively bluetooth can be disabled via the GUI.

## Compilation failure

[dev-qt/qtwebkit](https://packages.gentoo.org/packages/dev-qt/qtwebkit) is one of the few packages known to consistently fail when the -j value on `MAKEOPTS` is set too high.

If you see mysterious build failure, try lowering your -j value. The safe value would be the number of processor times thread (**not** that plus one).

Similar case has been found when compiling with -j option while KDE Plasma is running (observed with [dev-qt/qtwebkit](https://packages.gentoo.org/packages/dev-qt/qtwebkit) and [dev-qt/qtwebengine](https://packages.gentoo.org/packages/dev-qt/qtwebengine)). The build failure would be accompanied with desktop program lagging (or crashing). If this happen, you might want to consider compiling under TTY.

In other case when you see out-of-memory failure, you may want to get rid of pipe on `CFLAGS`.

## Plasma Browser Integration not working in Firefox

For the Plasma Browser Integration to work, not only must [kde-plasma/plasma-browser-integration](https://packages.gentoo.org/packages/kde-plasma/plasma-browser-integration) and the [browser extension](https://addons.mozilla.org/en/firefox/addon/plasma-integration/) be installed, but also the **browser history** has to be **enabled**.

## Device permissions issues and missing shutdown/reboot options

When experiencing authorization or permissions issues in an OpenRC profile make sure that [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) is present, [properly configured](https://wiki.gentoo.org/wiki/Elogind) and `elogind` USE flag is globally enabled.

### Missing suspend or hibernate options

Beyond that, suspend and hibernate options depend on that support being enabled in the kernel, see also: [Suspend and hibernate](https://wiki.gentoo.org/wiki/Suspend_and_hibernate).

## Can't unmount /home

If an error like this appears:

*   Unmounting /home ...
*   in use but fuser finds nothing  [ !! ]

Reinstalling [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta) without [kde-plasma/plasma-vault](https://packages.gentoo.org/packages/kde-plasma/plasma-vault) may help.

**`/etc/portage/package.use`**

## Pinentry GPG dialogue for KDE Plasma isn't working

For example when using KMail to sign emails with PGP, the private key needs to be decrypted. If this key has a password, a Pinentry dialogue tries to open. To enable the Qt version, these configuration files need to be edited.

**`~/.gnupg/gpg.conf`**

```
# !! Remove this line from file if it exists:
# pinentry-mode loopback
```
**`~/.gnupg/gpg-agent.conf`**

```
 /usr/bin/pinentry-qt
```
## zkde\_screencast\_unstable\_v1 does not seem to be available when trying to screencast on Wayland

Make sure to install [kde-plasma/kwin](https://packages.gentoo.org/packages/kde-plasma/kwin) with the `screencast` USE flag.

## Wrong theme applied to KDE apps outside Plasma

If [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta) is installed, the environment variable `QT_QPA_PLATFORMTHEME` should be set to `kde`.
If it is not, [gui-apps/qt6ct](https://packages.gentoo.org/packages/gui-apps/qt6ct) should be installed and `QT_QPA_PLATFORMTHEME` set to `qt6ct`.

## Blurry fonts on Plasma 6.x with Wayland and Nvidia

Many users are reporting blurry fonts with Plasma 6.x in Wayland with Nvidia drivers. The issue has more implications regarding how Plasma KDE is rendering fonts via the graphical acceleration, and certain other conditions. For example nouveau users do not report this issue.
The solution for blurry fonts with Wayland and nvidia drivers are editing `/etc/environment` or any other \*profile, and add the line:

FREETYPE_PROPERTIES="cff:no-stem-darkening=0 autofitter:no-stem-darkening=0"

## Plasma: Cursor "works", but black screen

One cause could be that KWin is installed, but other Plasma packages forming the desktop, wallpaper, etc. are not. This can occur if the KDE global USE flag is set in make.conf, but one is not using a KDE desktop profile and has mistakenly not yet emerged plasma-meta, in which case KWin can be installed standalone and will seemingly log in without error leading to a black screen with a cursor.

By pressing Alt + F2 from the black screen, one should be able to access KRunner and launch a working terminal emulator if one is emerged, even with minimal KDE packages installed. Then further diagnosis can be done.

Start by confirming Plasma is emerged:

`root #``emerge --ask --noreplace kde-plasma/plasma-meta`
If using OpenRC, also confirm that elogind is emerged and set to runlevel boot:

`root #``emerge --ask --noreplace elogind``root #``rc-update add elogind boot`
If there are still issues, check if BASH is the default shell for the current user encountering this issue. Remove from the file `~/.bashrc` the following lines if found:

exec fish

exec zsh

If not caused by the above system configuration issues, the remaining culprit is probably `xwayland` segfaulting related to NVIDIA hardware and Plasma Wayland. It is recommended to confirm that the `xwayland` package is built with sane CFLAGS. Possible culprits (until someone make a regression and report it to [https://bugs.gentoo.org](https://bugs.gentoo.org) ) : `-fdevirtualize-at-ltrans -fno-semantic-interposition -fipa-pta`.

Running this command from a terminal emulator may provide a fix for the issue:

`user $``kstart5 plasmashell`
## Plasma Wayland AMDGPU: Fullscreen 3D gameplay under Proton freezes unless a window or panel overlaps the screen

This behaviour can be caused by direct scanout. If the game works in a window, or with another window layered over the top. Adding `KWIN_DRM_NO_DIRECT_SCANOUT=1` to the environment before the Plasma session starts may resolve this issue.

This can be done in the /etc/environment file.

**`/etc/environment`**

```
KWIN_DRM_NO_DIRECT_SCANOUT=1
```
## Glitchy/unresponsive TTY2 after emerging Plasma

If using NVIDIA graphics and no display manager (observed on Optimus configuration with NVIDIA RTX 3060 Laptop/Intel 12th gen), it is possible after emerging Plasma that TTY2 will become unresponsive (dropping key presses, freezing, showing loading screens over the tty, etc.). This is most likely caused by Plymouth incompatibility with NVIDIA proprietary drivers; if Plymouth is pulled in as a dependency, it takes over display during boot on TTY2 without any further configuration from the user simply by being emerged.

To fix, first disable the Plymouth USE flag on plasma-meta:

**`/etc/portage/package.use/plasma`**

Alternatively, one can apply the -plymouth USE flag globally through make.conf:

**`/etc/portage/make.conf`**

Also ensure that Plymouth is not selected in the world set:

`root #``emerge --ask --deselect sys-boot/plymouth`
To be safe, one can add Plymouth to package.mask to avoid accidental future installs. This will give an error when trying to emerge packages if sys-boot/plymouth would be pulled in:

**`/etc/portage/package.mask/plymouth`**

Then, do a full system update to prepare for a depclean:

`root #``emerge --ask --verbose --update --deep --newuse @world`
If sys-boot/plymouth was added to package.mask and emerge complains about sys-boot/plymouth being masked, one will have to find what other installed programs depend on Plymouth and modify their corresponding USE flags, then reattempt an update until it succeeds.

Then, depclean to remove sys-boot/plymouth.

`root #``emerge --ask --depclean`
Once Plymouth is uninstalled and the new USE flags are applied, reboot the system and TTY2 should work again.

## Plasmashell crashed with "Too many open files" error

On Wayland systems with [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) plasmashell keeps periodically crashing with "Too many open files" error. lsof indicates growing open file descriptors for plasmashell on hovering cursor on widgets:

`user $``lsof -p $(pidof plasmashell) | grep sync_file | wc -l`
1012

There issue with NVIDIA's drivers and [gui-libs/egl-wayland](https://packages.gentoo.org/packages/gui-libs/egl-wayland). Update [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) to >=590.48.01 which uses new [gui-libs/egl-wayland2](https://packages.gentoo.org/packages/gui-libs/egl-wayland2) where issue is fixed<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.
