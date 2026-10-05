<!-- source: https://wiki.gentoo.org/wiki/Samba/Guide | group: Gentoo Wiki (Main) | wiki-title: Samba/Guide -->
---
title: Samba/Guide
url: https://wiki.gentoo.org/wiki/Samba/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-28"
fingerprint: b499e959dc232888
license: CC BY-SA 4.0
---

# Samba/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This Samba guide is designed to help the reader move a network from many different clients speaking different languages, to many different machines that speak a common language. The ultimate goal is to help differing architectures and technologies, come together in a productive, happy coexisting environment.

Following the directions outlined in this article should provide an excellent step towards a peaceful cohabitation between Windows operating systems and virtually all known variations of \*nix.

## Introduction

### Purpose

This article originally started as a FAQ, morphed into a HOWTO document, then was converted into a wiki article. The original intent was to explore the functionality and power of Gentoo, Portage, and the flexibility of USE flags. Like so many other projects, it was quickly discovered what was missing in the Gentoo realm: there were not any Samba guides catered for Gentoo users. Gentoo users are more demanding than most; they require performance, flexibility, and customization. This does not however imply that this article was not intended for other distributions; rather that it was *designed* to work with a highly customized version of Samba.

This article provides a details guide showing users how to share files and printers between Windows and \*nix PCs. It will cover how to mount and manipulate shares.

In addition to shares few other topics will be mentioned, but they are out of the scope of this article. These will be noted as they are presented.

