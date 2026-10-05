<!-- source: https://wiki.gentoo.org/wiki/Systemd/systemd-resolved | group: Gentoo Wiki (Main) | wiki-title: Systemd/systemd-resolved -->
---
title: Systemd/systemd-resolved
url: https://wiki.gentoo.org/wiki/Systemd/systemd-resolved
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-02"
fingerprint: cf9870fb2992b3c2
license: CC BY-SA 4.0
---

# Systemd/systemd-resolved

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

systemd provides a address name resolution (DNS) daemon which can be used in conjunction with systemd-networkd.

To have systemd manage address name resolution, replace /etc/resolv.conf file with a symlink to systemd-resolved's stub resolver and (re)start the systemd-resolved service:

`root #````
ln -sf ../run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```
`root #````
systemctl enable --now systemd-resolved.service
```
A resolvectl user space utility exists to control the name resolution manager for systems running systemd-resolved.

`user $``resolvectl --help````
resolvectl [OPTIONS...] COMMAND ...                                             
                                                                                
Send control commands to the network name resolution manager, or
resolve domain names, IPv4 and IPv6 addresses, DNS records, and services. 
                                                                                
Commands:                                                                       
  query HOSTNAME|ADDRESS...    Resolve domain names, IPv4 and IPv6 addresses
  service [[NAME] TYPE] DOMAIN Resolve service (SRV)                  
  openpgp EMAIL@DOMAIN...      Query OpenPGP public key                
  tlsa DOMAIN[:PORT]...        Query TLS public key                                                                                                             
  status [LINK...]             Show link and server status
  statistics                   Show resolver statistics                        
  reset-statistics             Reset resolver statistics
  flush-caches                 Flush all local DNS caches                
  reset-server-features        Forget learnt DNS server feature levels                                                                                          
  monitor                      Monitor DNS queries
  dns [LINK [SERVER...]]       Get/set per-interface DNS server address
  domain [LINK [DOMAIN...]]    Get/set per-interface search domain             
  default-route [LINK [BOOL]]  Get/set per-interface default route flag
  llmnr [LINK [MODE]]          Get/set per-interface LLMNR mode          
  mdns [LINK [MODE]]           Get/set per-interface MulticastDNS mode
  dnsovertls [LINK [MODE]]     Get/set per-interface DNS-over-TLS mode
  dnssec [LINK [MODE]]         Get/set per-interface DNSSEC mode
  nta [LINK [DOMAIN...]]       Get/set per-interface DNSSEC NTA                                                                                                 
  revert LINK                  Revert per-interface configuration
  log-level [LEVEL]            Get/set logging threshold for systemd-resolved
Options:
  -h --help                    Show this help
     --version                 Show package version
     --no-pager                Do not pipe output into a pager
  -4                           Resolve IPv4 addresses
  -6                           Resolve IPv6 addresses
  -i --interface=INTERFACE     Look on interface
  -p --protocol=PROTO|help     Look via protocol
  -t --type=TYPE|help          Query RR with DNS type
  -c --class=CLASS|help        Query RR with DNS class
     --service-address=BOOL    Resolve address for services (default: yes)
     --service-txt=BOOL        Resolve TXT records for services (default: yes)
     --cname=BOOL              Follow CNAME redirects (default: yes)
     --validate=BOOL           Allow DNSSEC validation (default: yes)
     --synthesize=BOOL         Allow synthetic response (default: yes)
     --cache=BOOL              Allow response from cache (default: yes)
     --zone=BOOL               Allow response from locally registered mDNS/LLMNR 
                               records (default: yes)
     --trust-anchor=BOOL       Allow response from local trust anchor (default:
                               yes)
     --network=BOOL            Allow response from network (default: yes)
     --search=BOOL             Use search domains for single-label names (default:
                               yes)
     --raw[=payload|packet]    Dump the answer as binary data
     --legend=BOOL             Print headers and additional info (default: yes)
     --json=MODE               Output as JSON
  -j                           Same as --json=pretty on tty, --json=short
                               otherwise 
See the resolvectl(1) man page for details.
```
### Systemd-resolved won't resolve hostnames without domain declared in external DNS servers

To fix this error, put the following line in the Resolve section of the configuration file:

**`/etc/systemd/resolved.conf`**

- [systemd/systemd-networkd](https://wiki.gentoo.org/wiki/Systemd/systemd-networkd) — configuration of wired network interfaces.
- [systemd](https://wiki.gentoo.org/wiki/Systemd) — a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems.
- [Network management](https://wiki.gentoo.org/wiki/Network_management) — describes possibilities for managing the network stack.

## External resources

- [Systemd-resolved](https://wiki.archlinux.org/title/Systemd-resolved) - Archlinux wiki article
