<!-- source: https://wiki.gentoo.org/wiki/Chromebook/Sound_configuration | group: Gentoo Wiki (Main) | wiki-title: Chromebook/Sound configuration -->
---
title: Chromebook/Sound configuration
url: https://wiki.gentoo.org/wiki/Chromebook/Sound_configuration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-25"
fingerprint: fe06ba124d21b3cd
license: CC BY-SA 4.0
---

# Chromebook/Sound configuration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes a way to configure sound for Chromebooks that do not have a valid ALSA UCM profile. There are at least two such devices. [\[1\]](https://wiki.gentoo.org#cite_note-1)[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Identification

### Kernel module

On a fresh system (or Gentoo Live Image), the module registers an attempt to load the firmware for the sound card. The log of this process looks similar to this:

`root #``dmesg`
\[   16.199710\] sof-audio-pci-intel-cnl 0000:00:1f.3: DSP detected with PCI class/subclass/prog-if info 0x040100
\[   16.199933\] sof-audio-pci-intel-cnl 0000:00:1f.3: DSP detected with PCI class/subclass/prog-if 0x040100
\[   16.200005\] sof-audio-pci-intel-cnl 0000:00:1f.3: bound 0000:00:02.0 (ops \_\_SCT\_\_tp\_func\_intel\_frontbuffer\_flush \[i915\])
\[   16.206761\] sof-audio-pci-intel-cnl 0000:00:1f.3: use msi interrupt mode
\[   16.245910\] sof-audio-pci-intel-cnl 0000:00:1f.3: NHLT table not found
\[   16.245960\] sof-audio-pci-intel-cnl 0000:00:1f.3: hda codecs found, mask 4
\[   16.245971\] Loading firmware: intel/sof/community/sof-cml.ri
\[   16.246288\] sof-audio-pci-intel-cnl 0000:00:1f.3: Direct firmware load for intel/sof/community/sof-cml.ri failed with error -2
\[   16.246292\] sof-audio-pci-intel-cnl 0000:00:1f.3: error: sof firmware file is missing, you might need to
\[   16.246294\] sof-audio-pci-intel-cnl 0000:00:1f.3:        download it from https://github.com/thesofproject/sof-bin/
\[   16.246295\] sof-audio-pci-intel-cnl 0000:00:1f.3: error: failed to load DSP firmware -2
\[   16.246566\] sof-audio-pci-intel-cnl 0000:00:1f.3: error: sof\_probe\_work failed err: -2

In this example, the module introduces itself as **sof-audio-pci-intel-cnl**. To find the actual name, it is necessary to map this name to the names of the currently loaded modules, which can be found, for example, with this command:

`root #``lspci -k````
00:1f.3 Multimedia audio controller: Intel Corporation Comet Lake PCH-LP cAVS
        DeviceName: Multimedia audio controller
        Subsystem: Intel Corporation Comet Lake PCH-LP cAVS
        Kernel driver in use: sof-audio-pci-intel-cnl
        Kernel modules: snd_hda_intel, snd_soc_skl, snd_sof_pci_intel_cnl
```
Obviously, the real name of the module is **snd\_sof\_pci\_intel\_cnl**.

This module cannot load the sound card without the firmware. This can be confirmed with the following command:

`root #``aplay -l`
aplay: device\_list:277: no soundcards found...

### Sound Open Firmware

The module found above requires the following firmware:

`root #``emerge --ask sys-firmware/sof-firmware`
After installing the firmware, the kernel module (which was found above) must be reloaded or the system must be rebooted (which is not possible in the case of Live Image). To reload the module, the following commands should be executed:

`root #``rmmod snd_sof_pci_intel_cnl``root #``modprobe snd_sof_pci_intel_cnl`
After the module reloads, it will reintroduce itself:

`root #``dmesg`
\[  989.351858\] sof-audio-pci-intel-cnl 0000:00:1f.3: DSP detected with PCI class/subclass/prog-if info 0x040100
\[  989.352038\] sof-audio-pci-intel-cnl 0000:00:1f.3: DSP detected with PCI class/subclass/prog-if 0x040100
\[  989.352132\] sof-audio-pci-intel-cnl 0000:00:1f.3: bound 0000:00:02.0 (ops \_\_SCT\_\_tp\_func\_intel\_frontbuffer\_flush \[i915\])
\[  989.358608\] sof-audio-pci-intel-cnl 0000:00:1f.3: use msi interrupt mode
\[  989.369584\] sof-audio-pci-intel-cnl 0000:00:1f.3: NHLT table not found
\[  989.369632\] sof-audio-pci-intel-cnl 0000:00:1f.3: hda codecs found, mask 4
\[  989.369648\] Loading firmware: intel/sof/community/sof-cml.ri
\[  989.369810\] sof-audio-pci-intel-cnl 0000:00:1f.3: Firmware info: version 2:2:0-57864
\[  989.369812\] sof-audio-pci-intel-cnl 0000:00:1f.3: Firmware: ABI 3:22:1 Kernel ABI 3:23:0
\[  989.369816\] sof-audio-pci-intel-cnl 0000:00:1f.3: unknown sof\_ext\_man header type 3 size 0x30
\[  989.502724\] sof-audio-pci-intel-cnl 0000:00:1f.3: Firmware info: version 2:2:0-57864
\[  989.502733\] sof-audio-pci-intel-cnl 0000:00:1f.3: Firmware: ABI 3:22:1 Kernel ABI 3:23:0
\[  989.522991\] Loading firmware: intel/sof-tplg/sof-cml-rt5682-max98357a.tplg
\[  989.523020\] sof-audio-pci-intel-cnl 0000:00:1f.3: Topology: ABI 3:22:1 Kernel ABI 3:23:0
\[  989.523169\] sof\_rt5682 sof\_rt5682: ASoC: Parent card not yet available, widget card binding deferred
\[  989.604307\] input: sof-rt5682 Headset Jack as /devices/pci0000:00/0000:00:1f.3/sof\_rt5682/sound/card0/input18
\[  989.604530\] input: sof-rt5682 HDMI/DP,pcm=2 as /devices/pci0000:00/0000:00:1f.3/sof\_rt5682/sound/card0/input19
\[  989.604686\] input: sof-rt5682 HDMI/DP,pcm=3 as /devices/pci0000:00/0000:00:1f.3/sof\_rt5682/sound/card0/input20
\[  989.604835\] input: sof-rt5682 HDMI/DP,pcm=4 as /devices/pci0000:00/0000:00:1f.3/sof\_rt5682/sound/card0/input21
\[  989.756580\] sof-audio-pci-intel-cnl 0000:00:1f.3: ipc tx error for 0x60010000 (msg/reply size: 108/20): -22
\[  989.756592\] sof-audio-pci-intel-cnl 0000:00:1f.3: HW params ipc failed for stream 1
\[  989.756597\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: error at snd\_soc\_pcm\_component\_hw\_params on 0000:00:1f.3: -22
\[  989.756620\]  DMIC16kHz: ASoC: error at \_\_soc\_pcm\_hw\_params on DMIC16kHz: -22
\[  989.756634\]  DMIC16kHz: ASoC: error at dpcm\_fe\_dai\_hw\_params on DMIC16kHz: -22
\[  989.757414\] sof-audio-pci-intel-cnl 0000:00:1f.3: ipc tx error for 0x60010000 (msg/reply size: 108/20): -22
\[  989.757427\] sof-audio-pci-intel-cnl 0000:00:1f.3: HW params ipc failed for stream 1
\[  989.757432\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: error at snd\_soc\_pcm\_component\_hw\_params on 0000:00:1f.3: -22
\[  989.757468\]  DMIC16kHz: ASoC: error at \_\_soc\_pcm\_hw\_params on DMIC16kHz: -22
\[  989.757483\]  DMIC16kHz: ASoC: error at dpcm\_fe\_dai\_hw\_params on DMIC16kHz: -22
\[  989.758235\] sof-audio-pci-intel-cnl 0000:00:1f.3: ipc tx error for 0x60010000 (msg/reply size: 108/20): -22
\[  989.758247\] sof-audio-pci-intel-cnl 0000:00:1f.3: HW params ipc failed for stream 1
\[  989.758251\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: error at snd\_soc\_pcm\_component\_hw\_params on 0000:00:1f.3: -22
\[  989.758275\]  DMIC16kHz: ASoC: error at \_\_soc\_pcm\_hw\_params on DMIC16kHz: -22
\[  989.758289\]  DMIC16kHz: ASoC: error at dpcm\_fe\_dai\_hw\_params on DMIC16kHz: -22
\[  989.760078\] sof-audio-pci-intel-cnl 0000:00:1f.3: ipc tx error for 0x60010000 (msg/reply size: 108/20): -22
\[  989.760089\] sof-audio-pci-intel-cnl 0000:00:1f.3: HW params ipc failed for stream 1
\[  989.760092\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: error at snd\_soc\_pcm\_component\_hw\_params on 0000:00:1f.3: -22
\[  989.760117\]  DMIC16kHz: ASoC: error at \_\_soc\_pcm\_hw\_params on DMIC16kHz: -22
\[  989.760130\]  DMIC16kHz: ASoC: error at dpcm\_fe\_dai\_hw\_params on DMIC16kHz: -22
\[  989.770762\] sof-audio-pci-intel-cnl 0000:00:1f.3: sof\_ipc3\_keyword\_dapm\_event: Cannot find PCM for DETECT9.0
\[  989.770770\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: PRE\_PMU: DETECT9.0 event failed: -22
\[  989.770777\] sof-audio-pci-intel-cnl 0000:00:1f.3: sof\_ipc3\_keyword\_dapm\_event: Cannot find PCM for DETECT9.0
\[  989.770780\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: POST\_PMU: DETECT9.0 event failed: -22
\[  989.770981\] sof-audio-pci-intel-cnl 0000:00:1f.3: sof\_ipc3\_keyword\_dapm\_event: Cannot find PCM for DETECT9.0
\[  989.770987\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: PRE\_PMD: DETECT9.0 event failed: -22
\[  989.770991\] sof-audio-pci-intel-cnl 0000:00:1f.3: sof\_ipc3\_keyword\_dapm\_event: Cannot find PCM for DETECT9.0
\[  989.770994\] sof-audio-pci-intel-cnl 0000:00:1f.3: ASoC: POST\_PMD: DETECT9.0 event failed: -22

Now the module loads the firmware and specifies the codec name (**rt5682**), which is necessary in case of custom kernel configuration. The module also produces a lot of errors that can be ignored.

### Speakers

After reloading the kernel module (see sections above), it is necessary to find out which device is responsible for the speakers (sink):

`root #``aplay -l`
\*\*\*\* List of PLAYBACK Hardware Devices \*\*\*\*
card 0: sofrt5682 \[sof-rt5682\], device 0: Port1 (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: sofrt5682 \[sof-rt5682\], device 2: HDMI1 (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: sofrt5682 \[sof-rt5682\], device 3: HDMI2 (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: sofrt5682 \[sof-rt5682\], device 4: HDMI3 (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: sofrt5682 \[sof-rt5682\], device 5: Speakers (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

In this example, the speakers (sink) belong to **card 0** as device **5**, which can be represented as device **plughw:0,5**

Now it is necessary to probe the device:

`root #``speaker-test --device plughw:0,5 --channels 2`
This command should send white noise to both speakers alternately. If there is sound, then this device is really a speaker. If there is no sound, other devices in the list should be probed. If there is still no sound, **plughw** should be replaced with **hw**.

### Microphone

After reloading the kernel module (see sections above), it is necessary to find out which device is responsible for the microphone (source):

`root #``arecord -l`
\*\*\*\* List of CAPTURE Hardware Devices \*\*\*\*
card 0: sofrt5682 \[sof-rt5682\], device 0: Port1 (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: sofrt5682 \[sof-rt5682\], device 1: DMIC (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: sofrt5682 \[sof-rt5682\], device 8: DMIC16kHz (\*) \[\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

In this example, the microphone (source) belong to **card 0** as device **1**, which can be represented as device **plughw:0,1**

To probe the micrphone, the following command should be used:

`root #``arecord --device plughw:0,1 --format S16_LE test-sound.wav`
The command will record the sound from the microphone to the file, so it is necessary to speak loudly for the test. `CTRL` + `C` is required to stop recording.

To hear the record the following command should be used (the device is the speaker (sink) that was found in the section above):

`root #``aplay --device plughw:0,5 --format S16_LE test-sound.wav`
If there is no sound, other microphone devices in the list should be probed. If there is still no sound, **plughw** should be replaced with **hw**.

## PipeWire configuration

Create the following configuration file (replace **plughw:0,5** and **plughw:0,1** with the devices found above):

**`/etc/pipewire/pipewire.conf.d/alsa.conf`**

Install [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire) with the following USE flags: **sound-server**, **pipewire-alsa**, **bluetooth** (optional).

Add the user to the following groups: **audio**, **pipewire**.

Launch PipeWire as described [here](https://wiki.gentoo.org/wiki/PipeWire#Sound_Server_Configuration), or in the case of Gentoo Live Image, simply re-login.

After all of this, the sound should work.

### Headphones (3.5mm jack) still do not work

In case PipeWire does not detect the 3.5mm jack, the sink can be defined manually. Connect the headphones to the jack and perform the steps described in the [Speakers section](https://wiki.gentoo.org#Speakers) (waiting for the sound from the headphones). In the case of the example given in that section, the device is **hw:0,0**.

Add an additional adapter to the above PipeWire configuration (replace **hw:0,0** with the proper device):

```
  { factory = adapter
    args = {
      factory.name	= api.alsa.pcm.sink
      node.name		= "alsa-sink-headphones"
      node.description	= "PCM Sink Headphones"
      media.class	= "Audio/Sink"
      api.alsa.path	= "hw:0,0"
    }
  }
```
Reload PipeWire and check the presence of the sink by running the following command:

`user $``wpctl status`
Audio
 ├─ Sinks:
 │  \*   30. PCM Sink                            \[vol: 0.55\]
 │      32. PCM Sink Headphones                 \[vol: 1.00\]

After that, it will be necessary to manually specify which sink to use:

`user $``wpctl set-default 32`
## Troubleshooting

### Error when loading the audio driver

On newer Coreboot versions it is possible to disable the Intel Management Engine in the BIOS settings. When disabled, the audio driver is not able to load. Enable the IME to fix the issue. [\[3\]](https://wiki.gentoo.org#cite_note-3)