This article is based on a compilation and merge of an excellent HOWTO provided in the [Gentoo forums](https://forums.gentoo.org) by Andreas "daff" Ntaflos and the collected knowledge of Joshua Preston. The link to this discussion is provided below for reference purposes:

### Before using this guide

There are a several other guides for setting up CUPS and/or Samba, please read them as well, as they may provide instructions concerning things left out of this article (intentional or otherwise). One such document is the very useful and well written [Gentoo Printing Guide](https://wiki.gentoo.org/wiki/Printing), as configuration issues and specific printer setup will not be discussed here.

### Requirements

The following software will be needed:

- [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba)
- [net-print/cups](https://packages.gentoo.org/packages/net-print/cups)
- [net-print/hplip](https://packages.gentoo.org/packages/net-print/hplip) (if an HP printer will be used)
- A kernel (2.6 and up)
- A printer (PS or non-PS)
- A working network consisting of more than one machine

The main package used here is [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba), however, a kernel with CIFS support enabled is needed in order to mount a Samba or Windows share from another computer. CUPS will be emerged if it has not been already installed.

## Getting acquainted with Samba

### The USE flags

Before anything is emerging, take a look at some of the various USE flags available to Samba. Depending on the network topology and the specific requirements of the server, the USE flags outlined below will define what to include or exclude from the emerging of Samba.

| [+regedit](https://packages.gentoo.org/useflags/+regedit) | Enable support for regedit command-line tool | 
| [+system-mitkrb5](https://packages.gentoo.org/useflags/+system-mitkrb5) | Use app-crypt/mit-krb5 instead of app-crypt/heimdal. | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [addc](https://packages.gentoo.org/useflags/addc) | Enable Active Directory Domain Controller support | 
| [ads](https://packages.gentoo.org/useflags/ads) | Enable Active Directory support | 
| [ceph](https://packages.gentoo.org/useflags/ceph) | Enable support for Ceph distributed filesystem via sys-cluster/ceph | 
| [client](https://packages.gentoo.org/useflags/client) | Enables the client part | 
| [cluster](https://packages.gentoo.org/useflags/cluster) | Enable support for clustering | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [fam](https://packages.gentoo.org/useflags/fam) | Enable FAM (File Alteration Monitor) support | 
| [glusterfs](https://packages.gentoo.org/useflags/glusterfs) | Enable support for Glusterfs filesystem via sys-cluster/glusterfs | 
| [gpg](https://packages.gentoo.org/useflags/gpg) | Use app-crypt/gpgme for AD DC | 
| [iprint](https://packages.gentoo.org/useflags/iprint) | Enabling iPrint technology by Novell | 
| [json](https://packages.gentoo.org/useflags/json) | Enable json audit support through dev-libs/jansson | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [lmdb](https://packages.gentoo.org/useflags/lmdb) | Enable LMDB backend for bundled ldb | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [profiling-data](https://packages.gentoo.org/useflags/profiling-data) | Enables support for collecting profiling data | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [quota](https://packages.gentoo.org/useflags/quota) | Enables support for user quotas | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [snapper](https://packages.gentoo.org/useflags/snapper) | Enable vfs\_snapper module (requires sys-apps/dbus) | 
| [spotlight](https://packages.gentoo.org/useflags/spotlight) | Enable support for spotlight backend | 
| [syslog](https://packages.gentoo.org/useflags/syslog) | Enable support for syslog | 
| [system-heimdal](https://packages.gentoo.org/useflags/system-heimdal) | Use app-crypt/heimdal instead of bundled heimdal. | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Enable libunwind usage for backtraces | 
| [winbind](https://packages.gentoo.org/useflags/winbind) | Enables support for the winbind auth daemon | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

Some things worth mentioning about the USE flags and different Samba functions include:

- ACLs on ext2/3 are implemented through extended attributes (EAs). EA and ACL kernel options for ext2 and/or ext3 will need to be enabled (depending on which file system is being used - both can be enabled).
- While Active Directory, ACL, and PDC functions are out of the intended scope of this article, the following links may be helpful:
- Please remove existing "test" USE flags as that would cause problem:

## Server software installation

### Emerging Samba

To start things correctly, be sure that all hostnames resolve correctly. Either have a working domain name system running on the network or appropriate entries in the /etc/hosts file. Certain things could break if hostnames do not point to the correct machines.

Hopefully an assessment can be made of what will actually be need in order to use Samba with the a particular setup. The setup used for this article is:

- cups
- readline
- pam

To optimize performance, size, and the time of the build, USE flags are specifically included or excluded.

First, add USE flags to make sure that when samba is built, it has proper support:

`root #````
echo "net-fs/samba cups pam" >> /etc/portage/package.use
```
Now install the Samba software:

`root #``emerge --ask net-fs/samba`
This will emerge Samba and CUPS.

### HP printers

The following package should be emerged if an HP printer will be used:

`root #``emerge --ask net-print/hplip`
## Server configuration

### Configuring Samba

The main Samba configuration file is located at /etc/samba/smb.conf and can be modified using a standard text editor. Root permissions must be obtained to open the file. It is divided in sections indicated by `[sectionname]`. Comments are defined by either `#` (hash tags) or `;` (semicolons). A sample smb.conf file is included below with comments and suggestions for modifications. If more details are required, see the man page for smb.conf (`man smb.conf`, the installed smb.conf.example, the Samba website, or any of the numerous Samba books available at local libraries or for purchase where ever such books are sold.

**`/etc/samba/smb.conf`**

**Samba configuration example**

```
[global]
## # Replace MYWORKGROUPNAME with the appropriate workgroup/domain
workgroup = ## MYWORKGROUPNAME
## # Of course this has no REAL purpose other than letting
# everyone knows it is not Windows!
# %v prints the version of Samba being used.
server string = Samba Server %v
## # CUPS will be used; it should be inserted here
printcap name = cups
printing = cups
load printers = yes
## # This line enables the log file and limits its size to less than 50kb.
log file = /var/log/samba/log.%m
max log size = 50
## # We are going to set some options for our interfaces...
socket options = TCP_NODELAY SO_RCVBUF=8192 SO_SNDBUF=8192
## # This is a good idea, what we are doing is binding the
# samba server to our local network.
# For example, if eth0 is the local network interface:
interfaces = lo eth0
bind interfaces only = yes
## # Specifies which IP address range is allowed to access Samba
# (this is for added security since this configuration does
# not use passwords):
hosts allow = 127.0.0.1 192.168.1.0/24
hosts deny = 0.0.0.0/0
## # Other options for this are USER, DOMAIN, ADS, and SERVER
# The default is user
security = user
# previously set to share, but this no longer works
## # No passwords will be used so a guest account should be enabled:
guest ok = yes
## # Now set print drivers information:
[print$]
comment = Printer Drivers
path = /etc/samba/printer ## # This path holds the driver structure
guest ok = yes
browseable = yes
read only = yes
## # Modify the following to "<username>,root" to add a second printer admin:
write list = root
## # Setup a printer to share. While the name is arbitrary it
# should be consistent throughout Samba and CUPS:
[HPDeskJet930C]
comment = HP DeskJet 930C Network Printer
printable = yes
path = /var/spool/samba
guest ok = yes
## # Modify the following to "<username>,root" to add a second printer admin:
printer admin = root
## # Now set the printer share. This should be
# browseable, printable, public, etc.:
[printers]
comment = All Printers
browseable = no
printable = yes
writable = no
guest ok = yes
path = /var/spool/samba
## # Modify the following to "<username>,root" to add a second printer admin:
printer admin = root
## # Create a new share that can read from or written to anywhere.
# This is kind of like a temp public share; anyone can do what
# they want here:
[public]
comment = Public Files
browseable = yes
create mode = 0766
guest ok = yes
path = /home/samba/public
```
Create the directories required for the minimum configuration of Samba to share the installed printer throughout the network:

`root #````
mkdir /etc/samba/printer
```
`root #````
mkdir /var/spool/samba
```
`root #``mkdir /home/samba/public`
At least one Samba user is required in order to install the printer drivers and to allow users to connect to the printer. Users must exist in the system's /etc/passwd file. The next example uses the root user, but others can be used as well.

`root #``pdbedit -a root`
The Samba passwords need not be the same as the system passwords in /etc/passwd

You will also need to update the /etc/nsswitch.conf file so that Windows systems can be found easily using NetBIOS:

`user $``nano -w /etc/nsswitch.conf`
hosts: files dns wins

### Configuring CUPS

This section is a little more complicated. CUPS' main configuration file is /etc/cups/cupsd.conf Its structure is similar to Apache's httpd.conf file, so many may find it familiar. Outlined in the example are the directives that need to be changed:

**`/etc/cups/cupsd.conf`**

Edit /etc/cups/mime.convs file to uncomment some lines. The changes to mime.convs and mime.types are needed to make CUPS print Microsoft Office document files.

The following line is found near the end of the file. Uncomment it:

**`/etc/cups/mime.convs`**

Edit /etc/cups/mime.types to uncomment some lines.

The following line is found near the end of the file. Uncomment it:

**`/etc/cups/mime.types`**

CUPS needs to be started on boot, and started immediately.

`root #````
rc-update add cupsd default
```
`root #``/etc/init.d/cupsd restart`
### Installing a printer for and with CUPS

First, go to [OpenPrinting](https://wiki.linuxfoundation.org/openprinting/database/databaseintro) to find and download the correct PPD file for the printer and CUPS. To do so, click the link Printer Listings to the left. Select the printers manufacturer and the model in the pull down menu, e.g. HP and DeskJet 930C. Click "Show". On the page coming up click the "recommended driver" link after reading the various notes and information. Then fetch the PPD file from the next page, again after reading the notes and introductions there. You may have to select your printers manufacturer and model again. Reading the [CUPS quickstart guide](https://wiki.linuxfoundation.org/openprinting/database/cupsdocumentation) is also very helpful when working with CUPS.

Now you have a PPD file for your printer to work with CUPS. Place it in /usr/share/cups/model . The PPD for the HP DeskJet 930C was named HP-DeskJet\_930C-hpijs.ppd . You should now install the printer. This can be done via the CUPS web interface or via command line. The web interface is found at [http://PrintServer:631](http://PrintServer:631) once CUPS is running.

`root #````
lpadmin -p HPDeskJet930C -E -v usb:/dev/ultp0 -m HP-DeskJet_930C-hpijs.ppd
```
`root #``/etc/init.d/cupsd restart`
Remember to adjust to what you have. Be sure to have the name (`-p` option) right (the name you set above during the Samba configuration!) and to put in the correct `usb:/dev/usb/blah` , `parallel:/dev/blah` or whatever device you are using for the printer.

The printer should now be available from the web interface and to print a test page.

### Installing the Windows printer drivers

Now that the printer should be working it is time to install the drivers for the Windows clients to work. Samba 2.2 introduced this functionality. Browsing to the print server in the Network Neighbourhood, right-clicking on the printer share and selecting "connect" downloads the appropriate drivers automatically to the connecting client, avoiding the hassle of manually installing printer drivers locally.

There are two sets of printer drivers for this. First, the Adobe PS drivers which can be obtained from [Adobe](https://www.adobe.com/support/downloads/main.html) (PostScript printer drivers). Second, there are the CUPS PS drivers, to be obtained by emerging [net-print/cups-windows](https://packages.gentoo.org/packages/net-print/cups-windows). There does not seem to be a difference between the functionality of the two, but the Adobe PS drivers need to be extracted on a Windows System since it's a Windows binary. Also the whole procedure of finding and copying the correct files is a bit more hassle. The CUPS drivers support some options the Adobe drivers don't.

This article uses the CUPS drivers for Windows. Install them as shown:

`root #``emerge --ask net-print/cups-windows`
Now use the cupsaddsmb script provided by the CUPS distribution. Be sure to read its man page (`man cupsaddsmb`), as it will tell which Windows drivers are needed to be copied to the proper CUPS directory. Once the drivers have been copied, restart CUPS by issuing /etc/init.d/cupsd restart to the command line. Next, run cupsaddsmb as shown:

`root #``cupsaddsmb -H PrintServer -U root -h PrintServer -v HPDeskJet930C`
Instead of HPDeskJet930C the `-a` option use be specified. This will "export all known printers":

`root #``cupsaddsmb -H PrintServer -U root -h PrintServer -a`


Here are common errors that may happen:

- The hostname given as a parameter for `-h` and `-H` ( `PrintServer` ) often does not resolve correctly and does not identify the print server for CUPS/Samba interaction. If an error like: **Warning: No PPD file for printer "CUPS\_PRINTER\_NAME" - skipping!** occurs, the first thing to do is substitute `PrintServer` with `localhost` and try again.
- The command fails with an **NT\_STATUS\_UNSUCCESSFUL** error. This error message is quite common, but can be triggered by many problems. It is unfortunately not very helpful. One thing to try is to temporarily set `security = user` in the smb.conf After/if the installation completes successfully, you should set it back to share, or whatever it was set to before.

This should install the correct driver directory structure under /etc/samba/printer . That would be /etc/samba/printer/W32X86/2/ The files contained should be the 3 driver files and the PPD file, renamed to YourPrinterName.ppd (the name given to the printer when installing it, see above).

Pending no errors or other complications drivers will now be installed.

### Installing the Web Service Discovery host daemon

#### wsdd

wsdd implements a Web Service Discovery host daemon. This enables (Samba) hosts, like your local NAS device, to be found by Web Service Discovery Clients like Windows.

![](https://wiki.gentoo.org/images/thumb/c/cd/Network_Neighbourhood_%28Windows_10%29.png/300px-Network_Neighbourhood_%28Windows_10%29.png)

##### Purpose

Since NetBIOS discovery is not supported by Windows anymore, wsdd makes hosts to appear in Windows again using the Web Service Discovery method. This is beneficial for devices running Samba, like NAS or file sharing servers on your local network.

##### Background

With Windows 10 version 1511, support for SMBv1 and thus NetBIOS device discovery was disabled by default. Depending on the actual edition, later versions of Windows starting from version 1709 ("Fall Creators Update") do not allow the installation of the SMBv1 client anymore. This causes hosts running Samba not to be listed in the Explorer's "Network (Neighborhood)" views. While there is no connectivity problem and Samba will still run fine, users might want to have their Samba hosts to be listed by Windows automatically.

##### Installing

First, you must [enable the GURU package repository.](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users#Adding_the_GURU_repository)

`root #``emerge net-misc/wsdd`
##### Starting the Wsdd service

Now configure Wsdd to start at boot and then start it.

###### OpenRC

`root #````
rc-update add wsdd default
```
`root #``/etc/init.d/wsdd start`
###### systemd

`root #````
systemctl --now enable wsdd.service
```
### Finalizing the setup

Lastly, setup the directories:

`root #````
mkdir /home/samba
```
`root #````
mkdir /home/samba/public
```
`root #````
chmod 755 /home/samba
```
`root #``chmod 755 /home/samba/public`
### Testing the Samba configuration

Test the configuration file to ensure that it is formatted properly and to verify all options have the correct syntax. To do this, run the testparm program:

`root #``/usr/bin/testparm`
Load smb config files from /etc/samba/smb.conf
Processing section "\[printers\]"
Global parameter guest account found in service section!
Processing section "\[public\]"
Global parameter guest account found in service section!
Loaded services file OK.
Server role: ROLE\_STANDALONE
Press enter to see a dump of your service definitions
 ...
 ...

### Starting the Samba service

Now configure Samba to start at boot and then start it.

#### OpenRC

`root #````
rc-update add samba default
```
`root #``/etc/init.d/samba start`
#### systemd

`root #````
systemctl --now enable smbd.service
```
`root #````
systemctl --now enable nmbd.service
```
### Checking the services

It might be prudent to check out logs at this time. Take a peak at the Samba shares using the smbclient command:

`root #``smbclient -L localhost`
Password:
## (A BIG list of services should be displayed here.)

## Configuration of the clients

### Printer configuration of \*nix based clients

Despite the variation or distribution, the only thing needed is CUPS. Do the equivalent on any other UNIX/Linux/BSD client.

`root #``emerge --ask net-print/cups``root #``nano -w /etc/cups/client.conf`
ServerName PrintServer      ## # your printserver name

That should be it. Nothing else will be needed.

When using only one printer, it will be set as the default printer. If the print server manages several printers, the administrator will have defined a default printer on the server. If a user would like to define a different default printer, use the lpoptions command.

First list the available printers:

`root #``lpstat -a`
HPDeskJet930C accepting requests since Jan 01 00:00
laser accepting requests since Jan 01 00:00

Now define the printer (in the example, HPDeskJet930C) as the default printer:

`root #``lpoptions -d HPDeskJet930C`
To enable printing on the Unix/Linux systems, either specify the printer to be used, or just use the default printer (second example):

`root #````
lp -d HPDeskJet930C anything.txt
```
`root #``lp foobar.whatever.ps`
Point a web browser to [http://printserver:631](http://printserver:631)`printserver` with the name of the *machine* which acts as the print server. Be careful to not use the name given to the cups print server if a different name was used.

Now is time to configure our kernel to support CIFS. Since I'm assuming we've all compiled at least one kernel, we'll need to make sure we have all the right options selected in our kernel. For simplicity's sake, make it a module for ease of use. It is the author's opinion that kernel modules are a good thing and should be used whenever possible.

**Kernel support**

Then make the module/install it; insert it with:

`root #``modprobe cifs`
Once the module is loaded, mounting a Windows or Samba share is possible. Use mount to accomplish this, as detailed below.

The syntax for mounting a Windows/Samba share is:

`root #``mount -t cifs [-o username=xxx,password=xxx] //server/share /mnt/point`
You can drop the `username` and `password` options if no password is needed.

`root #``mount -t cifs //PrintServer/public /mnt/public`
After mounting the share, it can be accessed as if it were a local drive.

### Printer configuration for Windows NT/2000/XP clients

Configuring printers is simply a bit of point-and-click. Browse to \\PrintServer and right click on the printer (HPDeskJet930C) and click connect. This will download the drivers to the Windows client which will enable applications (such as Word or Acrobat) to offer HPDeskJet930C as an available printer.

### Access for Windows XP clients

The Samba server smb.conf requires at least the following for enabling access from Windows XP clients.

**`/etc/samba/smb.conf`**

Modify the Windows XP registery with the following, exporting/saving the original default first:

Change LMCompatibilityLevel=0 to LMCompatibilityLevel=5

Restart the Samba server and Windows XP client.

### Debugging Samba server access

Can view more Samba server debug info by adding the following to the /etc/smb.conf and restarting the Samba server:

**`/etc/samba/smb.conf`**

### KDE Plasma 5

**kio-extras** contains Samba support, but it requires the [samba](https://packages.gentoo.org/useflags/samba) [USE flag](https://wiki.gentoo.org/wiki/USE_flag).

## See also

## External resources

- [CUPS Homepage](https://www.cups.org/)
- [Samba Homepage](https://www.samba.org/) , especially the [chapter on Samba/CUPS configuration](https://www.samba.org/samba/docs/man/Samba-HOWTO-Collection/CUPS-printing.html)
- [OpenPrinting](https://wiki.linuxfoundation.org/openprinting/start)
- [Kurt Pfeifle's Samba Print HOWTO](https://www.openprinting.org/download/kpfeifle/SambaPrintHOWTO/) ( This HOWTO really covers *ANYTHING* and *EVERYTHING* I've written here, plus a LOT more concerning CUPS and Samba, and generally printing support on networks. An interesting read with lots and lots of details.)
- [FreeBSD Diary's CUPS Topic](http://www.freebsddiary.org/cups.php)

### Troubleshooting

See [this page](https://www.openprinting.org/download/kpfeifle/SambaPrintHOWTO/Samba-HOWTO-Collection-3.0-PrintingChapter-11th-draft.html#37) from Kurt Pfeifle's "Printing Support in Samba 3.0" manual. Lots of useful tips there! Be sure to look this one up before posting questions and problems. Using this resource the solution may be quickly found.
