<!-- source: https://wiki.gentoo.org/wiki/ZoneMinder | group: Gentoo Wiki (Main) | wiki-title: ZoneMinder -->
---
title: ZoneMinder
url: https://wiki.gentoo.org/wiki/ZoneMinder
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-27"
fingerprint: f794396a7fc6a3b7
license: CC BY-SA 4.0
---

# ZoneMinder

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ZoneMinder** is open source software used to capture, analyse, record, and monitor cameras.

## Installation

### Dependencies

This guide includes optional steps for configuring ZoneMinder with php-fpm.

The following required packages must be configured properly for ZoneMinder to work:

- Web Server
  - Apache 2: [www-servers/apache](https://wiki.gentoo.org/wiki/Apache2)
  - Nginx: [www-servers/nginx](https://wiki.gentoo.org/wiki/Nginx)
- MySQL: [dev-db/mariadb](https://wiki.gentoo.org/wiki/MySQL)
- PHP: [dev-lang/php](https://wiki.gentoo.org/wiki/PHP)

### USE flags

Select the web daemon you want to use. Apache2 is the default.

**`/etc/portage/package.use/zoneminder`**

Enable the following USE flags for [dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php), [app-eselect/eselect-php](https://packages.gentoo.org/packages/app-eselect/eselect-php), and [media-video/ffmpeg](https://packages.gentoo.org/packages/media-video/ffmpeg):

**`/etc/portage/package.use/zoneminder`**

### Unmask packages

There are a few unstable packages in the tree for \~amd64

**`/etc/portage/package.accept_keywords/zoneminder`**

### Apache Modules

Proxy is needed for FPM-PHP support.

**`/etc/portage/make.conf`**

### Emerge

Install MariaDB, this was decoupled from ebuild allowing users to run database on another host:

`root #``emerge --ask dev-db/mariadb`
Install ZoneMinder:

`root #``emerge --ask www-misc/zoneminder`
## Web Server

Upstream primarily develops support for Apache, Gentoo ebuild provides an alternative path using Nginx.

### Apache

- Select correct version of PHP:

`root #``eselect php list fpm`
\[1\]   php7.4
  \[2\]   php8.0 \*

Recent versions of ZoneMinder will warn if the local timezone is unset and reverts to using UTC, instead of throwing failures as seen with previous releases.

- Edit your active php module /etc/php/MODULE-VERSION)/php.ini to reflect the following:

**`/etc/php/fpm-php8.0/php.ini`**

```
date.timezone = <Country/City>
```
- Edit /etc/conf.d/apache2 and add `-D PHP -D PROXY` to `APACHE2_OPTS`:

**`/etc/conf.d/apache2`**

```
APACHE2_OPTS="-D DEFAULT_VHOST -D INFO -D SSL -D SSL_DEFAULT_VHOST -D LANGUAGE -D PHP -D PROXY"
```
- Edit /etc/apache2/modules.d/70\_mod\_php.conf to [activate PHP-FPM](https://wiki.gentoo.org/wiki/Apache#Enabling_PHP-FPM_through_mod_proxy_fcgi_in_Apache_2.4)

**`/etc/apache2/modules.d/70_mod_php.conf`**

```
        <FilesMatch "\.(php|php[57]|phtml)$">
                SetHandler "proxy:unix:/var/run/php-fpm/www.sock|fcgi://localhost"
        </FilesMatch>
...
```
- An example configuration file for Apache is placed under /usr/share/doc/zoneminder\*/

**`/etc/apache2/vhosts.d/zoneminder_vhost.conf`**

```
<VirtualHost *:80>
    ServerName localhost
    #ServerAlias zoneminder
    ErrorLog /var/log/apache2/zoneminder_error_log
    <IfModule log_config_module>
        CustomLog /var/log/apache2/zoneminder_access_log common
    </IfModule>
    RewriteEngine On
    RewriteCond %{REQUEST_URI} !^/\.well\-known/acme\-challenge/
    RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
</VirtualHost>
<VirtualHost *:443>
    ServerName localhost
    #ServerAlias zoneminder
    SSLEngine on
    SSLCertificateFile /etc/ssl/apache2/zoneminder.crt
    SSLCertificateKeyFile /etc/ssl/apache2/zoneminder.key
    # enable HTTP/2, if available
    Protocols h2 http/1.1
    # HTTP Strict Transport Security (mod_headers is required) (63072000 seconds)
    Header always set Strict-Transport-Security "max-age=63072000"
    ErrorLog /var/log/apache2/zoneminder_ssl_error_log
    <IfModule log_config_module>
        CustomLog /var/log/apache2/zoneminder_ssl_access_log common
    </IfModule>
    Include /etc/apache2/vhosts.d/zoneminder_vhost.include
</VirtualHost>
# modern configuration
SSLProtocol             all -SSLv3 -TLSv1 -TLSv1.1 -TLSv1.2
SSLHonorCipherOrder     off
SSLSessionTickets       off
SSLUseStapling On
SSLStaplingCache "shmcb:logs/ssl_stapling(32768)"
```
**`/etc/apache2/vhosts.d/zoneminder_vhost.include`**

```
# gentoo apache2 vhost include file
ServerAdmin webmaster@localhost
DocumentRoot "/usr/share/zoneminder/www"
## if zoneminder is the only thing on this named vhost you might want to
## redirect root / to /zm/ so you don't have to remember to type the path
#RedirectMatch ^/$ /zm/
# Order matters. This alias must come first.
Alias /zm/cache "/var/cache/zoneminder"
<Directory "/var/cache/zoneminder">
    Options -Indexes +FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
ScriptAlias /zm/cgi-bin "/usr/libexec/zoneminder/cgi-bin"
Alias /zoneminder "/usr/share/zoneminder/www"
Alias /zm "/usr/share/zoneminder/www"
<Directory "/usr/share/zoneminder/www">
    Options -Indexes +MultiViews +FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
<Directory "/usr/libexec/zoneminder/cgi-bin">
    Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
    AllowOverride All
    Require all granted
</Directory>
# For better visibility, the following directives have been migrated from the
# default .htaccess files included with the CakePHP project.
# Parameters not set here are inherited from the parent directive above.
<Directory "/usr/share/zoneminder/www/api">
    RewriteEngine on
    RewriteRule ^$ app/webroot/ [L]
    RewriteRule (.*) app/webroot/$1 [L]
    RewriteBase /zm/api
</Directory>
<Directory "/usr/share/zoneminder/www/api/app">
    RewriteEngine on
    RewriteRule ^$ webroot/ [L]
    RewriteRule (.*) webroot/$1 [L]
    RewriteBase /zm/api
</Directory>
<Directory "/usr/share/zoneminder/www/api/app/webroot">
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]
    RewriteBase /zm/api
</Directory>
```
#### FPM-PHP

/etc/php/fpm-php8.0/fpm.d/www.conf needs to be adjusted to listen on UNIX socket and run as Apache. While a separate UID/GUI could be created for this, the /var/log/zm/ and /etc/zm/conf.d/private.conf permissions would also need to be adjusted.

**`/etc/php/fpm-php8.0/fpm.d/www.conf`**

```
...
[www]
user = apache
group = apache
listen = /var/run/php-fpm/www.sock
listen.owner = apache
listen.group = apache
...
```


### Nginx

See [Nginx#Multiple\_site\_access](https://wiki.gentoo.org/wiki/Nginx#Multiple_site_access) for enabling multiple site support in /etc/nginx/nginx.conf

An example configuration file for Nginx is placed under /usr/share/doc/zoneminder\*/

**`/etc/nginx/conf.d/zoneminder.conf`**

```
server {
    listen 80;
    listen [::]:80;
    http2 on;
    server_name = localhost $hostname;
    access_log /var/log/nginx/zoneminder.access_log main;
    error_log /var/log/nginx/zoneminder.error_log info;
    location / {
        return 301 https://$host$request_uri;
    }
}
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    http2 on;
    server_name = localhost $hostname;
    root "/usr/share/zoneminder/www";
    access_log /var/log/nginx/zoneminder.ssl_access_log main;
    error_log /var/log/nginx/zoneminder.ssl_error_log info;
    ssl_certificate "/etc/ssl/nginx/zoneminder.crt";
    ssl_certificate_key "/etc/ssl/nginx/zoneminder.key";
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;
    # modern configuration
    ssl_protocols TLSv1.3;
    ssl_prefer_server_ciphers off;
    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;
    # Auto redirect to server/zm when no url suffix was given
    #location = / {
    #    return 301 zm;
    #}
    location /zm/cgi-bin {
        gzip off;
        alias "/usr/libexec/zoneminder/cgi-bin";
        include /etc/nginx/fastcgi_params_zoneminder;
        fastcgi_param SCRIPT_FILENAME $request_filename;
        fastcgi_pass unix:/run/fcgiwrap-zoneminder.sock;
    }
    location /zm/cache {
        alias "/var/cache/zoneminder";
    }
    location /zm {
        gzip    off;
        alias   "/usr/share/zoneminder/www";
        index   index.php;
        location ~ \.php$ {
            try_files           $uri =404;
            expires             epoch;
            include             /etc/nginx/fastcgi_params_zoneminder;
            fastcgi_param       SCRIPT_FILENAME         $request_filename;
            fastcgi_index       index.php;
            fastcgi_pass        unix:/run/php-fpm/www.sock;
        }
        location ~ \.(jpg|jpeg|gif|png|ico)$ {
            access_log  off;
            expires     33d;
        }
        location /zm/api/ {
            alias   "/usr/share/zoneminder/www";
            rewrite ^/zm/api(.+)$ /zm/api/app/webroot/index.php?p=$1 last;
        }
    }
}
```
#### FastCGI Params

We need to adjust the default SCRIPT\_FILENAME parameter for the above path overrides to work.

`root #``cp /etc/nginx/fastcgi_params /etc/nginx/fastcgi_params_zoneminder`
Comment out this line:

**`/etc/nginx/fastcgi_params_zoneminder`**

```
# fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
...
```
#### FPM-PHP

/etc/php/fpm-php8.3/fpm.d/www.conf needs to be adjusted to listen on UNIX socket and run as Apache. While a separate UID/GUI could be created for this, the /var/log/zm/ and /etc/zm/conf.d/99-local.conf permissions would also need to be adjusted.

**`/etc/php/fpm-php8.3/fpm.d/www.conf`**

```
...
[www]
user = nginx
group = nginx
listen = /run/php-fpm/www.sock
listen.owner = nginx
listen.group = nginx
...
```
#### Spawn-FCGI

Ngnix needs an external CGI processor.

The ebuild provides a custom www-servers/spawn-fcgi init.d script which incorporates www-servers/multiwatch to creates one unix socket for nginx and forks multiple children behind it for /usr/libexec/zoneminder/cgi-bin

ZoneMinder Montage page opens a connection to each camera stream, adjust FCGI\_CHILDREN for the number of cameras you have.

**`/etc/conf.d/spawn-fcgi.zoneminder`**

## Configuration

### MySQL

See [MariaDB](https://wiki.gentoo.org/wiki/MariaDB) for initial installation.

- Create ZoneMinder's database, `zm`, and setup a new user `zmuser`.

`root #``mysql -p`
mysql> create database zm;
mysql> grant lock tables,alter,drop,select,insert,update,delete,create,index,alter routine,create routine, trigger,execute on zm.\* to 'zmuser'@localhost identified by 'zmpass';
Query OK, 0 rows affected (0.00 sec)
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
mysql> exit;

- Import the database schema and base data, the .sql script is created by the configure phase above, so make sure you do that first.

`root #``mysql -p zm < /usr/share/zoneminder/db/zm_create.sql`
### ZoneMinder

To make custom changes, create a new configuration file, with an extension of .conf, under the /etc/zm/conf.d folder, containing your desired modifications. Adjust the permissions to protect credentials and make it readable by your web server.

For example, creating new /etc/zm/conf.d/99-local.conf

`root #````
touch /etc/zm/conf.d/99-local.conf
```
`root #````
chmod 640 /etc/zm/conf.d/99-local.conf
```
`root #````
chown root:apache /etc/zm/conf.d/99-local.conf
```
Configuration files

- Edit your new local configuration file and add the above MySQL database username and password:

**`/etc/zm/conf.d/99-local.conf`**

```
# ZoneMinder database user
ZM_DB_USER=zmuser
# ZoneMinder database password
ZM_DB_PASS=zmpass
```
## Init

### OpenRC

Add PHP to the default runlevel and start the service:

`root #````
rc-update add php-fpm default
```
`root #````
rc-service php-fpm start
```
Add MySQL to the default runlevel and start the service:

`root #````
rc-update add mysql default
```
`root #````
rc-service mysql start
```
Add ZoneMinder to the default runlevel and start:

`root #````
rc-update add zoneminder default
```
`root #````
rc-service zoneminder start
```
#### Apache

Add Apache to the default runlevel and start the service:

`root #````
rc-update add apache2 default
```
`root #````
rc-service apache2 start
```
#### Nginx

Add Nginx to the default runlevel and start the service:

`root #````
rc-update add nginx default
```
`root #````
rc-service nginx start
```
Add spawn-fcgi to the default runlevel and start the service:

`root #````
rc-update add spawn-fcgi.zoneminder default
```
`root #````
rc-service spawn-fcgi.zoneminder start
```
### systemd

Enable the Apache to start on system boot and start the service:

`root #````
systemctl enable apache2
```
`root #````
systemctl start apache2
```
Enable the PHP service to start on system boot and start the service:

`root #````
systemctl enable php-fpm
```
`root #````
systemctl start php-fpm
```
Enable the MySQL service to start on system boot and start the service:

`root #````
systemctl enable mysql
```
`root #````
systemctl start mysql
```
Enable the ZoneMinder service to start on system boot and start the service:

`root #````
systemctl enable zoneminder
```
`root #````
systemctl start zoneminder
```
## SELinux

When using [SELinux](https://wiki.gentoo.org/wiki/SELinux), first create a SELinux policy for the application:

**`local_zoneminder.te`**

**Type enforcement rules for ZoneMinder**

Now build the modules:

`root #````
checkmodule -M -m -o local_zoneminder.mod local_zoneminder.te 
```
`root #````
semodule_package -o local_zoneminder.pp -m local_zoneminder.mod
```
`root #````
semodule -i local_zoneminder.pp
```
## Other

Shared memory:

`root #``sysctl kernel.shmmax=536870912`
### Upgrading

ZoneMinder includes a Perl utility zmupdate.pl to update the database schema as needed. Note: a Perl interpreter will required to run the script.

Normally you just need to run zmupdate.pl without parameters, credentials are read from configurations under /etc/zm. If you have a unique upgrade situation, you can specify the previous version and database credentials for the `zm` database:

`root #````
/usr/bin/zmupdate.pl --version=1.30.2 --user=zmuser --pass=zmpass
```
Initiating database upgrade to version 1.30.4 from version 1.30.2
Please ensure that ZoneMinder is stopped on your system prior to upgrading the database.
Press enter to continue or ctrl-C to stop :
Do you wish to take a backup of your database prior to upgrading?
This may result in a large file in /var/tmp/zm if you have a lot of events.
Press 'y' for a backup or 'n' to continue : y
Creating backup to /var/tmp/zm/zm-1.30.2.dump. This may take several minutes.
Database successfully backed up to /var/tmp/zm/zm-1.30.2.dump, proceeding to upgrade.
Upgrading database to version 1.30.4
Loading config from DB
Saving config to DB
Upgrading DB to 1.30.3 from 1.30.2
Database successfully upgraded to version 1.30.3.
Upgrading DB to 1.30.4 from 1.30.2
Database successfully upgraded to version 1.30.4.
Database upgrade to version 1.30.4 successful.

## See also

- [PHP](https://wiki.gentoo.org/wiki/PHP) — a general-purpose server-side scripting language to produce dynamic web pages.
- [Apache](https://wiki.gentoo.org/wiki/Apache) — an efficient, extensible [web server](https://wiki.gentoo.org/wiki/Category:Web_Servers). It is one of the most popular web servers used the Internet.
- [Nginx](https://wiki.gentoo.org/wiki/Nginx) — a robust, small, high performance [web server](https://wiki.gentoo.org/wiki/Category:Web_servers) and reverse proxy server.
- [MariaDB](https://wiki.gentoo.org/wiki/MariaDB) — is an enhanced, drop-in [MySQL](https://wiki.gentoo.org/wiki/MySQL) replacement.
