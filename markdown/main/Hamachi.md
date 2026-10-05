<!-- source: https://wiki.gentoo.org/wiki/Hamachi | group: Gentoo Wiki (Main) | wiki-title: Hamachi -->
---
title: Hamachi
url: https://wiki.gentoo.org/wiki/Hamachi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-25"
fingerprint: f228ab49291683e7
license: CC BY-SA 4.0
---

# Hamachi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Hamachi is a cross platform VPN tunneling engine.

## Install

### Emerge

`root #``emerge --ask net-vpn/logmein-hamachi`
## Configuration

### Services

#### OpenRC

To immediately start the logmein-hamachi service:

`root #``rc-service logmein-hamachi start`
To have the service start during the default runlevel (system boot), enter:

`root #``rc-update add logmein-hamachi default`
#### Systemd

To start the logmein-hamachi service:

`root #``systemctl start logmein-hamachi`
To start the service automatically at boot level, enter:

`root #``systemctl enable logmein-hamachi`
If made any change to its config, restart it with:

`root #``systemctl restart logmein-hamachi`
### Server

`root #``hamachi login``root #``hamachi set-nick gentoo``root #``hamachi create 12345678`
Set a strong password

Be polite, and delete your network when you're done.

`root #``hamachi delete 12345678``root #``hamachi set-ip-mode ipv4`
To show the PID, IP, status, client, ID, and nickname:

`root #``hamachi`
### Client

`root #``hamachi login``root #``hamachi set-nick alsogentoo``root #``hamachi do-join 12345678`
Enter the server's strong password.

## Usage

### Invocation

To display all available commands:

`root #``hamachi ?`
