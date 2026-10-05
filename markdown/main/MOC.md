<!-- source: https://wiki.gentoo.org/wiki/MOC | group: Gentoo Wiki (Main) | wiki-title: MOC -->
---
title: MOC
url: https://wiki.gentoo.org/wiki/MOC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-17"
fingerprint: f58cfd4ae9a68f93
license: CC BY-SA 4.0
---

# MOC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**MOC** (**m**usic **o**n **c**onsole) is a console audio player for Linux/UNIX designed to be powerful and easy to use.

## Installation

### Emerge

The Gentoo repository has [media-sound/moc](https://packages.gentoo.org/packages/media-sound/moc) though the xdch47 overlay allows building without FFmpeg version dependency conflicts.

`root #``emerge media-sound/moc`
## Configuration

### Theme

T, arrow keys, enter, then q changes themes.

To save it, \~/.config/moc must be changed to reference a theme from /usr/share/moc/themes:

`user $``ls -1 /usr/share/moc/themes`
black\_orange\_theme
black\_theme
blue\_theme
darkdot\_theme
example\_theme
green\_theme
moca\_theme
nightly\_theme
red\_theme
transparent-background
white\_theme
yellow\_red\_theme

**`~/.config/moc`**

## Usage

### Invocation

`user $``mocp --help````
Music On Console (version 2.6-alpha3-p1)                                                                                                                                                        
Usage: mocp [OPTIONS] [FILE|DIR ...]                                                                                                                                                            
                                                                                                                                                                                                
General options:                                                                                                                                                                                
  -M, --moc-dir=DIR                 Use the specified MOC directory instead of the default                                                                                                      
  -m, --music-dir                   Start in MusicDir                                                                                                                                           
  -C, --config=FILE                 Use the specified config file instead of the default (conflicts with '--no-config')                                                                         
      --no-config                   Use program defaults rather than any config file (conflicts with '--config')                                                                                
  -O, --set-option='NAME=VALUE'     Override the configuration option NAME with VALUE                                                                                                           
  -F, --foreground                  Run the server in foreground (logging to stdout)    
  -S, --server                      Only run the server
  -R, --sound-driver=DRIVERS        Use the first valid sound driver
  -A, --ascii                       Use ASCII characters to draw lines
  -T, --theme=FILE                  Use the selected theme file (read from ~/.moc/themes if the path is not absolute)
  -y, --sync                        Synchronize the playlist with other clients
  -n, --nosync                      Don't synchronize the playlist with other clients
Server commands:
  -P, --pause                       Pause
  -U, --unpause                     Unpause
  -G, --toggle-pause                Toggle between playing and paused
  -s, --stop                        Stop playing
  -f, --next                        Play the next song
  -r, --previous                    Play the previous song
  -k, --seek=N                      Seek by N seconds (can be negative)
  -j, --jump=N{%,s}                 Jump to some position in the current track
  -v, --volume=[+,-]LEVEL           Adjust the PCM volume
  -x, --exit                        Shutdown the server
  -a, --append                      Append the files/directories/playlists passed in the command line to playlist
  -e, --recursively                 Alias for --append
  -q, --enqueue                     Add the files given on command line to the queue
  -c, --clear                       Clear the playlist
  -p, --play                        Start playing from the first item on the playlist
  -l, --playit                      Play files given on command line without modifying the playlist
  -t, --toggle=CONTROL              Toggle a control (shuffle, autonext, repeat)
  -o, --on=CONTROL                  Turn on a control (shuffle, autonext, repeat)
  -u, --off=CONTROL                 Turn off a control (shuffle, autonext, repeat)
  -i, --info                        Print information about the file currently playing
  -Q, --format=FORMAT               Print formatted information about the file currently playing
Miscellaneous options:
  -V, --version                     Print version information
      --echo-args                   Print POPT-interpreted arguments
      --usage                       Print brief usage
  -h, --help                        Print extended usage
Environment variables:
  MOCP_OPTS                         Additional command line options
  MOCP_POPTRC                       List of POPT configuration files
```
### Player

To start the player with a directory:

`user $``mocp ~/Music`
Control the player with keyboard [MOC shortcuts](https://gist.github.com/chiragparekh/8868544a8b3cc4d6d6ba):
