<!-- source: https://wiki.gentoo.org/wiki/SELinux/chromium | group: Gentoo Wiki (Main) | wiki-title: SELinux/chromium -->
---
title: SELinux/chromium
url: https://wiki.gentoo.org/wiki/SELinux/chromium
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2013-05-08"
fingerprint: dfd2981a5ff13380
license: CC BY-SA 4.0
---

# SELinux/chromium

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Structure

### Domains

The chromium\_t domain is used for the chromium web browser and is usable by regular users or operators.

Next to the main domain, you'll also find chromium\_renderer\_t. This is for the individual renderer processes within Chromium. The renderer process domain is transitioned to dynamically (chromium is SELinux-aware on this and defines the transitions itself).

### File types/labels

The following table lists the file type/labels defined in the chromium module.

| Type | Function | Description | 
|---|---|---|
| chromium\_exec\_t | Entrypoint | Entrypoint domain for the chromium\_t dmoain | 
| chromium\_tmp\_t |  | Label for temporary files, links and named pipes | 
| chromium\_tmpfs\_t |  | Label for the tmpfs-files, needed for interaction with Xorg | 
| chromium\_xdg\_config\_t |  | Label for the chromium configuration (\~/.config/chromium) | 
| chromium\_xdg\_cache\_t |  | Label for the chromium cache (\~/.cache/chromium) | 

## Using the chromium SELinux module

By default, the chromium domain will be somewhat restricted. Few functions that you might want to enable are controlled by booleans.

### SELinux boolean: chromium\_read\_user\_content

When enabled, the chromium browser is allowed to read user content (the user home files).

The default is yes, to allow users to use chromium to browse local files.

### SELinux boolean: chromium\_manage\_user\_content

When enabled, the chromium browser is allowed to manage (modify) user content. This is also needed if the user wants to download files into a local directory.

If you want to download files but don't want to give the chromium browser these rights, use a dedicated directory and label it chromium\_xdg\_cache\_t. From a policy development point of view, we are going to look into supporting the other XDG types for this (Download, Desktop, Documents, Music, Pictures, Videos).

The default is no.

### SELinux boolean: chromium\_use\_java

When enabled, the chromium browser is allowed to invoke java for its java plugin support.

Due to the way java plugins are handled (and depending on the plugin used), this will result in browsers having access to their temporary directories (but only directories) as the same directory is used for the control sockets or pipes.

The default is no.

### SELinux boolean: chromium\_read\_system\_info

When enabled, the chromium browser is allowed to access various system information resources (/sys/kernel/debug, /sys/bus, /sys/devices, /proc, etc. The browser uses this to optimize its own resources (like memory management) and support for specific devices (like handling web cams, etc.)

The default is no.
