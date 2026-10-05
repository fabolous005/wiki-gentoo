<!-- source: https://wiki.gentoo.org/wiki/Owncloud | group: Gentoo Wiki (Main) | wiki-title: Owncloud -->
---
title: ownCloud
url: https://wiki.gentoo.org/wiki/Owncloud
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-02-02"
fingerprint: d229a749fc87898c
license: CC BY-SA 4.0
---

# ownCloud

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



ownCloud is a free, open source, Dropbox-like file synchronization and cloud service. Since it can be setup and hosted on private servers it has no set limit on storage capacity or the number of connected users.

## Introduction

### Features

- File storage in conventional directory structures or via WebDAV;
- Encryption of user files;
- Synchronization of ownCloud Client running Windows (Windows XP, Vista, 7 and 8), Mac OS X (10.6 or better), or Linux;
- Calendar (also as CalDAV);
- Task scheduler;
- Address book (also as CardDAV);
- Music streaming (through Ampache);
- User and group administration (via OpenID or Lightweight Directory Access Protocol);
- Sharing of content across groups or public URLs;
- Online text editor with syntax highlighting and code folding;
- Bookmarking;
- URL shortening Suite;
- Photo gallery;
- Video viewer;
- Portable Document Format viewer (using PDF.js);
- Viewer for OpenDocument Files (.odt, .odp, .ods);
- Firefox Sync/Mozilla Sync hosting — Mozilla Firefox users can store all history, form data, bookmarks etc. in an ownCloud server;
- External storage such as Dropbox, GoogleDrive, or Amazon S3 can be mounted to an ownCloud installation;
- Customizable with one-click application install;
- Logging Module: supports logging of file-related actions, logs, who accessed what, when and from where. Only available ownCloud Business, Enterprise and Education Editions.

### Requirements

The following will be needed in order for ownCloud to function properly:

- A web server - ([Apache](https://wiki.gentoo.org/wiki/Apache), [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd), [Nginx](https://wiki.gentoo.org/wiki/Nginx), or likewise).
- webapp-config - ([app-admin/webapp-config](https://packages.gentoo.org/packages/app-admin/webapp-config)) to easily install ownCloud Server to an vhost directory.
- PHP 5.4.9 or greater - for owncloud-5.0.13-r1 and up.

## Installation

### USE flags

If `vhosts` USE will be enabled, so do before the install. The following is a list of possible USE flags.

*www-apps/owncloud*correct?

### Emerge

Install ownCloud:

`root #``emerge --ask www-apps/owncloud`
#### Optional: Installing ownCloud server in the vhost directory via webapp-config

### Updates

#### Optional: Updating ownCloud server in the vhost directory via webapp-config

This command will update the installed ownCloud server which is located in the directory cloud to the version 5.0.14

`root #``webapp-config -h myDomain.com -d cloud -U owncloud 5.0.14`
Requires the webapp-config command (from [app-admin/webapp-config](https://packages.gentoo.org/packages/app-admin/webapp-config)):

If the `vhosts` USE flag is enabled, this command will install the ownCloud server version 5.0.13-r1 for the domain `myDomain.com` to the directory cloud which will (normally) be located in /var/www/myDomain.com/htdocs/

`root #``webapp-config -h myDomain.com -d cloud -I owncloud 5.0.13-r1`
Make sure that this directory is added as a vhosts directory.

## Configuration

### Performance tweaks

Activate ownCloud Cronjobs ([for more information](https://doc.owncloud.com/server/admin_manual/configuration/server/background_jobs_configuration.html)):

`root #``crontab -u apache -e`
\*/15  \*  \*  \*  \* php -f /var/www/mydomain.com/htdocs/cloud/cron.php

#### Enable APCu

`root #``PHP_TARGETS="php5-4" emerge -av dev-php/pecl-apcu`
Then restart the php-fpm service:

`root #``/etc/init.d/php-fpm restart`
To verify if apc is working, copy apc.php in the www folder

`root #``cp /usr/share/php/apc/apc.php /var/www/`
Open it with a preferred web browser.

### Misc tweaks

#### Disable integrity check

[Currently](https://github.com/owncloud/core/issues/24218), the integrity checker complains when there is a .webapp file in the root of the OwnCloud installation directory. This file is generated, and required, by the webapp-config tool used to install all web apps on Gentoo. One possible temporary solution to this is to disable integrity check. This can be accomplished by adding the following to config.php:

**`config/config.php`**

```
'integrity.check.disabled' => true
```
## Usage

### ownCloud client

Install the client by emerging the following package:

`root #``emerge --ask net-misc/owncloud-client`
## Removal

### Optional: Removing ownCloud server from the vhost directory via webapp-config

This command will remove the installed ownCloud server version 5.0.14 which is located in the directory cloud.

`root #``webapp-config -h myDomain.com -d cloud -C owncloud 5.0.14`
### gnome-keyring support

You may have to install libgnome-keyring to enable owncloud-client support for gnome-keyring <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

`root #``emerge --ask gnome-base/libgnome-keyring`
Otherwise it may try to use kwallet.

## See also

- [Dropbox](https://wiki.gentoo.org/wiki/Dropbox) — a closed source file synchronization and cloud service utility.
- [SparkleShare](https://wiki.gentoo.org/wiki/SparkleShare) — a cross platform, free, open source, Dropbox-like, git-based collaboration and file sharing tool.
- [Webapp-config](https://wiki.gentoo.org/wiki/Webapp-config) — Gentoo's installer for web-based applications.
