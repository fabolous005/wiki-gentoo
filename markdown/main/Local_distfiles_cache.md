<!-- source: https://wiki.gentoo.org/wiki/Local_distfiles_cache | group: Gentoo Wiki (Main) | wiki-title: Local distfiles cache -->
---
title: Local distfiles cache
url: https://wiki.gentoo.org/wiki/Local_distfiles_cache
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-01"
fingerprint: "8eaf5f4936053d53"
license: CC BY-SA 4.0
---

# Local distfiles cache

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details some approaches to setting up a local distfiles cache which will save bandwidth when several machines are running Gentoo on the same local area network. It is a good idea to cache distfiles on one of them and let the rest download from that one. This saves bandwidth for both the network owner and the public source mirrors.

## Setting up the server

Select the machine that is going to serve the local distfiles cache and set it up. It will need to have enough free disk space to store all of the packages that are used by any machines on the network. The server machine will also need to be available whenever any other computer emerges a new package, and other computers on the network will need to be able to reach it using a static IP address or hostname.

There are a number of ways in which the cache can be implemented. This page describes a simple setup using [net-misc/apt-cacher-ng](https://packages.gentoo.org/packages/net-misc/apt-cacher-ng).

An alternative is to configure [www-servers/nginx](https://packages.gentoo.org/packages/www-servers/nginx) to act as a proxy server for any available mirror, a configuration example can be found below.

### Using [net-misc/apt-cacher-ng](https://packages.gentoo.org/packages/net-misc/apt-cacher-ng)

#### Installing software

Install net-misc/apt-cacher-ng:

`root #``emerge --ask net-misc/apt-cacher-ng`
#### Configuring for Gentoo

Apt-cacher-ng includes support for the Gentoo source mirrors, but some configuration is still needed. Since it is designed primarily for use with the Debian package management system, there are some types of files used in Gentoo distfiles which by default it refuses to download. To change this behavior, create the file /etc/apt-cacher-ng/gentoo.conf:

**`/etc/apt-cacher-ng/gentoo.conf`**

As mentioned in this [forum post](https://forums.gentoo.org/viewtopic-p-8561293.html?sid=31d5c3ab21b66d486c6e7923d712b50a#8561293), using apt-cacher-ng as the portage http proxy breaks the openpgp key refresh process. To avoid that, configure apt-cacher-ng to pass through https traffic:

**`/etc/apt-cacher-ng/gentoo.conf`**

**Pass https requests without caching**

#### Configuring mirrors

Configure the list of public Gentoo mirrors from which apt-cacher-ng will download source packages:

**`/etc/apt-cacher-ng/backends_gentoo`**

#### Starting the service

Now start the cache service:

`root #``rc-service apt-cacher-ng start`
To start the service at boot:

`root #``rc-update add apt-cacher-ng default`
### Using [www-servers/nginx](https://packages.gentoo.org/packages/www-servers/nginx)

Instead of using [net-misc/apt-cacher-ng](https://packages.gentoo.org/packages/net-misc/apt-cacher-ng) and in case [www-servers/nginx](https://packages.gentoo.org/packages/www-servers/nginx) is already available, the latter can be used as a caching proxy for LAN clients.

A major disadvantage of this setup is that the proxy server, if it uses itself, stores all packages twice, once in the nginx proxy cache and once in ${DISTFILES}.

#### Setting up nginx VHost

Add the following to /etc/nginx/nginx.conf or some other file included into nginx server configuration:

**`/etc/nginx/nginx.conf`**

This configuration retains downloaded packages for at most 7 days until they would be redownloaded by nginx. Also, the maximum disk space used by the cache is up to 10GB and nginx will pause all subsequent requests for the same file until either one request has been completed by downloading from the mirror server or the one-minute-timeout has been reached. These values could and should be modified to local necessities, especially so, if downstream is slower to deliver large package files.

#### Restart nginx server

`root #``nginx -t`
If everything is reported ok, nginx can be forced to be restarted or to reload its configuration:

`root #``nginx -s restart``root #``nginx -s reload`
After this, clients can use nginx as a caching proxy as outlined below.

## Setting up clients

On all machines which are to use the distfiles cache (including the cache server itself), add the following to /etc/portage/make.conf:

**`/etc/portage/make.conf`**

## Open issues

- Apt-cacher-ng installs a cron job to delete unreferenced files from the cache. Since the Gentoo cache contains no index files, this probably deletes either everything or nothing from it.

- If `sync-type` is set to `webrsync` in /etc/portage/repos.conf, apt-cacher-ng will cache the snapshots. This is good if multiple machines are set up to use webrsync, but in the preferable case that a [local rsync mirror](https://wiki.gentoo.org/wiki/Local_Mirror) is being used, it is just a waste of space.
