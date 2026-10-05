<!-- source: https://wiki.gentoo.org/wiki/Nginx | group: Gentoo Wiki (Main) | wiki-title: Nginx -->
---
title: nginx
url: https://wiki.gentoo.org/wiki/Nginx
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-29"
fingerprint: ec2e3649f607b5c6
license: CC BY-SA 4.0
---

# nginx

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**NGINX** is a robust, small, high performance web server and reverse proxy server. It is a good alternative to popular web servers like [Apache](https://wiki.gentoo.org/wiki/Apache) and [lighttpd](https://wiki.gentoo.org/wiki/Lighttpd).

Before immediately installing the [www-servers/nginx](https://packages.gentoo.org/packages/www-servers/nginx) package, first take a good look at the USE flags for NGINX.

NGINX uses modules to enhance its features. To simplify the maintenance of this modular approach, the NGINX ebuild uses `[USE_EXPAND](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE_EXPAND)` flags to denote which modules should be installed.

- HTTP related modules can be enabled through the `NGINX_MODULES_HTTP` variable
- Stream (generic TCP/UDP proxying) related modules can be enabled through the `NGINX_MODULES_STREAM` variable
- Mail (POP3/IMAP4/SMTP proxying) related modules can be enabled through the `NGINX_MODULES_MAIL` variable

These variables need to be set in /etc/portage/package.use, if it is a file, or in a file inside it, for example /etc/portage/package.use/nginx. The variables descriptions can be found in [/var/db/repos/gentoo/profiles/desc/nginx\_modules\_http.desc](https://gitweb.gentoo.org/repo/gentoo.git/plain/profiles/desc/nginx_modules_http.desc), [/var/db/repos/gentoo/profiles/desc/nginx\_modules\_stream.desc](https://gitweb.gentoo.org/repo/gentoo.git/plain/profiles/desc/nginx_modules_stream.desc), and [/var/db/repos/gentoo/profiles/desc/nginx\_modules\_mail.desc](https://gitweb.gentoo.org/repo/gentoo.git/plain/profiles/desc/nginx_modules_mail.desc).

For example, to enable the `fastcgi` module:

**`/etc/portage/package.use/nginx`**

```
 NGINX_MODULES_HTTP: fastcgi
```

| [+http](https://packages.gentoo.org/useflags/+http) | Enable core HTTP support | 
| [+modules](https://packages.gentoo.org/useflags/+modules) | Enable loadable module support | 
| [aio](https://packages.gentoo.org/useflags/aio) | Enable asynchronous I/O support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable support for debugging log | 
| [libatomic](https://packages.gentoo.org/useflags/libatomic) | Use dev-libs/libatomic\_ops instead of builtin atomic operations | 
| [mail](https://packages.gentoo.org/useflags/mail) | Enable POP3/IMAP4/SMTP mail proxy server | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [stream](https://packages.gentoo.org/useflags/stream) | Enable generic TCP/UDP proxying and load balancing | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

With the USE flags set, install [www-servers/nginx](https://packages.gentoo.org/packages/www-servers/nginx):

`root #``emerge --ask www-servers/nginx`
The default NGINX configuration defines an HTTP virtual server listening on loopback address but does not define a root directory. To test out the installation, use an existing directory or create a new root directory, for example /var/www/localhost/htdocs:

`root #``mkdir -p /var/www/localhost/htdocs`
Then, uncomment the `root` directive inside the `server` block:

**`/etc/nginx/nginx.conf`**

**Setting the root directive**

```
server {
    listen 127.0.0.1;
    server_name localhost;
 
    # Substitute the directory below for the one you use.
    root /var/www/localhost/htdocs;
}
```
The NGINX package installs an init service script and a systemd unit allowing administrators to stop, start, or restart the service. If running OpenRC, issue the next command to start the NGINX service:

`root #``rc-service nginx start`
If using systemd, use the following command to start NGINX:

`root #``systemctl start nginx.service`
To verify that NGINX is properly running, point a web browser to [http://localhost](http://localhost) or use a command-line tool like curl:

`user $``curl http://localhost`
The NGINX configuration is specified in the /etc/nginx/nginx.conf file.

The following example shows a single-site access, without dynamic capabilities (such as [PHP](https://wiki.gentoo.org/wiki/PHP)).

**`/etc/nginx/nginx.conf`**

**Gentoo's default configuration**

```
user nginx nginx;
worker_processes auto;
 
events {
    # NGINX refuses to start if the 'events' section is not present. Yet,
    # NGINX does not seem to care whether this section is non-empty.
}
 
http {
    # Maximum hash table size is increased to accommodate for a large
    # mime.types file that is shipped on Gentoo.
    types_hash_max_size 4096;
    include /etc/nginx/mime.types.nginx;
 
    sendfile on;
 
    server {
        listen 127.0.0.1;
        server_name localhost;
 
        # Substitute the directory below for the one you use.
        root /var/www/localhost/htdocs;
    }
}
```
It is possible to leverage the `include` directive to split the configuration in multiple files:

**`/etc/nginx/nginx.conf`**

**Multisite configuration**

```
user nginx nginx;
worker_processes auto;
 
events {
    # NGINX refuses to start if the 'events' section is not present. Yet,
    # NGINX does not seem to care whether this section is non-empty.
}
 
http {
    # Maximum hash table size is increased to accommodate for a large
    # mime.types file that is shipped on Gentoo.
    types_hash_max_size 4096;
    include /etc/nginx/mime.types.nginx;
 
    sendfile on;
 
    include /etc/nginx/conf.d/*.conf;
}
```
**`/etc/nginx/conf.d/local.conf`**

**Simple host**

```
server {
    listen 127.0.0.1;
    server_name localhost;
 
    root /var/www/localhost/htdocs;
}
```
**`/etc/nginx/conf.d/local-ssl.conf`**

**Simple SSL host**

```
server {
    # Specifying port with no address.
    listen 443 ssl;
    server_name host.tld;
    ssl_certificate /etc/ssl/nginx/host.tld.pem;
    ssl_certificate_key /etc/ssl/nginx/host.tld.key;
}
```
Add the following lines to the NGINX configuration to enable PHP support. In this example NGINX is exchanging information with the PHP process via a UNIX socket.

**`/etc/nginx/nginx.conf`**

**Enabling PHP support**

```
# ...
http {
# ...
    server {
    # ...
        location ~ \.php$ {
            # Test for non-existent scripts or throw a 404 error
            # Without this line, nginx will blindly send any request ending in .php to php-fpm
            try_files $uri =404;
            include /etc/nginx/fastcgi_params;
            fastcgi_pass unix:/run/php-fpm.socket;
        }
    }
}
```
To support this setup, PHP needs to be built with FastCGI Process Manager support ([dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php)), which is handled through the `fpm` USE flag:

`root #``echo "dev-lang/php fpm" >> /etc/portage/package.use/php`
Rebuild PHP with the `fpm` USE flag enabled:

`root #``emerge --ask dev-lang/php`
For PHP 7.0 and newer PHP versions use following configuration:

**`/etc/php/fpm-php8.2/fpm.d/www.conf`**

**Running PHP with UNIX socket support**

```
listen = /run/php-fpm.socket
listen.owner = nginx
```
Set the timezone in the php-fpm php.ini file. Substitute the `<PUT_TIMEZONE_HERE>` text in the FileBox below with the appropriate timezone information:

**`/etc/php/fpm-php8.2/php.ini`**

**Setup timezone in php.ini**

```
date.timezone = <PUT_TIMEZONE_HERE>
```
Start the php-fpm daemon:

`root #``rc-service php-fpm start`
Add php-fpm to the default runlevel:

`root #``rc-update add php-fpm default`
Restart nginx with changed configuration:

`root #``rc-service nginx restart`
Alternatively, for systemd:

`root #````
systemctl enable php-fpm@8.2
```
`root #````
systemctl start php-fpm@8.2
```
`root #``systemctl restart nginx.service`
The next example shows how to allow access to a particular URL (in this case /nginx\_status) only to:

- certain hosts (e.g. *192.0.2.1 127.0.0.1*)
- and IP networks (e.g. *198.51.100.0/24*)

**`/etc/nginx/nginx.conf`**

**Enabling and configuring an IP access lists for /nginx\_status page**

```
http {
    server {
        location /nginx_status {
            stub_status on;
            allow 127.0.0.1/32;
            allow 192.0.2.1/32;
            allow 198.51.100.0/24;
            deny all;
        }
    }
}
```
NGINX allows limiting access to resources by validating the user name and password:

**`/etc/nginx/nginx.conf`**

**Enabling and configuring user authentication for the / location**

```
http {
    server {
        location / {
            auth_basic "Authentication failed";
            auth_basic_user_file domain.htpasswd;
        }
    }
}
```
The domain.htpasswd file can be generated using:

`user $``echo -n 'foo:' >> domain.htpasswd`
This will create the domain.htpasswd file, containing a row for the user 'foo'.

`user $``openssl passwd >> domain.htpasswd`
This will add the password to the line for the user 'foo'. The password will be asked on the standard input. Once it's over, the file could be opened and will contain something like this:

**`/etc/nginx/domain.htpasswd`**

**Content of the domain.htpasswd file, for user foo with a ciphered password**

The password is not in plain text, rather it is encrypted with OpenSSL.

The GeoIP2 module makes use of GeoIP2 databases by [Maxmind](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data?lang=en) or similar. Using Maxmind is already supported in Gentoo through [net-misc/geoipupdate](https://packages.gentoo.org/packages/net-misc/geoipupdate). However, [registration of an account](https://www.maxmind.com/en/geolite2/signup) is required in order to obtain a free license key and download the free database.

Once an account is created, install and configure geoipupdate:

`root #``emerge --ask net-misc/geoipupdate`
Enter the account and license key:

**`/etc/GeoIP.conf`**

**Add your account info**

After that, you'll need to download the databases:

`root #````
geoipupdate
```
In order receive updates automatically in the future, add this command to a weekly cronjob or systemd timer.

To enable to modules and rebuild NGINX:

**`/etc/portage/package.use/nginx`**

**Add the modules to NGINX**

Rebuild NGINX with the third party modules enabled:

`root #``emerge --ask www-servers/nginx`
Once NGINX has been rebuild, point NGINX to the databases and the GeoIP2 variables:

**`/etc/nginx/nginx.conf`**

**Pointing to the GeoIP2 databases and its values**

The `auto_reload` option will allow updating the database without restarting NGINX.

For the GeoIP2 values to show up in a PHP application, assign them as **fastcgi\_param**

**`/etc/nginx/fastcgi.conf`**

**Add GeoIP2 support to PHP**

Start NGINX web server:

`root #``rc-service nginx start`
Stop NGINX web server:

`root #``rc-service nginx stop`
Add NGINX to the default runlevel so that the service starts automatically on system reboot:

`root #``rc-update add nginx default`
Reload NGINX configuration without dropping connections:

`root #``rc-service nginx reload`
Restart the NGINX service:

`root #``rc-service nginx restart`
Start NGINX web server:

`root #``systemctl start nginx`
Stop NGINX web server:

`root #``systemctl stop nginx`
Check the status of the service:

`root #``systemctl status nginx`
Enable service to start automatically on system reboot:

`root #``systemctl enable nginx`
Reload NGINX configuration without dropping connections:

`root #``systemctl reload nginx`
Restart the NGINX service:

`root #``systemctl restart nginx`
In case of problems, the following commands can help troubleshoot the situation.

Verify that the running NGINX configuration has no errors:

`root #``rc-service nginx configtest`
nginx                     | \* Checking NGINX's configuration ...
nginx                     |nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx                     |nginx: configuration file /etc/nginx/nginx.conf test is successful            \[ ok \]

Alternatively, if using systemd:

`root #``/usr/sbin/nginx -t`
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

By running nginx with the `-t` option, it will validate the configuration file without actually starting the nginx daemon. Use the `-c` option with the full path to the file to test configuration files in non-default locations. See nginx(8) for details.

Check if nginx processes are running:

`user $``ps aux | egrep 'nginx|PID'`
PID TTY      STAT   TIME COMMAND
26092 ?        Ss     0:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
26093 ?        S      0:00 nginx: worker proces

Verify NGINX daemon is listening on the right TCP port (such as 80 for HTTP or 443 for HTTPS):

`root #``ss -tulpn | grep :80````
tcp   LISTEN 0      0          0.0.0.0:80         0.0.0.0:*    users:(("nginx",pid=6253,fd=52),("nginx",pid=6252,fd=52))
```
- [Apache](https://wiki.gentoo.org/wiki/Apache) — an efficient, extensible [web server](https://wiki.gentoo.org/wiki/Category:Web_Servers). It is one of the most popular web servers used the Internet.
- [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) — a fast and lightweight [web server](https://wiki.gentoo.org/wiki/Category:Web_servers).

- [https://nginx.org/en/docs/beginners\_guide.html](https://nginx.org/en/docs/beginners_guide.html) - A nginx beginner's guide. Helpful for those who do not know much about nginx.
- [https://github.com/nginxinc/nginx-wiki](https://github.com/nginxinc/nginx-wiki) - The archived NGINX wiki.
- [https://github.com/h5bp/server-configs-nginx](https://github.com/h5bp/server-configs-nginx) - H5BP nginx config.
- [https://gentoo.org/support/news-items/2025-07-05-nginx-packaging-changes.html](https://gentoo.org/support/news-items/2025-07-05-nginx-packaging-changes.html)
