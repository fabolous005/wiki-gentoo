<!-- source: https://wiki.gentoo.org/wiki/Resolv.conf | group: Gentoo Wiki (Main) | wiki-title: Resolv.conf -->
---
title: resolv.conf
url: https://wiki.gentoo.org/wiki/Resolv.conf
hostname: gentoo.org
sitename: resolv.conf
date: "2025-06-20"
fingerprint: "9a4163592c36bb82"
license: CC BY-SA 4.0
---

# resolv.conf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The /etc/resolv.conf file is used to configure hostname resolution. It may be manually configured by the system administrator, or may be automatically populated by various network configuration and management tools. It is commonly used to manage DNS requests on Linux systems.

## Available software

Many tools exist to help manage the /etc/resolv.conf file (or symlink).

### DHCPCD

By default, [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) overwrites /etc/resolv.conf with the suggested configuration of the connected network. This behavior can be disabled by modifying /etc/dhcpcd.conf:

**`/etc/dhcpcd.conf`**

Alternatively, one may edit /etc/resolv.conf.head or /etc/resolv.conf.tail, which will be appended to the start or end of /etc/resolv.conf, respectively.

### NetworkManager

#### OpenRC

When using [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) on an OpenRC profile, /etc/resolv.conf should be a symlink pointing at /run/NetworkManager/resolv.conf. This will likely not result in an automatically managed /etc/resolv.conf file for the system, unless the `resolvconf` USE flag has been enabled for [net-misc/networkmanager](https://packages.gentoo.org/packages/net-misc/networkmanager).

For information on using different DNS plugins with NetworkManager see the dns and rc-manager parts under MAIN SECTION of man 5 NetworkManager.conf.

#### systemd

When using a systemd profile, systemd-resolved is a natural choice to be used as a DNS resolver for NetworkManager. It is enabled by starting the service and creating a symlink (see below).

If migrating from a different profile, the /etc/resolv.conf file should be removed, then a symlink created to the systemd resolver:

`root #````
rm /etc/resolv.conf
```
`root #````
systemctl enable --now systemd-resolved
```
`root #````
ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```
`root #````
systemctl restart NetworkManager
```
`root #````
ls -l /etc/resolv.conf
```
lrwxrwxrwx 1 root root 32 Jan 12 14:09 /etc/resolv.conf -> /run/systemd/resolve/stub-resolv.conf

### openresolv

The [net-dns/openresolv](https://packages.gentoo.org/packages/net-dns/openresolv) package integrates with several other packages to dynamically update /etc/resolv.conf when network configuration changes occur. When using openresolv, /etc/resolv.conf should be a *regular file*.

When the resolvconf command provided by this package is installed, many programs will use this instead of their own plumbing to create and update the /etc/resolv.conf file. To prevent resolvconf from updating the file, the /etc/resolvconf.conf file can be modified:

**`/etc/resolvconf.conf`**

### systemd-resolved

There are multiple options for setting up a resolv.conf symlink to work with systemd-resolved.

Two main features are supported:

- Stub resolver (Recommended): the nameserver is pointed at systemd-resolved, listening on 127.0.0.53. DNS queries are executed by systemd-resolved.
- Search path: The "search" directive is used to configure default domain names to be used when resolving single-word DNS queries.

The available features depend on which target is used for the symlink.

| Symlink target | Stub resolver | Search path | 
|---|---|---|
| /run/systemd/resolve/stub-resolv.conf |  |  | 
| /run/systemd/resolve/resolv.conf |  |  | 
| /lib/systemd/resolv.conf |  |  | 

### Netifrc

When using [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) to configure static addresses it is also possible to specific dns configuration like so

**`/etc/conf.d/net`**

where the IP address and domain are just examples, and eth0 refers to the associated interface

[Netifrc](https://wiki.gentoo.org/wiki/Netifrc) will then create a /etc/resolv.conf from these settings

## Configuration

### resolv.conf file syntax

The syntax of /etc/resolv.conf is relatively straight-forward. Each line specifies an option: most commonly `nameserver`, `search`, or `domain`. Not every line is necessary; it is entirely possible to have a blank file, while maintaining a working system.

#### nameserver

The nameserver option takes the IP address that the resolver should query, either in IPv4 or IPv6 form. Multiple nameserver lines may be specified, but each line must have only one IP address. A maximum of three lines should be present in total. If more than one IP address is present, the resolver queries them in the order listed. If no nameserver entries are present, it will default to 127.0.0.1.

#### search

The search option accepts up to six domains, separated by tabs or spaces. This is used if the resolver receives a request without any dots in it. It will then try to resolve the query with the search domain appended, then the original request. For example, if the search domain was example.com, and http://test/index.html was requested, it would first query http://test.example.com/index.html, and then http://test/index.html.

#### domain

The domain directive is an obsolete name for the search directive that handles one search list entry only. For more syntax options, check man 5 resolv.conf.

### Example configuration

**`/etc/resolv.conf`**

[1.1.1.1](https://one.one.one.one) and [8.8.8.8](https://developers.google.com/speed/public-dns) are popular DNS servers hosted by Cloudflare and Google, respectively.

## Troubleshooting

### Control of /etc/resolv.conf

To determine which network management utility is controlling the /etc/resolv.conf file, two methods exist:

#### head

Typically, the network management utility will write a comment at the top of the file specifying the name of the managing program. This can be checked using head:

`user $``head -n 1 /etc/resolv.conf`
\# Generated by resolvconf

#### symlink target

/etc/resolv.conf will either be a regular file or a symlink to another file. When a symlink, it is easy to reveal what network management tool is controlling the file by looking at the printed path to the target:

`user $``ls -l /etc/resolv.conf`
lrwxrwxrwx 1 root root 32 Jan 12 14:09 /etc/resolv.conf -> /run/systemd/resolve/resolv.conf

## External resources

- [https://zwischenzugs.com/2018/06/08/anatomy-of-a-linux-dns-lookup-part-i/](https://zwischenzugs.com/2018/06/08/anatomy-of-a-linux-dns-lookup-part-i/) - An excellent blog series that describes how DNS works on Linux systems.
- [https://wiki.archlinux.org/index.php/Resolv.conf](https://wiki.archlinux.org/index.php/Resolv.conf)
- [https://forums.gentoo.org/viewtopic-t-1118492.html](https://forums.gentoo.org/viewtopic-t-1118492.html)
