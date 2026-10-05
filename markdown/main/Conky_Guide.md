<!-- source: https://wiki.gentoo.org/wiki/Conky/Guide | group: Gentoo Wiki (Main) | wiki-title: Conky/Guide -->
---
title: Conky/Guide
url: https://wiki.gentoo.org/wiki/Conky/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: a4a1591a36b29ffc
license: CC BY-SA 4.0
---

# Conky/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide describes how to install and configure the system monitor known as Conky.

## Background

### Introduction to Conky

So you have a Gentoo machine, and have already learned 30 different commands to monitor different aspects of what your computer is doing at the current moment. What do you do from here? Isn't there an easier way to monitor system performance and see what it's doing, as well as the resources it's using to perform all those tasks? This is what a system monitor, such as Conky, provides.

### What it does

Unlike other system monitors such as top, Conky can run as a window in an X session, or by drawing to the root window (there is also an option to have Conky display information to stdout, but we won't discuss that here). It displays the information it has gathered through the use of both text, progress bars, and graphs. Also, unlike top, the way it is formatted is completely user-configurable. In addition to monitoring the system itself, Conky can also give you information about several music players (such as Music Player Daemon and Audacious Media Player), tell you how many new messages are in your mail spool, and plenty more. If the functionality you require isn't in Conky yet, it is a simple matter of writing a script to get the information you would like - some examples of this, which have already been done are RSS feeds, POP3 e-mail message count, local weather, boinc status, and even the status of Portage.

## Installing Conky

### Base install

Gentoo provides an ebuild to quickly and easily install Conky. Pay particular attention to the USE flags. You'll most likely want X11 support (`X`), and make sure you select the USE flags for any music players (other than MPD) which you want, such as Audacious (`audacious`). If you want to use the TCP port monitor, be SURE to disable the `ipv6` use flag, as the port monitor is for IPv4 systems only.

In addition, the `truetype` USE flag compiles support for TrueType fonts with the use of Xft. Most users will want this as well.

You can add syntax highlighting for Conky's configuration file with the `vim-syntax` and `nano-syntax` USE flags, depending on your preferred editor.

`root #``echo app-admin/conky truetype audacious -ipv6 >> /etc/portage/package.use`
Once you have your USE flags correctly set up, it's time to install Conky!

`root #``emerge --ask conky`
You can test Conky to see how it will look by running the command `conky` in a terminal. This will likely give you a good reference to how it will look and what you want to change, add or even remove.

`user $``conky`
Once you have an idea of how Conky looks, you can now move on to configuring it!

### Configuring Conky

By default, Conky will look for a configuration file in the user's home directory located at \~/.conkyrc. This file contains all the configuration options, and the static text, colors and other variables which control what data is shown to the user. Conky also provides a great sample configuration, located at /etc/conky/conky.conf.

`user $``cp /etc/conky/conky.conf ~/.conkyrc`
Now, open up the sample configuration in the text editor of your choice. You may notice that there are two separate sections of the configuration file. The first section of the file, contains the program configuration options and controls how it acts. This includes things such as the `update_interval`, or how often Conky will update the information on the screen. The second section contains the actual text, graphs, and variables which are rendered on the screen. This includes things such as the system uptime (`$uptime`), CPU usage (`$cpu`) and anything else you want to be shown. The first section of the file starts right from the beginning, the second section consists of everything after the line which says `TEXT`. Comments in the file start with `#`, but keep in mind that even if a line is commented out in the second section of the file, the text will still be rendered to the screen.

Lists of all the available configuration options and variables are kept at [http://conky.sourceforge.net/config\_settings.html](http://conky.sourceforge.net/config_settings.html) and [http://conky.sourceforge.net/variables.html](http://conky.sourceforge.net/variables.html) . Besides, there are a few great screenshots along with sample configurations and scripts at [http://conky.sourceforge.net/screenshots.html](http://conky.sourceforge.net/screenshots.html).

## Extending Conky

### Beyond the built-in variables

So you've gotten this far, and have scoured the Conky documentation for that extra variable which Conky just doesn't seem to have... You're in luck! Conky provides several variables for just this reason! `$exec` Will run a command every time Conky updates, `$execi` will run a command at a specified interval and `$texeci` will run a command in its own thread at a specified interval.

### Customise to have weather (temperature, etc.) with [https://open-meteo.com/](https://open-meteo.com/), curl and jq

First, let's add the `curl` use flag

`root #``echo app-admin/conky curl >> /etc/portage/package.use`
Install jq :

`root #``emerge --ask jq`
We'll choose our city using the open-weather documentation [https://open-meteo.com/en/docs](https://open-meteo.com/en/docs)

For York in England, we get the following url :
[https://api.open-meteo.com/v1/dwd-icon?latitude=53.9576&longitude=-1.0827¤t=temperature\_2m&timezone=auto](https://api.open-meteo.com/v1/dwd-icon?latitude=53.9576&longitude=-1.0827¤t=temperature_2m&timezone=auto)

With the variable `${texeci 60 curl "URL"}` we're going to run curl every 60 seconds.

And We'll choose a few options for curl:

`-s` : silent mode.

`--retry 5` : try 5 times maximum.

`--retry-max-tiume 30` : after 30 seconds, even if the 5 attempts have not been made, give up.

We're going to parse the result of the url and read the temperature of curl return and jq


Our final command can be written as follows to display York's temperature in °C:
