<!-- source: https://wiki.gentoo.org/wiki/Apache | group: Gentoo Wiki (Main) | wiki-title: Apache -->
---
title: Apache
url: https://wiki.gentoo.org/wiki/Apache
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-27"
fingerprint: "3410fa5a44c72bc4"
license: CC BY-SA 4.0
---

# Apache

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Apache HTTP Server** is an efficient, extensible [web server](https://wiki.gentoo.org/wiki/Category:Web_servers). It is one of the most popular web servers used the Internet.

## Installation

### USE flags


### USE flags for
            [www-servers/apache](https://packages.gentoo.org/packages/www-servers/apache)
            
            The Apache Web Server

| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [+suexec-caps](https://packages.gentoo.org/useflags/+suexec-caps) | Install suexec with capabilities instead of SUID | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Add support for sys-libs/gdbm (GNU database libraries) | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [static](https://packages.gentoo.org/useflags/static) | Link in apache2 modules statically rather then plugins | 
| [suexec](https://packages.gentoo.org/useflags/suexec) | Install suexec with apache | 
| [suexec-syslog](https://packages.gentoo.org/useflags/suexec-syslog) | Log suexec to syslog instead of to a separate file | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [threads](https://packages.gentoo.org/useflags/threads) | Add threads support for various packages. Usually pthreads | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

#### Multi-Processing Module

To use the Apache event or worker MPM, enable the Apache threads USE flag:

**`/etc/portage/package.use`**

**Apache threads support**

```
www-servers/apache threads
```
To use the Apache event MPM, add the following to package.use:

**`/etc/portage/package.use/apache2`**

**Apache event MPM**

```
*/* APACHE2_MPMS: event
```
To use the Apache worker MPM, add the following to package.use:

**`/etc/portage/package.use/apache2`**

**Apache worker MPM**

```
*/* APACHE2_MPMS: worker
```
If no Multi-Processing Module (MPM) is selected, the default MPM is used. The default MPM depends on platform capabilities (like threads support), read more in the [official Apache docs](https://httpd.apache.org/docs/current/en/mpm.html#defaults).

Then emerge [www-servers/apache](https://packages.gentoo.org/packages/www-servers/apache):

`root #``emerge --ask www-servers/apache`
### Global support

Enabling the `apache2` global USE flag provides support for Apache in other packages. This may cause [www-servers/apache](https://packages.gentoo.org/packages/www-servers/apache) to be installed automatically if a package depending on Apache has been emerged.

**`/etc/portage/make.conf`**

**Adding Apache to system USE flags**

```
USE="apache2"
```
After configuring USE variables, update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
## Configuration

### Files

There are two main files that configure Apache2's behavior on the system:

- Gentoo's Apache2 init script configuration file /etc/conf.d/apache2

- Apache2 server's conventional configuration file /etc/apache2/httpd.conf

#### /etc/conf.d/apache2

The apache2 file located in /etc/conf.d is Gentoo's init script configuration file. The only active line in this file is the `APACHE2_OPTS` variable line:

**`/etc/conf.d/apache2`**

```
APACHE2_OPTS="-D DEFAULT_VHOST -D INFO -D SSL -D SSL_DEFAULT_VHOST -D LANGUAGE"
```
This line defines options that will be interpreted by the various configuration files using the `<IfDefine option-name>` statement to activate or deactivate some part of the whole configuration. This will be returned to later in the article.

#### /etc/apache2/httpd.conf

The httpd.conf file is Apache server's conventional configuration file. In fact this file is only an **entry point** for configuration. The whole configuration is split in many files in the /etc/apache2/ directory, that are assembled together using the `Include` directive. For example, the statement `Include /etc/apache2/modules.d/*.conf`, in httpd.conf, aims at including all the files in /etc/apache2/modules.d/ which name ends with .conf.

Taking into account what has been said in the subsection above, and as module configuration files (files in /etc/apache2/modules.d) almost always start with the `<IfDefine module-name>`, the content of one file inside /ect/apache2/modules.d, will ONLY be assembled with the rest of the configuration, if the matching option is set using a `-D module-name` flag in the `APACHE2_OPTS` variable in the /etc/conf.d/apache2 file. The 00\_default\_settings.conf configuration file is an exception to this rule as it doesn't start with an `IfDefine` statement and therefore is always included in the resulting configuration.

### Defaults

After a fresh install of an Apache server, the configuration resulting from the assemblage of the different configuration files is as follows. Start with the entry point /etc/apache2/httpd.conf.

**`/etc/apache2/httpd.conf`**

```
ServerRoot "/usr/lib64/apache2"
  
#Module loaded unconditionally, assuming the USE flag is no unset in
# /etc/portage/make.conf or in /etc/portage/package.use
LoadModule actions_modulemodules/mod_actions.so
...
#other modules loaded that way : alias_module, auth_basic_module, authn_alias_module,
# authn_anon_module, authn_dbm_module, authn_default_module, authn_file_module, 
# authz_dbm_module, authz_default_module, authz_groupfile_module, authz_host_module, 
# authz_owner_module, authz_user_module, autoindex_module,  cgi_module,  cgid_module, 
# deflate_module, dir_module, env_module, expires_module, ext_filter_module, filter_module,
#  headers_module, include_module,  log_config_module, logio_module, mime_module,  
# mime_magic_module, negotiation_module, rewrite_module, setenvif_module, 
# speling_module,ssl_module, status_module, unique_id_module, usertrack_module, host_alias_module
  
  
#Modules loaded conditionally, assuming matching USE flag is not unset in
# /etc/portage/make.conf or in /etc/portage/package.use (flag to be set in )
<IfDefine AUTHNZ_LDAP>
LoadModule authnz_ldap_module modules/mod_authnz_ldap.so
</IfDefine>
#other modules loaded that way : cache_module, dav_module, dav_fs_module,
# dav_lock_module,disk_cache_module,  file_cache_module, info_module, ldap_module,
# mem_cache_module, userdir_module
  
  
User apache
Group apache
  
# Supplemental configuration
#**************************************************************************************vvv
#this part is included via Include /etc/apache2/modules.d/*.conf 
  
#included from /etc/apache2/modules.d/00_default_settings.conf-------------v
#this is always included as there is not option to deactivate it.
Timeout 300
KeepAlive On
MaxKeepAliveRequests 100
KeepAliveTimeout 15
UseCanonicalName Off
AccessFileName .htaccess
ServerTokens Prod
TraceEnable off
ServerSignature On 
HostnameLookups Off
EnableMMAP On
EnableSendfile On
FileEtag INode MTime Size
ContentDigest Off
ErrorLog /var/log/apache2/error_log
LogLevel warn
  
<Directory />
	Options FollowSymLinks
	AllowOverride None
	Require all denied
</Directory>
  
<IfModule dir_module>
	DirectoryIndex index.html index.html.var
</IfModule>
<FilesMatch "^\.ht">
	Require all denied
</FilesMatch>
#--------------------------------------------------------------------------^
  
#included from 00_mod_info.conf--------------------------------------------v
<IfDefine INFO>
<Location /server-info>
	SetHandler server-info
	Require host 127.0.0.1
</Location>
</IfDefine>
#--------------------------------------------------------------------------^
  
#--------------------------------------------------------------------------v
#included from 00_languages.conf
# Settings for hosting different languages.
<IfDefine LANGUAGE>
  
	AddLanguage ca .ca
	...
	AddLanguage zh-TW .zh-tw
  
	LanguagePriority en ca cs da de el eo es et fr he hr it ja ko ltz nl nn no pl pt pt-BR ru sv zh-CN zh-TW
  
	ForceLanguagePriority Prefer Fallback
  
	AddCharset us-ascii.ascii	.us-ascii
	AddCharset ISO-8859-1		.iso8859-1 .latin1
	...
	AddCharset shift_jis		.shift_jis .sjis
</IfDefine>
#---------------------------------------------------------------------------^
#**************************************************************************************^^^
  
  
#***************************************************************************************vvv
#this part is included via Include /etc/apache2/vhosts.d/*.conf 
#from 00_default_ssl_vhost.conf-----------------------------------------------------vv
<IfDefine SSL>
	<IfDefine SSL_DEFAULT_VHOST>
		<IfModule ssl_module>
			Listen 443
  
			<VirtualHost _default_:443>
				ServerName localhost
                                #------------------------------------------v
				# this part is included via Include /etc/apache2/vhosts.d/default_vhost.include
				ServerAdmin root@localhost
				DocumentRoot "/var/www/localhost/htdocs"
  
  	
				<Directory "/var/www/localhost/htdocs">
	   				Options Indexes FollowSymLinks
	   				AllowOverride All
	   				Require all granted
				</Directory>
  
				<IfModule alias_module>
	   				ScriptAlias /cgi-bin/ "/var/www/localhost/cgi-bin/"
				</IfModule>
  
				<Directory "/var/www/localhost/cgi-bin">
	   				AllowOverride None
	   				Options None
	   				Require all granted
				</Directory>
        			#end of Include ---------------------------^
  	
				ErrorLog /var/log/apache2/ssl_error_log
  
				<IfModule log_config_module>
					TransferLog /var/log/apache2/ssl_access_log
				</IfModule>
				SSLEngine on
				SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP:+eNULL
				SSLCertificateFile /etc/ssl/apache2/server.crt
				SSLCertificateKeyFile /etc/ssl/apache2/server.key
  
				<FilesMatch "\.(cgi|shtml|phtml|php)$">
					SSLOptions +StdEnvVars
				</FilesMatch>
  
				<Directory "/var/www/localhost/cgi-bin">
					SSLOptions +StdEnvVars
				</Directory>
  
				<IfModule setenvif_module>
					BrowserMatch ".*MSIE.*" \
					nokeepalive ssl-unclean-shutdown \
					downgrade-1.0 force-response-1.0
				</IfModule>
  
  
				<IfModule log_config_module>
					CustomLog /var/log/apache2/ssl_request_log \
					"%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
					</IfModule>
			</VirtualHost>
		</IfModule>
	</IfDefine>
</IfDefine>
#---------------------------------------------------------------------------------^^
#from 00_default_vhost.conf-------------------------------------------------------vv
<IfDefine DEFAULT_VHOST>
	Listen 80
	NameVirtualHost *:80
  
	<VirtualHost *:80>
		ServerName localhost
  
		#---------------------------------------------------------------v
		# this part is included via Include /etc/apache2/vhosts.d/default_vhost.include
		ServerAdmin root@localhost
		DocumentRoot "/var/www/localhost/htdocs"
  
  	
		<Directory "/var/www/localhost/htdocs">
	   		Options Indexes FollowSymLinks
	   		AllowOverride All
	   		Require all granted
		</Directory>
  
		<IfModule alias_module>
	   		ScriptAlias /cgi-bin/ "/var/www/localhost/cgi-bin/"
		</IfModule>
  
		<Directory "/var/www/localhost/cgi-bin">
	   		AllowOverride None
	   		Options None
	   		Require all granted
		</Directory>
        	#end of Include -----------------------------------------------^
  
		<IfModule mpm_peruser_module>
			ServerEnvironment apache apache
		</IfModule>
	</VirtualHost>
</IfDefine>
#-----------------------------------------------------------------------------------^^
# end of include ****************************************************************************************^^^
```
### First sign of life

Start the server, as described in the [Usage section](https://wiki.gentoo.org/wiki/Apache#Usage).

As visible in the initial configuration above, the pre-installed virtual host's `DocumentRoot`  directory is /var/www/localhost/htdocs, its server name is *localhost*. In addition an index.html file is provided in the `DocumentRoot` directory, thus to check whether everything is correctly installed or not, point a browser to [https://localhost/](https://localhost/).

An "It works!" message should be printed on the page.

### Enabling mod\_security

**[ModSecurity](https://en.wikipedia.org/wiki/ModSecurity)** is a rule-based web application firewall that monitors web service traffic, to block attacks exploiting known vulnerabilities.

Install [www-apache/mod\_security](https://packages.gentoo.org/packages/www-apache/mod_security):

`root #``emerge --ask www-apache/mod_security`
Enable the `SECURITY` module in the apache2 file's `APACHE2_OPTS` variable:

**`/etc/conf.d/apache2`**

**Enabling the security module**

```
APACHE2_OPTS="... -D SECURITY"
```
Control this module by editing /etc/apache2/modules.d/79\_modsecurity.conf and /etc/apache2/modules.d/80\_modsecurity-crs.conf files. The file /usr/share/doc/mod\_security-\*/modsecurity.conf-recommended.bz2 contains the recommended configuration [https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual#A\_Recommended\_Base\_Configuration](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual#A_Recommended_Base_Configuration). Copy the basic configuration to the /etc/apache2/modules.d/79\_modsecurity.conf and tweak the settings according to your needs. Unpack the file /usr/share/doc/modsecurity-crs-\*/crs-setup.conf.example.bz2 to a suitable location, e.g. /etc/conf.d/crs-setup.conf, tweak it, and include the unpacked file in /etc/apache2/modules.d/80\_modsecurity-crs.conf. Finally, restart Apache. Look for the errors in the Apache log files of your sites and act accordingly.

### Enabling LUA support

Install Apache with the USE flag `apache2_modules_lua` enabled.

For security reasons this USE flag may be masked. To reverse, edit package.use.mask first:

**`/etc/portage/profile/package.use.mask`**

**Reversing the LUA module mask**

```
www-servers/apache -apache2_modules_lua
```
### Enabling PHP support

Install [PHP](https://wiki.gentoo.org/wiki/PHP) with the `apache2` USE flag and enable the module:

**`/etc/conf.d/apache2`**

**Enabling the PHP module**

```
APACHE2_OPTS="... -D PHP"
```
Before testing if the PHP module works, check that the file /etc/apache2/modules.d/70\_mod\_php.conf exists and contains the following definition:

**`/etc/apache2/modules.d/70_mod_php.conf`**

**Verifying the PHP module will be loaded**

```
<IfDefine PHP>
	# The mod_php.so symlink is controlled by
	# eselect-php. However, the module name changed from
	# php5_module to php7_module so we can't blindly load whatever
	# is there. Instead we let eselect-php manage a small
	# configuration file that loads the appropriate module.
	#
	# This is relative to ServerRoot (see httpd.conf).
	Include ../../../var/lib/eselect-php/mod_php.conf
 
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
If it does not exist create it.

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
Now, suppress or rename /var/www/localhost/htdocs/index.html and open the test page: [https://localhost/](https://localhost/)

A table describing the PHP settings should be visible.

#### Modify PHP versions

To change the version of PHP handled by Apache, first list the available versions for the `apache2` Server Application Programming Interface (SAPI):

`root #``eselect php list apache2`
\[1\]   php5.3
 \[2\]   php5.4 \*
 \[3\]   php5.5

Change it to the version of choice:

`root #``eselect php set apache2 N`
Substitute `N` in the example above to the requested number in the output of eselect php list apache2 as displayed earlier on.

### Virtual hosts

For each virtual host, provide a `DocumentRoot` directory that is reachable and accessible by the Apache daemon. Add a virtual host configuration file (VirtualHost.conf) in the /etc/apache2/vhosts.d directory which uses this `DocumentRoot` and the virtual host server name. Do not forget to add an entry for this domain name in /etc/hosts.

To assign the apache user/group ownership on the virtual host files, use chown like in the following example:

`root #``chown apache:apache /var/www/sitename`
Below are two example virtual host definitions, one for domainname1.com and one for domainname2.com. Notice the different `DocumentRoot` and `ServerName` directives even though the host itself (`*:80`) remains the same:

It is recommended to provide an IP based virtual host definition as well. This allows the administrator to put up a message for users that try to reach a site through its IP address:

After inserting virtual hosts, the server needs to be (gracefully) restarted for the new sites to become active.

### Enabling PHP through fcgid

Install [www-apache/mod\_fcgid](https://packages.gentoo.org/packages/www-apache/mod_fcgid) and [dev-lang/php](https://packages.gentoo.org/packages/dev-lang/php). The PHP package requires `cgi` to be set as a USE flag:

`root #``emerge --ask www-apache/mod_fcgid dev-lang/php`
Edit the mod\_fcgid.conf file:

**`/etc/apache2/modules.d/20_mod_fcgid.conf`**

```
<IfDefine FCGID>
LoadModule fcgid_module modules/mod_fcgid.so
SocketPath /var/run/fcgidsock
SharememPath /var/run/fcgid_shm
  
AddHandler php-fcgid .php
AddType application/x-httpd-php .php
Action php-fcgid /fcgid-bin/php-fcgid-wrapper
# max request 128mb
FcgidMaxRequestLen 134217728
<Location /fcgid-bin/>
        SetHandler fcgid-script
        Options +ExecCGI
</Location>
</IfDefine>
```
Create the needed directory:

`root #``mkdir /var/www/localhost/htdocs/fcgid-bin`
Symlink it for the PHP wrapper:

`root #``ln -s /usr/bin/php-cgi /var/www/localhost/htdocs/fcgid-bin/php-fcgid-wrapper`
Enable the `FCGID` module:

**`/etc/conf.d/apache2`**

**Enabling the fcgid module**

```
APACHE2_OPTS="... -D FCGID"
```
Finally restart Apache and check the `phpinfo()` site created earlier. The value of `Server API` should be *CGI/FastCGI*

### Enabling PHP-FPM through mod\_proxy\_fcgi in Apache 2.4

The following pre-requisites must be satisfied to enable PHP-FPM through mod\_proxy\_fcgi:

- \>= PHP 5.3
- \>= Apache 2.4

Furthermore, there are a few restrictions on the availability of functionality within Apache 2.4:

- \>= Apache 2.4.9, if you want to [communicate with PHP-FPM over UNIX sockets](https://wiki.apache.org/httpd/PHP-FPM#Performance_and_Pitfalls)
- \>= Apache 2.4.10, if you want to use [SetHandler](https://github.com/php/php-src/pull/694) instead of [ProxyPassMatch](https://wiki.apache.org/httpd/PHP-FPM#Example).

The following configuration will only work with Apache 2.4.10 and newer. It relies on the `FilesMatch` directive and be placed within the main server config or `VirtualHosts`.  The location of the UNIX socket is determined by the `listen` directive in the php-fpm.conf configuration file, allowing for specifying separate pools per site or function.

In the following example, `FilesMatch` is placed within the PHP module config file of Apache:

**`/etc/apache2/modules.d/70_mod_php.conf`**

**Using PHP-FPM (recommended for Apache 2.4.10 and newer)**

```
<IfDefine PHP>
        <FilesMatch "\.php$">
                SetHandler "proxy:unix:/var/run/php-fpm/www.sock|fcgi://localhost"
        </FilesMatch>
  
	# Set it to handle the files
	<IfModule mod_mime.c>
		AddHandler application/x-httpd-php .php .php5 .phtml
		AddHandler application/x-httpd-php-source .phps
	</IfModule>
  
	DirectoryIndex index.php index.phtml
 </IfDefine>
```
Or, you can use ProxPassMatch -- the only option if the Apache version is between 2.4.0 and 2.4.8, inclusive.

**`/etc/apache2/modules.d/70_mod_php.conf`**

**Using PHP-FPM (recommended for Apache 2.4.8 and older)**

```
<IfDefine PHP>
        # Send all requested PHP files to PHP-FPM via fcgi://PHP_FPM_LISTEN_ADDRESS:PHP_FPM_LISTEN_PORT/DOCUMENT_ROOT/$1
        ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/localhost/htdocs/$1
  
	# Set it to handle the files
	<IfModule mod_mime.c>
		AddHandler application/x-httpd-php .php .php5 .phtml
		AddHandler application/x-httpd-php-source .phps
	</IfModule>
  
	DirectoryIndex index.php index.phtml
 </IfDefine>
```
By default the `listen` directive is not set to a socket. First create the directory for the socket file:

`root #``mkdir /var/run/php-fpm`
Next, update the php-fpm.conf file as follows:

**`php-fpm.conf`**

**Updating listen directive**

```
; Set permissions for unix socket, if one is used. In Linux, read/write
; permissions must be set in order to allow connections from a web server. Many
; BSD-derived systems allow connections regardless of permissions.
; Default Values: user and group are set as the running user
;                 mode is set to 0666
listen.owner = apache
listen.group = apache
;listen.mode = 0666
 
 ; The address on which to accept FastCGI requests.
 ; Valid syntaxes are:
 ;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific address on
 ;                            a specific port;
 ;   'port'                 - to listen on a TCP socket to all addresses on a
 ;                            specific port;
 ;   '/path/to/unix/socket' - to listen on a unix socket.
 ; Note: This value is mandatory.
 ;listen = 127.0.0.1:9000
listen = /var/run/php-fpm/www.sock
```
Then enable both the `PHP` and `PROXY` modules:

**`/etc/conf.d/apache2`**

**Enabling PHP and proxy modules**

```
APACHE2_OPTS="... -D PHP -D PROXY"
```
### Web frameworks and Apache

Some of the web frameworks that can work with Apache are covered on the wiki:

### HTTPS with TLS certificates from Let’s Encrypt

It is important that any public-facing web server provide "secure" HTTPS access. Often, sites providing HTTPS will be configured to redirect HTTP requests to the HTTPS equivalent URL.

[Let’s Encrypt](https://letsencrypt.org) is a not-for-profit certificate authority that issues free TLS certificates. certbot is a utility available in the [Gentoo repository](https://wiki.gentoo.org/wiki/Ebuild_repository) for easily requesting and installing TLS certificates, and automatically setting up HTTPS access for Apache.

See the [Let's Encrypt](https://wiki.gentoo.org/wiki/Let%27s_Encrypt) article for information about using certbot. The [EFF](https://eff.org) also have specific instructions on using [certbot on Gentoo](https://certbot.eff.org/instructions?ws=apache&os=gentoo) to configure Apache.

## Usage

### Services

#### OpenRC

Start the Apache server:

`root #``rc-service apache2 start`
Add Apache to the default runlevel:

`root #``rc-update add apache2 default`
Restart the Apache service:

`root #``rc-service apache2 restart`
Reload Apache configuration files:

`root #``rc-service apache2 reload`
#### systemd

Start the Apache server:

`root #``systemctl start apache2`
Add Apache to the default runlevel:

`root #``systemctl enable apache2`
Restart the Apache service:

`root #``systemctl restart apache2`
## Troubleshooting

### Resources

The Apache server can be difficult to configure properly. Below are some resources that may be helpful when issues occur:

### Testing

Verifying IP interfaces and ports on which Apache2 is running on and listening to:

`root #``ss -tlpn | grep apache````
LISTEN 0      0       0.0.0.0:80       0.0.0.0:*    users:(("apache2",pid=1401,fd=3),("apache2",pid=1399,fd=3),("apache2",pid=1396,fd=3))
LISTEN 0      0       0.0.0.0:443      0.0.0.0:*    users:(("apache2",pid=1401,fd=3),("apache2",pid=1399,fd=3),("apache2",pid=1396,fd=3))
```
Testing if a connection to a Apache server is working on localhost:

`user $``telnet localhost 80`
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^\]'.

Interrupt the connection test with `Ctrl`+`c` and `Enter`.

### apr\_sockaddr\_info\_get() failed for \<System\_Hostname>

**Error:**

apache2: apr\_sockaddr\_info\_get() failed for System\_Hostname

**Resolution:**

When this occurs, add the host name to the /etc/hosts file:

**`/etc/hosts`**

**Adding a Hostname for Apache**

```
127.0.0.1 localhost System_Hostname
```
## See also

- [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) — a fast and lightweight [web server](https://wiki.gentoo.org/wiki/Category:Web_servers).
- [Nginx](https://wiki.gentoo.org/wiki/Nginx) — a robust, small, high performance [web server](https://wiki.gentoo.org/wiki/Category:Web_servers) and reverse proxy server.
- [Category:Web application](https://wiki.gentoo.org/wiki/Category:Web_application) - category containing some projects that can be configured to run with Apache.
