<!-- source: https://wiki.gentoo.org/wiki/Redshift | group: Gentoo Wiki (Main) | wiki-title: Redshift -->
---
title: Redshift
url: https://wiki.gentoo.org/wiki/Redshift
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: fa279b123217ca84
license: CC BY-SA 4.0
---

# Redshift

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Redshift is an automatic color temperature adjustment to help reduce monitor eye strain induced by working in low-light conditions. Depending on the location (longitude and latitude) of the user's computer system, Redshift will change the color of the screen from the default bluish color to a orange/reddish color during evening hours, and then back again to bluish tint during daylight hours.

## Installation

### USE flags


| [appindicator](https://packages.gentoo.org/useflags/appindicator) | Build in support for notifications using the libindicate or libappindicator plugin | 
| [geoclue](https://packages.gentoo.org/useflags/geoclue) | Control dependency on app-misc/geoclue | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 

### Emerge

After setting necessary USE flags, install Redshift:

`root #``emerge --ask x11-misc/redshift`
## Configuration

### Files

A configuration file can be created in each users home directory at \~/.config/redshift.conf

In order for Redshift to start, it will require longitude and latitude information inside the configuration file.

`user $``redshift -l manual:help`
### Saving lon/lat

Redshift can be quickly established in working order by adding correct location information into the configuration file. Lat/Lon values can be found searching for the closest city or region at [Wikipedia](https://www.wikipedia.org) or with a map service such as [OpenStreetMap](https://www.openstreetmap.org/).

When using OpenStreetMap, simply search for the location. When it appears on the map tiles click the Export button (should be near the top left). The center of the displayed tiles will be the exact Lat/Lon location. The Lat value is the second number from the end displayed in the URL. The Lon value is the last number on the end.

A finished .conf file will look something like this:

**`~/.config/redshift/redshift.conf`**

```
[redshift]
location-provider=manual
[manual]
lat=47.6587803
lon=-117.4260466
```
## Service

### OpenRC

Redshift does not currently include an OpenRC init script.

### systemd

`user $````
systemctl --user enable redshift.service
```
`user $````
systemctl --user start redshift.service
```
## Usage

### Invocation

`user $``redshift -h`
Usage: redshift -l LAT:LON -t DAY:NIGHT \[OPTIONS...\]
Set color temperature of display according to time of day.
  -h		Display this help message
  -v		Verbose output
  -V		Show program version
  -b DAY:NIGHT	Screen brightness to apply (between 0.1 and 1.0)
  -c FILE	Load settings from specified configuration file
  -g R:G:B	Additional gamma correction to apply
  -l LAT:LON	Your current location
  -l PROVIDER	Select provider for automatic location updates
  		(Type \`list' to see available providers)
  -m METHOD	Method to use to set color temperature
  		(Type \`list' to see available methods)
  -o		One shot mode (do not continuously adjust color temperature)
  -O TEMP	One shot manual mode (set color temperature)
  -p		Print mode (only print parameters and exit)
  -x		Reset mode (remove adjustment from screen)
  -r		Disable temperature transitions
  -t DAY:NIGHT	Color temperature to set at daytime/night
The neutral temperature is 6500K. Using this value will not
change the color temperature of the display. Setting the
color temperature to a value higher than this results in
more blue light, and setting a lower value will result in
more red light.
Default values:
  Daytime temperature: 6500K
  Night temperature: 4500K
Please report bugs to \<https://github.com/jonls/redshift/issues>

## Troubleshooting

### No display manager

When not using a [Display manager](https://wiki.gentoo.org/wiki/Display_manager), users may encounter a similar error in the output of systemctl --user status redshift

`user $``systemctl --user status redshift`
Jan 04 17:55:16 systemd\[633\]: Started Redshift display colour temperature adjustment.
Jan 04 17:55:16 redshift\[2874594\]: \`RANDR Query Version' returned error -1
Jan 04 17:55:16 redshift\[2874594\]: Initialization of randr failed.
Jan 04 17:55:16 redshift\[2874594\]: Trying next method...
Jan 04 17:55:16 redshift\[2874594\]: X request failed: XOpenDisplay
Jan 04 17:55:16 redshift\[2874594\]: Initialization of vidmode failed.
Jan 04 17:55:16 redshift\[2874594\]: Trying next method...
Jan 04 17:55:16 redshift\[2874594\]: No more methods to try.
Jan 04 17:55:16 systemd\[633\]: redshift.service: Main process exited, code=exited, status=1/FAILURE

To verify this, simply issue redshift in a terminal. If redshift starts up correctly, then this is likely the issue.

There are a couple simple solutions to this problem:

1. Install and use a [Display manager](https://wiki.gentoo.org/wiki/Display_manager)
2. Add redshift & to the user \~/.xinitrc file
