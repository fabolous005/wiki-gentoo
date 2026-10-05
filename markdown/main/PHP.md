<!-- source: https://wiki.gentoo.org/wiki/PHP | group: Gentoo Wiki (Main) | wiki-title: PHP -->
---
title: PHP
url: https://wiki.gentoo.org/wiki/PHP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-28"
fingerprint: "7e001a54d18438c4"
license: CC BY-SA 4.0
---

# PHP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**PHP** is a general-purpose server-side scripting language to produce dynamic web pages. It began development in 1993 and was first officially released in 1995. By the turn of the millennium, PHP began to displace [Perl](https://wiki.gentoo.org/wiki/Perl) as the preferred "P" in the [LAMP stack](<https://en.wikipedia.org/wiki/LAMP_(software_bundle)>) before being challenged in turn by [Python](https://wiki.gentoo.org/wiki/Python). Even so, PHP remains a popular server-side scripting language for dynamic websites and web applications, perhaps in no small part due to the popularity of the [WordPress](https://wiki.gentoo.org/wiki/WordPress) blogging platform.

## Installation

### USE flags

The most important USE flags of the PHP package, the SAPI (Server API) USE flags, are apache2, cgi, and fpm. Alongside these flags, there is an elaborate list of flags to enable various features of the PHP interpreter:


