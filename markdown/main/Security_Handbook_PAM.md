<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/PAM | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/PAM -->
---
title: Security Handbook/PAM
url: https://wiki.gentoo.org/wiki/Security_Handbook/PAM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: b0adcbfa4c53b760
license: CC BY-SA 4.0
---

# Security Handbook/PAM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[PAM](https://wiki.gentoo.org/wiki/PAM) is a suite of shared libraries that provide an alternate way of providing user authentication in programs. The `pam` USE flag is enabled by default in Gentoo. Although the default PAM settings in Gentoo are reasonable, there is always room for improvement.


First install [sys-libs/cracklib](https://packages.gentoo.org/packages/sys-libs/cracklib) to allow password policies to be set:

`root #``emerge --ask sys-libs/cracklib`
**`/etc/pam.d/passwd`**

```
     required pam_unix.so shadow nullok
account  required pam_unix.so
password required pam_cracklib.so difok=3 retry=3 minlen=8 dcredit=-2 ocredit=-2
password required pam_unix.so md5 use_authtok
session  required pam_unix.so
```
This will add the cracklib which will ensure that the user passwords are at least 8 characters and contain a minimum of 2 digits, 2 other characters, and are more than 3 characters different from the last password. The PAM [cracklib documentation](https://man7.org/linux/man-pages/man8/pam_cracklib.8.html) can be reviewed for more available options.

**`/etc/pam.d/sshd`**

```
     required pam_unix.so nullok
auth     required pam_shells.so
auth     required pam_nologin.so
auth     required pam_env.so
account  required pam_unix.so
password required pam_cracklib.so difok=3 retry=3 minlen=8 dcredit=-2 ocredit=-2 use_authtok
password required pam_unix.so shadow md5
session  required pam_unix.so
session  required pam_limits.so
```
Every service not configured with a PAM file in /etc/pam.d will use the rules in /etc/pam.d/other. The defaults are set to deny, as they should be.

Also, pam\_warn.so can be added to generate more elaborate logging. And pam\_limits can be used, which is controlled by /etc/security/limits.conf. See the [/etc/security/limits.conf](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations#.2Fetc.2Fsecurity.2Flimits.conf) section for more on these settings.

**`/etc/pam.d/other`**

```
     required pam_deny.so
auth     required pam_warn.so
account  required pam_deny.so
account  required pam_warn.so
password required pam_deny.so
password required pam_warn.so
session  required pam_deny.so
session  required pam_warn.so
```
- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
