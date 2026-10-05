<!-- source: https://wiki.gentoo.org/wiki/Ratbagd | group: Gentoo Wiki (Main) | wiki-title: Ratbagd -->
---
title: libratbag
url: https://wiki.gentoo.org/wiki/Ratbagd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-01"
fingerprint: c640dad3df1339ef
license: CC BY-SA 4.0
---

# libratbag

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



libratbag provides ratbagd, a DBus daemon to configure input devices, mainly gaming mice. The daemon provides a generic way to access the various features exposed by these mice and abstracts away hardware-specific and kernel-specific quirks.

## Installation

### USE flags


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask dev-libs/libratbag`
### Service

#### systemd

To start ratbagd on boot:

`root #``systemctl enable --now ratbagd`
For non root access to [ratbagd] user should be in group `plugdev`

`root #``usermod -aG plugdev larry`
## Testing

You can test that ratbagd is running and can see your devices by using the \`ratbagctl\` command:

`root #``ratbagctl list`
Running this command as your user will ensure that your user is in the correct groups.
