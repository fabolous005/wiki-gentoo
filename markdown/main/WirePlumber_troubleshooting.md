<!-- source: https://wiki.gentoo.org/wiki/WirePlumber/troubleshooting | group: Gentoo Wiki (Main) | wiki-title: WirePlumber/troubleshooting -->
---
title: WirePlumber/troubleshooting
url: https://wiki.gentoo.org/wiki/WirePlumber/troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-03"
fingerprint: e4e526172d3ff282
license: CC BY-SA 4.0
---

# WirePlumber/troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## WirePlumber troubleshooting

Refer also to the [PipeWire/troubleshooting](https://wiki.gentoo.org/wiki/PipeWire/troubleshooting) page.

### General tips

#### Logging

Those using [OpenRC user services](https://wiki.gentoo.org/wiki/OpenRC#User_services) can enable logging by adding the following to $XDG\_CONFIG\_HOME/rc/rc.conf, creating that file if necessary:

rc\_logger="YES"
rc\_log\_path="\<log\_location>"

where `XDG_CONFIG_HOME` defaults to \~/.config/, and `<log_location>` should be replaced by the desired location for the log file.

Those using systemd can use [journalctl(1)](https://man.archlinux.org/man/journalctl.1.en) [to review logs.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) 

Those using [PipeWire/gentoo-pipewire-launcher](https://wiki.gentoo.org/wiki/PipeWire/gentoo-pipewire-launcher) can enable logging by configuring the `GENTOO_WIREPLUMBER_LOG` variable in gentoo-pipewire-launcher.conf, as described in the gentoo-pipewire-launcher(1) man page.

Once logging is enabled, the log level can be controlled at runtime with [wpctl(1)](https://man.archlinux.org/man/wpctl.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $````
wpctl set-log-level D     # enable debug logging
```
`user $````
wpctl set-log-level -     # restore default logging
```
Refer to [the "Debug Logging" section of the WirePlumber documentation](https://pipewire.pages.freedesktop.org/wireplumber/daemon/logging.html) for details about debug logging output.

#### Files

When searching for the cause of a problem, by sure to clear any relevant temporary/cached files, e.g. $XDG\_STATE\_HOME/wireplumber.

### Stuttering

Stuttering in applications like [media-sound/ardour](https://packages.gentoo.org/packages/media-sound/ardour) might be resolved using the following script. Replace `<device>` in the script with the relevant node name, which can be found by running the command:

`user $``wpctl status -n`
**`~/.config/wireplumber/main.lua.d/latency.lua`**

```
table.insert(alsa_monitor.rules, {
  matches = {
    {
      -- <device> must be replaced with the device name
      { "node.name", "equals", "<device>" },
    },
  },
  apply_properties = {
    -- If 64 doesn't work, try bigger values that are a power of 2 (128, 256, 512, 1024, 2048, etc.)
    ["api.alsa.headroom"] = 64,
  },
})
```
Once this script has been added, restart PipeWire.

If this does not resolve the issue, more information can be found [here](https://gitlab.freedesktop.org/pipewire/pipewire/-/issues/2257) and [here](https://forum.manjaro.org/t/howto-troubleshoot-crackling-in-pipewire/82442).

#### Stuttering in a VM

Stuttering in a VM may be caused by insufficient headroom for [ALSA](https://wiki.gentoo.org/wiki/ALSA). By default, on a VM, the headroom is increased to 2048 in /usr/share/wireplumber/wireplumber.conf.d/alsa-vm.conf.

Stuttering might be eliminated by raising the headroom to 8096:
[\[2\]](https://wiki.gentoo.org#cite_note-2)

**`/etc/wireplumber/wireplumber.conf.d/30-alsa.conf`**

```
monitor.alsa.rules = [
  {
    matches = [
      # This matches the value of the 'node.name' property of the node.
      {
        node.name = "~alsa_output.*"
      }
    ]
    actions = {
      # Apply all the desired node specific settings here.
      update-props = {
        api.alsa.period-size   = 1024
        api.alsa.headroom      = 8192
      }
    }
  }
]
```
### Disabling or delaying audio sink suspension

By default, audio devices are put into standby after five seconds of no audio. This can be annoying, because some devices take a few seconds to start up again. However, the delay can be configured or the functionality can be disabled completely.

To make it simple, the following user config applies to all audio sources and audio sinks.

`session.suspend-timeout-seconds = 0` disables the suspend functionality. Alternatively, the delay can be increased.

**`~/.config/wireplumber/wireplumber.conf.d/51-disable-suspension.conf`**

```
monitor.alsa.rules = [
  {
    matches = [
      {
        # Matches all sources
        node.name = "~alsa_input.*"
      },
      {
        # Matches all sinks
        node.name = "~alsa_output.*"
      }
    ]
    actions = {
      update-props = {
        session.suspend-timeout-seconds = 0
      }
    }
  }
]
# bluetooth devices
monitor.bluez.rules = [
  {
    matches = [
      {
        # Matches all sources
        node.name = "~bluez_input.*"
      },
      {
        # Matches all sinks
        node.name = "~bluez_output.*"
      }
    ]
    actions = {
      update-props = {
        session.suspend-timeout-seconds = 0
      }
    }
  }
]
```
Once the script has been added, restart the `pipewire` and `pipewire-pulse` services, or reboot.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) WirePlumber 0.15.5 documentation, "Debug Logging", ["Relationship with the PipeWire log handler & PIPEWIRE\_DEBUG"](https://pipewire.pages.freedesktop.org/wireplumber/daemon/logging.html#relationship-with-the-pipewire-log-handler-pipewire-debug).
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Pipewire troubleshooting instructions](https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/Troubleshooting#stuttering-audio-in-virtual-machine). Retrieved on 2024-09-02
