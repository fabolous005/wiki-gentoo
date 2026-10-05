<!-- source: https://wiki.gentoo.org/wiki/Skype | group: Gentoo Wiki (Main) | wiki-title: Skype -->
---
title: Skype
url: https://wiki.gentoo.org/wiki/Skype
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-08"
fingerprint: b6613c5189eff9de
license: CC BY-SA 4.0
---

# Skype

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of

**2025-08-07**, this article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**Skype** is a proprietary application owned by Microsoft for instant messaging, VoIP calls, and video conversations.

## Installation

### Kernel config



### USE flags

*net-im/skypeforlinux*correct?

### Emerge

Install [net-im/skypeforlinux](https://packages.gentoo.org/packages/net-im/skypeforlinux):

`root #``emerge --ask net-im/skypeforlinux`
### Additional USE flags

The following settings will need to be in /etc/portage/package.accept\_keywords/skypeforlinux or some equivalent.

`root #``echo "=net-im/skypeforlinux-8.45.0.41 ~amd64" >> /etc/portage/package.accept_keywords/skypeforlinux`
Edit your pinentry usage in this way.

`root #``echo ">=app-crypt/pinentry-1.0.0-r2 gnome-keyring" >> /etc/portage/package.use/pinentry`
and then just install the package.

`root #``emerge skypeforlinux`
## Troubleshooting

### Auto-launching at KDE login

For some reason, [KDE](https://wiki.gentoo.org/wiki/KDE) cannot remember Skype in the session it saves upon log out. This workaround will fool it into saving Skype in the session.

**`~/.kde4/Autostart/skype.sh`**

```
#!/bin/sh
skypeforlinux
```
To make the Skype main window (but not chat windows) go to a desktop or any other features of Window Rules with KDE, use this regular expression under Window Title in the settings window:

### No video (despite video drivers being installed/loaded)

#### Permissions

It is possible, that your user does not have privileges to access video device. Example privileges:

`user $``ls -al /dev/video0`
crw-rw----+ 1 root video 81, 0 Nov 25 22:03 /dev/video0

Add each Skype user to the video group:

`root #``gpasswd -a larry video`
Where larry is substituted for the appropriate username.
