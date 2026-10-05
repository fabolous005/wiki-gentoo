<!-- source: https://wiki.gentoo.org/wiki/PAM_securetty | group: Gentoo Wiki (Main) | wiki-title: PAM securetty -->
---
title: PAM securetty
url: https://wiki.gentoo.org/wiki/PAM_securetty
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-13"
fingerprint: ba2008344d87a9dc
license: CC BY-SA 4.0
---

# PAM securetty

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page goes over restricting root authentication with [PAM](https://wiki.gentoo.org/wiki/PAM).

## pam\_securetty.so

The pam\_securetty.so module allows the system administrator to restrict the TTYs that root is able to authenticate on. The module first checks if a plaintext file /etc/securetty exists and is not world writable. If the file exists and the tests pass, the contents will be used as a list of "secure" TTYs.

This module only affects root. If enabled, pam\_securetty.so will always return `PAM_SUCCESS` for authentication attempts by non-root users.

## Configuration

### /etc/securetty

The format of /etc/securetty is one device name per line, without the /dev prefix. The following example states that /dev/tty1 and /dev/tty2, as well as the implicit kernel console device, are secure. Authentication attempts on any other TTYs will be rejected.

**`/etc/securetty`**

### /etc/pam.d/system-auth

/etc/pam.d/system-auth is used to configure the rules that PAM follows whenever system authentication needs to be done. This applies to physical logins as well as su and anything else which utilizes PAM's authentication capabilities. Taking a backup of the current PAM configuration will make it easy to revert changes if needed.

An example PAM configuration snippet which uses the pam\_securetty.so module is:

**`/etc/pam.d/system-auth`**

The module also provides an option, `noconsole`, which rejects authentication attempts on the kernel console unless that device is also specified in /etc/securetty.

## Troubleshooting

### Can't determine the TTY

If /var/log/auth.log contains an entry, such as the following, then PAM is unable to determine the TTY that an authenticating service is running on and will reject the authentication attempt:

**`/var/log/auth.log`**

A likely cause is that service is either not setting the `PAM_TTY` variable correctly or is otherwise unable to set it reliably. A workaround is to use the pam\_succeed\_if.so module to skip the pam\_securetty.so checks for that service:

**`/etc/pam.d/system-auth`**

The `quiet_fail` option means that pam\_succeed\_if.so will not log failed tests. Without it, every PAM authentication attempt *not* originating from service would contain a line stating the test failed. To not log successes or failures, use the `quiet` option instead. These options help keep /var/log/auth.log a bit tidier.

### Can't log in

If no user can log in, then the PAM configuration is likely broken. If there is still an active root login session available, then simply edit the current configuration or revert to the backup mentioned above. [See here](https://wiki.gentoo.org/wiki/YubiKey#Troubleshooting) for instructions on how to fix it if no root login sessions are available (e.g. after a reboot).

## See also

- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
- [YubiKey](https://wiki.gentoo.org/wiki/YubiKey) — a hardware security device that can be used to safely store cryptographic keys, OTP tokens, and challenge response seeds
- [Google Authenticator](https://wiki.gentoo.org/wiki/Google_Authenticator) — describes an easy way to setup two-factor authentication on Gentoo.
- [OATH-Toolkit](https://wiki.gentoo.org/wiki/OATH-Toolkit) — toolkit for (OTP) One-Time Password authentication using HOTP/TOTP algorithms.

## External resources

- [pam.conf(5)](http://www.man7.org/linux/man-pages/man5/pam.conf.5.html), the man page describing PAM configuration files.
- [pam\_securetty(8)](http://www.man7.org/linux/man-pages/man8/pam_securetty.8.html), the man page for the PAM module.
- [pam\_succeed\_if(8)](http://www.man7.org/linux/man-pages/man8/pam_succeed_if.8.html), the man page for the PAM module.
