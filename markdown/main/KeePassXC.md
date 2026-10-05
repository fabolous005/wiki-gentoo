<!-- source: https://wiki.gentoo.org/wiki/KeePassXC | group: Gentoo Wiki (Main) | wiki-title: KeePassXC -->
---
title: KeePassXC
url: https://wiki.gentoo.org/wiki/KeePassXC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-15"
fingerprint: "820ca0cbc5d3bde6"
license: CC BY-SA 4.0
---

# KeePassXC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**KeePassXC** is a modern, secure, open-source, and cross-platform password manager. It is a fork of KeePassX that aims to incorporate stalled pull requests, features, and bug fixes that never made it into the main KeePassX repository.

## Installation

### USE Flags


| [+keyring](https://packages.gentoo.org/useflags/+keyring) | Enable support for use as the the system keyring | 
| [+network](https://packages.gentoo.org/useflags/+network) | Enable network support (e.g. for downloading favicons) | 
| [+ssh-agent](https://packages.gentoo.org/useflags/+ssh-agent) | Use KeePassXC to unlock SSH keys | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [autotype](https://packages.gentoo.org/useflags/autotype) | Add support to autotype the passwords into other applications | 
| [browser](https://packages.gentoo.org/useflags/browser) | Enable communication with web browser plugins | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [keeshare](https://packages.gentoo.org/useflags/keeshare) | Enable KeeShare sharing integration | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [yubikey](https://packages.gentoo.org/useflags/yubikey) | Enable database unlocking via hardware keys supporting YubiKey-style HMAC-SHA1 protocol | 

### Emerge

To install **KeePassXC**:

`root #``emerge --ask app-admin/keepassxc`
## Configuration

### Files

KeepassXC configuration file containing basic user settings

- \~/.config/keepassxc/keepassxc.ini - Local (per user) configuration file.

### Secret Service

KeePassXC also supports the Secret Service API, which allows client applications to securely store secrets in a service running in the user’s login session.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> To enable KeePassXC to handle the Secret Service API, following steps are required:

1. A new group or database must be created, either via the command-line interface or the graphical user interface. This group or database will be used for integration and can be accessed by applications via libsecret.
2. The newly created group or database must be exposed to other applications by selecting it in the Database Settings (Database --> Database Settings --> Secret Service Integration) and confirming the selection.
3. Now the Secret Service Integration in the settings must be activated, to allow applications to handle their secrets in the created group or database.

If it is not possible to activate the Secret Service Integration of KeePassXC because another Secret Service API is running (e.g. the gnome-secret service) the related secret service must be stopped and removed from auto-start. The [desktop environment documentation](https://wiki.gentoo.org/wiki/Desktop_environment) (if any, otherwise the [users environment](https://wiki.gentoo.org/wiki/Window_manager)) should be referred for guidance on how to do so. A general approach could be to remove the file  /etc/xdg/autostart/gnome-keyring-secrets.desktop if the blocking service is gnome-keyring. Please make sure to make a backup of the file before removing it.

It is possible that the gnome-keyring secret service or another integration is starting before KeePassXC secret service. This can occur if an application requiring the Secret Service integration, starts before KeePassXC secret service API is running, resulting in KeePassXC's integration being blocked and the other service is loaded.

To resolve this, it is possible to simply remove the blocking application. For gnome-keyring for example:

`root #``emerge --ask --depclean --verbose gnome-base/gnome-keyring`
## Usage

`user $``keepassxc`
### Secret Service

### Invocation

`user $``keepassxc --help````
Usage: keepassxc [options] [filename(s)]
KeePassXC - cross-platform password manager
Options:
  -h, --help                   Displays help on commandline options.
  --help-all                   Displays help including Qt specific options.
  -v, --version                Displays version information.
  --config <config>            path to a custom config file
  --localconfig <localconfig>  path to a custom local config file
  --lock                       lock all open databases
  --keyfile <keyfile>          key file of the database
  --pw-stdin                   read password of the database from stdin
  --debug-info                 Displays debugging information.
  --allow-screencapture        allow screenshots and app recording
                               (Windows/macOS)
Arguments:
  filename(s)                  filenames of the password databases to open (*.kdbx)
```
## Troubleshooting

### KeePassXC cannot detect smart card

If KeePassXC cannot detect a hardware key/security key/smart card for Challenge-Response, install [app-crypt/ccid](https://packages.gentoo.org/packages/app-crypt/ccid); this package contains drivers for various smart cards.

`root #``emerge --ask app-crypt/ccid`

After the package is installed, restart the pcscd service.

`root #``rc-service pcscd restart`

Now try restarting KeePassXC then re-plugging the smart card; the smart card should be detected. If KeePassXC still cannot detect the smart card, additional steps might need to be taken depending on the manufacturer; a common step is the need to add [udev](https://wiki.gentoo.org/wiki/Udev) rules for the card; to find the correct rules for the card, see the manufacturer's documentation.



## Removal

### Unmerge

**KeePassXC** can be removed with unmerging it:

`root #``emerge --ask --depclean --verbose app-admin/keepassxc`


## See also

- [KeePassXC/cli](https://wiki.gentoo.org/wiki/KeePassXC/cli) — a command line interface for the KeePassXC password manager.
- [Password management tools](https://wiki.gentoo.org/wiki/Password_management_tools) — This meta article is dedicated to secure password generation, auditing of generated passwords for security, and management of existing passwords.
- [PCSC-Lite](https://wiki.gentoo.org/wiki/PCSC-Lite) — implements the PC/SC international standard for PC to smartcard reader communication.

## External resources

- [Documenting KeePass KDBX4 file format](https://palant.info/2023/03/29/documenting-keepass-kdbx4-file-format/)
- [Discussion on CVE-2023–35866](https://keepassxc.org/blog/2023-06-20-cve-202335866/)
- [What's the difference between KeePass / KeePassX / KeePassXC?](https://superuser.com/questions/878902/whats-the-difference-between-keepass-keepassx-keepassxc)
