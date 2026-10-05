<!-- source: https://wiki.gentoo.org/wiki/ProxyAutoConfig | group: Gentoo Wiki (Main) | wiki-title: ProxyAutoConfig -->
---
title: ProxyAutoConfig
url: https://wiki.gentoo.org/wiki/ProxyAutoConfig
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-01-31"
fingerprint: f6fddc51bc022881
license: CC BY-SA 4.0
---

# ProxyAutoConfig

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## How it works

Many web clients have the possibility to detect proxy settings for their current network automatically. This can be done via

- DHCP
- WPAD
- manual URL configuration

Once the proxy autoconfiguration file is obtained, clients evaluate it to see how to connect to a proxy server.

### The Proxy Auto Configuration (PAC) file

A PAC file is a simple javascript file clients can evaluate to get their configuration. For each request, the client executes the javascript, passing along the URL and host name it would like to make the request to. The script will return a proxy name to use for this server/URL, or DIRECT if there is no proxy for this host or protocol.

### Getting the PAC file to the client

In order to get to the PAC file, a client can use several options:

- local file
- The pac file can be stored locally and applications can be pointed to that file
- remote file
- Applications can be pointed to an URL which serves out the PAC file.
- DHCP
- DHCP servers can provide information where a PAC file is available
- WPAD
- Following a set of conventions, clients can automagically obtain the correct PAC file for the network they're currently in

The last two options are explained in this article, because once a client is set up it can work the same way regardless which network it is in.

#### Web Proxy Auto-Discovery (WPAD)

WPAD works like this:

A client tries to figure out is domain name by stripping its own host name from the FQDN it got from DHCP (or wherever). It will then try to contact a HTTP server by the name of wpad.\<domain>. If it can't find one, and the domain name has one ore more subdomains, it will strip the first subdomain and try again to find a server named wpad.\<domain> up until the top-level domain is reached.

From those HTTP servers it will request a file called /wpad.dat which should be a PAC file like the one created above.

For example:

