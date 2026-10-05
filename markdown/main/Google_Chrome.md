<!-- source: https://wiki.gentoo.org/wiki/Google_Chrome | group: Gentoo Wiki (Main) | wiki-title: Google Chrome -->
---
title: Google Chrome
url: https://wiki.gentoo.org/wiki/Google_Chrome
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-02"
fingerprint: "6b52168081b755eb"
license: CC BY-SA 4.0
---

# Google Chrome

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Chrome** is Google's proprietary (closed source) web browser. Much of the source code is released in parallel as [Chromium](https://wiki.gentoo.org/wiki/Chromium), however there are binary blobs present in Chrome including a Pepper-based version PPAPI of the Adobe Flash Player.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

Currently there are several versions of Google Chrome available in the main Gentoo repository:

- [www-client/google-chrome](https://packages.gentoo.org/packages/www-client/google-chrome) - A stable version of the web browser from Google.
- [www-client/google-chrome-beta](https://packages.gentoo.org/packages/www-client/google-chrome-beta) - A beta version of the web browser of Google.
- [www-client/google-chrome-unstable](https://packages.gentoo.org/packages/www-client/google-chrome-unstable) - An unstable version of the web browser from Google.

### USE flags

Each of the packages listed above contains the following USE flags:


### Accept License

In order to install Google Chrome, the user needs to accept the 'google-chrome' license agreement. A copy of the license can be found at '/var/db/repos/gentoo/licenses/google-chrome'. Read with:

`user $``less /var/db/repos/gentoo/licenses/google-chrome`
And to agree:

`root #``echo "www-client/google-chrome google-chrome" >> /etc/portage/package.license`
### Emerge

Select one of the Chrome packages to emerge above. Here the primary stable Chrome package will be installed:

`root #``emerge --ask www-client/google-chrome`
## Configuration

Most configuration aspects can be found in the [Chromium article](https://wiki.gentoo.org/wiki/Chromium). Head over there for configuration information.

### Screensharing with Pipewire

Change 'WebRTC PipeWire support' to 'Enabled' in chrome://flags

### Policies

To configure custom policies in Chrome, the policy section in the Chromium article can be used: [Chromium#Policies](https://wiki.gentoo.org/wiki/Chromium#Policies).

However, please note that the directory where Google Chrome looks for policies can be slightly different. According to the documentation the lookup path for installed policies should be: /etc/opt/chrome/policies[\[2\]](https://wiki.gentoo.org#cite_note-2)

### No emojis?

Emerge [media-fonts/noto-emoji](https://packages.gentoo.org/packages/media-fonts/noto-emoji).

## Troubleshooting

### Last open pages not restored with info: "Google Chrome didn't shut down correctly"

#### Systemd

1. Create a service file with your favourite editor: FILE**`/etc/systemd/system/kill-chrome-gracefully.service`** \[Unit\] Description=Help Chrome close gracefully DefaultDependencies=no Before=shutdown.target \[Service\] Type=oneshot User=root Group= root ExecStart=killall chrome --wait \[Install\] WantedBy=halt.target reboot.target shutdown.target
2. Load it: `root #``systemctl daemon-reload`
3. Enable it: `root #``systemctl enable kill-chrome-gracefully.service`

## See also

- [Chromium](https://wiki.gentoo.org/wiki/Chromium) — the open source browser that [Google Chrome] and many other browsers are based on.
- [Firefox](https://wiki.gentoo.org/wiki/Firefox) — [open source](https://en.wikipedia.org/wiki/Open_source), [multiplatform](https://en.wikipedia.org/wiki/Cross-platform_software), [web browser](https://wiki.gentoo.org/wiki/Recommended_applications#Web_browsers) developed by [Mozilla](https://en.wikipedia.org/wiki/Mozilla).
