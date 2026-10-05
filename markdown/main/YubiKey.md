<!-- source: https://wiki.gentoo.org/wiki/YubiKey | group: Gentoo Wiki (Main) | wiki-title: YubiKey -->
---
title: YubiKey
url: https://wiki.gentoo.org/wiki/YubiKey
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-17"
fingerprint: "98a95c9c2026ed64"
license: CC BY-SA 4.0
---

# YubiKey

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The YubiKey is a hardware security device that can be used to safely store cryptographic keys, OTP tokens, and challenge response seeds which can be used for authentication or encryption.

Modern YubiKeys have an [OpenPGP module](https://support.yubico.com/hc/en-us/articles/360013790259-Using-Your-YubiKey-with-OpenPGP) which can be used to store GPG keys, they also include U2F modules which can be used for authentication.

## Hardware

The following tables list all current (2023-04-28) YubiKey devices and their module support as stated on the Yubico website[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

An in-depth table showing the features of current YubiKeys is located on their [store](https://www.yubico.com/us/store/compare/)

### YubiKey 5 FIPS series

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiKey 5C NFC FIPS <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> |  |  |  |  |  |  | 
| YubiKey 5 NFC FIPS <sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> |  |  |  |  |  |  | 
| YubiKey 5Ci FIPS <sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> |  |  |  |  |  |  | 
| YubiKey 5C FIPS <sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> |  |  |  |  |  |  | 
| YubiKey 5 Nano FIPS <sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> |  |  |  |  |  |  | 
| YubiKey 5C Nano FIPS <sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup> |  |  |  |  |  |  | 

### YubiKey 5 BIO series

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiKey Bio - FIDO Edition <sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup> |  |  |  |  |  |  | 
| YubiKey C Bio - FIDO Edition <sup>[\[10\]](https://wiki.gentoo.org#cite_note-10)</sup> |  |  |  |  |  |  | 

### Security Key Series

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| Security Key NFC - Enterprise Edition <sup>[\[11\]](https://wiki.gentoo.org#cite_note-11)</sup> |  |  |  |  |  |  | 
| Security Key C NFC - Enterprise Edition <sup>[\[12\]](https://wiki.gentoo.org#cite_note-12)</sup> |  |  |  |  |  |  | 
| Security Key C NFC <sup>[\[13\]](https://wiki.gentoo.org#cite_note-13)</sup> |  |  |  |  |  |  | 
| Security Key by Yubico <sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup> |  |  |  |  |  |  | 
| FIDO U2F Security Key <sup>[\[15\]](https://wiki.gentoo.org#cite_note-15)</sup> |  |  |  |  |  |  | 
| Security Key NFC <sup>[\[16\]](https://wiki.gentoo.org#cite_note-16)</sup> |  |  |  |  |  |  | 

### YubiKey 5 Series

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiKey 5C NFC <sup>[\[17\]](https://wiki.gentoo.org#cite_note-17)</sup> |  |  |  |  |  |  | 
| YubiKey 5 Nano <sup>[\[18\]](https://wiki.gentoo.org#cite_note-18)</sup> |  |  |  |  |  |  | 
| YubiKey 5C Nano <sup>[\[19\]](https://wiki.gentoo.org#cite_note-19)</sup> |  |  |  |  |  |  | 
| YubiKey 5 NFC <sup>[\[20\]](https://wiki.gentoo.org#cite_note-20)</sup> |  |  |  |  |  |  | 
| YubiKey 5Ci <sup>[\[21\]](https://wiki.gentoo.org#cite_note-21)</sup> |  |  |  |  |  |  | 
| YubiKey 5C <sup>[\[22\]](https://wiki.gentoo.org#cite_note-22)</sup> |  |  |  |  |  |  | 

### YubiKey FIPS (4 Series)

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiKey C Nano FIPS (4 Series) <sup>[\[23\]](https://wiki.gentoo.org#cite_note-23)</sup> |  |  |  |  |  |  | 
| YubiKey FIPS (4 series) <sup>[\[24\]](https://wiki.gentoo.org#cite_note-24)</sup> |  |  |  |  |  |  | 
| YubiKey Nano FIPS (4 series) <sup>[\[25\]](https://wiki.gentoo.org#cite_note-25)</sup> |  |  |  |  |  |  | 
| YubiKey C FIPS (4 series) <sup>[\[26\]](https://wiki.gentoo.org#cite_note-26)</sup> |  |  |  |  |  |  | 

### YubiHSM Series

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiHSM 1 <sup>[\[27\]](https://wiki.gentoo.org#cite_note-27)</sup> |  |  |  |  |  |  | 
| YubiHSM2 <sup>[\[28\]](https://wiki.gentoo.org#cite_note-28)</sup> |  |  |  |  |  |  | 

### Legacy Devices

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiKey Edge-n <sup>[\[29\]](https://wiki.gentoo.org#cite_note-29)</sup> |  |  |  |  |  |  | 
| YubiKey Edge <sup>[\[30\]](https://wiki.gentoo.org#cite_note-30)</sup> |  |  |  |  |  |  | 
| YubiKey NEO <sup>[\[31\]](https://wiki.gentoo.org#cite_note-31)</sup> |  |  |  |  |  |  | 
| YubiKey NEO-n <sup>[\[32\]](https://wiki.gentoo.org#cite_note-32)</sup> |  |  |  |  |  |  | 
| YubiKey Nano <sup>[\[33\]](https://wiki.gentoo.org#cite_note-33)</sup> |  |  |  |  |  |  | 
| YubiKey Standard <sup>[\[34\]](https://wiki.gentoo.org#cite_note-34)</sup> |  |  |  |  |  |  | 

### YubiKey 4 Series

| Device | FIDO2 | U2F | OTP | OATH | PIV (PC/SC) | OpenPGP | 
|---|---|---|---|---|---|---|
| YubiKey 4 <sup>[\[35\]](https://wiki.gentoo.org#cite_note-35)</sup> |  |  |  |  |  |  | 
| YubiKey 4C Nano <sup>[\[36\]](https://wiki.gentoo.org#cite_note-36)</sup> |  |  |  |  |  |  | 
| YubiKey 4 Nano <sup>[\[37\]](https://wiki.gentoo.org#cite_note-37)</sup> |  |  |  |  |  |  | 
| YubiKey 4C <sup>[\[38\]](https://wiki.gentoo.org#cite_note-38)</sup> |  |  |  |  |  |  | 

### Kernel

**Enable support for raw HID devices**

## Usage

The different modes of operation of YubiKeys also require different ways for software to interact with them:

- U2F (through generic HID devices)
- FIDO (through generic HID devices)
- Yubico OTP (through libusb)
- Oath TOTP/HOTP (through libusb)
- PIV Smart Card (through PC/SC)
- PGP Smart Card (through a GnuPG-specific PC/SC interface)

### U2F & FIDO

To use Yubikey as U2F/FIDO device, generic HID (*hidraw*) devices may be used.

[sys-auth/pam\_u2f](https://packages.gentoo.org/packages/sys-auth/pam_u2f) and [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) with the *security-key USE* flag depend on [dev-libs/libfido2](https://packages.gentoo.org/packages/dev-libs/libfido2), which is required to make use of the FIDO2 functions of YubiKeys.

This mode of interacting with YubiKeys is used by:

### Yubico OTP & Oath TOTP/HOTP

To use Yubikey in some modes, such as OTP challenge-response, raw USB access may be used. This can be either directly or through a library such as [sys-auth/libyubikey](https://packages.gentoo.org/packages/sys-auth/libyubikey).

This mode of interacting with Yubikeys is used by:

### PIV Smart Card

To use Yubikey as a PIV Smart Card, it can be accessed according to the PC/SC specification (short for "Personal Computer/Smart Card"). [sys-apps/pcsc-lite](https://packages.gentoo.org/packages/sys-apps/pcsc-lite) provides the daemon pcscd-service to interact with smart cards. Instructions for setting up PC/SC can be found at [PCSC-Lite](https://wiki.gentoo.org/wiki/PCSC-Lite).

This mode of interacting with Yubikeys is used by:

### GPG

Some Yubikeys also run a OpenPGP Smart Card applet. Although it's technically PC/SC, GnuGPG is used directly to interact with the Yubikey. This mode of interacting with Yubikeys is used by:

- [YubiKey/GPG](https://wiki.gentoo.org/wiki/YubiKey/GPG)
- [YubiKey/SSH](https://wiki.gentoo.org/wiki/YubiKey/SSH) through GPG

## Configuration

There are various utilities for the configuration of Yubikeys:

- [app-crypt/yubioath-flutter-bin](https://packages.gentoo.org/packages/app-crypt/yubioath-flutter-bin) allows interface-configuration and generating TOTP-Codes, it is officially called [Yubico-Authenticator](https://www.yubico.com/products/yubico-authenticator/). It requires the pcscd-service, which is described below.
- [app-crypt/yubikey-manager](https://packages.gentoo.org/packages/app-crypt/yubikey-manager) aka `ykman` allows configuration of OTP, FIDO2, PIV, and enabling/disabling different interfaces (e.g. NFC)
- [app-crypt/yubikey-manager-qt](https://packages.gentoo.org/packages/app-crypt/yubikey-manager-qt)([deprecated](https://github.com/Yubico/yubikey-manager-qt/issues/361#issuecomment-2277590483), switch over to the [Yubico Authenticator](https://developers.yubico.com/yubioath-flutter/)) a GUI for [app-crypt/yubikey-manager](https://packages.gentoo.org/packages/app-crypt/yubikey-manager)
- [sys-auth/yubico-piv-tool](https://packages.gentoo.org/packages/sys-auth/yubico-piv-tool) CLI-tool for PIV configuration
- [sys-auth/yubikey-personalization-gui](https://packages.gentoo.org/packages/sys-auth/yubikey-personalization-gui) aka `ykinfo` allows very low-level and batch configuration of Yubikeys

## See also

- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
- [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) — a free implementation of the OpenPGP standard (RFC 4880).
- [Google Authenticator](https://wiki.gentoo.org/wiki/Google_Authenticator) — describes an easy way to setup two-factor authentication on Gentoo.
- [OATH-Toolkit](https://wiki.gentoo.org/wiki/OATH-Toolkit) — toolkit for (OTP) One-Time Password authentication using HOTP/TOTP algorithms.

## External resources

- [Yubico Support](https://support.yubico.com/hc/en-us/), Contains many articles on YubiKey configuration
