<!-- source: https://wiki.gentoo.org/wiki/Dropbox | group: Gentoo Wiki (Main) | wiki-title: Dropbox -->
---
title: Dropbox
url: https://wiki.gentoo.org/wiki/Dropbox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-26"
fingerprint: cb241f5b2ef701d7
license: CC BY-SA 4.0
---

# Dropbox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Dropbox** is a closed source file synchronization and cloud service utility.

## Installation

### USE flags


### Emerge

Dropbox uses the CC BY-ND 3.0 license, which must be accepted before merging the package. This license is not generally considered to be free<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, and more information can be found on the [Creative Commons website](https://creativecommons.org/licenses/by-nd/3.0/).

If /etc/portage/package.license is a file, add the following line:

**`/etc/portage/package.license`**

Alternatively, if /etc/portage/package.license/ is a directory, create the file /etc/portage/package.license/dropbox, and add the line there.

Emerge the package:

`root #``emerge --ask net-misc/dropbox`
As per the post-install message from the dropbox ebuild, it is recommended to run the following command to prevent dropbox from autoupdating (which may break the tray icon):

`user $``install -dm0 ~/.dropbox-dist`
## Configuration

Set the `DROPBOX_USERS` variable to the regular user name in /etc/conf.d/dropbox:

**`/etc/conf.d/dropbox`**

To start Dropbox, run:

`user $``dropbox start`
This will launch the dropbox login page in a web browser, where credentials can be entered to connect an account. It will create these directories in the user's home directory:

- $HOME/Dropbox/
- $HOME/.dropbox-dist/
- $HOME/.dropbox/

### OpenRC

On OpenRC installations, Dropbox can be added to the default runlevel:

`root #``rc-update add dropbox default`
### Systemd

**2026-07-26**, the information in this section is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=Dropbox&action=edit).

On systemd, it is necessary to pass a username as the instance identifier to enable it:

`root #``systemctl enable dropbox@larry`
To display a tray-icon, it is necessary to define which display to use:

`root #``systemctl edit dropbox@larry`
### KDE

#### Dolphin file manager

Dropbox integration with Dolphin is provided by [kde-apps/dolphin-plugins-dropbox](https://packages.gentoo.org/packages/kde-apps/dolphin-plugins-dropbox). This can be installed by running:

`root #``emerge --ask kde-apps/dolphin-plugins-dropbox`
## Troubleshooting

### libappindicator warning message

Dropbox may show a warning message indicating that libappindicator cannot be found. This means that Dropbox auto-updated itself and broke the libappindicator symlink. This is explained in the following post-install message:

To resolve this, reinstall [net-misc/dropbox](https://packages.gentoo.org/packages/net-misc/dropbox), and then follow the above instructions.

### Outdated dropbox version

Dropbox may report that an outdated version is installed. If this is an issue, a newer version can be installed by unmasking `~amd64` for the [net-misc/dropbox](https://packages.gentoo.org/packages/net-misc/dropbox) package. This can be done by adding the following to /etc/portage/package.accept\_keywords (or in a separate file if this location is a directory):

**`/etc/portage/package.accept_keywords`**

## See also

- [SparkleShare](https://wiki.gentoo.org/wiki/SparkleShare) — a cross platform, free, open source, Dropbox-like, git-based collaboration and file sharing tool.
- [Owncloud](https://wiki.gentoo.org/wiki/Owncloud) — a free, open source, Dropbox-like file synchronization and cloud service.
