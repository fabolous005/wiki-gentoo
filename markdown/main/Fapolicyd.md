<!-- source: https://wiki.gentoo.org/wiki/Fapolicyd | group: Gentoo Wiki (Main) | wiki-title: Fapolicyd -->
---
title: Fapolicyd
url: https://wiki.gentoo.org/wiki/Fapolicyd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-03"
fingerprint: b08dc40a855f231a
license: CC BY-SA 4.0
---

# Fapolicyd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

fapolicyd is a simple application whitelisting daemon for Linux. fapolicyd provides a software framework that controls the execution of applications based on a user-defined policy<sup>[\[1\]](https://wiki.gentoo.org#cite_note-Red_Hat-1)</sup>. It is one of the most efficient ways to prevent running untrusted and possibly malicious applications on the system.

### Installation

`root #``emerge --ask sys-apps/fapolicyd`
### Components

fapolicyd provides the following components:

- The **fapolicyd service** fapolicyd
- **fapolicyd command-line utility** fapolicyd-cli
- **fapolicyd package manager plugins** ebuilddb, rpmdb, debdb
- **fapolicyd rule language** see /usr/share/fapolicyd/sample-rules/README-rules
- **fagenrules script** - Run to generate and update rules from fragment files.

### Configuration

The fapolicyd service configuration is located in /etc/fapolicyd/. The structure is as follows:

- /etc/fapolicyd/fapolicyd.trust: contains a list of manually trusted files (the file database).
- /etc/fapolicyd/rules.d/: directory for files containing allow and deny execution rules.
- /etc/fapolicyd.conf: fapolicy daemon configuration options.

### Policy

The policy is evaluated from top to bottom with the first match winning. The current design for policy is that it is split up into units of rules that are designed to work together\[^2^\]\[2\]. They are copied into /etc/fapolicyd/rules.d/. When the service starts, the systemd service file runs fagenrules which assembles the units of rules into a comprehensive policy<sup>[\[2\]](https://wiki.gentoo.org#cite_note-fapolicyd_repository-2)</sup>.

### Trust

The fapolicyd framework introduces the concept of trust. An application is trusted when it is properly installed by the system package manager, and therefore it is registered in the system package manager database<sup>[\[1\]](https://wiki.gentoo.org#cite_note-Red_Hat-1)</sup>. The fapolicyd daemon can be configured to use the RPM database, the Deb database, or the Ebuild database as a list of trusted binaries and scripts.
