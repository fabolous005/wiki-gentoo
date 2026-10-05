<!-- source: https://wiki.gentoo.org/wiki/PhpVirtualBox | group: Gentoo Wiki (Main) | wiki-title: PhpVirtualBox -->
---
title: phpVirtualBox
url: https://wiki.gentoo.org/wiki/PhpVirtualBox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-08-06"
fingerprint: ff09c67add218552
license: CC BY-SA 4.0
---

# phpVirtualBox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**phpVirtualBox** is a web-based administration utility for [VirtualBox](https://wiki.gentoo.org/wiki/VirtualBox).

## Installation

Currently, [app-emulation/phpvirtualbox](https://packages.gentoo.org/packages/app-emulation/phpvirtualbox) has no stable version, so you need to add it to your package.accept\_keywords file.

`root #``emerge --ask app-emulation/phpvirtualbox`
## Configuration

phpVirtualBox does not need a VirtualBox installation to be present on the same box where phpVirtualBox is installed. phpVirtualBox can connect to other machines where your virtual machines are installed. phpVirtualBox requires a web-server to be present on your box, so we will demonstrate an Apache configuration using SSL certificate to encrypt your communication with the server.

### Verify VirtualBox installation

VirtualBox **must** be compiled with the `vboxwebsrv` USE flag on the host where the virtual machines are located.

**`/etc/portage/package.use`**

### Create SSL certificate

First, we create our shiny new SSL certificate

- Create our **secret** private key

`root #``openssl genrsa -des3 -out virtualbox-server.key 4096`
- You probably want to remove your private key, otherwise apache will ask you for it every time you start it

`root #``openssl rsa -in virtualbox-server.key -out virtualbox-server.key`
- Create our new Certificate Singing Request (CSR)

`root #``openssl req -new -key virtualbox-server.key -out virtualbox-key.csr`
- Sign the CSR using our private key

`root #``openssl x509 -req -days 365 -in virtualbox-key.csr -signkey virtualbox-server.key -out virtualbox-server.crt`
- Place certificate and private key somewhere safe

`root #``cp virtualbox-server.{key,crt} /etc/apache2/ssl-data/.`
### Configure Apache virtual host

Assuming you already have a working Apache installation, you need to create a new virtualhost for your phpVirtualBox installation. This entry should look like this:

**`/etc/apache2/vhost.d/99-virtualbox.conf`**

**phpVirtualBox vhost configuation**

```
<VirtualHost *:443>
        ServerName phpvirtualbox.mydomain.example # Change this to your domain name
        DocumentRoot /usr/share/webapps/phpvirtualbox/4.1.7/htdocs/
        SSLEngine On
        SSLOptions  StrictRequire
        SSLProtocol all -SSLv2
        SSLCertificateFile /etc/apache2/ssl-data/virtualbox-server.crt
        SSLCertificateKeyFile /etc/apache2/ssl-data/virtualbox-server.key
        <Directory />
                AllowOverride All
        </Directory>
        <Location />
                Options Indexes FollowSymLinks
                Order deny,allow
                SSLRequireSSL
        </Location>
        ErrorLog /var/log/apache2/virtualbox_error.log
</VirtualHost>
```
The previous configuration is just an example, your installation/configuration may be a lot different.

### Configure phpVirtualBox installation

The key file for your phpVirtualBox installation is the config.php file deriving from config.php-example file

`root #````
cd /usr/share/webapps/phpvirtualbox/4.1.7/htdocs
```
`root #````
cp config.php-example config.php
```
Edit this file and change the following variables

**`/usr/share/webapps/phpvirtualbox/4.1.7/htdocs/config.php`**

**PHPVirtualBox configuation**

```
var $username = 'john'; # user who owns the virtual machines on the remote (or local) server
var $password = 'mysecretpass';  #password for that user
/* SOAP URL of vboxwebsrv (not phpVirtualBox's URL) */
var $location = 'http://192.168.1.205:18083/'; # Where are the virtual machines? This can either be 'localhost' or the IP or the remote server where your virtual machines are installed
```
## Verification

- Start the vboxwebsrv init service on the host where your virtual machines are located (in our case on 192.168.1.205)

`root #``/etc/init.d/vboxwebsrv start`
- Restart Apache on the host where your phpVirtualBox was installed

`root #``/etc/init.d/apache2 restart`
- Connect to phpVirtualBox installation

Open your browser and connect to your phpVirtualBox by typing:

https://phpvirtualbox.mydomain.example

- You should be able to login using the default credentials

user: admin
pass: admin
