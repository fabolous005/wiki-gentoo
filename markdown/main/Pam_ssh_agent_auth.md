<!-- source: https://wiki.gentoo.org/wiki/Pam_ssh_agent_auth | group: Gentoo Wiki (Main) | wiki-title: Pam ssh agent auth -->
---
title: Pam ssh agent auth
url: https://wiki.gentoo.org/wiki/Pam_ssh_agent_auth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-13"
fingerprint: a209415819a119b9
license: CC BY-SA 4.0
---

# Pam ssh agent auth

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**pam\_ssh\_agent\_auth** is the [PAM](https://wiki.gentoo.org/wiki/PAM) module that allows a locally installed [SSH](https://wiki.gentoo.org/wiki/SSH) key to authenticate for [sudo](https://wiki.gentoo.org/wiki/Sudo).

This is useful for those who are not happy with completely passwordless sudo, but do not want to be frequently typing passwords.

## Installation

### Emerge

`root #``emerge --ask pam_ssh_agent_auth`
## Configuration

### Create SSH keys

Have each user that would like this capability to follow the guide on the [SSH page](https://wiki.gentoo.org/wiki/SSH#Create_keys) to create SSH keys.

### PAM sudo file

Configure sudo to try using public keys, then fall back to normal password authentication:

**`/etc/pam.d/sudo`**

Configure sudoers to preserve the environment variable `SSH_AUTH_SOCK`:

**`/etc/sudoers`**

### Add desired user's public key

Repeat this process for each user desired for sudo authentication:

`root #``cat /home/<user>/.ssh/*.pub >> /etc/ssh/sudo_authorized_keys`
### Extra: Launch ssh-agent at login

`user $``echo "ssh-add" >> ~/.bash_profile`
## See also

- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