| [+cli](https://packages.gentoo.org/useflags/+cli) | Enable the Command-Line Interface (CLI) SAPI | 
| [+ctype](https://packages.gentoo.org/useflags/+ctype) | Enable the Character type checking (ctype) extension | 
| [+fileinfo](https://packages.gentoo.org/useflags/+fileinfo) | Enable the File Information extension | 
| [+filter](https://packages.gentoo.org/useflags/+filter) | Enable the Data Filtering extension | 
| [+flatfile](https://packages.gentoo.org/useflags/+flatfile) | Add dbm support for flat files | 
| [+iconv](https://packages.gentoo.org/useflags/+iconv) | Enable support for the iconv character set conversion library | 
| [+jit](https://packages.gentoo.org/useflags/+jit) | Enable PCRE JIT support | 
| [+opcache](https://packages.gentoo.org/useflags/+opcache) | Enables built-in opcode cache, replacing pecl-apc et al. | 
| [+opcache-jit](https://packages.gentoo.org/useflags/+opcache-jit) | Enable Just In Time (JIT) compilation within the opcache extension | 
| [+phar](https://packages.gentoo.org/useflags/+phar) | Enables the phar extension to provide phar archive support | 
| [+posix](https://packages.gentoo.org/useflags/+posix) | Add support for POSIX-compatible functions | 
| [+session](https://packages.gentoo.org/useflags/+session) | Add persistent session support | 
| [+simplexml](https://packages.gentoo.org/useflags/+simplexml) | Enable the SimpleXML extension | 
| [+tokenizer](https://packages.gentoo.org/useflags/+tokenizer) | Add support for the PHP file parser | 
| [+xml](https://packages.gentoo.org/useflags/+xml) | Add support for XML files | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [apache2](https://packages.gentoo.org/useflags/apache2) | Add Apache2 support | 
| [apparmor](https://packages.gentoo.org/useflags/apparmor) | Support FPM application confinement through sys-libs/libapparmor | 
| [argon2](https://packages.gentoo.org/useflags/argon2) | Enable password hashing algorithm from app-crypt/argon2 | 
| [avif](https://packages.gentoo.org/useflags/avif) | Add AV1 Image Format (AVIF) support | 
| [bcmath](https://packages.gentoo.org/useflags/bcmath) | Enable the BCMath Arbitrary Precision Mathematics extension | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Add support for sys-libs/db (Berkeley DB) | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [calendar](https://packages.gentoo.org/useflags/calendar) | Add support for calendars (not using mcal!) | 
| [capstone](https://packages.gentoo.org/useflags/capstone) | Support opcache JIT disassembly through dev-libs/capstone | 
| [cdb](https://packages.gentoo.org/useflags/cdb) | Add support for the CDB database engine from the author of qmail | 
| [cgi](https://packages.gentoo.org/useflags/cgi) | Add CGI script support | 
| [cjk](https://packages.gentoo.org/useflags/cjk) | Add support for Multi-byte character languages (Chinese, Japanese, Korean) | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [embed](https://packages.gentoo.org/useflags/embed) | Enable embed SAPI | 
| [enchant](https://packages.gentoo.org/useflags/enchant) | Enable the Enchant spelling library extension using app-text/enchant | 
| [exif](https://packages.gentoo.org/useflags/exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [ffi](https://packages.gentoo.org/useflags/ffi) | Enable the Foreign Function Interface (FFI) extension using dev-libs/libffi | 
| [fpm](https://packages.gentoo.org/useflags/fpm) | Enable the FastCGI Process Manager SAPI | 
| [ftp](https://packages.gentoo.org/useflags/ftp) | Add FTP (File Transfer Protocol) support | 
| [gd](https://packages.gentoo.org/useflags/gd) | Enable the Image Processing and GD extension (requires media-libs/libjpeg-turbo and media-libs/libpng) | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Add support for sys-libs/gdbm (GNU database libraries) | 
| [gmp](https://packages.gentoo.org/useflags/gmp) | Add support for dev-libs/gmp (GNU MP library) | 
| [imap](https://packages.gentoo.org/useflags/imap) | Add support for IMAP (Internet Mail Application Protocol) | 
| [inifile](https://packages.gentoo.org/useflags/inifile) | Add dbm support for .ini files | 
| [intl](https://packages.gentoo.org/useflags/intl) | Enables the intl extension for extended internalization support | 
| [iodbc](https://packages.gentoo.org/useflags/iodbc) | Add support for iODBC library | 
| [ipv6](https://packages.gentoo.org/useflags/ipv6) | Support IPv6 connectivity in fopen and friends, and v6 address conversions in functions like inet\_pton and inet\_ntop. | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [ldap-sasl](https://packages.gentoo.org/useflags/ldap-sasl) | Add SASL support for the PHP LDAP extension using dev-libs/cyrus-sasl | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [lmdb](https://packages.gentoo.org/useflags/lmdb) | Enable support for dev-db/lmdb db backend | 
| [mhash](https://packages.gentoo.org/useflags/mhash) | Add support for the mhash library | 
| [mssql](https://packages.gentoo.org/useflags/mssql) | Add support for Microsoft SQL Server database | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [mysqli](https://packages.gentoo.org/useflags/mysqli) | Add support for the improved mySQL libraries | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [oci8-instant-client](https://packages.gentoo.org/useflags/oci8-instant-client) | Use dev-db/oracle-instantclient-basic as Oracle provider instead of requiring a full Oracle server install | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Add ODBC Support (Open DataBase Connectivity) | 
| [pcntl](https://packages.gentoo.org/useflags/pcntl) | Enable the Process Control extension | 
| [pdo](https://packages.gentoo.org/useflags/pdo) | Enable the PHP Data Objects extension | 
| [phpdbg](https://packages.gentoo.org/useflags/phpdbg) | Enable the PHP Debug Command Line SAPI (like gdb for php) | 
| [png](https://packages.gentoo.org/useflags/png) | Add support for libpng (PNG images) | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [qdbm](https://packages.gentoo.org/useflags/qdbm) | Add support for the qdbm (Quick Database Manager) library | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [session-mm](https://packages.gentoo.org/useflags/session-mm) | Include dev-libs/mm support for session storage | 
| [sharedmem](https://packages.gentoo.org/useflags/sharedmem) | Enable the shmop extension | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Add support for the Simple Network Management Protocol if available | 
| [soap](https://packages.gentoo.org/useflags/soap) | Add support for SOAP (Simple Object Access Protocol) | 
| [sockets](https://packages.gentoo.org/useflags/sockets) | Add support for tcp/ip sockets | 
| [sodium](https://packages.gentoo.org/useflags/sodium) | Enable support for crypto through dev-libs/libsodium | 
| [spell](https://packages.gentoo.org/useflags/spell) | Add dictionary support | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [sysvipc](https://packages.gentoo.org/useflags/sysvipc) | Enable the PHP System-V semaphore, shared memory and IPC extension | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [test-full](https://packages.gentoo.org/useflags/test-full) | Run slower tests | 
| [threads](https://packages.gentoo.org/useflags/threads) | Add threads support for various packages. Usually pthreads | 
| [tidy](https://packages.gentoo.org/useflags/tidy) | Add support for HTML Tidy | 
| [tokyocabinet](https://packages.gentoo.org/useflags/tokyocabinet) | Enable support for dev-db/tokyocabinet db backend | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [unicode](https://packages.gentoo.org/useflags/unicode) | Add support for Unicode | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 
| [xmlreader](https://packages.gentoo.org/useflags/xmlreader) | Enable the XMLReader extension | 
| [xmlwriter](https://packages.gentoo.org/useflags/xmlwriter) | Enable the XMLWriter extension | 
| [xpm](https://packages.gentoo.org/useflags/xpm) | Add support for XPM graphics format | 
| [xslt](https://packages.gentoo.org/useflags/xslt) | Build the XSL extension | 
| [zip](https://packages.gentoo.org/useflags/zip) | Enable support for ZIP archives | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

The ebuild installs a modified "production" version of php.ini that has tighter security settings and default paths adjusted for Gentoo systems. Both upstream "production" and "development" php.ini files are installed to /usr/share/doc/php-\*.

### Extensions

To install extensions, first decide which versions of PHP to compile the extensions for. This is done by setting the `PHP_TARGETS` variable:

**`/etc/portage/package.use/php`**

```
 PHP_TARGETS: php8-2
```
More than one version can be defined; just add in the additional versions separated by a space. Note that the php slot is named "8.2" and the corresponding `PHP_TARGETS` value is `php8-2`. This is due to current restrictions on USE names.

### Emerge

After making the above USE flag configurations it is necessary to update the system so the changes take effect:

`root #``emerge --ask --update --changed-use --deep @world`
## Running multiple versions of PHP

One of the great advantages of using Gentoo for PHP development is the version slotting. It is very simple to swap between multiple versions of PHP as well as run multiple versions simultaneously. This is all done with the eselect command provided by the [app-eselect/eselect-php](https://packages.gentoo.org/packages/app-eselect/eselect-php) package which should get emerged in automatically by installing PHP.

For example, selecting different versions of PHP can allow a system to run PHP 7.0 for the cli SAPI but PHP 5.6 for the system's web server. It also allows system administrators or application developers to quickly test an application on different versions of PHP.

To list the available versions for the **cli** SAPI module use the following syntax:

`root #``eselect php list cli`
\[1\]   php5.5 
 \[2\]   php5.6 \*   
 \[3\]   php7.0

The `*` (asterisk) marks current active version for the selected module. To check the other SAPIs simply replace `cli` with `fpm`,`cgi` or `apache2` modules.

To swap versions, use:

`root #``eselect php set cli 3`
The number `3` in this example corresponds to the number in the output of the `list` sub-command used above. PHP version 7.0 is now used for the command-line:

`root #``php -v`
PHP 7.0.10-pl0-gentoo (cli) (built: Aug 23 2016 12:38:44) ( NTS )

## Configuration

The PHP configuration is at /etc/php, which contains one subdirectory for each Server API (SAPI) and for each PHP version. For instance, configuration files for the PHP-5.6 apache2 SAPI are installed in /etc/php/apache2-php5.6.

### Web servers

To use PHP in a server-side fashion, a web server needs to be installed and configured to use PHP. A number of popular web servers are briefly touched upon next.

#### Nginx

Simply emerge [nginx](https://wiki.gentoo.org/wiki/Nginx) to install it:

`root #``emerge --ask www-servers/nginx`
Once Nginx has been installed, modify the server section of /etc/nginx/nginx.conf to look something like this:

**`/etc/nginx/nginx.conf`**

**Part of the nginx configuration to enable PHP through FastCGI**

Now start the related services to have a working site. The php-fpm init script starts the PHP FastCGI Process Manager. FastCGI allows web servers to offload the PHP calculations to this process manager.

When running OpenRC as the service manager:

`root #````
rc-service nginx start
```
`root #````
rc-service php-fpm start
```
For systemd:

`root #````
systemctl start nginx
```
`root #````
systemctl start php-fpm@7.2
```
#### lighttpd

Make sure that lighttpd has the `php` USE flag is disabled:

**`/etc/portage/package.use/lighttpd`**

Lighttpd ships with a default FastCGI config file, but unfortunately, it is written to work with the old PHP FCGI SAPI only, instead of a general FCGI setup. Since the `php` USE flag has been disabled, it will not be included, and rightly so. However it does provide a good foundation for a configuration file that can be used with FPM.

Edit /etc/lighttpd/mod\_fastcgi.conf to look something like this:

**`/etc/lighttpd/mod_fastcgi.conf`**

**Enable FastCGI on lighttpd**

```
server.modules += ("mod_fastcgi")
fastcgi.server = ( ".php" =>
  ( "localhost" =>
    (
      "host" => "127.0.0.1",
      "port" => "9000"
    )
  )
)
```
Note the `host` and `port` parts.

Since Gentoo ships with a working php-fpm.ini file, located in /etc/php/fpm-php5/php-fpm.ini, and init script, starting the services is all that is needed:

`root #````
/etc/init.d/php-fpm start
```
`root #````
/etc/init.d/lighttpd start
```
#### Apache (mod\_php)

To configure Apache to load the PHP5 module (mod\_php), add `-D PHP` to `APACHE2_OPTS` variable in /etc/conf.d/apache2. Users might remember that previously, the PHP version had to be added as well (like `-D PHP5`). However, since [app-eselect/eselect-php](https://packages.gentoo.org/packages/app-eselect/eselect-php) version 0.8.1, the variable is changed to just `-D PHP` to allow future major versions to be easily integrated.

**`/etc/conf.d/apache2`**

**Configure Apache to load mod\_php**

```
## (settings for PHP5 and above)
APACHE2_OPTS="-D PHP"
```
Make sure that PHP is built with the `apache2` USE flag.

If for some reason the system is missing the PHP module integration file /etc/apache2/modules.d/70\_mod\_php.conf, currently provided by [app-eselect/eselect-php](https://packages.gentoo.org/packages/app-eselect/eselect-php) when installed with `apache2` USE flag set, manually insert it. Its current content is displayed below.

**`/etc/apache2/modules.d/70_mod_php.conf`**

```
<IfDefine PHP>
	# The mod_php.so symlink is controlled by
	# eselect-php. However, the module name changed from
	# php5_module to php7_module so we can't blindly load whatever
	# is there. Instead we let eselect-php manage a small
	# configuration file that loads the appropriate module.
	Include "/var/lib/eselect-php/mod_php.conf"
 
	# Tell apache that mod_php should handle PHP files.
	#
	# NOTE: Avoiding AddHandler/AddType for security (bug
	# #538822). Please read the related news item!
	<FilesMatch "\.(php|php[57]|phtml)$">
		SetHandler application/x-httpd-php
	</FilesMatch>
 
	# PHP source files which are meant to be displayed as
	# syntax-highlighted source code.
	<FilesMatch "\.phps$">
		SetHandler application/x-httpd-php-source
	</FilesMatch>
 
	DirectoryIndex index.php index.phtml
</IfDefine>
```
Although multiple PHP versions can be installed on a system, Apache can only use a single PHP version with mod\_php. Support for multiple PHP versions on Apache is available using fpm. The eselect php command is used to switch between active mod PHP versions.

To change the version of PHP handled by Apache, first list the available versions for the `apache2` Server Application Programming Interface (SAPI):

`root #``eselect php list apache2`
\[1\]   php5.3
 \[2\]   php5.4 \*
 \[3\]   php5.5

Change it to the version of choice:

`root #``eselect php set apache2 N`
Substitute `N` in the example above to the requested number in the output of eselect php list apache2 as displayed earlier on.

To verify the PHP module works, create a test page:

**`/var/www/localhost/htdocs/index.php`**

**PHP test page**

```
<html>
 <body>
  <?php phpinfo(); ?>
 </body>
</html>
```
Now, suppress or rename /var/www/localhost/htdocs/index.html and open the test page: [http://localhost/](http://localhost/)

A table describing the PHP settings should be visible.

#### Apache (fcgid)

See [enabling PHP through fcgid](https://wiki.gentoo.org/wiki/Apache#Enabling_PHP_through_fcgid) in the [Apache](https://wiki.gentoo.org/wiki/Apache) article.

#### Apache (mod\_proxy\_fcgi)

## See also

- [Upgrading to PHP 5.6](https://wiki.gentoo.org/wiki/PHP/Upgrading_to_PHP_5.6), guide to upgrade older installations to 5.6
- [Upgrading to PHP 7.1](https://wiki.gentoo.org/wiki/PHP/Upgrading_to_PHP_7.1), guide to upgrade older installations to 7.1
- [Enabling PHP support](https://wiki.gentoo.org/wiki/Apache#Enabling_PHP_support) in the Apache article
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.
- [Python](https://wiki.gentoo.org/wiki/Python) — an extremely popular cross-platform object oriented programming language.
