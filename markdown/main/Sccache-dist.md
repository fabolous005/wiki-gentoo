<!-- source: https://wiki.gentoo.org/wiki/Sccache-dist | group: Gentoo Wiki (Main) | wiki-title: Sccache-dist -->
---
title: sccache-dist
url: https://wiki.gentoo.org/wiki/Sccache-dist
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: e621a25bb161b953
license: CC BY-SA 4.0
---

# sccache-dist

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**Article status**

- Add instructions how to run with OpenRC

sccache-dist can be used to distribute compilation and compilation cache across network.

Can be used as a replacement for [ccache](https://wiki.gentoo.org/wiki/Ccache), [distcc](https://wiki.gentoo.org/wiki/Distcc), and icecream. Distributed cache and compilation is useful for improving compilation performance.

## Enabling distributed compilation

[sccache](https://wiki.gentoo.org/wiki/Sccache) must be installed with the USE flags [dist-client](https://packages.gentoo.org/useflags/dist-client) [and](https://wiki.gentoo.org/wiki/USE_flag) [dist-server](https://packages.gentoo.org/useflags/dist-server)[.](https://wiki.gentoo.org/wiki/USE_flag)

For each host in the distribution network, create a directory for configuration files:

`root #````
install -o root -g root -d /etc/sccache
```
### Scheduler

One host must act as a scheduler. It needs to be online for the other hosts to be able to distribute jobs.

Generate a shared authentication token:

`root #````
sccache-dist auth generate-shared-token
```
Generate a scheduler key:

`root #````
sccache-dist auth generate-jwt-hs256-key
```
Add scheduler config:

**`/etc/sccache/scheduler.conf`**

```
public_addr = "<public address of the host>:<port>"
[client_auth]
type = "token"
token = "<shared token>"
[server_auth]
type = "jwt_hs256"
secret_key = "<scheduler key>"
```
The scheduler host can now generate authentication tokens for other hosts:

`root #````
sccache-dist auth generate-jwt-hs256-server-token --server <server public address>:<port> --config /etc/sccache/scheduler.conf
```
#### systemd

Add a systemd service override:

**`/etc/systemd/system/sccache-scheduler.service.d/override.conf`**

```
[Service]
ExecStart=
ExecStart=/usr/bin/sccache-dist scheduler --config /etc/sccache/scheduler.conf
Environment="SCCACHE_NO_DAEMON=1"
```
Start the scheduler:

`root #````
systemctl daemon-reload
```
`root #````
systemctl start sccache-scheduler
```
### Server

A host that can compile distributed jobs must be configured as a server.

Create a directory for the builder:

`root #````
install -o root -g root -d /var/cache/sccache/builder/build
```
Create a directory for the builder toolchains:

`root #````
install -o root -g root -d /var/cache/sccache/builder/toolchains
```
Add server config:

**`/etc/sccache/server.conf`**

```
public_addr = "<public address of the server>:<port>"
toolchain_cache_size = 10737418240
cache_dir = "/var/cache/sccache/builder/toolchains"
scheduler_url = "<public address of the scheduler host>:<port>"
[builder]
type = "overlay"
build_dir = "/var/cache/sccache/builder/build"
bwrap_path = "/usr/bin/bwrap"
[scheduler_auth]
type = "jwt_token"
token = "<scheduler auth token>" # Generate on scheduler host with "sccache-dist auth generate-jwt-hs256-server-token --server <server public address>:<port> --config /etc/sccache/scheduler.conf"
```
#### systemd

Add a systemd service override:

**`/etc/systemd/system/sccache-server.service.d/override.conf`**

```
[Service]
ExecStart=
ExecStart=/usr/bin/sccache-dist server --config /etc/sccache/server.conf
Environment="SCCACHE_NO_DAEMON=1"
```
Start the server:

`root #````
systemctl daemon-reload
```
`root #````
systemctl start sccache-server
```
### Client

Add client config:

**`/etc/sccache/client.conf`**

```
[dist]
scheduler_url = "http://<scheduler public address>:<port>"
toolchains = []
toolchain_cache_size = 5368709120
cache_dir = "/var/cache/sccache/cache/sccache-dist-client"
[dist.auth]
type = "token"
token = <shared token> # Generated with "sccache-dist auth generate-shared-token", must match on all clients and the scheduler.
[cache.disk]
size = 21474836480
dir = "/var/cache/sccache/cache"
```
Create hard links for compiler executable names that point to /usr/bin/sccache:

`root #````
install -o root -g root -d /usr/local/libexec/sccache
```
`root #````
for __SCCACHE_BIN_LINK in ar c++ c99 cc clang clang++ cpp g++ gcc i686-pc-linux-gnu-clang i686-pc-linux-gnu-clang++ x86_64-pc-linux-gnu-c++ x86_64-pc-linux-gnu-cc x86_64-pc-linux-gnu-clang x86_64-pc-linux-gnu-clang++ x86_64-pc-linux-gnu-g++ x86_64-pc-linux-gnu-gcc ; do ln -v /usr/bin/sccache "/usr/local/libexec/sccache/$__SCCACHE_BIN_LINK" ; done
```
Portage does not have official support for sccache and some sandboxing features need to be disabled to be able to distribute. Add following values to make.conf:

**`/etc/portage/make.conf`**

```
PATH="/usr/local/libexec/sccache:${PATH}"
FEATURES="${FEATURES} -ipc-sandbox -network-sandbox -network-sandbox-proxy -pid-sandbox"
SANDBOX_WRITE="${SANDBOX_WRITE}:/var/cache/sccache/cache"
RUSTC_WRAPPER="/usr/bin/sccache"
SCCACHE_SERVER_PORT=4227
SCCACHE_CONF="/etc/sccache/client.conf"
SCCACHE_NO_DAEMON=1
SCCACHE_START_SERVER=0
```
#### systemd

Add a service file for the sccache client.

**`/etc/systemd/system/sccache-client.service`**

```
[Unit]
Description=sccache client daemon
Wants=network-online.target
After=network-online.target
[Service]
Environment="SCCACHE_CONF=/etc/sccache/client.conf"
Environment="SCCACHE_NO_DAEMON=1"
Environment="SCCACHE_START_SERVER=1"
Environment="SCCACHE_LOG=info"
Environment="SCCACHE_MAX_FRAME_LENGTH=104857600"
Environment="SCCACHE_SERVER_PORT=4227"
Environment="SCCACHE_IDLE_TIMEOUT=0"
ExecStart=/usr/bin/sccache
User=portage
Group=portage
[Install]
WantedBy=multi-user.target
```
`root #````
systemctl daemon-reload
```
`root #````
systemctl start sccache-client
```
### Troubleshooting

#### Enabling debug logs

Add `SCCACHE_LOG=debug` to the environment variables of the sccache scheduler/server/client process.

#### Package fails to build

Setting CC/CXX/CPP and RUSTC\_WRAPPER together can break some Rust builds. The sccache compiler ends up using /usr/libexec/sccache/xxx as linker, causing sccache to call itself, which results in a hanging state.

Some builds might fail due to some problems related to relative paths.

Disabling the sccache compiler can be done by adding to the start of make.conf:

**`/etc/portage/make.conf`**

```
ORIGINAL_PATH="${PATH}"
```
Add a /etc/portage/env file:

**`/etc/portage/env/disable-sccache`**

```
PATH="${ORIGINAL_PATH}"
RUSTC_WRAPPER=""
```
Disable sccache for the package in /etc/portage/package.env:

**`/etc/portage/package.env`**

```
 disable-sccache
```
