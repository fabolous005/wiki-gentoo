<!-- source: https://wiki.gentoo.org/wiki/SHOUTcast/Installation | group: Gentoo Wiki (Main) | wiki-title: SHOUTcast/Installation -->
---
title: SHOUTcast/Installation
url: https://wiki.gentoo.org/wiki/SHOUTcast/Installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-31"
fingerprint: d4131652f9af5ba9
license: CC BY-SA 4.0
---

# SHOUTcast/Installation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This guide will walk through the steps needed to setup a streaming radio server with SHOUTcast Server and SHOUTcast Trans.

## Setting up SHOUTcast Server

### Installing the files

The SHOUTcast Server can be found in media-sound/SHOUTcast-server-bin. It can install it with the following command:

`root #``emerge --ask shoutcast-server-bin`
The SHOUTcast Server will now be installed. The next step is configuring the new SHOUTcast Server.

### Configuring SHOUTcast Server

Now that SHOUTcast Server is installed, it must be configured. The configuration file can be found in /etc/shoutcast/sc\_serv.conf. Begin with the configuration. Make sure you are root, and open the configuration file with your favorite editor:

`root #``$EDITOR /etc/shoutcast/sc_serv.conf`
This will bring up the SHOUTcast Server configuration file for viewing. /etc/shoutcast/sc\_serv.conf is the configuration file with all the different options that can be set. Take a look on how to setup these particular options.

### Mandatory Options

This is where the maximum number of users is set. As the caption states, it is foolish to setup 100 users on a 256kbps upload. If the SHOUTcast Server is to serve a LAN, the `MaxUser` setting can probably set MUCH higher (to the 100 mentioned easily). Please remember to not abuse whatever bandwidth is being used. Bandwidth comes at quite a high price to ISPs and some will cut your account, fine you high costs to makeup, or both.

Here is where you setup the password. The password itself is clear text. For security purposes, I STRONGLY recommend that you don't use passwords that are used to access critical system components or other sensitive information. Make this as random as possible, with a combination of letters and numbers. This password will be what SHOUTcast Trans (or any other content provider) will use to connect and provide streaming content.

This value sets up what port users will connect to on your SHOUTcast Server. The default is 8000, as it is what most mp3 server capable programs will default to (Audacious, winamp, etc.). As it states, if you wish to use a port less than 1024, you will need to be root. However, I strongly urge against using any port lower than 1024 for your SHOUTcast Server.

This sets the location of the SHOUTcast server log file. The ebuild has it set to /dev/null, so you will need to change it in order to get a real log. I have it setup in the basic /var/log location. You can have it log to wherever you need.

This displays information on the current song to stdout every second. This is disabled by the ebuild so that the SHOUTcast daemon can run as silently as possible. Set this to 1 if you want these updates each second. However, I recommend you use the status page instead.

This is disabled by default in the ebuild to make the daemon run as silently as possible. This will log any events (connects, disconnects, etc) to stdout as they happen in real time. However, because the log file does the same thing, I recommend using it instead.

Just as it states, this value will set how many of the most recently played /played.html will display. If you put more than 20, you should probably consider more coffee.

This setting enables or disables logging for directory modifications by the DNAS (Distributed Network Audio Server), or SHOUTcast for short. Recommended for those who wish to have the most secure logging possible. Basic home/casual users probably don't need this.

This specifies whether or not you want to log hits to the HTTP server that SHOUTcast provides. Once again, recommended for those who wish the most secure logging possible, but not recommended for home/casual users.

The first option enables W3C logging. This type of logging can be easily parsed by the programs listed. This is highly recommended for those who wish to have the most in depth statistics possible. The second option specifies where to store the W3C log. This is set to /dev/null by the ebuild.

### Network Configuration

