<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Securing_services | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Securing services -->
---
title: Security Handbook/Securing services
url: https://wiki.gentoo.org/wiki/Security_Handbook/Securing_services
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-02"
fingerprint: "1e19f918fd277b84"
license: CC BY-SA 4.0
---

# Security Handbook/Securing services

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section is on ensuring daemons are secure.

### Console usage

The /etc/securetty file allows system administrators to specify which TTY (terminal) devices the root user can use to login.

It is suggested to comment out all lines except `vc/1` on system using using devfs and all lines except `tty1` when using udev. This will ensure the root user only can login once and only on one terminal.

**`/etc/securetty`**

### Apache

Apache comes with a pretty decent configuration file. From a security perspective, some things can be improved. Binding Apache to one network interface' IP address and preventing it from volunteering information are two steps that can be taken to harden Apache.

If the `ssl` USE flag was not disabled before emerging Apache the server should be SSL enabled. Inside the /etc/apache2/vhosts.d/ directory example configuration files can be found. These are working examples and it is best to verify or disable them.

It is important to define configuration(s) to listen to a particular IP address (rather than all available IP addresses on the system). For instance the 00\_default\_vhost.conf file:

**`/etc/apache2/vhosts.d/00_default_vhost.conf`**

```
# Make it listen on a single network interface's IP address
Listen 127.0.0.1
```
We also recommend you to disable showing any information about your Apache installation to the world. By default, the configuration will add server version and virtual host name to server-generated pages. To disable this, change the `ServerSignature` variable to `Off`:

**`/etc/apache2/vhosts.d/00_default_vhost.conf`**

```
ServerSignature Off
```
Apache is compiled with `--enable-shared=max` and `--enable-module=all`. This will by default enable all modules, so you should comment out all modules in the LoadModule section (LoadModule and AddModule) that you do not use in the main /etc/apache2/httpd.conf configuration file. When using OpenRC, restart the service by executing /etc/init.d/apache2 restart.

