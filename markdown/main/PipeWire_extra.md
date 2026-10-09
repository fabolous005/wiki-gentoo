<!-- source: https://wiki.gentoo.org/wiki/PipeWire/extra | group: Gentoo Wiki (Main) | wiki-title: PipeWire/extra -->
---
title: PipeWire/extra
url: https://wiki.gentoo.org/wiki/PipeWire/extra
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-12"
fingerprint: "34108e170736da17"
license: CC BY-SA 4.0
---

# PipeWire/extra

[PipeWire](https://wiki.gentoo.org/wiki/PipeWire)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page contains information about more advanced PipeWire configuration and usage.


## Sound server configuration


### Sample rates

PipeWire uses a global sample rate in the audio processing pipeline. All signals are converted to this sample rate and then converted to the sample rate of the device.

Setting `default.clock.allowed-rates` to contain rates supported by the output device eliminates the need to resample<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

To change the global sample rate:

**`pipewire.conf`**

**Change default sample rate to 192000 Hz**

```
context.properties = {
    default.clock.rate = 192000
    default.clock.allowed-rates = [ 192000 48000 44100 ]  # Up to 16 can be specified
}
```
### SOFA-based virtual surround (7.1 to stereo)

PipeWire's [filter-chain](https://docs.pipewire.org/page_module_filter_chain.html) module can convert a multichannel (e.g. 7.1) application output into a binaural stereo signal for headphones, using measured Head-Related Transfer Function (HRTF) data stored in a [SOFA](https://www.sofaconventions.org/) file. Unlike simple channel downmixing (which only adjusts gain/panning), this convolves each surround channel against real head/ear measurements, producing a result that has a genuine sense of directionality over headphones.

This differs from the more commonly documented HeSuVi-style convolver approach, which uses a single multichannel WAV file with 8 fixed HRIRs baked in. Using a .sofa file directly with the built-in sofa filter type gives you control over the exact azimuth/elevation used per channel, and lets you try different HRTF datasets without needing a pre-converted WAV.

Since [media-libs/libmysofa-1.3.5](https://packages.gentoo.org/packages/media-libs/libmysofa) and [media-video/pipewire\[sofa\]-1.6.8](https://packages.gentoo.org/packages/media-video/pipewire) are currently **\~amd64**; keyword them explicitly and re-emerge.

Commonly recommended free datasets can be found here: [SOFA Conventions file index](https://www.sofaconventions.org/mediawiki/index.php/Files)

- **SADIE II** — includes a Neumann KU100 dummy-head measurement, common default choice
- **ARI** (Acoustics Research Institute, Vienna) — 200+ individual human subjects
- **SONICOM** — modern, actively maintained, well-documented dataset

Download a `.sofa` file and place it somewhere in `HOME`, e.g. \~/.config/pipewire/sofa/.

This has been tested successfully under an OpenRC-based system, but it should work under systemd as well.

Create the following file:

**`~/.config/pipewire/pipewire.conf.d/sink-virtual-surround-7.1-sofa.conf`**

**SOFA-based virtual surround**

```
# Generic 7.1 -> stereo virtual surround using a SOFA HRTF file.
# Replace SOFA_FILE_PATH with the full path to your .sofa file.
 
context.modules = [
    {   name = libpipewire-module-filter-chain
        flags = [ nofail ]
        args = {
            node.description = "Virtual Surround Sink (SOFA 7.1)"
            media.name       = "Virtual Surround Sink (SOFA 7.1)"
 
            filter.graph = {
                nodes = [
                    {   type   = sofa
                        name   = sofa_FL
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 30  "Elevation" = 0 }
                    }
                    {   type   = sofa
                        name   = sofa_FR
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 330  "Elevation" = 0 }
                    }
                    {   type   = sofa
                        name   = sofa_FC
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 0  "Elevation" = 0 }
                    }
                    {   type   = sofa
                        name   = sofa_SL
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 90  "Elevation" = 0 }
                    }
                    {   type   = sofa
                        name   = sofa_SR
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 270  "Elevation" = 0 }
                    }
                    {   type   = sofa
                        name   = sofa_RL
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 135  "Elevation" = 0 }
                    }
                    {   type   = sofa
                        name   = sofa_RR
                        label  = spatializer
                        config = { filename = "<SOFA_file_path>" }
                        control = { "Azimuth" = 225  "Elevation" = 0 }
                    }
 
                    # LFE is not directional; pass it straight through.
                    {   type  = builtin
                        name  = copyLFE
                        label = copy
                    }
 
                    {   type  = builtin
                        name  = mixerL
                        label = mixer
                        control = {
                            "Gain 1" = 1.0  "Gain 2" = 1.0  "Gain 3" = 1.0
                            "Gain 4" = 1.0  "Gain 5" = 1.0  "Gain 6" = 1.0
                            "Gain 7" = 1.0  "Gain 8" = 0.5
                        }
                    }
                    {   type  = builtin
                        name  = mixerR
                        label = mixer
                        control = {
                            "Gain 1" = 1.0  "Gain 2" = 1.0  "Gain 3" = 1.0
                            "Gain 4" = 1.0  "Gain 5" = 1.0  "Gain 6" = 1.0
                            "Gain 7" = 1.0  "Gain 8" = 0.5
                        }
                    }
                ]
 
                links = [
                    { output = "sofa_FL:Out L" input = "mixerL:In 1" }
                    { output = "sofa_FR:Out L" input = "mixerL:In 2" }
                    { output = "sofa_FC:Out L" input = "mixerL:In 3" }
                    { output = "sofa_SL:Out L" input = "mixerL:In 4" }
                    { output = "sofa_SR:Out L" input = "mixerL:In 5" }
                    { output = "sofa_RL:Out L" input = "mixerL:In 6" }
                    { output = "sofa_RR:Out L" input = "mixerL:In 7" }
                    { output = "copyLFE:Out"   input = "mixerL:In 8" }
 
                    { output = "sofa_FL:Out R" input = "mixerR:In 1" }
                    { output = "sofa_FR:Out R" input = "mixerR:In 2" }
                    { output = "sofa_FC:Out R" input = "mixerR:In 3" }
                    { output = "sofa_SL:Out R" input = "mixerR:In 4" }
                    { output = "sofa_SR:Out R" input = "mixerR:In 5" }
                    { output = "sofa_RL:Out R" input = "mixerR:In 6" }
                    { output = "sofa_RR:Out R" input = "mixerR:In 7" }
                    { output = "copyLFE:Out"   input = "mixerR:In 8" }
                ]
 
                # Standard 7.1 channel order: FL FR FC LFE RL RR SL SR
                inputs = [
                    "sofa_FL:In" "sofa_FR:In" "sofa_FC:In" "copyLFE:In"
                    "sofa_RL:In" "sofa_RR:In" "sofa_SL:In" "sofa_SR:In"
                ]
                outputs = [ "mixerL:Out" "mixerR:Out" ]
            }
 
            capture.props = {
                node.name      = "effect_input.sofa_surround"
                media.class    = "Audio/Sink"
                audio.channels = 8
                audio.position = [ FL FR FC LFE RL RR SL SR ]
            }
            playback.props = {
                node.name      = "effect_output.sofa_surround"
                node.passive   = true
                audio.channels = 2
                audio.position = [ FL FR ]
            }
        }
    }
]
```
Replace every occurrence of `<SOFA_file_path>` with the full path to the `.sofa` file.

Restart PipeWire to use the new config.

The new sink, "Virtual Surround Sink (SOFA 7.1)", should now appear as an output option in pavucontrol / pwvucontrol / qpwgraph / etc. Set an 8-channel-capable application's output to this sink to have its 7.1 audio rendered to stereo through the spatializer.

Testing can be done by using [mpv](https://wiki.gentoo.org/wiki/Mpv) with 7.1 audio tracks; there are free options available online. Each announced channel should be audibly positioned in roughly the correct direction (front-left toward the left, rear-right behind-right, etc). These positions can be altered for preference. If a specific channel seems too quiet or too loud the gain can also be modified as needed.


## Usage


### Setting the sample rate at runtime

pw-metadata -n settings \<node-id> clock.rate can be used to adjust the clock rate at runtime. For example, assuming a node-id of 0:

`user $``pw-metadata -n settings 0 clock.rate 384000`
Found "settings" metadata 31
set property: id:0 key:clock.rate value:384000 type:(null)

Results can be verified with pw-metadata -n settings.

This procedure can also be used for other settings.


### Streaming audio over a network via RTP

Refer to [the relevant upstream documentation](https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/Guide-Network-RTP).

An error code of 69 when restarting PipeWire means the configuration contains errors.

An error code of 70 indicates an attempt was made to create an RTP connection, but it failed. Check whether the IP addresses and ports in the configurations are correct.
