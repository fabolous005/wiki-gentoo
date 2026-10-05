<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Web_Access | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Web Access -->
---
title: Complete Virtual Mail Server/Web Access
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Web_Access
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: a31b291849ba67c0
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Web Access

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

One heavily used function of E-mail, is webmail access. Some users use it solely as their access, others use it as backup or when their own system is not available. There are several options available, with [mail-client/squirrelmail](https://packages.gentoo.org/packages/mail-client/squirrelmail) and [mail-client/roundcube](https://packages.gentoo.org/packages/mail-client/roundcube) being the most common ones. Squirrelmail can be used if bandwidth is a huge issue, roundcube is recommended otherwise and the focus of this article.

## Installing roundcube

Roundcube can store its configuration in a database. Things like local addressbook, identities etc. can be stored there as well.

Here there are several options available. Use a local, per vhost based SQLite storage pool, use a postgres/mysql database per vhost or have all vhosts share the same database. There can be advantages for any of the above options. If webmail, for various domains, is on the same host as the imap server, then using one database for all domains is fine and can be an advantage. It could be desired that each domain, each vhost gets its own webmail interface (different skin and plugins) where all users could still log in. Meaning in the event of one of the vhosts webmail being broken, they can still access their e-mail normally using a different vhost. Whether the users settings 'live' in all vhosts or not, is based on the imap server. Roundcube bases its user id on the `mail_host` e.g. the imap server and thus when using localhost the user 'profile' is identical everywhere.

### USE flags


| [change-password](https://packages.gentoo.org/useflags/change-password) | Allow users to change passwords through Roundcube | 
| [enigma](https://packages.gentoo.org/useflags/enigma) | Add dependencies to support server-side GnuPG signing and/or encryption through the enigma plugin | 
| [exif](https://packages.gentoo.org/useflags/exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [fileinfo](https://packages.gentoo.org/useflags/fileinfo) | Use the dev-lang/php fileinfo extension to guess the mime\_type of attachments | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [spell](https://packages.gentoo.org/useflags/spell) | Add dictionary support | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [vhosts](https://packages.gentoo.org/useflags/vhosts) | Add support for installing web-based applications into a virtual-hosting environment | 
| [zip](https://packages.gentoo.org/useflags/zip) | Enable support for ZIP archives | 

### Emerge

After choosing the appropiate USE flags, roundcube should be emerged:

`root #``emerge --ask mail-client/roundcube`
Then, create a directory and install roundcube into it:

`root #``mkdir /var/www/mail``root #``webapp-config -h mail -d / -I roundcube 1.5.2`
#### Installing roundcube to Apache using TLS

Apache should be configured to serve roundcube under *mail.example.com*. For this, the following config file should be put under /etc/apache2/vhosts.d:

**`/etc/apache2/vhosts.d/02_mail.conf`**

**Example apache config**

```
<VirtualHost *:80>
        ServerName mail.example.com
        Redirect permanent / https://mail.example.com/
</VirtualHost>
<VirtualHost *:443>
        ServerAdmin webmaster@example.com
        DocumentRoot /var/www/mail/htdocs/
        ServerName mail.example.com
        <Directory /var/www/mail/htdocs/>
                Require all granted
                AllowOverride All
                Options FollowSymlinks MultiViews
                <IfModule mod_dav.c>
                        Dav off
                </IfModule>
        </Directory>
</VirtualHost>
```
Once certbot is installed, the necessary certificates should be generated using the following command:

`root #``certbot --apache --rsa-key-size 4096 --staple-ocsp --hsts`
/etc/apache2/vhosts.d/02\_mail.conf should automatically be changed by certbot, the final file should look like this:

**`/etc/apache2/vhosts.d/02_mail.conf`**

**Secure apache config**

```
<VirtualHost *:80>
        ServerName mail.example.com
        Redirect permanent / https://mail.example.com/
        RewriteEngine on
        RewriteCond %{SERVER_NAME} =mail.example.com
        RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
<VirtualHost *:443>
        ServerAdmin webmaster@example.com
        DocumentRoot /var/www/mail/htdocs/
        ServerName mail.example.com
        Header always set Strict-Transport-Security "max-age=15552000"
        Include /etc/letsencrypt/options-ssl-apache.conf
        
        <Directory /var/www/mail/htdocs/>
                Require all granted
                AllowOverride All
                Options FollowSymlinks MultiViews
                <IfModule mod_dav.c>
                        Dav off
                </IfModule>
        </Directory>
        SSLCertificateFile /etc/letsencrypt/live/mail.example.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/mail.example.com/privkey.pem
        SSLUseStapling on
</VirtualHost>
<IfModule mod_ssl.c>
SSLStaplingCache shmcb:/var/run/apache2/stapling_cache(128000)
</IfModule>
```
### Using Postgres

Unless there is some serious security concern, only one database user is required to access all databases used by roundcube:

`root #``createuser -U postgres -D -P -R -S roundcube`
Enter password for new role: $password
Enter it again: $password

Roundcube will also need a database. Different names will have to be chosen when using different databases per domain:

`root #``createdb -U postgres roundcubemail -O roundcube`
## Configuring Roundcube

Configuring roundcube has become incredible easy. Just visit [https://mail.example.com/installer/](https://mail.example.com/installer/) and follow the on screen instructions, correcting any issues as they pop up.

Below are the old instructions when manually creating the config.inc.php file. The installer should still be used to create the database, which it has a button for.

### Configuring Roundcube (manually)

Configuring roundcube is as simple as editing two configuration files in /var/www/webmail.example.com/htdocs/roundcube/config/.

To access the database only the following line needs to be changed. The user name chosen earlier is *roundcube*.  Replace *pass* in this line with the password chosen earlier:

**`/var/www/webmail.example.com/htdocs/roundcube/config/db.inc.php`**

**Database access**

```
$rcmail_config['db_dsnw'] = 'pgsql://roundcube:pass@localhost/roundcubemail';
```
Next listed are the essential changes required for roundcube operation. More can be done to personalize the setup:

**`/var/www/webmail.example.com/htdocs/roundcube/config/main.inc.php`**

**Roundcube configuration**

```
-$rcmail_config['default_host'] = '';
+$rcmail_config['default_host'] = 'localhost';
 
-$rcmail_config['smtp_server'] = '';
+$rcmail_config['smtp_server'] = 'localhost';
 
 // SMTP username (if required) if you use %u as the username Roundcube
 // will use the current username for login
+$rcmail_config['smtp_user'] = '';
-$rcmail_config['smtp_user'] = '%u';
 
 // SMTP password (if required) if you use %p as the password Roundcube
 // will use the current user's password for login
-$rcmail_config['smtp_pass'] = '';
+$rcmail_config['smtp_pass'] = '%p';
 
 // this key is used to encrypt the users imap password which is stored
 // in the session record (and the client cookie if remember password is enabled).
 // please provide a string of exactly 24 chars.
-$rcmail_config['des_key'] = 'rcmail-!24ByteDESkey*Str';
+$rcmail_config['des_key'] = '$supersecret24bytestring';
```
Starting with Roundcube version 0.6, Roundcube needs to have the fully qualified names for the default storage folders, such as Trash, Drafts etc:

**`/var/www/webmail.example.com/htdocs/roundcube/config/main.inc.php`**

**Setup fully qualified folder names**

```
-$rcmail_config['drafts_mbox'] = 'Drafts';
+$rcmail_config['drafts_mbox'] = 'INBOX.Drafts';
 
// NOTE: Use folder names with namespace prefix (INBOX. on Courier-IMAP)
-$rcmail_config['junk_mbox'] = 'Junk';
+$rcmail_config['junk_mbox'] = 'INBOX.Junk';
 
// NOTE: Use folder names with namespace prefix (INBOX. on Courier-IMAP)
-$rcmail_config['sent_mbox'] = 'Sent';
+$rcmail_config['sent_mbox'] = 'INBOX.Sent';
 
// NOTE: Use folder names with namespace prefix (INBOX. on Courier-IMAP)
-$rcmail_config['trash_mbox'] = 'Trash';
+$rcmail_config['trash_mbox'] = 'INBOX.Trash';
 
// NOTE: Use folder names with namespace prefix (INBOX. on Courier-IMAP)
-$rcmail_config['default_imap_folders'] = array('INBOX', 'Drafts', 'Sent', 'Junk', 'Trash');
+$rcmail_config['default_imap_folders'] = array('INBOX', 'INBOX.Drafts', 'INBOX.Sent', 'INBOX.Junk', 'INBOX.Trash');
```
## Testing roundcube

Log into webmail via [https://mail.example.com/](https://mail.example.com/) and the test mailbox should show. Also sending mail should be working correctly.
