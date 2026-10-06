<!-- source: https://wiki.gentoo.org/wiki/Collectd-web | group: Gentoo Wiki (Main) | wiki-title: Collectd-web -->
---
title: collectd-web
url: https://wiki.gentoo.org/wiki/Collectd-web
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-21"
fingerprint: adf2104abf837f85
license: CC BY-SA 4.0
---

# collectd-web

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**collectd-web** is a web-based Perl CGI front-end for RRD data collected by [collectd](https://wiki.gentoo.org/wiki/Collectd).

## Installation

### Emerge

Install [www-apps/collectd-web](https://packages.gentoo.org/packages/www-apps/collectd-web):

`root #``emerge --ask collectd-web`
To display the graphs, [net-analyzer/rrdtool](https://packages.gentoo.org/packages/net-analyzer/rrdtool) needs the USE flags `graph` and `perl`.

### Web server

You need also a [web server](https://wiki.gentoo.org/wiki/Category:Web_servers).

The CGI scripts have to be executable:

`root #``chmod +x /usr/share/webapps/collectd-web/*/hostroot/cgi-bin/*.cgi`
#### Built-in web server

The collectd-web project offers a simple web server to process the CGI scripts:

`root #````
cd /usr/share/webapps/collectd-web/*/hostroot/
```
`root #````
python2 runserver <ip> <port>
```
Now you need to set up a reverse proxy which will serve static files from /usr/share/webapps/collectd-web/0.4.0/htdocs for / and for /cgi-bin/ it will proxy the request to the above set up \<ip> \<port>.

#### Apache

It is possible to use [Apache](https://wiki.gentoo.org/wiki/Apache) to serve the pages. Configuration of the corresponding virtual host needs to be adjusted so that it knows where to find cgi scripts of collectd-web. For default vhost make following modification:

**`/etc/apache2/vhosts.d/00_default_vhost.conf`**

**Defining alias to cgi scripts location**

```
<VirtualHost *:80>
...
    ScriptAlias /collectd-web/cgi-bin/ "/var/www/localhost/cgi-bin/"
...
</VirtualHost>
```
#### nginx

This is an example server section for [nginx](https://wiki.gentoo.org/wiki/Nginx) using [www-misc/fcgiwrap](https://packages.gentoo.org/packages/www-misc/fcgiwrap). This setup does not require using the built-in webserver and downloading the extra python script.

`root #``emerge --ask fcgiwrap``root #````
systemctl enable fcgiwrap.socket
```
`root #````
systemctl start fcgiwrap.socket
```
**`/etc/nginx/conf.d/collectd-web.conf`**

**Example nginx confiugration for collectd**

```
server {
    listen 80;
    large_client_header_buffers 4 16k; # for large cookies and headers
    location / {
        root  /usr/share/webapps/collectd-web/0.4.0/htdocs; # this serves the static files
    }
    location /cgi-bin/ {
        gzip off;
        root  /usr/share/webapps/collectd-web/0.4.0/hostroot; # this is the parent directory of cgi-bin
        fastcgi_pass  unix:/var/run/fcgiwrap.sock;
        include /etc/nginx/fastcgi_params;
        fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }
}
```
Save this as a separate file as suggested above and include it in /etc/nginx/nginx.conf and restart nginx:

`root #``systemctl restart nginx`
The web interface is then available on [http://localhost](http://localhost)

## Configuration

Configure [Collectd#rrdtool\_plugin](https://wiki.gentoo.org/wiki/Collectd#rrdtool_plugin)

Define where collectd-web shall look for rrd files:

**`/etc/collectd/collection.conf`**

```
datadir: "/var/lib/collectd/rrd"
```
## Usage

Point your browser at the reverse proxy or (in case you using default Apache configuration) at [http://localhost/collectd-web](http://localhost/collectd-web).