The SrcIP variable sets what IP streaming content is coming from. This can be another server (relaying), localhost (regular), or any other IP that your interface supports. Setting to localhost prevents any other server from using your SHOUTcast Server as a broadcast source. The default is ANY and will cause your SHOUTcast Server to source from any IP. Security wise, it is better to set this to something specific.

This determines which IP on your interface you will allow users to connect to. This can be localhost (if you're anti-social and wish only to stream to yourself), a private IP (for instance, 192.168.0.101, for hosting to a local network), or your external IP (for instance, 209.204.249.201, for streaming to a WAN, but not a LAN). In most cases, you can reach your own stream by using 127.0.0.1 instead of what is listed here. ANY lets your SHOUTcast Server bind to all IP addresses on all available interfaces.

This has 2 functions. First is the port to connect to yp.SHOUTcast.com with. yp.SHOUTcast.com is nullsoft's page for public servers so users know where to listen in on. Users can search for your station from this page. The secondary use is for web proxies. Set this to the port you use for proxy connects, and set DestIP to your proxy for streaming.

This option specifies whether or not you want to perform reverse DNS lookups on clients. This would take an IP address and try to find out the corresponding host name. Use this for logging purposes to create a more detailed report.

This specifies that you are acting as a relay server. Relay servers are often used to take a low bandwidth connection that can only stream to one client, and use its own higher bandwidth to serve to more clients. RelayPort specifies the port and IP address of the SHOUTcast Server you wish to relay for. Comment this out if you don't plan on using your server as a relay.

### Server Configuration

Setting this will create a broadcaster and administrator. The broadcaster can log in with Password, and view connections. However, should the person want to kick/ban/administer the server, they must have the Admin password. This option creates more specific roles for your server. This is recommended for instances where the system administrator is not the same person as the broadcaster.

This specifies whether or not users are kicked out if the stream disconnects for any reason. This is set to 0, so that clients will either timeout themselves, or keep trying to buffer a stream. Use this if you expect short interruptions at any time.

This specifies when the SHOUTcast Server should give up waiting for a source (mainly a relay server) to stream content from. Anywhere from 30 to 60 should be a reasonable value for this.

The ContentDir specifies where to put on demand content. For example, if you wish to stream an announcement to employees, you could use this for that purpose. The SHOUTcast Server ebuild sets this to /opt/SHOUTcast/content for you. To use this, put an mp3 in the content directory, then point your browser to [http://example.com](http://example.com):\[port\]/content/mp3name.pls

This allows you to configure an intro file. Every time users connect, they'll hear this file played. As it states, the stream bit rate and the intro song bit rate must match, or else things will break. You can, however, put something such as intro128.mp3 and intro64.mp3, and it will play intro128.mp3 to users connecting to a 128kbps stream, and intro64.mp3 for users connecting at 64kbps.

This is the same as above, but will be played when the stream source ends, instead of when users disconnect. This will only work if AutoDumpUsers is set to 0.

This sets up a non-variable title for your SHOUTcast server. Use this if your source stream differs from your SHOUTcast Server's name. This will NOT work with relay servers.

This is the same as TitleFormat except that the URL listed above is used instead of the source stream's URL.

This specifies whether or not you want to be listed as a public server even if your relay server/source plug-in is listed as such.

AllowRelay determines if other servers are allowed to relay your content. If you don't think you'll ever relay at all, set this to No.

AllowPublicRelay specifies whether or not you wish to be listed in the SHOUTcastpublic directory if the server you're relaying to is already listed. Note that PublicServer can override this setting.

Just leave this as is.

### Access Configuration

I'm not to sure why you'd need this one. Basically, if a user is on for too many minutes, disconnect them. Only thing I can think of is to kick idlers off, or people you think should be doing other things than listening to your stream. Value is measured in minutes.

This is the filename for the list of clients that are banned from your server. The default is sc\_serv.ban, but you can use whatever name you so desire with this setting.

As grim as it sounds, Rip actually stands for "Reserved IP". Use this for your friends or other people you consider more important than random users. If you are currently streaming to the max number of users possible, and one of your rip members tries to get on, it will kick the longest listening person from the server to get them on.

This allows for only Rip members to connect to your SHOUTcast Server. You can either use this for private radio streams, or to make it so that only certain relays will be able to access your stream.

### Mass Configuration

Basically, if you're running lots of SHOUTcast Servers, it would be a dire pain to change all the log/ban/etc. files to something unique for every configuration. Instead, you can set Unique to something, and $ will be replaced with whatever Unique is set to. For example, if one file had Unique=Jazz and another had Unique=Rock, then Log=/var/log/$.log would produce /var/log/Jazz.log on one configuration file and /var/log/Rock.log on another configuration file. Much easier when dealing with multiple SHOUTcast Servers on similar configurations.

If you're running multiple SHOUTcast Servers and wish to utilize similar configuration variables without setting them all for each configuration file, you can set this to point to a file that contains settings that are similar between multiple configurations.

### Optimization Configuration

On multiple CPU systems, use this setting to force the SHOUTcast Server to utilize CpuCount # of processors. The default to assign one thread to each processor, and have listeners across all the threads. If you set this lower than your total processor count, this will leave processors free to do other things.

The SHOUTcast Server will use the sleep value in determining the gap between sending data. The higher the value, the longer the gap, the lower the value, the shorter the gap and the more CPU usage SHOUTcast Server will take up. On slower systems, as it states, you might want to lower this so that the SHOUTcast Servers sends data more and more frequently to users. Best to leave as is.

Probably don't need to worry about this setting to much unless you use custom XML parsers to create custom statistics for you server. If the XML parser cannot handle whitespace and line feeds in XML, set this to Yes, and all should work.

### Configuration Conclusion

Your SHOUTcast Server should now be configured. For businesses that are using SHOUTcast, I recommend turning on WC3 logging, as it can be easily parsed, and used for creating custom statistics. You should also enable the AdministratorPassword. You might also wish to enable some of the mass configuration options if you're creating multiple SHOUTcast Servers.

With the configuration setup, we'll now work on getting SHOUTcast up and running. We'll start with simple on demand streaming for a simple startup, then work on SHOUTcast Trans later (as it is somewhat more involved).

## Getting started with SHOUTcast Server

### Setting up on demand streaming

On demand streaming, as shown in the configuration chapter, automatically sets up on demand play lists for mp3 files in the content directory. The SHOUTcast server ebuild has a directory setup in /opt/SHOUTcast/content for all your on demand mp3's. Let's get started by creating a simple on demand streaming mp3.

First we'll need to get an mp3 from somewhere and put it in the content directory. We'll take this sample.mp3 file from an /Mp3 directory I have created.

`root #````
cp sample.mp3 /opt/SHOUTcast/content/
```
`root #````
cd /opt/SHOUTcast/content/
```
`root #``ls`
sample.mp3

OK, so the file is copied over now. Now we'll need to startup our SHOUTcast Server so the file can be accessed.

`root #``/etc/init.d/shoutcast start````
 * Starting Shoutcast Server...
*******************************************************************************
** SHOUTcast Distributed Network Audio Server
** Copyright (C) 1998-2004 Nullsoft, Inc.  All Rights Reserved.
** Use "sc_serv filename.ini" to specify an ini file.
*******************************************************************************
                                                                         [ ok ]
```
The little banner is there to make sure that nothing dies right away (i.e. so you know your server actually started). Your SHOUTcast Server is now started! Because of the nature of on demand content, you will ONLY be able to access it from a browser. MPlayer or anything else won't be able to stream it as is. I use kmplayer in order to access the stream directly from my browser. You can see the result on the next image.

Some people have Audacious setup to handle their audio mime types, so your browser may spawn Audacious up in order to play the resulting streaming content. Now that you are able to work with on demand content, we'll now work on using SHOUTcast Trans to create a true streaming radio server.

## Setting up SHOUTcast Trans

### SHOUTcast Trans introduction

SHOUTcast Trans stands for SHOUTcast Trans(coder), as it is able to transcode mp3's to lower or higher bit rates. SHOUTcast Trans works by streaming mp3's from a play list specified in the configuration file. We'll begin to setup the configuration for SHOUTcast Trans, so that we can have a real to goodness streaming radio station. We'll begin by opening the configuration file for SHOUTcast Trans, which just so happens to be located in /etc/shoutcast/sc\_trans.conf.

`root #``emerge --ask shoutcast-trans-bin``root #``vi /etc/shoutcast/sc_trans.conf`
Now that we have the SHOUTcast Trans configuration file open, we'll begin to setup the streaming source.

### Configuring SHOUTcast Trans

This setting tells SHOUTcast where to find its streaming media content from. This setting requires an existing file, so let's go ahead and create a play list. I'll create one real quick from my /Mp3 directory referred to earlier.

`user $``find /Mp3 -type f -name "*.mp3" > /opt/SHOUTcast/playlists/playlist.lst`
Now that the play list is setup, we point the configuration file to it, and SHOUTcast Trans will now know what files to stream.

This setting decides where to send the streaming content. In this guide, it will be the SHOUTcast Server's IP and port that you setup previously (DestIP and PortBase).

The is the same password that you setup in the SHOUTcast Server configuration.

This sets up the title of your stream (i.e. Radio One), the URL (i.e. [http://www.radio-one.com](http://www.radio-one.com)), and the Genre (i.e. Electronica Trance Tribal).

This will point to the log file for SHOUTcast Trans. All your logging goes here.

Decide on whether or not you want your play list to play random songs from your list each time. Most will set this to 1. If you're going to be accepting song requests, set this to 0 and I'll explain how to do that later on.

Bitrate sets up the bit rate for your stream. This can be from 8000 (8kbps) to 128000 (128kbps). SampleRate sets the sampling rate of the stream. This can be anything from 11025 (11025kHz) to 44100 (44100kHz). Channels sets how many channels your stream will broadcast. This can be anything from 1 (mono) to 2 (stereo). Quality sets the stream quality. This is somewhat still controlled by the Bitrate/SampleRate/Channels. This is where you deal with how compressed the stream is. 1 gives you best quality, 10 gives you the best speed. Keep your connection in mind when you set these values! Use the guide given in order to figure out what your mp3's should be streamed at.

This sets up song cross fading. Setting this to 0 will disable cross fading. If you set it to 1, Song 1 will fade out and Song 2 will fade in. If you set it to 2, Song 1 will fade in and Song 2 will fade out. The length is how long in ms the cross fade occurs.

This decides whether or not you wish to use the ID3 tag for information about the mp3.

This sets up whether or not streams should be publicly listed when relaying to a server. Remember PublicServer in sc\_serv.conf can over-ride this!

This sets up the information on how to reach you (the DJ). You can setup AIM or ICQ channels for song requests/anything. You can setup your own IRC channel as well, so that you can interact with multiple users at once.

### SHOUTcast Trans Setup Conclusion

Your SHOUTcast Trans is now ready to stream to your SHOUTcast Server! We'll now get started on streaming your mp3's.

## Getting Started With SHOUTcast Trans

### Starting up SHOUTcast Trans

As I most often use SHOUTcast Trans with SHOUTcast Server, I tend to startup SHOUTcast Trans, which in turns starts up SHOUTcast for you (much easier). So we'll go ahead and get SHOUTcast Trans started.

`root #``/etc/init.d/shoutcast_trans start````
 * Starting Shoutcast Server...
*******************************************************************************
** SHOUTcast Distributed Network Audio Server
** Copyright (C) 1998-2004 Nullsoft, Inc.  All Rights Reserved.
** Use "sc_serv filename.ini" to specify an ini file.
*******************************************************************************
                                                  [ ok ]
* Starting Shoutcast Trans...                     [ ok ]
```
### Listening to the SHOUTcast Trans stream

Now that SHOUTcast Trans is started, we'll start listening to the stream. I use MPlayer in this example to play the stream.

...
Playing http://127.0.0.1:8000/.
Connecting to server 127.0.0.1\[127.0.0.1\]:8000 ...
Name   : Chris Gentoo Beats
Genre  : JPOP Electronica And More!
Website: http://www.gentoo.org
Public : no
Bitrate: 128kbit/s
Cache size set to 1024 KBytes
Connected to server: 127.0.0.1
Cache fill:  9.38% (98304 bytes)    Audio file detected.
==========================================================================
Opening audio decoder: \[mp3lib\] MPEG layer-2, layer-3
MP3lib: init layer2 and 3 finished, tables done
mpg123: Can't rewind stream by 156 bits!
AUDIO: 44100 Hz, 2 ch, 16 bit (0x10), ratio: 16000->176400 (128.0 kbit)
Selected audio codec: \[mp3\] afm:mp3lib (mp3lib MPEG layer-2, layer-3)
==========================================================================
Checking audio filter chain for 44100Hz/2ch/16bit -> 44100Hz/2ch/16bit...
AF\_pre: af format: 2 bps, 2 ch, 44100 hz, little endian signed int
AF\_pre: 44100Hz 2ch Signed 16-bit (Little-Endian)
AO: \[oss\] 44100Hz 2ch Signed 16-bit (Little-Endian) (2 bps)
Building audio filter chain for 44100Hz/2ch/16bit -> 44100Hz/2ch/16bit...
Video: no video
Starting playback...

This was somewhat clipped. The -cache variable was put in to over-ride my somewhat larger buffering settings. And voilÃ ! You're now listening to streaming media! In the next chapter, we'll show you how to do a little bit more with your SHOUTcast Server.

## Advanced SHOUTcast Usage

### Business Usage

Businesses can use SHOUTcast in a number of ways:

1. Use on demand content streaming to make more interesting daily announcements.
2. Have streaming public announcements available as they happen, let your clients know what's going on, on the spot! Then archive them as on demand content streaming for future reference.
3. Do interviews as streaming media and archive them as on demand content streaming.

There are more possibilities on how to utilize SHOUTcast Server for businesses. Use live streaming media instead of boring old text!

### DJ-ing with SHOUTcast

SHOUTcast Server is one of the most popular servers for both new and veteran DJs alike. For those just starting, there are a few ways to increase the user experience of your SHOUTcast Server. Having an intro song is very key. It gives the users an idea of what your station is all about. Be sure to include this! Post your server on yp.SHOUTcast.com (described in the SHOUTcast Server configuration section) so that everyone knows where you are. One of the most unique things is to be able to take requests. To set up requesting, first turn Shuffle off in sc\_Trans.conf. Have about, I'd say, 10 or so songs ready to get you started. Then start requesting song requests in the middle. When someone requests a song, simple add it to the end of your play list, and then you can use this script here to control what SHOUTcast Trans does with your play list:

When you've added the song to the play list, you need to tell SHOUTcast Trans that your play list has changed with the new request entry.

`user $``djcontrol reload`
You should now let the users know after what song the requests will start. Or if you want, you can keep skipping with:

`user $``djcontrol next`
Be careful not to skip too much, as there is no previous control. Once you hit their song, the requesting begins. I'd get about 5 or so requests before you start requesting. This way you don't run all the way back to the beginning. If you start to lack in requests and expect that your request hour is over with, then simply copy your next session's play list over the requests play list and reload the play list. Once the current song is over, it will go back to the new play list.

### Conclusion

That ends it for the SHOUTcast Server and SHOUTcast Trans tutorial. I hope you benefited from the information here and please email me any comments or suggestions for this page! Enjoy your new streaming SHOUTcast Server!