| Client Name: | laptop.office.corporate.example.org | 
| First Server tried: | [http://wpad.office.corporate.example.org/wpad.dat](http://wpad.office.corporate.example.org/wpad.dat) | 
| Second Server tried: | [http://wpad.corporate.example.org/wpad.dat](http://wpad.corporate.example.org/wpad.dat) | 
| Last Server tried | [http://wpad.example.org/wpad.dat](http://wpad.example.org/wpad.dat) | 

## Creating the PAC file

For details on which commands are supported in this file, see:

A simple PAC file looks like this:

**`proxy.pac`**

```
function FindProxyForURL(url, host) {
    var proxy = "PROXY proxy.example.org:8080; DIRECT";
    var direct = "DIRECT";
    // no proxy for local hosts without domain:
    if(isPlainHostName(host)) return direct;
    //We only cache http
     if (
         url.substring(0, 4) == "ftp:" ||
         url.substring(0, 6) == "rsync:"
        )
    return direct;
    // proxy everything else:
    return proxy;
}
```
The [pacparser](https://github.com/manugarg/pacparser) tool can be used to test that the PAC file is functioning correctly.

Example:

/usr/bin/pactester -p proxy.pac -u http://www.gentoo.org -h gentoo.org
PROXY proxy.example.org:8118; DIRECT
/usr/bin/pactester -p proxy.pac -u rsync://rsync.gentoo.org -h gentoo.org
DIRECT

If the return value of the script is DIRECT, the client won't use a proxy. The line "PROXY proxy.example.org:8080; DIRECT" will tell clients to first try to use the host proxy.example.org at port 8080 as a proxy, and if that fails, go direct. Multiple PROXY strings can be provided for redundancy or load balancing.

## DHCP server configuration

Some operating systems can use information provided via DHCP to obtain the proxy autoconfiguration file. Here is how to make the ISC dhcpd server ([net-misc/dhcp](https://packages.gentoo.org/packages/net-misc/dhcp)) serve this information:

In dhcpd.conf in the general section define a new option with code 252 and in the section for the network provide the value of the config server valid for that network.

**`/etc/dhcp/dhcpd.conf`**

## DNS server configuration

The responsible DNS Server must have records for the wpad.\<domain> servers. How to set up an or configure a DNS server is out of scope here, but a simple modification to the records of ISC bind would look like this:

**`/etc/bind/pri/example.org.zone`**

## Serving the WPAD file

Now that there is a PAC file and DNS points to the correct server, all that is left is actually serving the file to clients.

### Apache setup

To use [www-servers/apache](https://packages.gentoo.org/packages/www-servers/apache), configure a (virtual) host which will respond to the wpad server name, and serve out the PAC file.

**`/etc/apache2/vhosts.d/00_server1.example.org.conf`**

```
NameVirtualHost 192.168.0.1:80
Listen 192.168.0.1:80
NameVirtualHost 192.168.0.1:80
<VirtualHost 192.168.0.1:80>
        ServerName server1.example.org
        ServerAlias wpad.example.org
        DocumentRoot "/var/www/example.org/htdocs"
        <Directory "/var/www/example.org/htdocs">
                AllowOverride All
                Options MultiViews FollowSymlinks SymLinksIfOwnerMatch IncludesNoExec
                Options -Indexes
                Order allow,deny
                Allow from all
        </Directory>
        # serve proxy autoconfig correctly:
        <Files "wpad.dat)">
                AddType application/x-ns-proxy-autoconfig dat
        </Files>
        <Files "proxy.pac">
                AddType application/x-ns-proxy-autoconfig pac
        </Files>
</VirtualHost>
```
Now all that is left is to copy our PAC file to /var/www/example.org/htdocs/. The author likes to call the file 'proxy.pac' because that name is used in lots of documentation. Add a symbolic link called wpad.dat to satisfy the WPAD naming convention.

`root #``ln -s proxy.pac wpad.dat`
## Configuring clients

| Application | Steps | Screenshot | Documentation | 
|---|---|---|---|
| Firefox | In the Preferences Window, chose Advanced, go to the Network Tab, click the Settings... button. | ![FFProxyAutoConfig.png](https://wiki.gentoo.org/images/thumb/8/8a/FFProxyAutoConfig.png/240px-FFProxyAutoConfig.png) | [Firefox Documentation](http://support.mozilla.org/en-US/kb/settings-network-updates-and-encryption?s=proxy&r=4&e=es&as=s#w_network-tab) | 
| Opera | Press Alt+P to bring up the Preferences, go to the Advanced Tab, chose Networking and click the Proxy Servers... button. | ![OpProxyAutoConfig.png](https://wiki.gentoo.org/images/thumb/b/b9/OpProxyAutoConfig.png/240px-OpProxyAutoConfig.png) | [Opera Documentation](http://www.opera.com/support/kb/view/332/) | 
| KDE | In System Settings, search for proxy, the first section is the proxy settings. | ![KDEProxyAutoConfig.png](https://wiki.gentoo.org/images/thumb/0/0a/KDEProxyAutoConfig.png/240px-KDEProxyAutoConfig.png) | [KDE Documentation](http://docs.kde.org/cgi-bin/desktopdig/search.cgi?show=stable/en/kde-runtime/kcontrol/proxy/index.html&collection=en&include=stable&q=proxy) | 
| GNOME | see link | -- no pic -- | [GNOME Documentation](http://library.gnome.org/users/gnome-help/stable/net-proxy.html.en) | 
| Windows/Internet Explorer | If you use the DHCP method Windows probably does the right thing automatically. Otherwise follow the link on the right | too much of a clickfest to provide a single screenshot. | [Microsoft Documentation](http://support.microsoft.com/kb/135982) | 

## Next steps

To finish the setup a proxy server would be needed. Some popular proxy servers are:
