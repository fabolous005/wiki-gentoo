<!-- source: https://wiki.gentoo.org/wiki/TV_Tuner | group: Gentoo Wiki (Main) | wiki-title: TV Tuner -->
---
title: TV Tuner
url: https://wiki.gentoo.org/wiki/TV_Tuner
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-28"
fingerprint: "3c988c4908f58dc9"
license: CC BY-SA 4.0
---

# TV Tuner

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

2025.04.14

An article about configuring and using **television (TV) tuners** with Gentoo Linux.

See also [VDR](https://wiki.gentoo.org/wiki/VDR) article.

(Need entries for your preferred method of viewing and recording. I use the console and figured some would be interested how I do this. ;-)

## Installation

### Kernel

Entries are needed for the Linux Kernel .config; analog and digital tuner drivers as required.

An example configuration in menuconfig follows.

**Example enabling support for DVB devices**

### Emerge

The package [media-tv/linuxtv-dvb-apps](https://packages.gentoo.org/packages/media-tv/linuxtv-dvb-apps) is needed:

`root #``emerge --ask linuxtv-dvb-apps`
## Analog tuner

These are pretty much deprecated within the US, however analog signals are still used extensively for Amateur Radio enthusiasts. Yes, Amateur Radio freaks even broadcast video signals.

### XawTV

Below describes the last stable release of XawTV within Gentoo Portage. As far as development, XawTV version 3 git sees regular updates while, XawTV version 4 git hasn't seen any development for years.

#### Install

Install [media-tv/xawtv](https://packages.gentoo.org/packages/media-tv/xawtv) with *zvbi* (build scantv) and *xext* (include xdga) USE flags. (These are only some suggested USE flags.)

`root #``emerge --ask xawtv`
**`$HOME/.xawtv`**

```
[global]
freqtab = usbcast
[defaults]
input = Television
norm = NTSC-M
[Camera]
input = Composite
```
(See man xawtvrc)

#### Scan for channels

Auto scan for channels:

`user $``scantv -a -C /dev/vbi -c /dev/video0 -f us-bcast -n NTSC-M`
(I'm having some problems with a HVR-1950 on the analog side with with scantv complaining about no vbi device. Try -C /dev/null and/or the latest GIT versions.)

Manually add channels using predefined listings:

`user $``cat /usr/share/xawtv/ntsc-bcast.list $HOME/.xawtv`
### IVTV-utils

ivtv-utils is another program for tuning analog channels, primarily for Hauppauge tuners. (I'm having much better success with ivtv-utils with my Hauppauge HVR-1950, analog side.)

#### Install

Install [media-tv/ivtv-utils](https://packages.gentoo.org/packages/media-tv/ivtv-utils):

`root #``emerge --ask ivtv-utils`
The .ivtv-tune doesn't need to be hand edited as it seems to be automatically created. Here it is anyways in case you feel like manually creating.

**`$HOME/.ivtv-tune`**

#### Scan for channels

ivtv-utils contains the frequency tables statically within it's code. No need to look for external files.

Get a list of supported frequency tables:

`user $``ivtv-tune --list-freqtable`
Tune to a channel:

`user $``ivtv-tune --freqtable=us-bcast --channel=4`
ivtv-tune can also utilize an existing $HOME/.xawtv file as well. See ivtv-utils --help for additional options.

From here, a script can be easily created for automated channel scanning with ivtv-tune. Basically; 1) Tune to a channel. 2) Wait for a second or two for channel lock and then check for a signal (ie. grep /sys/class/pvrusb2/sn-6202710/ctl\_signal\_present/cur\_val). 3) Change to next channel and repeat. A small script can also be created for simply changing channels, as well as incorporating IR support. (TV-Viewer is one such example.)

### MythTV

- Able to manage analog video tuners
- Can be difficult to use from a computer desktop
- Requires multiple dependencies
- There is a wiki page specifically for [MythTV](https://wiki.gentoo.org/wiki/MythTV) on Gentoo.

### The Video Disk Recorder (VDR)

- I would pressume this too can manage analog tuners
- No experience with this package as it requires quite a bit of configuration and knowledge of dependencies.
- Can also be difficult to use from a computer desktop as it was designed for dedicated computers.
- TODO: Insert link to it's Wiki page when available.

## Digital tuner

### CLI or console method

#### Scan for channels

##### dvbscan

To get a list of channels from your digital TV USB or PCI tuning device, the generic dvbscan application from the package [media-tv/linuxtv-dvb-apps](https://packages.gentoo.org/packages/media-tv/linuxtv-dvb-apps) can be used.

DVBScan requires initial tuning data file to find your local frequencies, and already has been prepackaged with numerous generic initial tuning data files. To acquire your own initial tuning data for your area, use [media-tv/w\_scan](https://packages.gentoo.org/packages/media-tv/w_scan).

You may also create the list of channels directly using w\_scan, thus skipping over the need for an initial tuning file, see w\_scans output formats.

`root #``emerge --ask w_scan`
This will generate the initial tuning data for US ATSC over-the-air digital TV:

`user $``w_scan -A1 -c US -fa -t3 -x`
Create an .mplayer/channel.conf for US ATSC over-the-air digital TV using LinuxTV dvbscan with LinuxTV packaged generic tuning data:

`user $``dvbscan -a 0 /usr/share/dvb/atsc/us-ATSC-center-frequencies-8VSB >~/.mplayer/channels.conf.new`
Create an .mplayer/channel.conf for US ATSC using dvbv5-scan: For dvbv5-scan:

`user $``dvbv5-scan --input-format=CHANNEL --output-format=ZAP --output /tmp/channels.conf /usr/share/dvb/atsc/us-NTSC-center-frequencies-8VSB`
##### w\_scan

w\_scan (media-tv/w\_scan) is newer and more robust then the previously mentioned dvbscan, as well as also previously mentioning having the ability to generate initial tuning data.

`user $``w_scan -A1 -X -c US -fa -t3 > ~/.mplayer/channels.conf.new`
##### Channel Naming Problems

The channel naming scheme for over-the-air digital TV is likely not well thought out in your area. The first field of this file is the lettered name field, of which, you may likely see duplicate identical names and no channel numbers. The remainder of the fields relate to the frequency.

For my area, the channel number reported by dvbscan on stdout is correct, but the channel lettered identification contains identical duplicates or is not well named. I have already emailed the linux-media mailing list on Sep 29 2011 to include a switch for writing the channel number to the file instead of the channel letter identification.

For the meantime, I've found a method of creating duplicate frequency entries, and then replacing the channel letter name field with the channel's number. More duplicated entries can be created to eleviate the need for typing ".1" for the first channel.

**`~/.mplayer/channels.conf.example`**

#### Playing channels

##### Linux Virtual Terminal with framebuffer

Compile [media-video/mplayer](https://packages.gentoo.org/packages/media-video/mplayer) with *fbcon* and *svga* USE flags. (In the latest mplayer versions, USE flag *svga* is deprecated.)

`root #``emerge --ask mplayer`
Compile [media-libs/svgalib](https://packages.gentoo.org/packages/media-libs/svgalib). This is a SVGA Linux Kernel driver for [framebuffer](https://wiki.gentoo.org/wiki/Framebuffer) console applications such as [media-video/mplayer](https://packages.gentoo.org/packages/media-video/mplayer) and [www-client/links](https://packages.gentoo.org/packages/www-client/links):

`root #``emerge --ask svgalib`
Make sure to have booted Linux with a framebuffer enabled virtual terminal(s). Make the necessary edits within the bootloader configuration file. (ie. Lilo/GRUB). (ie. Add "vga=788" or "video=uvesafb:nocrtc,ywrap,mtrr:3,1280x1024-16@60")

Up until quite recently, MPlayer could only be used within a plain Linux virtual terminal. It is now possible to use MPlayer even within a Linux virtual terminal running [GNU Screen](https://wiki.gentoo.org/wiki/Screen)! Full size video playback using MPlayer within Linux virtual terminals doesn't seem to work here.

###### MPlayer: Slow CPU or graphics card

For most scenarios, playing live streams on slower machines is not possible. One solution around this is to first record the MPEG streams to file and then play them using your media player.

The following will resize your previously recorded MPEG to something adequately smaller. Use lowres=1 or lowres=2 depending on how slow your system is.

`user $``mplayer -vfm ffmpeg -lavdopts lowres=1:fast:skiploopfilter=all -idx -framedrop test.mpg`
##### From console within Xorg

There's a multitude of media players, of which, mplayer seems to be the most popular and lightest on resources.

`user $``mplayer dvb://11`
MPlayer will use the first field of each line within the channels.conf file for playback. If you have duplicate entries like I do and haven't renamed them as I've done so above, you'll need to specify the entire line from the channels.conf to play the second, third, fourth, ... duplicated entries:

`user $``mplayer 'dvb://KTVF DT:545028615:8VSB:49:52:3'`
#### EPG Guide Data

There's several methods for grabbing EPG TV Guide data and printing to stdout.

##### Method 1: dvbstreamer

[Q. How do I update the EPG data ...](http://sourceforge.net/apps/mediawiki/dvbstreamer/index.php?title=FAQ)
(Untested)

##### Method 2: atsc\_epg

This atsc\_epg (media-tv/linuxtv-dvb-apps) is a true gem as it prints EPG data to stdout which can later be easily parsed or saved.

`root #``emerge --ask linuxtv-dvb-apps`
(According to LinuxTV.org, their linuxtv-dvb-apps package is now considered legacy code. No recent snapshot has been made of their [dvb-apps GIT](http://linuxtv.org/hg/dvb-apps) repository since.)

Also, [LinuxTV](http://linuxtv.org/wiki/index.php/LinuxTV_dvb-apps) has a wealth of information on it's linuxtv-dvb-apps package.

Usage example. Take an entry from the $HOME/.mplayer/channels.conf (from dvbscan) and strip the first field. (ie. "KUAC-DTO:189028615:8VSB:49:52:3")

`user $` `atsc_epg -a0 -f 189028615:8VSB:49:52:3`
This will print a very nicely formated schedule to stdout!

Now automating this process to use $HOME/.mplayer/channels.conf (dvbscan) data is a bit of a trick, especially if channels are updated. However, notice the atsc\_epg command only needs a frequency and atsc\_epg will tell you the channel number(s) the frequency will resolve to on stdout as well. (ie. This one PBS frequency will broadcast all four PBS channel's EPG data along with printing each of the four channel numbers to stdout.)

###### Use EPG Data for syncing time

With a little grep or awk scripting and placing into /etc/init.d, could easily use atsc\_epg EIT/ETT scheduling data for syncing your local computer's time.

For example:

$  atsc\_epg -a0 -f 189028615:8VSB:49:52:3
tuning to 189028615 Hz, please wait...
tuner locked.
system time: Mon Nov 28 19:18:27 2011
TS STT time: Mon Nov 28 19:18:43 2011
MGT table:
...

As you can see, my system is synced using a popular NTP client daemon and it's approximately 15 seconds slow when compared to the of the locally broadcasted EIT/ETT scheduling data. If you record frequently or rarely have local Internet access for syncing your computer's time, this might be the next best method, if not the best method for keeping your computer's time accurate.

##### Issues with only one available tuner

The catch is, you need to have this run in the background while you are not recording as it will use the only tuner on your card for getting the broadcasted EPG data. When a recording starts, the recording script needs to check for this dvbstreamer and kill it before the recording starts, else the recording will fail to initiate as the only tuner is being used. The data being retrieved is simple ASCII text and can be likely easily parsed by even a script. I just haven't had the time to write such a script or console program, and the EPG data broadcasted here is not consistent with more accurate web based options such as TitanTV.

#### Scheduling

You can schedule to records a channel by creating a script to record (mencoder) a certain number of minutes (or hours) using vixie-cron (ie. crontab -e).

##### Create a recording script

One method is to create a script for each channel, such as the one below. One problem with this is, transmitter technicians keep changing the channel naming and frequencies. So it's wise to use my suggestion about creating aliases within the channels.conf file, noted at the beginning.

###### Recording using MPlayer's Mencoder

**`~/bin/record-ch9.1-60m.sh`**

```
#!/bin/bash - 
#===============================================================================
# FILE:         record-ch9-60m.sh
# USAGE:        ./record-ch9-60m.sh 
# DESCRIPTION:  Record Channel 9 for 60 minutes
#===============================================================================
mencoder 'dvb://KUAC-DTC:189028615:8VSB:49:52:3' -really-quiet -of avi \
  -ovc copy -oac copy -o /stored/tv/ch9_60min-`date +%Y%m%d-%H%M`.mpg &
pidof_mencoder=`echo $!`
sleep 59m # Only record for 58-59 minutes so we don't conflict next scheduled
          # recording.
kill $pidof_mencoder
```
###### Recording using zap and cat|dd (preferred method)

A combination of azap, czap, or tzap, along side cat or dd can be used to record from a digital DVB device. I've already taken the liberty and done much of the work and created the **[record-dvb.sh](http://rogerx.sdf.org/files/bin/record-dvb.sh)** already posted within the above Note.

The **[record-dvb.sh](http://rogerx.sdf.org/files/bin/record-dvb.sh)** accepts the channel id and number of minutes as options. The script then calls /usr/bin/azap alongside /bin/dd and backgrounds them both, then sleeps for the specified amount of time. File naming is also handled well within the script and only modifying the variables at the very top of the script should be required.

To test this recording method on your computer, issue the following two commands.

Set the channel using azap, czap or tzap:

`user $``$ azap -c $HOME/.mplayer/channels.conf -r 9.1`
Once a successful channel lock has been established, use /bin/cat or /bin/dd to record the stream:

`user $``$ cat /dev/dvb/adapter0/dvr0 > test.mpg``user $``dd if=/dev/dvb/adapter0/dvr0 of=test.mpg conv=noerror`
##### Schedule a recording

Setup a crontab entry, as user, root or within /etc/cron, but I prefer a user cron entry.

`user $``crontab -e`
\# Record Doctor Who
  #   Sundays @ 23:00
  #0 23 \* \* 0 $HOME/bin/record-ch9.1-60m.sh
  #0 23 \* \* Sun $HOME/bin/record-ch9.1-60m.sh
   0 23 \* \* Sun $HOME/bin/record-dvb.sh -c 9.1 -m 60
  #   Thursdays @ 20:00
  #0 20 \* \* 4 $HOME/bin/record-ch9.1-60m.sh
  #0 20 \* \* Thu $HOME/bin/record-ch9.1-60m.sh
   0 20 \* \* Thu $HOME/bin/record-dvb.sh -c 9.1 -m 60

Save and exit your console editor of choice. No need to restart cron as changes take effect immediately.

(Read 'man 5 crontab' for an explanation of crontab field names.)

### MythTV

- Able to manage digital video tuners
- Can be difficult to use from a computer desktop
- Requires multiple dependencies
- There is a wiki page specifically for [MythTV](https://wiki.gentoo.org/wiki/MythTV) on Gentoo.

### The Video Disk Recorder (VDR)

- implements an digital video recorder for digital tuners on dedicated computers
- huge bunch of Plugins extending its functionality
  - stream audio/video
  - can be used on desktops using Plugins xine or xineliboutput
  - teletext
  - analog TV (only TV cards with hardware MPEG Encoder)
  - advertising search
  - channel scan
  - server/client
  - remote timers
  - DVD burning
  - other media
  - ...
- [english wiki](http://www.linuxtv.org/vdrwiki/index.php/Main_Page) available on linuxtv, but the [German wiki](http://www.vdr-wiki.de/wiki/index.php/Hauptseite) is by far more up to date.

## Troubleshooting

### MPlayer exits with "vo: x11 uninit called but X11 not initialized" during DVB playback

Problem: When attempting to play video provided by the V4L "/dev/dvb/" devices, a "vo: x11 uninit called but X11 not initialized" error may occur.

Solution: This error or bug seems to occur consistently with the latest stable =media-video/ffmpeg-1.2.6-r1 and media-video/mplayer-1.2\_pre20130729 packages within Gentoo are used. Unmask and install =media-video/mplayer-1.2\_pre20141011\*, which requires =media-video/ffmpeg-2.2.12\* as well. Where the bug occurs, I do not know, but it is likely within MPlayer as it's complaining about X11 not being initialized, and "-vo null -ao null" has no effect. Furthermore, this bug appears to be solved within later versions, so the bug is likely known.

UPDATE: This bug still occurs with the updated packages, but only when connecting the USB DVB device to the USB Version 1 ports. So this bug would appear to be an issue with using a Hauppauge HVR-1950 or USB DVB device connected to USB Version 1 ports. Likely something within the Linux kernel is not waiting long enough for initialization, as I've seen this problem duplicated even when using very simple azap/dd or record-dvb.sh toos. Basically the DVB USB device is connected via a USB Version 1 port, the USB DVB device appears to be transmitting a mangled stream. (ie. During playback of the recorded stream with MPlayer, mpegts: PES packet size mismatch)

### Latest linuxtv-dvb-apps are old!

The latest media-tv/linuxtv-dvb-apps are deprecated, or are on their way out with media-libs/libv4l\[utils\] being the successor. The media-tv/linuxtv-dvb-apps package provides the zap executables required for use by my record-dvb.sh mentioned above. Porting record-dvb.sh to the newer media-libs/libv4l\[utils\] will occur once media-libs/libv4l\[utils\] are stable, and depending whether the v4l-utils still exist after the developers plan to port the v4l-utils to libv4l. I assume they will still exist, and porting will only require modification of the zap executable call within record-dvb.sh

### Test DVB signal strength

Users can test antenna reception signal strength using the following newer media-tv/v4l tool:

`user $``dvbv5-scan --adapter 0 -I CHANNEL ~/.mplayer/channels-initial_data_v3.conf``user $``dvbv5-scan --adapter 0 -I CHANNEL /usr/share/dvb/atsc/us-NTSC-center-frequencies-8VSB`
The channels-initial\_data\_v3.conf being within the following layout, where 207000000 is the frequency and 8VSB is notation for automatic tuning for ATSC.

**`$HOME/.mplayer/channels-initial_data_v3.conf`**

```
A 207000000     8VSB
```
The us-NTSC-center-frequencies-8VSB, or your file based on your location or TV standard, is provided by media-tv/linuxtv-dvb-apps package. Notice, you can enter any frequency you desire with the channels-initial\_data\_v3.conf file for dvbv5-scan tool! A splendid example; if you have a weak intermittent broadcast station, you do not need to rely solely on w\_scan for which currently only scans entire frequency spectrum during each initialization. The dvbv5-scan executable gladly scans one frequency, or a grouping of specific frequencies unlike w\_scan. Most though should rely on w\_scan for their first broadcast channel's file. Then if they have problems, dvbv-scan along with researching the FCC database for frequency usage should be considered the next step.

### Electronic Programming Guide (EPG/EIT) Fails with atsc\_epg, dvbsnoop, ...

I've noticed lately the Electronic Programming Guide (EPG/EIT) failing with atsc\_epg, dvbsnoop, tv\_garb\_dvb, etc. Reason being within my area, the EPG/EIT data is now being broadcast assigned to a different packet address of 0x1D00 or PID 0x1D00 hexadecimal value, or PID 7424 decimal value, instead of PID 0x12? (ie. azap -a 0 -f 0 -d 0 -c .mplayer/channels.conf -r WQLN-HD", while within another terminal use "dvbsnoop -s sec 0x1D00 -b -n 200 > ./EIT.bin" should provide better results.) I'm not sure if this is dependent upon the area broadcast transmitters, or more likely a recent upgrade to the previous ATSC standard or specification. The dvbsnoop tool (media-video/dvbsnoop) can be utilized to find the EID PID of the EPG/EIT data after tuning with azap within another terminal. (ie. "dvbsnoop -s pidscan", "dvbsnoop -s sec 0x1D00 -b -n 200", or just use "dvbsnoop 0x1D00" as well as substituting decimal values for hexadecimal values.) Also note, atsc\_epg tends to fail on 64bit platforms due to a "Value can't be converted to integer" segfault. On 32 bit platforms, atsc\_epg appears to be grabbing EPG data from the proper (0x1D00) PID's or addresses.

## External resources

- [Dvbscan - LinuxTVWiki](http://linuxtv.org/wiki/index.php/Dvbscan)
- [Zap - LinuxTVWiki](http://linuxtv.org/wiki/index.php/Zap#Tuning_a_channel)
- [W scan - LinuxTVWiki](http://linuxtv.org/wiki/index.php/W_scan) - An alternative, faster and easier channel scanner then DVBScan.
- [record-dvb.sh](http://rogerx.sdf.org/files/bin/record-dvb.sh) - Bash Script records using LinuxTV dvb-apps' zap. Written for using with Cron.
