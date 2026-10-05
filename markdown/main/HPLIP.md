<!-- source: https://wiki.gentoo.org/wiki/HPLIP | group: Gentoo Wiki (Main) | wiki-title: HPLIP -->
---
title: HPLIP
url: https://wiki.gentoo.org/wiki/HPLIP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-26"
fingerprint: fec39a5accb53980
license: CC BY-SA 4.0
---

# HPLIP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**HPLIP** (**HP L**inux **I**maging and **P**rinting) provides printer drivers for HP devices. It also includes scanner and fax support as well as service tools for various multi-purpose peripherals.

## Is HPLIP required?

Using HPLIP is recommended for most HP inkjet or laserjet based printers. There may, however, be devices which work with a default CUPS install, which provides [driverless printing](https://wiki.gentoo.org/wiki/Driverless_printing), adequate drivers or PPD files.

Some devices have features that can only be used when a binary plugin is enabled. This plugin must be installed in addition to the drivers. Hence, it is needed if those features (like better printing quality, faster printing or scanning) are to be used. See the [binary plugins](https://wiki.gentoo.org/wiki/HPLIP#Binary_plugins) section for more information.

## Installation

For printing support, it is recommended to use the new hpcups driver, which can be enabled with the `hpcups` USE flag. The old hpijs driver is still included when building HPLIP with the `hpijs` USE flag.

The default install enables dynamically generated PPD files at runtime. Some printers may still require static PPD files. If hp-setup has problems, try enabling the `static-ppds` USE flag and rebuilding HPLIP.

For USB printers [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) has to be built with the `usb` USE flag. This way it makes use of the [dev-libs/libusb](https://packages.gentoo.org/packages/dev-libs/libusb) user-space tool which replaces kernel USB printer support (`CONFIG_USB_PRINTER`). In case of problems disable the `usb` USE flag for [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) and activate the kernel functionality again.

To be able to set up a network printer, the 1.5 series of [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) has to be built with USE `avahi` or `slp`. The 1.6 series of [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) has dropped slp support and one can choose to build it with USE `avahi` or `zeroconf`. To be able to print on a network printer, HPLIP needs to be built with USE `snmp`.

For some scanner devices, the appropriate SANE backend needs to be activated. To find out which backend is required for the device, go to the SANE project's [driver search engine](http://www.sane-project.org/sane-mfgs.html) and search for "HEWLETT-PACKARD" as manufacturer. This provides a list of all supported devices including the required backend. For instance, if the device needs the hp backend, the following must be added to the portage configuration to build it when installing [media-gfx/sane-backends](https://packages.gentoo.org/packages/media-gfx/sane-backends):

**`/etc/portage/package.use/sane`**

```
 SANE_BACKENDS: hp
```
**`/etc/sane.d/dll.conf`**

```
# Add support for the HP-specific backend.  Needs net-print/hplip installed.
hpaio
```
### Kernel

USB-connected devices require basic kernel USB support. When using the old kernel USB printer driver - which means *not* using the `usb` USE flag on [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) - the following kernel options must be set:

**Optional: USB printer support**

If preferred this driver can be enabled as module which will be called `usblp`. This also makes testing the `usb` USE flag easier as one can simply switch between the two methods by either disabling the USE flag and loading the kernel module, or enabling the USE flag and unloading the kernel module via modprobe. This saves kernel recompilations and reboots; only HPLIP needs to be recompiled for the USE flags changes to take effect. If the module should be permanently disabled, it can be blacklisted to prevent automatic loading at boot time.

Load the `usblp` kernel module:

`root #``modprobe usblp`
Unload the `usblp` kernel module:

`root #``modprobe -r usblp`
Blacklist the `usblp` kernel module:

`root #``echo "blacklist usblp" >> /etc/modprobe.d/blacklist.conf`
Parallel-port-connected devices require the following kernel options:

**Optional: Parallel port printer support**

The above two methods are usually already enabled within most kernels.

Network-connected devices do not require special kernel drivers but basic kernel network support.

### Software

The following table shows the current USE flags for the [net-print/hplip](https://packages.gentoo.org/packages/net-print/hplip):


| [+hpcups](https://packages.gentoo.org/useflags/+hpcups) | Build the hpcups driver for cups (by HP) | 
| [+snmp](https://packages.gentoo.org/useflags/+snmp) | Add support for net-analyzer/net-snmp which enables this driver to work over networks (both for server and client) | 
| [X](https://packages.gentoo.org/useflags/X) | Enables scanner GUI dependencies with USE="scanner" where media-gfx/xsane is preferred over media-gfx/sane-frontends | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [fax](https://packages.gentoo.org/useflags/fax) | Enable fax on multifunction devices which support it | 
| [hpijs](https://packages.gentoo.org/useflags/hpijs) | Build the IJS driver for cups (Foomatic) | 
| [kde](https://packages.gentoo.org/useflags/kde) | Enables kde-misc/skanlite as scanner GUI with USE="scanner X" | 
| [libusb0](https://packages.gentoo.org/useflags/libusb0) | Depend on virtual/libusb SLOT 0. Some old printers do not work with virtual/libusb SLOT 1. | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Only build internal hpijs/hpcups driver (not recommended at all, make sure you know what you are doing) | 
| [parport](https://packages.gentoo.org/useflags/parport) | Enable parallel port for devices which require it | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [scanner](https://packages.gentoo.org/useflags/scanner) | Enable scanner on multifunction devices which support it | 
| [static-ppds](https://packages.gentoo.org/useflags/static-ppds) | Use statically-generated PPDs instead of Dynamic PPDs. Although this is deprecated some printers may still need it to work properly. Use this flag if hp-setup fails to find/create a valid PPD file | 

Install [net-print/hplip](https://packages.gentoo.org/packages/net-print/hplip):

`root #``emerge --ask net-print/hplip`
## Configuration

### Printers and faxes

If HPLIP was installed with the `qt5` USE flag enabled, use the "HP Device Manager" to configure the devices.

Alternatively run hp-setup in a terminal:

`root #``hp-setup`
For a network printer, also specify the printer's IP address:

`root #``hp-setup -i 192.168.1.27`
Afterwards restart the CUPS daemon:

`root #``rc-service cupsd restart`
### Scanners

Overall, scanners should just work and will be detected with XSane, etc.

## Testing

### Printer

After completing hp-setup it should allow for printing a test page. This can be done later as well using the "HP Device Manager" or the CUPS web interface which is located at [http://localhost:631/](http://localhost:631/).

### Scanner

Use xsane and then press the preview button to ensure the scanner driver is installed and working properly.

## Upgrading

Every time after upgrading HPLIP it is advised to run `hp-setup -r` to remove all printers and configure them again as described above in the configuration section.

## Binary plugins

There are some devices which require a binary plugin to use all functions the device provides. A list with devices requiring a binary plugin including the reasons why it is needed is available at the project's [homepage](http://hplipopensource.com/hplip-web/plugin.html). Gentoo currently does not support the binary plugin. However, hp-setup tries to automatically install the plugin.

The installed files are out of portage's control and are located under /usr/share/hplip/data/firmware/, /usr/share/hplip/data/plugins/ and /usr/share/hplip/prnt/plugins/. For installing just the plugin without configuring printer queues hp-plugin is available.

If installing the plugin with hp-setup or hp-plugin fails, the plugin installer is available for download [at the openprinting site](http://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/). To install the plugin the downloaded .run file has to executed in a shell:

`root #``sh hplip-version-plugin.run`
Afterwards follow the instructions of the installer similar to hp-setup or hp-plugin.

## Troubleshooting

### Hp-setup won't accept root password

First, create a group called "sudo":

`root #``groupadd sudo`
Then, add your user to the "sudo" group:

`root #``usermod -a -G sudo myuser`
### hp-doctor

hp-doctor is a diagnostic utility included with HPLIP that can help find problems. It will give an error that Gentoo is not supported but will work.

`user $``hp-doctor`
It may give errors about missing hp-upgrade and missing python3-pyqt4 which will not cause problems. Other issues should be investigated as they may indicate an actual problem.

The error below indicates a network problem. Make sure the ip address can be pinged. If using DHCP, make sure the printer is also configured to use DHCP. Some printer models allow the "IPv4 Config Method" to be set via the contol panel on the printer itself - make sure it is set to DHCP.

error: Unable to communicate with device (code=12): hp:/net/\[printer name ....ip=xxx.xxx.xxx.xxx\]

### Printer not found

**Problem**: hp-setup or HP Device Manager discovery does not find network printer.

**Solution**: Check that [net-print/hplip](https://packages.gentoo.org/packages/net-print/hplip) has the `snmp` USE flag enabled and SNMP is enabled in the printer settings.

### Printing paused

**Problem**: Printing does not immediately start after submitting a job.

**Solution**: Check the CUPS printer administration interface and make sure the printer is not in a paused state.

### HPLIP ebuild upgrades do not upgrade binary plugin(s)

**Problem**: The HPLIP ebuild itself does not upgrade the binary plugin(s) when the ebuild is upgraded.  A symptom of this problem: segmentation faults might occur when starting XSane. (i.e. The HP LaserJet M1522nf requires a binary plugin for using the scanning feature.)

**Solution 1**: Following the upgrade instructions should take care of this as hp-setup tries to upgrade the plugin. If there are still problems HPLIP comes with hp-plugin which can be used for installing the plugin. As a last resort one can download the plugin from [OpenPrinting](http://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/) and install it manually.

**Solution 2**: Lately, running hp-info (with HPLIP built with the `qt5` USE flag) via a user terminal will eventually auto detect the version conflict of the binary part of the installed driver and ask the user to automatically upgrade, including issuing a prompt for the root password via a Qt widget prompt.

### Printing weird characters

**Problem**: The printout contains weird looking characters.

**Solution**: The printer likely cannot handle Postscript Level 3 output. To work around this, edit the printer's PPD file and change `*LanguageLevel: "3"` to `*LanguageLevel: "2"`.

See [Launchpad bug #277404](https://launchpad.net/bugs/277404) which was opened in 2008 and [Freedesktop.org bug #19640](https://bugs.freedesktop.org/show_bug.cgi?id=19640) for more information.

### SANE segfaulting when using HPAIO backend

Ensure that the `dbus` service is running. HPAIO segfaults silently if this is not the case.

### SANE/XSANE not finding hpaio scanner

**Problem**: SANE or XSANE can still not find a hpaio-based scanner.

**Solution**: SANE/XSANE `SANE_BACKENDS` or [media-gfx/sane-backends](https://packages.gentoo.org/packages/media-gfx/sane-backends) used to require `hp` or the `hp` USE flag.  Currently, the 'hp' sane-backend appears no longer needed for accessing HPLIP hpaio devices.  Also, if no `SANE_BACKENDS` devices are specified within the /etc/portage/make.conf, almost all sane-backend devices are compiled by default during compilation of [media-gfx/sane-backends](https://packages.gentoo.org/packages/media-gfx/sane-backends) package.  This appears to go against the main purpose of the USE flag function and inhibits a very long compilation on x86 platforms.

It may be prudent to still insert `SANE_BACKENDS="hp"` (or at least some driver even though it is not needed) within the /etc/portage/make.conf file or within the package USE flags.  There also used to be an older `SANE_BACKENDS="sane_backends_hp"` format, so ensure that the configuration is adapted towards the newer format.

More current and relevant areas to check for resolving this specific problem: make sure the user is in the scanner, usb and/or lp groups of the /etc/groups file. If scanimage -L and the HPLIP hp-scan work as root, something must be awry with permissions. If even scanning as root fails, especially with HPLIP's hp-scan, check to ensure the plugin is properly installed/updated and the device is connected.

One more area to check when all of the above is set up correctly: make sure the user can read files within /etc/sane.d/ (specifically the /etc/sane.d/dll.conf file).

When all else fails, strace (as provided by [dev-util/strace](https://packages.gentoo.org/packages/dev-util/strace)) may provide more in-depth feedback, but explaining how to work with strace is out of scope for this document.
