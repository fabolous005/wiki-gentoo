<!-- source: https://wiki.gentoo.org/wiki/Tor/Docker_container_image | group: Gentoo Wiki (Main) | wiki-title: Tor/Docker container image -->
---
title: Tor/Docker container image
url: https://wiki.gentoo.org/wiki/Tor/Docker_container_image
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-31"
fingerprint: "3985f36b1d650dca"
license: CC BY-SA 4.0
---

# Tor/Docker container image

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A single binary, statically linked [Docker](https://wiki.gentoo.org/wiki/Docker) image can be created in a few steps.

## Portage

### Static libraries

Some static versions of libraries are needed (the dynamic ones will stay):

**`/etc/portage/package.use/torstatic`**

`root #``emerge --ask --update --deep --newuse @world`
For getting a reminder in the [Portage logs](https://wiki.gentoo.org/wiki/Portage_log):

**`/etc/portage/bashrc`**

```
() {
	case $CATEGORY/$PN in
		dev-libs/libevent | dev-libs/openssl | sys-libs/libcap | sys-libs/libseccomp | sys-libs/zlib)
			ewarn
			ewarn "Remember to rebuild static Tor (net-vpn/tor)."
			ewarn
			;;
		net-vpn/tor)
			ewarn
			ewarn "Remember to create a new Docker image."
			ewarn
			;;
	esac
}
```
### Tor static

**`/etc/portage/package.env`**

**`/etc/portage/env/torstatic.conf`**

**`/etc/portage/package.accept_keywords/tor`**

**Consider updating as soon as possible.**

`root #``emerge --ask --verbose net-vpn/tor`
This script rebuilds the tor binary when the static libraries have been rebuild.

**`torstatic-updater.sh`**

```
#!/bin/bash
LAST_EMERGE=$(tac /var/log/emerge.log | sed \
	-e '/openssl-compat/d' \
	-e '/libcap-ng/d' \
	-e '/zlib-ng/d' | grep -m1 -o \
	-e 'completed emerge .* dev-libs/libevent' \
	-e 'completed emerge .* dev-libs/openssl' \
	-e 'completed emerge .* net-vpn/tor' \
	-e 'completed emerge .* sys-libs/libcap' \
	-e 'completed emerge .* sys-libs/libseccomp' \
	-e 'completed emerge .* sys-libs/zlib')
if [[ $LAST_EMERGE == *net-vpn/tor ]]; then
	echo "No rebuild needed."
else
	echo "Rebuild needed."
	emerge -av net-vpn/tor
fi
```
## Image creation

Paths are relative to a chosen working directory.

`root #````
mkdir -p image/etc/tor image/usr/bin image/var/lib/tor
```
`root #````
chmod 750 image/var/lib/tor
```
`root #````
chown 43:43 image/var/lib/tor
```
`root #````
file /usr/bin/tor # Check "statically linked".
```
`root #````
cp -a /usr/bin/tor image/usr/bin/
```
**`image/etc/tor/torrc`**

**-rw-r--r-- root root**

With the [seccomp](https://packages.gentoo.org/useflags/seccomp) [USE flag enabled,](https://wiki.gentoo.org/wiki/USE_flag) `Sandbox 1` should be added here (see the [main Tor wiki](https://wiki.gentoo.org/wiki/Tor#Sandbox)).

`root #````
tor -f image/etc/tor/torrc --verify-config # Example in /etc/tor/torrc.sample.
```
`root #````
tar -czf gentoo-tor.tar.gz --numeric-owner --mtime='' -C image/ .
```
Please note that:

- Docker has logging capabilities of its own.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>
- The first started container gets `172.17.0.2`, if no fixed IP has been configured.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>
- The [GeoIP](https://wiki.gentoo.org/wiki/Tor#Rules_for_Tor_circuits) files are omitted, but could still be added.



## Firewall

Adding firewall rules for Tor is a bit tricky, because Docker does its magic in [iptables](https://wiki.gentoo.org/wiki/Iptables) and [nftables](https://wiki.gentoo.org/wiki/Nftables). Own rules should be build around it.

### iptables

A handy modification would be to add `rc_after="iptables ip6tables"` to /etc/conf.d/docker. Then /var/lib/iptables/rules-save and/or /var/lib/ip6tables/rules-save will be loaded in the runlevel first. It's safe to add rules to the `INPUT` and `OUTPUT` chains of the `*filter` table.

Adding more rules to any table *after* Docker and the network have started, could be done in /etc/conf.d/net and /etc/dhcpcd.enter-hook. Only don't flush the chains Docker is using! Own rules can be added/deleted one by one with the iptables/ip6tables options `-A`, `-I` and `-D`. It would be wise to investigate what rules Docker already did add, using the iptables-save and ip6tables-save commands, and use `SAVE_ON_STOP="no"` in /etc/conf.d/iptables and /etc/conf.d/ip6tables.

### nftables

Support for nftables is still labeled as "experimental" by upstream.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> One might want to plan migration carefully, and in the meantime add:

**`/etc/conf.d/docker`**

**(app-containers/docker-29.0.0 and higher)**

## Docker

And finally testing the image in Docker:

**`Dockerfile`**

`user $````
docker build -t gentoo-tor .
```
`user $````
docker run --rm --ulimit nofile=30000 -p 127.0.0.1:9050:9050/tcp --name tor gentoo-tor
```
It might be a good idea to add `--cap-drop ALL --security-opt no-new-privileges` to docker run<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>, to prevent privilege escalation attacks (needs USE flag [caps](https://packages.gentoo.org/useflags/caps)[).](https://wiki.gentoo.org/wiki/USE_flag)

### Persistent data

Security can be increased by letting Tor's Entry Guards[\[5\]](https://wiki.gentoo.org#cite_note-5)<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> survive deletion when the container is removed. To achieve that: add `-v tor:/var/lib/tor` to docker run. A persistent Docker volume<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> 'tor' will be created and mounted automatically. Keep adding this option (for the current and updated gentoo-tor images).

A separate volume also increases performance of the data store.

### Testing

Not my IP.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Docker Docs, [View container logs](https://docs.docker.com/engine/logging/). Retrieved on 2025/11/18.
2. [↑](https://wiki.gentoo.org#cite_ref-2) Stack Overflow, [Assign static IP to Docker container](https://stackoverflow.com/questions/27937185/assign-static-ip-to-docker-container). Retrieved on 2025/11/18.
3. [↑](https://wiki.gentoo.org#cite_ref-3) Docker Docs, [Docker with nftables](https://docs.docker.com/engine/network/firewall-nftables/). Retrieved on 2026/05/31.
4. [↑](https://wiki.gentoo.org#cite_ref-4) Docker Docs, [docker container run](https://docs.docker.com/reference/cli/docker/container/run/). Retrieved on 2025/11/18.
5. [↑](https://wiki.gentoo.org#cite_ref-5) Tor Support, [What are Entry Guards?](https://support.torproject.org/about-tor/how-tor-works/entry-guards/). Retrieved on 2025/11/17.
6. [↑](https://wiki.gentoo.org#cite_ref-6) Whonix Documentation, [Tor Entry Guards](https://www.whonix.org/wiki/Tor_Entry_Guards). Retrieved on 2025/11/17.
7. [↑](https://wiki.gentoo.org#cite_ref-7) Docker Docs, [Volumes](https://docs.docker.com/engine/storage/volumes/). Retrieved on 2025/11/17.
