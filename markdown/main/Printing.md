<!-- source: https://wiki.gentoo.org/wiki/Printing | group: Gentoo Wiki (Main) | wiki-title: Printing -->
---
title: Printing
url: https://wiki.gentoo.org/wiki/Printing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-23"
fingerprint: bc8b9743f525b98a
license: CC BY-SA 4.0
---

# Printing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

This document covers the installation and maintenance of printers using CUPS and [Samba](https://wiki.gentoo.org/wiki/Samba). It covers local installation and networked installations and contains instructions on using shared printers from other operating systems.

For background information on standards related to printing, such as IPP, AirPrint and IPP Everywhere, refer to the [Printing/Standards](https://wiki.gentoo.org/wiki/Printing/Standards) page.

For information about printers that support 'driverless printing', refer to [this OpenPrinting database](https://openprinting.github.io/printers/).

For information about printers that require a driver, and the level of Linux support for such models, refer to [this OpenPrinting database](https://openprinting.org/printers).

## Printing and Gentoo Linux

In this document we will cover how to use CUPS, the [Common Unix Printing System](https://www.cups.org/), to set up a local or networked printer. It will not go into too much detail since the project has [documentation](https://www.cups.org/documentation.html) available for advanced usage.

## Installation

### Kernel

When a user desires to install a printer on a system the first step is knowing how the printer will be attached to the system. Is it through a local port like LPT or USB, or is it networked? If it is networked, does it use the Internet Printing Protocol (IPP) or the Microsoft Windows CIFS protocol (Microsoft Windows Sharing)?

The next few sections explain what minimal kernel configuration is needed to get a printer connected in Gentoo. Of course, this depends on *how* the printer is going to be attached to the system, so for convenience the instructions have been separated.

In the next configuration examples, the necessary support will be added *into* the kernel, not as modules. Building the kernel this way is not mandatory; if desired modular support can be easily added, just be sure to remember to load the appropriate modules!

#### Locally attached printer (LPT)

The LPT port is generally used to identify the parallel printer port. You need to enable parallel port support first, then PC-style parallel port support (unless using a SPARC system) after which you enable parallel printer support.

**Parallel Port printer configuration**

#### Locally attached printer (USB)

USB printing is supported by CUPS with the [usb](https://packages.gentoo.org/useflags/usb) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) enabled. This uses the libusb library for user space USB support.

Some older software titles might still require the in-kernel USB printer support. If built as a module, this module would be called usblp:

**USB Printer support**

However, using the in-kernel USB printer support is [considered obsolete](https://forums.gentoo.org/viewtopic-t-1053530-start-10.html). Only pursue this when needed.

#### Remotely attached printer (IPP and LPD)

To be able to connect to a remotely attached printer through the [Internet Printing Protocol](https://en.wikipedia.org/wiki/Internet_Printing_Protocol) or the [Line Printer Daemon protocol](https://en.wikipedia.org/wiki/Line_Printer_Daemon_protocol) the kernel needs to have networking support. Assuming the kernel has that already, continue with CUPS.

#### Remotely attached printer (CIFS)

The kernel must support [CIFS](https://wiki.gentoo.org/wiki/CIFS) file system:

**CIFS printer configuration**

### USE flags

Some packages are aware of the [cups](https://packages.gentoo.org/useflags/cups) [USE flag](https://wiki.gentoo.org/wiki/USE_flag). CUPS has a few optional features that might be of interest. To enable or disable those features, use the USE flags associated with them.


| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [openssl](https://packages.gentoo.org/useflags/openssl) | Use dev-libs/openssl instead of net-libs/gnutls for TLS support | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [usb](https://packages.gentoo.org/useflags/usb) | Add USB support to applications that have optional USB support (e.g. cups) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xinetd](https://packages.gentoo.org/useflags/xinetd) | Add support for the xinetd super-server | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

`root #``emerge --ask net-print/cups`
#### Recommendations

To avoid any future errors or headaches when using CUPS, you should also install the [net-print/cups-meta](https://packages.gentoo.org/packages/net-print/cups-meta) package.


| [+browsed](https://packages.gentoo.org/useflags/+browsed) | Include support for the cups-browsed daemon. | 
| [+foomatic](https://packages.gentoo.org/useflags/+foomatic) | Include support for the foomatic-rip printer driver. Strongly recommended. | 
| [+poppler](https://packages.gentoo.org/useflags/+poppler) | Include support for the app-text/poppler filters. | 
| [+postscript](https://packages.gentoo.org/useflags/+postscript) | Enable support for the PostScript language (often with ghostscript-gpl or libspectre) | 
| [pdf](https://packages.gentoo.org/useflags/pdf) | Add general support for PDF (Portable Document Format), this replaces the pdflib and cpdflib flags | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

`root #``emerge --ask net-print/cups-meta`
## Samba

To enable [Samba](https://wiki.gentoo.org/wiki/Samba) support, [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba) needs to be installed with CUPS support. Update the /etc/portage/package.use file or directory to enable the `cups` USE flag:

**`/etc/portage/package.use`**

**Enabling cups USE flag for Samba**

Then (re)install Samba:

`root #``emerge --ask --changed-use net-fs/samba`
## Avahi

CUPS uses [Avahi](https://wiki.gentoo.org/wiki/Avahi) internally when built with the `zeroconf` USE flag to scan for printers on the local network. To use Avahi hostnames to connect to networked printers, set up .local hostname resolution and restart the CUPS service. CUPS and cups-filters need to be built with the `zeroconf` USE flag as well.  Use the [driverless(1)](https://man.archlinux.org/man/driverless.1.en) [command (provided by the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [net-print/cups-filters](https://packages.gentoo.org/packages/net-print/cups-filters) package) for listing available printers.

`user $``driverless list`
## Configuration

### Printing group

Any user that needs to print should be added to the lp group:

`root #``gpasswd -a larry lp`
In order to be able to add printers and edit them via CUPS's web interface, any system user that is allowed to edit these settings should be in the lpadmin group:

`root #``gpasswd -a larry lpadmin`
### Service

#### OpenRC

If the printer is attached to the system locally, and the printer needs to be available every boot, the CUPS daemon will need to load automatically on start-up. Make sure the printer is attached and powered on before the CUPS daemon is started.

`root #````
rc-service cupsd start
```
`root #````
rc-update add cupsd default
```
#### systemd

To start the CUPS daemon immediately and to make it start when the system boots, issue:

`root #````
systemctl start cups.service
```
`root #````
systemctl enable cups.service
```
### HTTP interface

Once the service is started, printers can be added by authenticated users. root is available by default and any member of the lpadmin group. Open up the following URL in a web browser:

### Files

The default CUPS server configuration located in /etc/cups/cupsd.conf is sufficient for most users. However, some users might need to make changes to the CUPS configuration.

In the next section covers a few changes that are often needed:

- Allow other systems to use the printer attached to this Linux workstation.
- Grant access to the CUPS administration from remote systems.
- Configure CUPS to support Windows PCL drivers. This is advised for Windows systems to be able to use a [Samba](https://wiki.gentoo.org/wiki/Samba)-shared printer since most Windows drivers are PCL drivers.
- Configure this system to use a printer attached to another system (not Windows share).

### Remote printer access

For other systems to use the printer through IPP, explicit access to the printer must be granted in the /etc/cups/cupsd.conf file. To share the printer using [Samba](https://wiki.gentoo.org/wiki/Samba), this change is not needed.

Open up /etc/cups/cupsd.conf in a favorite text editor and add in an `Allow` line for the system(s) that should be able to reach to the printer. In the next example, access is granted to the printer from localhost and from any system whose IP address starts with `192.168.0`.

**`/etc/cups/cupsd.conf`**

**Allowing remote access to the printer**

```
<Location />
  Order allow,deny
  Allow localhost
  Allow from 192.168.0.*
</Location>
```
This line broadcasts browsing information to the clients on the network; it will let network users know when the printer is available:

**`/etc/cups/cupsd.conf`**

**Broadcast info**

```
BrowseAddress 192.168.0.*:631
```
The port CUPS listens to will also need to be specified so that it will respond to printing requests from other machines on the network:

**`/etc/cups/cupsd.conf`**

**Port configuration**

```
Listen *:631
#Listen localhost:631
```
The CUPS server reject a hostname or server alias in the HTTP request with "Bad request" message. It works with IP-addresses by default. So if you want to print or browse CUPS interface by using a hostname or domain, add the ServerAlias parameter:

**`/etc/cups/cupsd.conf`**

**Server alias configuration**

```
ServerAlias *
```
### CUPS remote administration

If remote administration is needed, then access to the CUPS administration will need to be granted from more systems than the localhost. Edit the /etc/cups/cupsd.conf file and have explicit access granted to each system that requires access. For instance, to grant access to a system with an IP address of 192.168.0.3:

**`/etc/cups/cupsd.conf`**

**Allowing remote access**

```
<Location /admin>
(...)
  Encryption Required
  Order allow,deny
  Allow localhost
  Allow 192.168.0.3
</Location>
```
Do not forget to restart the CUPS daemon after making changes to /etc/cups/cupsd.conf by issuing the /etc/init.d/cupsd restart command (for OpenRC users) or systemctl restart cupsd.service (for systemd users).

### Enable support for Windows PCL drivers

PCL drivers send raw data to the print server. To enable raw printing on CUPS, edit /usr/share/cups/mime/mime.types and uncomment the `application/octet-stream` line if it is not already uncommented. Then edit /usr/share/cups/mime/mime.convs and do the same, if it is not already uncommented.

**`/usr/share/cups/mime/mime.types`**

**Enable support for raw printing**

**`/usr/share/cups/mime/mime.convs`**

Do not forget to restart the CUPS daemon after making these changes by running /etc/init.d/cupsd restart (for OpenRC users) or systemctl restart cupsd.service (for systemd users).

### Setting up a remote printer

If the printers are attached to a remote CUPS-powered server the system can be easily configured to use the remote printer by modifying the /etc/cups/client.conf file.

Assuming the printer is attached to a system called `printserver.mydomain`, open up /etc/cups/client.conf with a favorite text editor and set the `ServerName` directive:

**`/etc/cups/client.conf`**

The remote system will have a default printer setting which will be used. To change the default printer, use the lpoptions command.

First list the available printers:

`root #``lpstat -a`
hpljet5p accepting requests since Jan 01 00:00
hpdjet510 accepting requests since Jan 01 00:00

Set the HP LaserJet 5P as the default printer:

`root #``lpoptions -d hpljet5p`
### Configuring a printer

#### Introduction

If the printer to be configured is remotely available through a different print server (running CUPS) then the following instructions are not needed. Instead, read [Setting up a Remote Printer](https://wiki.gentoo.org/wiki/Printing#Setting_up_a_remote_printer).

#### Detecting the printer

If a USB printer or parallel port printer was powered on when the Linux system booted, it might be possible to retrieve information from the kernel stating successful detection of the printer. This is merely an indication of print detection and not a requirement.

`user $``dmesg | grep -i print`
parport0: Printer, Hewlett-Packard HP LaserJet 2100 Series

For a USB connected printer:

`user $``lsusb`
(...)
Bus 001 Device 007: ID 03f0:1004 Hewlett-Packard DeskJet 970c/970cse


The [lpinfo](https://www.cups.org/doc/man-lpinfo.html) command can be used in order to list all connected printers:

`root #``lpinfo -v`
network ipp
network http
network socket
network https
network ipps
network lpd
network lpd://BRW67890ABCDEF/BINARY\_P1

Running lpinfo -l -v will give a more verbose output.



#### Listing available drivers

To list all available drivers, execute the following command:

`user $``lpinfo -m`
lpinfo is not chatty and can be a little tricky to use. If any issue arises, see man lpinfo for more information.

#### Installing the printer

To have the printer installed on the system, fire up a browser and point it to [http://localhost:631](http://localhost:631). The CUPS web interface should be displayed from which all administrative tasks can be performed.

Go to Administration and enter the root login and password information of the box. Then, when the administrative interface has been reached, click on Add Printer. A new screen will be displayed allowing the following information to be entered:

- The *spooler name*, a short but descriptive name used on the system to identify the printer. This name should not contain spaces or any special characters. For instance, for the HP LaserJet 5P could be titled `hpljet5p`.
- The *location*, a description where the printer is physically located (for instance "bedroom", or "in the kitchen right next to the dish washer", etc.). This is to aid in maintaining several printers.
- The *description*, a full description of the printer. A common use is the full printer name (like "HP LaserJet 5P").

The next screen requests the device the printer listens to. The choice of several devices will be presented. The next table covers a few possible devices, but the list is not exhaustive.

| Device | Description | 
|---|---|
| AppSocket/HP JetDirect | This special device allows for network printers to be accessible through a HP JetDirect socket. Only specific printers include support for this option. | 
| Internet Printing Protocol (IPP or HTTP) | Used reach the remote printer through the IPP protocol either directly (IPP) or through HTTP. | 
| LPD/LPR Host or Printer | Select this option if the printer is remote and attached to a LPD/LPR server. | 
| Parallel Port #1 | Select when the printer is locally attached to a parallel port (LPT). When the printer is automatically detected its name will be appended to the device. | 
| USB Printer #1 | Select when the printer is locally attached to a USB port. The printer name should automatically be appended to the device name. | 

If installing a remote printer, the URL to the printer will be queried:

- An LPD printer server requires a `lpd://hostname/queue` syntax.
- An HP JetDirect printer requires a `socket://hostname` syntax.
- An IPP printer requires a `ipp://hostname/printers/printername` or `http://hostname:631/printers/printername` syntax.

Next, select the printer manufacturer in the adjoining screen along with the model type and number in the subsequent screen. For many printers multiple drivers will be available. Select one now or search on [OpenPrinting Printer List](https://www.openprinting.org/printers) for a good driver. Drivers are easily able to be changed later.

Once the driver is selected, CUPS will inform that the printer has been added successfully to the system. Navigate to the printer management page on the administration interface and select Configure Printer to change the printer's settings (resolution, page format, ...).

#### Testing and reconfiguring the printer

To verify if the printer is working correctly, go to the printer administration page, select the printer and click on Print Test Page.

If the printer does not seem to work correctly, click on Modify Printer to reconfigure the printer. The same screens as during the first installation will appear but the defaults will now be the current configuration.

If the printer does not function, clues may be found by looking at the CUPS error log located at /var/log/cups/error\_log. In the next example a permission error is discovered, probably due to a wrong Allow setting in the /etc/cups/cupsd.conf file.

`root #``tail /var/log/cups/error_log`
(...)
E \[11/Jun/2005:10:23:28 +0200\] \[Job 102\] Unable to get printer status (client-error-forbidden)!

#### Installing the best driver

Many printer drivers exist; to find out which one has the best performance the job, visit the [OpenPrinting Printer List](https://www.openprinting.org/printers). Select the brand and type/model of the printer to find out what driver the site recommends. For instance, for the HP LaserJet 5P, the site recommends the `ljet4` driver.

Download the PPD file from the site and place it in /usr/share/cups/model then run /etc/init.d/cupsd restart (for OpenRC users) or systemctl restart cupsd.service (for systemd users) as root. This will make the driver available through the CUPS web interface. Now reconfigure the printer as described above.

#### Enabling job accounting in for Xerox printers

High-end Xerox printers (often a gray, cabinet sized device) use XCPT PDL, and XML based, and poorly documented XPIF ticketing instruction format.

XCPT filter in Cups never made it to a release grade, and the work on it was eventually dropped and all XPIF must be input into a PPD manually. Luckily, it's largely a direct copy of IPP, using XML syntax. After peeking into docs available online, we can craft an arbitrary XPIF command using corresponding IPP attributes.

To configure XPIF solely for ticketing/accounting, drop the following into any PPD:

It will draw a dropdown box in any printing ui compliant with CUPS PPD extensions to enter the id.

The long term solution would still be for Xerox to fully publish XPIF, and XCPT specifications, to allow for a proper XPIF cups filter to be developed.

### Using special printer drivers

#### Introduction

Some printers require specific drivers or provide additional features that are not enabled through the regular configuration process (described above). This chapter will discuss a selection of printers and how they are made to work with Gentoo Linux.

#### Gutenprint driver

The [Gutenprint](http://gimp-print.sourceforge.net/) drivers are high-quality, open source printer drivers for various Canon, Epson, HP, Lexmark, Sony, Olympus and PCL printers supporting CUPS. They also support ghostscript, The Gimp, and other applications.

[net-print/gutenprint](https://packages.gentoo.org/packages/net-print/gutenprint) contains gutenprint drivers. Note the ebuild requests to quite a few USE flags. At minimum `cups` and `ppds` must enabled for gutenprint drivers to work properly.

`root #``emerge --ask net-print/gutenprint`
When the emerge process has finished, the gutenprint drivers will be available through the CUPS web interface.

#### HPLIP driver

See [HPLIP Driver](https://wiki.gentoo.org/wiki/HPLIP).

#### Lexmark driver

Most Lexmark printers are handled by their "Universal Printer Driver":

`root #``emerge --ask net-print/lexmark-upd-ppd`
Once this is installed, there is a single Lexmark driver available in the CUPS setup wizard that should work with most printers and MFDs.

#### PNM2PPA driver

PPA is an HP technology that focuses on sending low-level processing to the system instead of the printer which makes the printer cheaper but more resource consuming.

If the [OpenPrinting](https://www.openprinting.org/printers) site informs the [pnm2ppa](http://pnm2ppa.sourceforge.net/) driver is the best option, then the [net-print/pnm2ppa](https://packages.gentoo.org/packages/net-print/pnm2ppa) filter will need to be installed on the system:

`root #``emerge --ask net-print/pnm2ppa`
Once installed, download the PPD file for the printer [OpenPrinting](https://www.openprinting.org/printers) and put it in the /usr/share/cups/model folder. Then configure the printer using the steps explained above.

#### SpliX driver

[SpliX](http://splix.ap2c.org/) is a set of CUPS printer drivers for SPL (Samsung Printer Language) printers. While SpliX drivers are available through [OpenPrinting](https://www.openprinting.org/printers) as well, the [net-print/splix](https://packages.gentoo.org/packages/net-print/splix) package allows for quick portage-managed installation of these drivers. To install, run:

`root #``emerge --ask net-print/splix`
and restart cupsd.

#### Brother printer drivers

See [Brother networked printer](https://wiki.gentoo.org/wiki/Brother_networked_printer).

#### Canon printer drivers

See the specific pages:

Some Canon printer drivers may require the [net-print/cnijfilter2](https://packages.gentoo.org/packages/net-print/cnijfilter2) package to function properly, to install it, use:

`root #``emerge --ask net-print/cnijfilter2`
### Printing to and from Microsoft Windows

#### Configuring a Windows client for IPP

Microsoft Windows supports IPP. To install a printer on Windows that is attached to a Linux box, fire up the Add Printer wizard and select Network Printer. When asked for the URI, use the `http://hostname:631/printers/queue` syntax.

To share the printer on the CIFS network, [Samba](https://wiki.gentoo.org/wiki/Samba) must be installed and configured correctly. Doing this is beyond the scope of this article, however a quick configuration of Samba for shared printers will be covered.

Open /etc/samba/smb.conf with a favorite text editor and add a `[printers]` section to it:

Navigate to the top of the smb.conf file until inside the `[global]` section. Locate the `printcap name` and `printing` settings and set each of them to `cups` (see the example below):

Make sure to enable [Windows PCL](https://wiki.gentoo.org/wiki/Printing#Enable_support_for_Windows_PCL_drivers) support in CUPS. Then, restart the smb service to have the changes take effect.

#### Configuring a Linux client for a Windows print server

First make sure the printer is shared on Windows systems and that [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba) has been emerged with the `cups` USE flag enabled (as instructed above).

To find the desired printer's URI, run the following command, substituting `server` with the computer that is to probe for [Samba](https://wiki.gentoo.org/wiki/Samba)-shared printers:

`user $``smbclient -N '\\server\'`
In the CUPS web interface, configure the printer as previously described. Notice CUPS has added another driver called `Windows Printer via SAMBA`. Select it and use the `smb://username:password@workgroup/server/printername` or `smb://server/printername` syntax for the URI.

#### Introduction

Many tools exist to help configure a printer, use additional printing filters, add features to printing capabilities, etc. This chapter will list a few of them. Be aware the list is not exhaustive and not meant to discuss each tool in great detail.

#### Gtk-LP - A GTK-powered printer configuration tool

With [net-print/gtklp](https://packages.gentoo.org/packages/net-print/gtklp), the installation, modification and configuration of a printer can be performed from a stand-alone GTK application. It uses CUPS and provides all standard CUPS capabilities. It is definitely worth checking out if the CUPS Web interface is disliked or if a stand-alone application for day-to-day printing routines is desired.

Install via:

`root #``emerge --ask net-print/gtklp`
#### Printer configuration tool for KDE Plasma

[KDE Plasma](https://wiki.gentoo.org/wiki/KDE) also has a printer config tool called [kde-plasma/print-manager](https://packages.gentoo.org/packages/kde-plasma/print-manager). It works with CUPS and provides a user-friendly interface to configure printers. Install it as follows:

`root #``emerge --ask kde-plasma/print-manager`
## Printing from the command line

List available printers and the default destination:

`user $``lpstat -p -d`
printer HLL2360D is idle.  enabled since Tue 30 Jul 2024 15:54:45
no system default destination

List accepting status of all printers:

`user $``lpstat -a`
HLL2360D accepting requests since Tue 30 Jul 2024 15:54:45

Specify a default destination:

`user $``lpoptions -d HLL2360D`
List the available options for a given printer, together with their current values:

`user $``lpoptions -p HLL2360D -l`
Print a page range:

`user $``lpr -o page-ranges=1-4,7,9-12 document.pdf`
For further examples of using CUPS from the command line, refer to [the CUPS "Command-Line Printing and Options" page](https://www.cups.org/doc/options.html).

## Removal

### USE flags

Packages that are currently installed with the `cups` USE flag must be modified. Search through /etc/portage/package.use to see if any packages explicitly have the `cups` flag and remove it.

Next, it may be necessary to remove the `cups` value from /etc/portage/make.conf's `USE` variable if it had been previously set.

### Unmerge

`root #``emerge --ask --depclean --verbose net-print/cups`
Finally, clean the system of any packages that are no longer needed as a result of CUPS being removed.

`root #``emerge --ask --depclean`
## Troubleshooting

### Debugging

To diagnose a problem related with CUPS enable debug logging in /etc/cups/cupsd.conf and restart cupsd service:

**`/etc/cups/cupsd.con`**

Log messages will appears in /var/log/cups/error\_log file.

### Error: No such file or directory

This error *may* come up when trying to print if you didn't emerge the [net-print/cups-meta](https://packages.gentoo.org/packages/net-print/cups-meta) package, to install it, use:

`root #``emerge --ask net-print/cups-meta`
### Error: Unable to convert file 0 to printable format

While having printing troubles and /var/log/cups/error\_log shows this message:

Re-emerge [app-text/ghostscript-gpl](https://packages.gentoo.org/packages/app-text/ghostscript-gpl) with the `cups` USE flag. You can either add `cups` to the system USE flags in /etc/portage/make.conf or enable it only for ghostscript-gpl as shown:

`root #``echo "app-text/ghostscript-gpl cups" >> /etc/portage/package.use`
Then run emerge app-text/ghostscript-gpl. When it has finished compiling, be sure to restart cupsd afterward.

When using OpenRC:

`root #``rc-service cupsd restart`
When using systemd:

`root #``systemctl restart cups`
### Error: Send-Document client-error-document-format-not-supported: Unsupported document-format "application/postscript"

If print jobs disappear and the cups error log contains the above error message, ensure that [net-print/cups-filters](https://packages.gentoo.org/packages/net-print/cups-filters) is installed with the [postscript](https://packages.gentoo.org/useflags/postscript) `USE` flag.

### USB printer is not detected

Assuming that cups is built with the `usb` USE flag, verify that the printer's character device has the correct permissions. For example:

`user $````
lsusb
Bus 002 Device 058: ID 04e8:3297 Samsung Electronics Co., Ltd ML-191x/ML-252x Laser Printer
```
There should be a character device for this printer at /dev/bus/usb/002/058.

`user $````
ls -l /dev/bus/usb/002/058
crw-rw-r-- 1 root android 189, 185 Apr 16 05:55 /dev/bus/usb/002/058
```
In this example, /lib64/udev/rules.d/80-android.rules over-zealously modified the permissions. This is [bug #644636](https://bugs.gentoo.org/show_bug.cgi?id=644636). Lets try fixing them:

`root #````
chgrp lp /dev/bus/usb/002/058
```
`root #``chmod 660 /dev/bus/usb/002/058`
Now we should see:

`user $````
ls -l /dev/bus/usb/002/058
crw-rw---- 1 root lp 189, 185 Apr 16 05:55 /dev/bus/usb/002/058
```
The printer likely is detected now. You should be able to add it, configure it (provided that you have a working driver) and print a test page. This implies a permissions problem. Assuming that your system uses udev/eudev for managing its /dev directory, you can make this change permanent by making a udev file:

**`/etc/udev/rules.d/99-printer.rules`**

**Custom Udev Rule**

```
SUBSYSTEMS=="usb", ATTRS{idVendor}=="04e8", ATTRS{idProduct}=="3297", MODE="0660", GROUP="lp"
```
Our device is "ID 04e8:3297" according to the earlier lsusb output. We split that into idVendor and idProduct as demonstrated in the example. Now udev should ensure that the correct permissions are set at every boot and at every hotplug.

## See also

- [Samba](https://wiki.gentoo.org/wiki/Samba) — a re-implementation of the [SMB/CIFS](https://wiki.gentoo.org/wiki/Smbnetfs) networking protocol, a Microsoft Windows alternative to Network File System ([NFS](https://wiki.gentoo.org/wiki/NFS)).
- [Driverless printing](https://wiki.gentoo.org/wiki/Driverless_printing)

## External resources

- [OpenPrinting database of printers that support 'driverless printing'](https://openprinting.github.io/printers/)
- [OpenPrinting database of printers that require a driver, and their level of Linux support](https://openprinting.org/printers)
- [Using Network Printers](https://www.cups.org/doc/network.html) - Documentation at CUPS.org.
- [Command-Line Printing and Options](https://www.cups.org/doc/options.html) - Documentation at CUPS.org.
