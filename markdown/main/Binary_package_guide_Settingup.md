<!-- source: https://wiki.gentoo.org/wiki/Binary_package_guide/Settingup | group: Gentoo Wiki (Main) | wiki-title: Binary package guide/Settingup -->
---
title: Binary package guide/Settingup
url: https://wiki.gentoo.org/wiki/Binary_package_guide/Settingup
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-12"
fingerprint: ad311a3ea1737f91
license: CC BY-SA 4.0
---

# Binary package guide/Settingup

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo binhost**

Portage supports a number of protocols for downloading binary packages: FTP, FTPS, HTTP, HTTPS, and SSH/SFTP. This leaves room for many possible binary package host implementations.

There is, however, no "out-of-the-box" method provided by Portage for distributing binary packages. Depending on the desired setup additional software will need to be installed.

To provide an authenticated approach for binary package mirrors, Portage can be configured to use the SSH protocol to access binary packages.

When using SSH, it is possible to use the root Linux user's SSH key (without passphrase as the installations need to happen in the background) to connect to a remote binary package host.

To accomplish this, make sure that the root user's SSH key is allowed on the server. This will need to happen for each machine that will connect to the SSH capable binary host:

`root #``cat root.id_rsa.pub >> /home/binpkguser/.ssh/authorized_keys`
The /etc/portage/binrepos.conf configuration could then look like so:

**`/etc/portage/binrepos.conf/ssh.conf`**

**Setting binrepos.conf up for SSH access**

```
[ssh]
priority = 1
sync-uri = ssh://binpkguser@binhostserver/var/cache/binpkgs
verify-signature = true
location = /var/cache/binhost/ssh
```
If the SSH server is listening to a different port (e.g 25), then it must be specified after the address, like so:

**`/etc/portage/binrepos.conf/ssh.conf`**

**Setting up binrepos.conf for SSH access on port 25**

```
[ssh]
priority = 1
sync-uri = ssh://binpkguser@binhostserver:25/var/cache/binpkgs
verify-signature = true
location = /var/cache/binhost/ssh
```
Portage ignores \~/.ssh/config by default, however you can setup your make.conf to use a custom ssh config like so:

**`/etc/portage/make.conf`**

**Setting up make.conf for custom SSH config**

```
PORTAGE_SSH_OPTS='-F /home/larry/.ssh/config'
```
When using binary packages on an internal network, it might be easier to export the packages through [NFS](https://wiki.gentoo.org/wiki/Nfs-utils) and mount it on the clients.

There are two ways of doing this:

1. Making it a 'remote' via /etc/portage/binrepos.conf which is the modern way (and uses *--getbinpkg*), or
2. Mounting the exported tree at /var/cache/binpkgs where Portage thinks it is 'local'

The modern way allows per-repo PGP verification and also 're-serving' without mixing up binaries from different sources.

The /etc/exports file could look like so:

**`/etc/exports`**

**Exporting the packages directory**

On the clients, the location can then be mounted **at a separate location**. An example /etc/fstab entry would look like so:

**`/etc/fstab`**

**Entry for mounting the packages folder**

Then configure Portage to know about this:

**`/etc/portage/binrepos.conf/nfs.conf`**

```
[nfs]
priority = 1
sync-uri = file:///opt/nfs-binpkgs
verify-signature = true
location = /var/cache/binhost/nfs
```
That is, there are three locations involved in total:

- server/binhost: /var/cache/binpkgs
- client: /opt/nfs-binpkgs is the mount location for NFS with the full set of binpkgs
- client: /var/cache/binhost/nfs is the local cached copy of any binaries used

Using the above exports, the client may not discover new binary packages or may fail to emerge them if the file permissions are insufficient. To fix this, change ownership of the exported PKGDIR from the host:

`root #``chown -v nobody:nobody /var/cache/binpkgs`
Set also the setgid bit so that new packages will inherit the group ownership:

`root #``chmod -v g+s /var/cache/binpkgs`
The ownership will also have to be changed individually (or recursively) for any packages that have already been created at this point.



A common approach for distributing binary packages is to create a web-based binary package host.

#### HTTPD

Install [www-servers/lighttpd](https://packages.gentoo.org/packages/www-servers/lighttpd) and configure it to provide read access to /etc/portage/make.conf's `PKGDIR` location.

**`/etc/lighttpd/lighttpd.conf`**

**lighttpd configuration example**

```
# add this to the end of the standard configuration
server.dir-listing = "enable"
server.modules += ( "mod_alias" )
alias.url = ( "/packages" => "/var/cache/binpkgs/" )
dir-listing.activate = "enable" # optional: keep it, if you want to have a nice listing at web page at the /packages path.
```
#### Caddy

To set up the [Caddy](https://wiki.gentoo.org/wiki/Caddy) HTTP server to provide a web-based binary package host, create a `Caddyfile` containing:

**`Caddyfile`**

Once that is created, run Caddy with:

`root #``caddy run --config /path/to/Caddyfile`
Then, on the client systems, configure /etc/portage/binrepos.conf accordingly:

**`/etc/portage/binrepos.conf/caddy.conf`**

**Using a web-based binary package host**

```
[caddy]
priority = 1
sync-uri = http://binhost.example.com/packages
verify-signature = true
location = /var/cache/binhost/caddy
```
#### nginx

To setup a web-based binhost utilizing [nginx](https://wiki.gentoo.org/wiki/Nginx), the default configuration `nginx.conf` will suffice, with a little tweaking. A full example of such looks like the following:

**`/etc/nginx/nginx.conf`**

#### http.server

Python includes a barebones HTTP server by default and is very simple to use. To setup a binhost with this, cd into the `PKGDIR` directory and run:

`user $``python http.server 8000`
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
