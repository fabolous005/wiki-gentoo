<!-- source: https://wiki.gentoo.org/wiki/Steamcmd | group: Gentoo Wiki (Main) | wiki-title: Steamcmd -->
---
title: Steamcmd
url: https://wiki.gentoo.org/wiki/Steamcmd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-14"
fingerprint: d3e8a4f9b083ae02
license: CC BY-SA 4.0
---

# Steamcmd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**steamcmd** is the command-line version of the [Steam](https://wiki.gentoo.org/wiki/Steam) client for dedicated servers. It uses [SteamPipe](https://developer.valvesoftware.com/wiki/SteamPipe) to download content and primarily used to set up game servers that are available through Steam.

## Installation

### Adding the Steam license

First it is needed to accept the Steam License(s):

If a global license file is used for all packages:

`root #``echo "games-server/steamcmd Steam license(s)" >> /etc/portage/package.license`
Else in it's own licence file, per package, as example:

`root #``echo "games-server/steamcmd Steam license(s)" >> /etc/portage/package.license/steamcmd`
### Emerge

`root #``emerge --ask games-server/steamcmd`
## Server deployment

There are known bugs requiring these commands to be ran several times rather than once.

### hlds

`steam ~/steamcmd/``./steamcmd.sh +login anonymous +force_install_dir "../hlds/" +app_set_config 90 +app_update 90 validate +quit`
### cstrike

`steam ~/steamcmd/``./steamcmd.sh +login anonymous +force_install_dir "../hlds/" +app_set_config 90 mod cstrike +app_update 90 mod cstrike validate +quit`
## server

### hlds

`steam ~/hlds/``./hlds_run +maxplayers 32`
### cstrike

`steam ~/hlds/``./hlds_run -game cstrike -autoupdate +maxplayers 32 +map de_dust2`
## Metamod

We will use metamod and amxmodx to make administration of your new servers easy.

`steam ~/hlds/<mod>``mkdir -p addons/metamod/dlls`
Download Metamod:

Decompress Metamod:

`steam ~/hlds/<mod>/addons/metamod/dlls/``tar -xf metamod*.tar.gz`
Remove Metamod Archive:

`steam ~/hlds/<mod>/addons/metamod/dlls/``rm metamod*.tar.gz`
Activate Metamod:

**`~/hlds/<mod>/liblist.gam`**

## amxmodx

As amxmodx is a metamod plugin, you will need to tell metamod to load amxmodx.

`steam ~/hlds/<mod>````
echo "linux addons/amxmodx/dlls/amxmodx_mm_i386.so" >> addons/metamod/plugins.ini
```
`steam ~/hlds/<mod>````
tar -xf amxmodx*.tar.gz
```
`steam ~/hlds/<mod>``rm amxmodx*.tar.gz`
Then download amxmodx mod specific files and install them to addons/amxmodx/ (sitting next to metamod)

amxmodx requires steam ids to know who has administrative powers over your server. To extract steam ids from halflife & mods open a game terminal using \~ & type status, look for your in game player name & copy down the id for later insertion into server files. See:

## Fast download FTP

Install a FTP server to enable fast downloading. Rsync maps and other resources to a FTP directory mirroring the hlds information with out copying passwords or exposing critical configurations.

## Downgrading steam packages

Packages can be downgraded with workflow described here [Steam/Client troubleshooting](https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting#SteamVR_doesn.27t_work_after_it_was_updated_.28as_steam_package.29_and_no_workaround_exists). AppID, DepotIDs, ChangelistIDs and paths should be replaced with proper values for this package.