Documentation is available at [https://www.apache.org](https://www.apache.org).

### Bind

One can find [Bind](https://wiki.gentoo.org/wiki/BIND) documentation at the [https://www.isc.org/bind-9-11-arm/](https://www.isc.org/). The [BIND 9 Administrator Reference Manual](https://www.isc.org/downloads/bind/doc/) is also in the doc/arm.

The newer [net-dns/bind](https://packages.gentoo.org/packages/net-dns/bind) BIND ebuilds support chrooting out of the box. After emerging bind follow the simple instructions.

### Djbdns

Djbdns is a DNS implementation on the security of which its author is willing to [bet money](https://cr.yp.to/djbdns/guarantee.html). It is very different from how Bind 9 works but worth a try. More information can be obtained from [https://cr.yp.to/djbdns.html](https://cr.yp.to/djbdns.html)

### FTP

Generally, using FTP (File Transfer Protocol) is a bad idea. It uses unencrypted data (ie. passwords are sent in clear text), listens on 2 ports (normally port 20 and 21), and attackers are frequently looking for anonymous logins for trading warez. Since the FTP protocol contains several security problems you should instead use [sftp](https://wiki.gentoo.org/wiki/Sftp) or HTTP. If this is not possible, secure your services as well as you can and prepare yourself.

#### Proftpd

Proftpd has had several security problems, but most of them seem to have been fixed. Nonetheless, it is a good idea to apply some enhancements:

**`/etc/proftpd/proftpd.conf`**

```
ServerName "My ftp daemon"
# Do not show the identity of the server
ServerIdent on "Go away"
# Makes it easier to create virtual users
RequireValidShell off
# Use alternative password and group file (passwd uses crypt format)
AuthUserFile "/etc/proftpd/passwd"
AuthGroupFile "/etc/proftpd/group"
# Permissions
Umask 077
# Timeouts and limitations
MaxInstances 30
MaxClients 10 "Only 10 connections allowed"
MaxClientsPerHost 1 "You have already logged on once"
MaxClientsPerUser 1 "You have already logged on once"
TimeoutStalled 10
TimeoutNoTransfer 20
TimeoutLogin 20
# Chroot everyone
DefaultRoot ~
# Do not run as root
User  nobody
Group nogroup
# Log every transfer
TransferLog /var/log/transferlog
# Problems with globbing
DenyFilter \*.*/
```
One can find documentation at [http://www.proftpd.org](http://www.proftpd.org).

#### Pure-ftpd

Pure-ftpd is an branch of the original trollftpd, modified for security reasons and functionality by Frank Dennis.

Use virtual users (never system accounts) by enabling the `AUTH` option. Set this to `-lpuredb:/etc/pureftpd.pdb` and create your users by using /usr/bin/pure-pw.

**`/etc/conf.d/pure-ftpd`**

```
AUTH="-lpuredb:/etc/pureftpd.pdb"
## Misc. Others ##
MISC_OTHER="-A -E -X -U 177:077 -d -4 -L100:5 -I 15"
```
Configure the  `MISC_OTHER` setting to deny anonymous logins (`-E`), chroot everyone (`-A`), prevent users from reading or writing to files beginning with a . (dot) (`-X`), max idle time (`-I`), limit recursion (`-L`), and a reasonable umask value.

Warning: Do not use the `-w` or `-W` options! If you want to have a warez site, stop reading this guide!

One can find documentation at [http://www.pureftpd.org](http://www.pureftpd.org).

#### Vsftpd

[Vsftpd](https://wiki.gentoo.org/wiki/Vsftpd) (short for very secure ftp) is a small ftp daemon running a reasonably default configuration. It is simple and does not have as many features as pureftp and proftp.

**`/etc/vsftpd`**

```
anonymous_enable=NO
local_enable=YES
#read only
write_enable=NO
#enable logging of transfers
xferlog_std_format=YES
idle_session_timeout=20
data_connection_timeout=20
nopriv_user=nobody
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chrootlist
ls_recurse_enable=NO
```
As you can see, there is no way for this service to have individual permissions, but when it comes to anonymous settings it is quite good. Sometimes it can be nice to have an anonymous ftp server (for sharing open source), and vsftpd does a really good job at this.

### MySQL

If you only need local applications to access the mysql database, uncomment the following line in /etc/mysql/my.cnf.

Disable network access:

**`/etc/mysql/my.cnf`**

Then we disable the use of the LOAD DATA LOCAL INFILE command. This is to prevent against unauthorized reading from local files. This is relevant when new SQL Injection vulnerabilities in PHP applications are found.

Disable LOAD DATA LOCAL INFILE in the \[mysqld\] section:

**`/etc/mysql/my.cnf`**

```
=local-infile=0
```
Next, we must remove the sample database (test) and all accounts except the local root account.

Removing sample database and all unnecessary users:

`mysql>````
drop database test;
```
`mysql>````
use mysql;
```
`mysql>````
delete from db;
```
`mysql>````
flush privileges;
```
### Netqmail

[Netqmail](https://wiki.gentoo.org/wiki/Netqmail) is often considered to be a very secure mail server. It is written with security (and paranoia) in mind. It does not allow relaying by default and has not had a security hole since 1996. Simply emerge netqmail and go configure!

### Samba

[Samba](https://wiki.gentoo.org/wiki/Samba) is a protocol to share files with Microsoft/Novell networks and it should not be used over the Internet. Nonetheless, it still needs securing.

**`/etc/samba/smb.conf`**

```
[global]
  # Bind to an interface
  interfaces = lo eth0 10.0.0.1/32
  # Only bind to listed interfaces 
  # (don't bind smbd to 0.0.0.0, make nmbd ignore martian broadcast sources) 
  bind interfaces only = yes
  directory security mask = 0700
  # allow traffic from 10.0.0.*
  hosts allow = 10.0.0.
  # Enables user authentication
  # (don't use the share mode)
  security = user
  # Disallow privileged accounts
  invalid users = root @wheel
  # Maximum size smb shows for a share (not a limit)
  max disk size = 102400
  # Uphold the password policy
  min password length = 8
  null passwords = no
  # Use PAM (if added support)
  pam password change = yes
```
Make sure that permissions are set correct on every share and remember to read the [documentation](https://www.samba.org/).

Now restart the server and add the users who should have access to this service. This is done though the command /usr/bin/smbpasswd with the parameter `-a`.

### SSH

The most important securing that OpenSSH needs is turning on a stronger authentication based on public key encryption. Too many sites (like [Sourceforge](https://www.sourceforge.net), [PHP](https://www.php.net) and [Apache](https://www.apache.org)) have suffered unauthorized intrusion due to password leaks or bad passwords.

**`/etc/ssh/sshd_config`**

```
# Do not enable DSA and ECDSA server authentication.
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key
# If you have a recent OpenSSH client disable weak ciphers and Message Authentication Code (MAC) by explicitly enabling stronger ciphers.
# check with ssh -Q cipher resp. ssh -Q mac which ciphers/ MACs are supported
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com
# Disable root login. Users should be using su or sudo to obtain root permissions.
PermitRootLogin no
# Turn on Public key authentication
PubkeyAuthentication yes
AuthorizedKeysFile      .ssh/authorized_keys
# Disable .rhost and normal password authentication.
HostbasedAuthentication no
PasswordAuthentication no
PermitEmptyPasswords no
# Only allow users in the wheel or admin group to login via SSH.
AllowGroups wheel admin
# In the above 'AllowedGroups' directive only allow the following users.
# Note: the @<domainname> is optional but replaces the older AllowHosts directive.
AllowUsers larry@gentoo.org cow@gentoo.org
# Logging
SyslogFacility AUTH
LogLevel INFO
# The ListenAddress directive should be changed to a single IP address
ListenAddress 127.0.0.1
```
Also verify `UsePAM yes` is not in the configuration file; it overrides the public key authentication mechanism. Alternatively `PasswordAuthentication` or `ChallengeResponseAuthentication` directives can be disabled. More information about these options can be found in the sshd\_config manual page (man 5 sshd\_config).

Now all that users have to do is create SSH public/private key pairs and type in a passphrase with the following command:

`root #``ssh-keygen -t ed25519`
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/larry/.ssh/id\_ed25519):\[Press enter\]
Created directory '/home/larry/.ssh'.
Enter passphrase (empty for no passphrase): \[Enter passphrase\]
Enter same passphrase again: \[Enter passphrase again\]
Your identification has been saved in /home/larry/.ssh/id\_ed25519.
Your public key has been saved in /home/larry/.ssh/id\_ed25519.pub.
The key fingerprint is:
SHA256:UZwgOwzktPyblYRMZjKnaD0HizvtnX+qVnk4liaZewI larry@gentoo

This will add two files to the user's \~/.ssh/ directory called id\_ed25519 and id\_ed25519.pub. The file named id\_ed25519 is the private key and should be accessible only to the user who created it. The other file id\_ed25519.pub is to be distributed to every remote server that requires SSH access. Add the key to the users home directory in the \~/.ssh/authorized\_keys file and the user should be able to login. This action can be performed in one-shot by using the ssh-copy-id command:

`user $``ssh-copy-id larry@remote`
Each user should guard their private key well. Put it on encrypted media that is easily accessible or keep it on their workstation (put this in the [password policy](https://wiki.gentoo.org/wiki/Security_Handbook/Pre-installation_concerns#Security_policies)).

For more information on SSH visit the [OpenSSH website](https://www.openssh.com/) and the [SSH](https://wiki.gentoo.org/wiki/SSH) article.

### TCP wrappers

TCP wrappers are a way of controlling access to services normally run by inetd (which Gentoo does not have), but it can also be used by xinetd and other services.

**`/etc/hosts.deny`**

**`/etc/hosts.allow`**

```
 LOCAL @wheel
time: LOCAL, .gentoo.org
```
As you can see the format is very similar to the one in /etc/security/access.conf. The tcpd facility supports a specific service; it does not overlap with /etc/security/access.conf. These settings only apply to services using TCP wrappers.

It is also possible to execute commands when a service is accessed (this can be used when activating relaying for dial-in users) but it is not recommended, since people tend to create more problems than they are trying to solve. An example could be that you configure a script to send an e-mail every time someone hits the deny rule, but then an attacker could launch a DoS attack by keep hitting the deny rule. This will create a lot of I/O and e-mails so don't do it!. Read the man 5 hosts\_access for more information.

### Using xinetd

[xinetd](https://en.wikipedia.org/wiki/xinetd) ([sys-apps/xinetd](https://packages.gentoo.org/packages/sys-apps/xinetd)) is a replacement for inetd (which Gentoo does not have), the Internet services daemon. It supports access control based on the address of the remote host and the time of access. It also provide extensive logging capabilities, including server start time, remote host address, remote user name, server run time, and actions requested.

As with all other services it is important to have a good default configuration. But since xinetd is run as root and supports protocols that you might not know how they work, we recommend not to use it. But if you want to use it anyway, here is how you can add some security to it:

`root #``emerge --ask sys-apps/xinetd sys-apps/tcp-wrappers`
And edit the configuration file:

**`/etc/xinetd.conf`**

```
{
 only_from = localhost
 instances = 10
 log_type = SYSLOG authpriv info
 log_on_success = HOST PID
 log_on_failure = HOST
 cps = 25 30
}
# This will setup pserver (cvs) via xinetd with the following settings:
# max 10 instances (10 connections at a time)
# limit the pserver to tcp only
# use the user cvs to run this service
# bind the interfaces to only 1 ip
# allow access from 10.0.0.*
# limit the time developers can use cvs from 8am to 5pm
# use tpcd wrappers (access control controlled in
# /etc/hosts.allow and /etc/hosts.deny)
# max_load on the machine set to 1.0
# The disable flag is per default set to no but I like having
# it in case of it should be disabled
service cvspserver
{
 socket_type = stream
 protocol = tcp
 instances = 10
 protocol = tcp
 wait = no
 user = cvs
 bind = 10.0.0.2
 only_from = 10.0.0.0
 access_times = 8:00-17:00
 server = /usr/sbin/tcpd
 server_args = /usr/bin/cvs --allow-root=/mnt/cvsdisk/cvsroot pserver
 max_load = 1.0
 log_on_failure += RECORD
 disable = no
}
```
For more information read man 5 xinetd.conf.

### X

By default [Xorg](https://wiki.gentoo.org/wiki/Xorg) is configured to act as an X server. This can be dangerous since X uses unencrypted TCP connections and listens for X clients.

But if you depend on using the workstation as a X server use the /usr/bin/xhost command with caution. This command allows clients from other hosts to connect and use your display. This can become handy if you need an X application from a different machine and the only way is through the network, but it can also be exploited by an attacker. The syntax of this command is /usr/bin/xhost +hostname

A more secure solution is to disable this feature completely by starting X with [startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) -- -nolisten tcp or disable it permanently in the configuration.

**`/usr/bin/startx`**

```
defaultserverargs="-nolisten tcp"
```
To make sure that [startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) does not get overwritten when emerging a new version of Xorg you must protect it. Add the following line to /etc/portage/make.conf:

```
CONFIG_PROTECT="/usr/bin/startx"
```
When using a graphical [login manager](https://wiki.gentoo.org/wiki/Login_manager), a different approach is needed.

#### GDM

For [gdm](https://wiki.gentoo.org/wiki/GNOME/gdm) (Gnome Display Manager):

**`/etc/X11/gdm/gdm.conf`**

```
[server-Standard]
command=/usr/X11R6/bin/X -nolisten tcp
```
#### XDM and KDM

For [xdm](https://wiki.gentoo.org/wiki/XDM) (X Display Manager) and [kdm](https://wiki.gentoo.org/wiki/KDE#Display_manager) (KDE Display Manager):

**`/etc/X11/xdm/Xservers`**

```
 local /usr/bin/X11/X -nolisten tcp
```




### Wayland

#### Input group

Avoid "input" Group: Do not add users to the input group.  In Wayland, the compositor is the *only* process that directly interacts with the raw input devices. It receives all keyboard, mouse, and touch events. Mediated approach is a core security feature of Wayland.

#### Desktop portals

Wayland security standardized API for granting granular permissions to desktop features. See [XDG/xdg-desktop-portal](https://wiki.gentoo.org/wiki/XDG/xdg-desktop-portal) — a frontend to implementations of the xdg-desktop-portal interface..
