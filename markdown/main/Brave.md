<!-- source: https://wiki.gentoo.org/wiki/Brave | group: Gentoo Wiki (Main) | wiki-title: Brave -->
---
title: Brave
url: https://wiki.gentoo.org/wiki/Brave
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-02"
fingerprint: "8b995b4fbdbbc9ad"
license: CC BY-SA 4.0
---

# Brave

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Brave** is a web browser focused on privacy, blocking trackers, and advertisements.

## Installation

### Prerequisites

The following must be installed in order for the rest of this article to work properly: [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git).

Skip the rest of this section if they are already installed.

Install required software:

`root #``emerge --ask app-eselect/eselect-repository dev-vcs/git`
### Set Portage to use the Brave ebuild repository

Issue the [command](https://wiki.gentoo.org/wiki/Eselect/Repository) to configure the [another-brave-overlay](https://github.com/falbrechtskirchinger/another-brave-overlay) ebuild repository for Portage, then synchronize that repository:

`root #````
eselect repository enable another-brave-overlay
```
`root #````
emerge --sync another-brave-overlay
```
### Emerge

Install Brave Stable:

`root #``emerge --ask www-client/brave-browser::another-brave-overlay`
Brave Beta:

`root #``emerge --ask www-client/brave-browser-beta::another-brave-overlay`
Brave Nightly:

`root #``emerge --ask www-client/brave-browser-nightly::another-brave-overlay`
## Configuration

Most configuration aspects can be found in the [Chromium article](https://wiki.gentoo.org/wiki/Chromium). Head over there for configuration information.

### Screensharing with Pipewire

Change 'WebRTC PipeWire support' to 'Enabled' in brave://flags

### Using Wayland backend

Change 'Preferred Ozone platform' to 'Wayland' in brave://flags

### Policies

It is possible to set specific policies for chromium based browsers like Brave. This can be useful especially if the browser should be accessible by users, but the content should be restricted to trusted sites. It can also be configured to restrict the access to specified URIs, like the file:// protocol, to prevent users from surfing the file system.

To set custom policies a JSON file must be created in /etc/brave/policies/managed/\<filename>.json

The JSON file must meet the following structure:[\[1\]](https://wiki.gentoo.org#cite_note-1)

An example JSON file could look like this:

This prevents the user from surfing on the file system using the file protocol, incognito mode, blocks the listed URIs and URLs, and the location and notifications. More settings, can be found in the policy list: [https://www.chromium.org/administrators/policy-list-3/](https://www.chromium.org/administrators/policy-list-3/). If configured for other users as a service, it is recommended to block all sites at first and then define the allowed sites, to avoid abuse of the service.

If the policy was configured properly can be proofed on the special page: brave://policy

Please note that this only blocks the user from visiting specified locations. It does not disable the protocols on the system, so other applications must be configured separately.


Additionally, Brave offers some specific policies that allow you to streamline the browser by removing certain features that you might want to disable such as Brave Rewards, Brave Wallet, VPN, AI Chat.

## See also

- [Eselect/Repository](https://wiki.gentoo.org/wiki/Eselect/Repository) — an [eselect](https://wiki.gentoo.org/wiki/Eselect) module for configuring [ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository) for [Portage](https://wiki.gentoo.org/wiki/Portage).
