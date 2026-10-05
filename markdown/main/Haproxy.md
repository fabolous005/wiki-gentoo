<!-- source: https://wiki.gentoo.org/wiki/Haproxy | group: Gentoo Wiki (Main) | wiki-title: Haproxy -->
---
title: Haproxy
url: https://wiki.gentoo.org/wiki/Haproxy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-26"
fingerprint: ff43e1dfd0833044
license: CC BY-SA 4.0
---

# Haproxy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Haproxy** (Originally High Availability Proxy) provides an industrial grade proxy to the Gentoo administrator routing traffic between ones frontend (web-facing) and backend (web-servers/web-services/databases).
It reports connectivity statistics, performs health checks upon backend services and supports load balancing.

Interestingly, if one has multiple frontend machines HAProxy will redirect clients from one machine to another as they are taken offline ensuring a consistent service (One stands under correction on this point).

It handles both TCP (Level 4 in the OSI model) and the HTTP (Level 7 in the OSI model) routing (Routing further protocols seems possible too e.g. mail).

SSL termination may be done at Haproxy or passed through to be termination at the backend (SNI).

## Installation

### USE flags


| [+crypt](https://packages.gentoo.org/useflags/+crypt) | Add support for encryption -- using mcrypt or gpg where applicable | 
| [+doc](https://packages.gentoo.org/useflags/+doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [+net_ns](https://packages.gentoo.org/useflags/+net_ns) | Enable network namespace support (CONFIG\_NET\_NS) | 
| [+pcre](https://packages.gentoo.org/useflags/+pcre) | Add support for Perl Compatible Regular Expressions | 
| [+slz](https://packages.gentoo.org/useflags/+slz) | Use dev-libs/libslz compression library | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [+threads](https://packages.gentoo.org/useflags/+threads) | Add threads support for various packages. Usually pthreads | 
| [51degrees](https://packages.gentoo.org/useflags/51degrees) | Device Detection using 51 Degrees | 
| [ech](https://packages.gentoo.org/useflags/ech) | Enable OpenSSL ECH | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [pcre-jit](https://packages.gentoo.org/useflags/pcre-jit) | Use JIT support for PCRE | 
| [prometheus-exporter](https://packages.gentoo.org/useflags/prometheus-exporter) | Also build the prometheus exporter | 
| [quic](https://packages.gentoo.org/useflags/quic) | Enable support for QUIC (RFC 9000); a UDP-based protocol intended to replace TCP | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tools](https://packages.gentoo.org/useflags/tools) | Install additional tools (halog, iprange) | 
| [wurfl](https://packages.gentoo.org/useflags/wurfl) | Device Detection using WURFL | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

### Emerge

`root #``emerge --ask net-proxy/haproxy`
### Additional software

The following software supports, compliments or integrates with Haproxy :

- HATop
- An interactive ncurses client and real-time monitoring, statistics displaying tool for the Haproxy TCP/HTTP load balancer
- Herald
- A load feedback and check agent for Haproxy
- Haproxystats
- A statistics collector which processes various statistics and pushes them to a graphing system (Graphite)

HAProxy serves the usual web-servers (Apache/Lighthttpd/NginX/Traefik) and databases (PostgreSQL/Redis/MySQL/CouchDB) and supports encryption (OpenSSL/LetsEncrypt). It will handle backend connections via unix and web sockets. These packages are used with haproxy and do not interact with it directly, hence are not listed here.

## Configuration

The global section, within Haproxy's configuration file, specifies the services permissions and behaviour upon ones system. This file is read sequentially and one may define One or more default block(s) may be set to define the common behaviour of the subsequent frontend/backend blocks; with later default blocks overriding the earlier ones.

In the example that follow one provide the minimal configuration necessary to enable some feature provided by Haproxy. By combining these examples one should be able to configure Haproxy for their own setup.

### Files

The following files and folders are used to configure Haproxy

\* /etc/haproxy/haproxy.cfg - The main configuration file
\* /etc/haproxy/certs - The SSL certificates folder
\* /etc/openssl/private - An alternate, possibly better, location for SSL certificates

#### Global

The haproxy user and group are configured by emerge during installation.

**`/etc/haproxy/haproxy.cfg`**

#### SSL

This terminates the secure connection and passes the decrypted traffic on to the backend. This assumes the backend is run on a secured internal network.

Haproxy uses a single certificate for authentication purposes, that is an ordered and combined key, thing and thing. If ones certificates are supplied by letsencrypts' certbot then they may use the following line to generate a combined certiifcate for haproxy. The combined certificates should be stored under either the Haproxy folder, /etc/haproxy/certs, or the OpenSSL one, /etc/openssl/private (The author is not sure which of these paths is the canonical one).

#### SNI

SNI is performed within the TCP layer (Level 4 in the OSI model) allowing frontend connections over HTTPS to be directed to the appropriate backend. Only very old browsers do not support this e.g. I.E. on WinXP.

### Unmerge

`root #``emerge --ask --depclean --verbose net-proxy/haproxy`
