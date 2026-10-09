<!-- source: https://wiki.gentoo.org/wiki/Iamb | group: Gentoo Wiki (Main) | wiki-title: Iamb -->
---
title: iamb
url: https://wiki.gentoo.org/wiki/Iamb
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-08"
fingerprint: f21147592b7c3faa
license: CC BY-SA 4.0
---

# iamb

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**iamb** is a terminal based Matrix client written in [Rust](https://wiki.gentoo.org/wiki/Rust).

## Installation

### Prerequisites

Ensure [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) is installed, to be able to enable the relevant overlay.

### Overlay

In order to install iamb, enable the GURU overlay and [sync](https://wiki.gentoo.org/wiki/Project:Portage/Sync) it:

`root #``eselect repository enable guru``root #``emerge --sync guru`
### Emerge

To install the client:

`root #``emerge --ask net-im/iamb::guru`
## Configuration

The configuration file will be in $XDG\_CONFIG\_HOME/iamb/ or $HOME/.config/iamb/.

### Profiles

Set up a profile to specify a username and homeserver URL:

**`~/.config/iamb/config.toml`**

```
[profiles.user]
user_id = "@username:example.com"
```
To use multiple profiles, add them to the configuration file:

**`~/.config/iamb/config.toml`**

```
default_profile = "user"
[profiles.user]
user_id = "@user:example.com"
[profiles.anotheruser]
user_id = "@anotheruser:example.com"
```
#### Explicit homeserver URLs

If the homeserver is in a different domain to the one mentioned in the `user_id`, iamb can use another domain:

**`~/.config/iamb/config.toml`**

```
[profiles.user]
user_id = "@username:example.com"
url = "https://example.com"
```
## Usage

To use the client, run iamb:

`user $``iamb`
When starting the client for the first time, be sure to select the correct login option ("password \[p\]" or "SSO \[s\]") for syncing.

Starting the client with specified profile can be done with `--profile` flag.

`user $``iamb --profile anotheruser`
