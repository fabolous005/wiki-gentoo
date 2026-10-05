<!-- source: https://wiki.gentoo.org/wiki/Firefox/Sync_Server | group: Gentoo Wiki (Main) | wiki-title: Firefox/Sync Server -->
---
title: Firefox/Sync Server
url: https://wiki.gentoo.org/wiki/Firefox/Sync_Server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-02-01"
fingerprint: "9de0a75b8b323985"
license: CC BY-SA 4.0
---

# Firefox/Sync Server

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**July 14, 2015**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Firefox/Sync_Server&action=edit).

This article explains how to run a private [Firefox](https://wiki.gentoo.org/wiki/Firefox) Sync server instance.

## Installation

Unless using a very particular setup with registration and storage servers in different locations install the [www-misc/mozilla-sync-server-full](https://packages.gentoo.org/packages/www-misc/mozilla-sync-server-full) package which is available through the klondike overlay.

### Emerging

After setting up the desired flags remove the keywords from the packages and run:

`root #``emerge --ask www-misc/mozilla-sync-server-full`
## Configuration

The package is now installed and the default configuration is in /etc/mozilla-sync-server/. The configuration of the daemon can be set using the .ini files whilst the configuration of sync itself is done in the .conf files. All of them are .ini-style files.

The first step is replacing the following line by the .conf file to use on the .ini file:

**`{{{filename}}}`**

**Log configuration**

Then ensure logs are saved in the proper place:

**`{{{filename}}}`**

**Log location**

Then you may need to edit the server.wsgi file so they load the proper .ini files for that replace the following line by the correct file:

**`{{{filename}}}`**

**ini\_file**

Finally edit the .conf file with the desired settings. The most important ones are the sqluri which define the path to the SQL databases and the fallback\_node which defines the URL to the server as seen by the client you may also want to disable the captcha.

For a list of parameters check: [http://docs.services.mozilla.com/server-devguide/configuration.html](http://docs.services.mozilla.com/server-devguide/configuration.html)

### Testing the server

Once configured test the server by following this line in /etc/mozilla-sync-server/:

`root #``bin/paster serve development.ini`
This will start the server listening on the port 5000. You will need to have paster installed. In general using this approach to run the server is a bad idea so you can run it behind a web server instead

### Running behind a web server

#### Apache

Emerge [www-apache/mod\_wsgi](https://packages.gentoo.org/packages/www-apache/mod_wsgi):

`root #``emerge --ask www-apache/mod_wsgi`
Create the `mozsync` user on the `mozsync` group if has not been created.

Merge the following with the vhost configuration (the first line may require modification):

**`{{{filename}}}`**

**Vhost configuration**
