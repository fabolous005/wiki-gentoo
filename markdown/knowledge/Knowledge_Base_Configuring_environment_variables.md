<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Configuring_environment_variables | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Configuring_environment_variables -->
---
title: Knowledge Base:Configuring environment variables
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Configuring_environment_variables
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: b8df3ac84eb73bdf
license: CC BY-SA 4.0
---

# Knowledge Base:Configuring environment variables

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

This document provides methods to initialize environment variables and run shell code at login.

## Environment

This information applies to users of [emerge](https://wiki.gentoo.org/wiki/Emerge), [bash](https://wiki.gentoo.org/wiki/Bash), [X](https://wiki.gentoo.org/wiki/X), [GNOME](https://wiki.gentoo.org/wiki/GNOME), and [Plasma](https://wiki.gentoo.org/wiki/Plasma).

## Analysis

Different shells, window servers, and compositors have different startup script locations.

## Resolution

To add environment variables to a startup script, open it with a text editor and add a key-value pair for each environment variable. [bash](https://wiki.gentoo.org/wiki/Bash) shell code is also allowed. For example, adding the following sets `MAKEOPTS` variable to `--jobs 12` when [emerge](https://wiki.gentoo.org/wiki/Emerge) starts:

**`/etc/portage/make.conf`**

```
MAKEOPTS="--jobs 12"
```
The following sections cover the startup script locations for different pieces of software.

- Global
- Files in [/etc/env.d/](https://wiki.gentoo.org/wiki//etc/env.d) (see [the Handbook](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/EnvVar#The_env.d_directory)).
- Emerge
- /etc/portage/make.conf.
- Emerge (per-package)
- Files in /etc/portage/env/.
- [bash](https://wiki.gentoo.org/wiki/Bash)
- $HOME/.bashrc.
- [X](https://wiki.gentoo.org/wiki/X) window managers and desktop environments
- $HOME/.xinitrc.
- [GNOME](https://wiki.gentoo.org/wiki/GNOME) on X and [Wayland](https://wiki.gentoo.org/wiki/Wayland)
- $HOME/.pam\_environment.
- [Plasma](https://wiki.gentoo.org/wiki/Plasma) on X and Wayland (pre-startup)
- Files in $HOME/.config/plasma-workspace/env/ with the .sh file extension (for example, /home/larry/.config/plasma-workspace/env/path.sh.
- [Plasma](https://wiki.gentoo.org/wiki/Plasma) 5 on X and Wayland (login)
- System settings → Startup and Shutdown → Autostart → Add... → Add application... (this file must have a [shebang](<https://en.wikipedia.org/wiki/shebang_(Unix)>)[shebang (Unix)](<https://en.wikipedia.org/wiki/shebang_(Unix)>) ).
