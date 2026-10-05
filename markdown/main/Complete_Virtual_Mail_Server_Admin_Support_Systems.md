<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Admin_Support_Systems | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Admin Support Systems -->
---
title: Complete Virtual Mail Server/Admin Support Systems
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Admin_Support_Systems
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-19"
fingerprint: "5799697afbea63d2"
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Admin Support Systems

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Administration interface

This document will not rely on any of the applications mentioned in this chapter. They can be installed and used, but a valid system should work without any of these in place. There is one exception however, [www-apps/postfixadmin](https://packages.gentoo.org/packages/www-apps/postfixadmin). The exception exists that postfixadmin will be used to create the initial tables. The reason is simple, postfixadmin could be used to administer the database and if it is not entirely happy about the table layout, things could go wrong. Thus letting postfixadmin create the tables, it is kept happy if ever it is decided to use it.

## Apache, PHP and PostgreSQL

Apache, PHP and PostgreSQL are extensive packages. Read the [Apache](https://wiki.gentoo.org/wiki/Apache) article about how to setup Apache. [PHP](https://wiki.gentoo.org/wiki/PHP) also has a wiki page dedicated to its installation. It should be made certain that all features expected from PHP and Apache work before continuing, as they will be important for the usage of webmail and statistics presentation.

One of the core components of the setup is [PostgreSQL](https://wiki.gentoo.org/wiki/PostgreSQL). Make sure to read through and follow the wiki article to set up the PostgreSQL infrastructure before continuing. Alternatively there is also [MySQL](https://wiki.gentoo.org/wiki/MySQL) or [LDAP](https://wiki.gentoo.org/index.php?title=LDAP&action=edit&redlink=1).

Connecting PostgreSQL and Apache (and others) together can happen in several ways, via the network or via a UNIX socket for example. While UNIX sockets are the fastest and slightly more secure, a TCP/IP connection might be desired when connecting across multiple (virtual) servers.

When using UNIX sockets, it is important however, that common filesystem permissions are used and thus apache and postfix may need to be in the postgres group:

`root #``gpasswd -a apache postgres`
## Postfixadmin

As mentioned in the introduction, postfixadmin will be used to create the tables. This to make sure that if postfixadmin would ever be used to administer the mail accounts etc, it will understand the table format.

If not done so already, [www-apps/postfixadmin](https://packages.gentoo.org/packages/www-apps/postfixadmin) should be emerged:

`root #``emerge --ask www-apps/postfixadmin`
### Apache configuration

Since postfixadmin is a web application, webapp-config will be used to install postfixadmin. Using a subdomain rather than a subdirectory is recommended for security and simplicity, while at the same time allowing the server to serve a web page in addition to the mail system.

First, create a new directory under /var/www named ***mailadmin***. Then install postfixadmin under this directory using the following command:

`root #``webapp-config -h mailadmin -d / -I postfixadmin 3.3.10`
Then, Apache should be configured to serve postfixadmin under *mailadmin.example.com*. For this, the following config file should be put under /etc/apache2/vhosts.d:

**`/etc/apache2/vhosts.d/01_mailadmin.conf`**

**Example apache config**

```
<VirtualHost *:80>
        ServerName mailadmin.example.com
        Redirect permanent / https://mailadmin.example.com/
</VirtualHost>
<VirtualHost *:443>
        ServerAdmin webmaster@example.com
        DocumentRoot /var/www/mailadmin/htdocs/public
        ServerName mailadmin.example.com
        <Directory /var/www/mailadmin/htdocs/public>
                Require all granted
                AllowOverride All
                Options FollowSymlinks MultiViews
                <IfModule mod_dav.c>
                        Dav off
                </IfModule>
        </Directory>
</VirtualHost>
```
### Securing the Apache server with TLS

Once certbot is installed, the necessary certificates should be generated using the following command:

`root #``certbot --apache --rsa-key-size 4096 --staple-ocsp --hsts`
/etc/apache2/vhosts.d/01\_mailadmin.conf should automatically be changed by certbot, the final file should look like this:

**`/etc/apache2/vhosts.d/01_mailadmin.conf`**

**Secure apache config**

```
<VirtualHost *:80>
        ServerName mailadmin.example.com
        Redirect permanent / https://mailadmin.example.com/
        RewriteEngine on
        RewriteCond %{SERVER_NAME} =mailadmin.example.com
        RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
<VirtualHost *:443>
        ServerAdmin webmaster@example.com
        DocumentRoot /var/www/mailadmin/htdocs/public
        ServerName mailadmin.example.com
        Header always set Strict-Transport-Security "max-age=15552000"
        Include /etc/letsencrypt/options-ssl-apache.conf
        
        <Directory /var/www/mailadmin/htdocs/public>
                Require all granted
                AllowOverride All
                Options FollowSymlinks MultiViews
                <IfModule mod_dav.c>
                        Dav off
                </IfModule>
        </Directory>
        SSLCertificateFile /etc/letsencrypt/live/mailadmin.example.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/mailadmin.example.com/privkey.pem
        SSLUseStapling on
</VirtualHost>
<IfModule mod_ssl.c>
SSLStaplingCache shmcb:/var/run/apache2/stapling_cache(128000)
</IfModule>
```
### Postgresql configuration

Postfixadmin also needs a database to store its data in and a user to access this database. The user postfixadmin will be created for postfixadmin to access the database, later another user will be created to read from this database. This way, administration and plain reading will be logically separated:

`root #``createuser -U postgres --pwprompt postfixadmin`
Enter password for new role: $password
Enter it again: $password

A database for this user will also be needed. It will be owned by postfixadmin:

`root #``createdb -U postgres --owner=postfixadmin postfix`
### Postfixadmin configuration

Next the postfixadmin configuration file needs to be edited to point to this database amongst other things.

**`/var/www/mailadmin/htdocs/config.inc.php`**

**Diff of postfixadmin configuration**

```
-$CONF['configured'] = false;
+$CONF['configured'] = true;
 
-$CONF['postfix_admin_url'] = '';
+$CONF['postfix_admin_url'] = 'https://mailadmin.example.com';
-$CONF['database_type'] = 'mysql';
+$CONF['database_type'] = 'pgsql';
 $CONF['database_host'] = 'localhost';
-$CONF['database_user'] = 'postfix';
-$CONF['database_password'] = 'postfixadmin';
+$CONF['database_user'] = 'postfixadmin';
+$CONF['database_password'] = '$password';
 $CONF['database_name'] = 'postfix';
 $CONF['database_prefix'] = '';
 
-$CONF['admin_email'] = 'postmaster@change-this-to-your.domain.tld';
+$CONF['admin_email'] = 'postmaster@example.com';
 
 
-    'abuse' => 'abuse@change-this-to-your.domain.tld',
-    'hostmaster' => 'hostmaster@change-this-to-your.domain.tld',
-    'postmaster' => 'postmaster@change-this-to-your.domain.tld',
-    'webmaster' => 'webmaster@change-this-to-your.domain.tld'
+    'abuse' => 'abuse@example.com',
+    'hostmaster' => 'hostmaster@example.com',
+    'postmaster' => 'postmaster@example.com',
+    'webmaster' => 'webmaster@example.com'
 
-$CONF['domain_path'] = 'NO';
+$CONF['domain_path'] = 'YES';
 
-$CONF['domain_in_mailbox'] = 'YES';
+$CONF['domain_in_mailbox'] = 'NO';
 
-$CONF['transport'] = 'NO';
+$CONF['transport'] = 'YES';
 
-$CONF['vacation_domain'] = 'autoreply.change-this-to-your.domain.tld';
+$CONF['vacation_domain'] = 'autoreply.example.com';
 
-$CONF['user_footer_link'] = "http://change-this-to-your.domain.tld/main";
+$CONF['user_footer_link'] = "https://example.com/";
 
-$CONF['footer_text'] = 'Return to change-this-to-your.domain.tld';
-$CONF['footer_link'] = 'http://change-this-to-your.domain.tld';
+$CONF['footer_text'] = 'Return to http://example.com/';
+$CONF['footer_link'] = 'https://example.com/';
```
Having postfixadmin generate the required tables, go to [https://mailadmin.example.com/setup.php](https://mailadmin.example.com/setup.php) and follow the instructions. Also any configuration errors or missing packages will be noted here. At this point, a password hash will also be generated and can be edited into the config file. After that, super-admin users can be added to the database from this page. To log into the administrative page, go to [https://mailadmin.example.com/](https://mailadmin.example.com/).
