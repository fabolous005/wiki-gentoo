<!-- source: https://wiki.gentoo.org/wiki/Fingerprint_Reader | group: Gentoo Wiki (Main) | wiki-title: Fingerprint Reader -->
---
title: Fingerprint reader
url: https://wiki.gentoo.org/wiki/Fingerprint_Reader
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-21"
fingerprint: "88888822361526cf"
license: CC BY-SA 4.0
---

# Fingerprint reader

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- how to enroll a fingerprint for a specific user
- GNOME/KDE integration and development status of this features
- Configure PAM to use fprintd

the three things above should be finished but they may need to be checked and expanded upon

Some laptops (especially those of the [ThinkPad](https://en.wikipedia.org/wiki/ThinkPad) persuasion) come with an integrated fingerprint reader which can be used for authentication.

With the warning being understood, it is perfectly acceptable to use a fingerprint to identify the user account before signing with key-based or another form of authentication.

## Available software

| Name | Package | Homepage | Description | 
|---|---|---|---|
| fprint | [sys-auth/fprintd](https://packages.gentoo.org/packages/sys-auth/fprintd) | [https://cgit.freedesktop.org/libfprint/fprintd/](https://cgit.freedesktop.org/libfprint/fprintd/) | fprint consists of several components. The primary being a daemon which provides access to fprint functionality through [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) to applications, such as login managers (GDM, KDM, ...), screen locking mechanisms etc. | 
| thinkfinger | [sys-auth/thinkfinger](https://packages.gentoo.org/packages/sys-auth/thinkfinger) | [http://thinkfinger.sourceforge.net/](http://thinkfinger.sourceforge.net/) | Support for the UPEK/SGS Thomson Microelectronics fingerprint reader, often seen in ThinkPad laptops. | 

## Enrolling a fingerprint

Enroll a fingerprint as a user:

`user $``fprintd-enroll`
To enroll a fingerprint to a specific user<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>, use the fprintd-enroll utility:

`root #``fprintd-enroll <user>`
To enroll a certain finger:

`root #``fprintd-enroll -f right-index-finger <user>`
To test if the finger is enrolled, use the fprintd-verify command:

`user $``fprintd-verify -f right-index-finger`
## Graphical Integration

KDE supports the adding/removing of fingerprints via their system settings app under the users tab by clicking configure fingerprint authentication.[\[4\]](https://wiki.gentoo.org#cite_note-4)

As for the enabling of it for graphical authentication, the system is able to login, wake up from sleep, and sudo in the terminal, however, it is unknown at this time whether you can replace the authentication popups.

## Configuring fprintd for use with PAM

[PAM](https://wiki.gentoo.org/wiki/PAM) is the authentication service used by Linux. To use a fingerprint reader with PAM, insert the following command in to the configuration file to make eligible for fingerprint.

**`/etc/pam.d/(pam.d service)`**

```
auth            sufficient      pam_fprintd.so
```
