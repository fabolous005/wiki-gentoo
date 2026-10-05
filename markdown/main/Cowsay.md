<!-- source: https://wiki.gentoo.org/wiki/Cowsay | group: Gentoo Wiki (Main) | wiki-title: Cowsay -->
---
title: cowsay
url: https://wiki.gentoo.org/wiki/Cowsay
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-20"
fingerprint: ce8e9c4a1cc2899b
license: CC BY-SA 4.0
---

# cowsay

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**cowsay** is a configurable talking cow, written in [Perl](https://wiki.gentoo.org/wiki/Perl), by Tony Monroe.

## Installation

### Emerge

[games-misc/cowsay](https://packages.gentoo.org/packages/games-misc/cowsay) is on Gentoo's main repository:

`root #``emerge --ask games-misc/cowsay`
## Usage

### Invocation

`user $``cowsay -h````
/usr/bin/cowsay version 3.7.0 calling Getopt::Std::getopts (version 1.13),
running under Perl version 5.38.2.
cowsay version 3.7.0
Usage:
    cowsay [-bdgpstwy] [-e <eyes>] [-f <cowfile> | -r [-C] ] 
        [-n] [-T <tongue>] [-W <wrapcolumn>]
        <message>
    cowsay -l    # List defined cows
    cowsay -h    # Displays this help screen
```
### Fortune

Cowsay supports the Fortune package.

First, emerge fortune:

`root #``emerge --ask games-misc/fortune-mod`
Then, use the following command to have cowsay write a fortune.

`user $``fortune | cowsay````
 _________________________________________ 
/ Bounders get bound when they are caught \
| bounding.                               |
|                                         |
\ -- Ralph Lewin                          /
 ----------------------------------------- 
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```
## xcowsay

Xcowsay is a graphical variant of cowsay that spawns a cartoon depiction of a cow on the user's screen.

### USE flags


### Emerge

`root #``emerge --ask games-misc/xcowsay`
## Usage

### Invocation

`user $``xcowsay -h````
Usage: xcowsay [OPTION]... [MESSAGE]...
Display a cow on your desktop with MESSAGE or standard input.
Options:
 -h, --help		Display this message and exit.
 -v, --version		Print version information.
 -t, --time=SECONDS	Number of seconds to display message for
 -r, --reading-speed=N	Number of milliseconds to delay per word.
 -f, --font=FONT	Set message font (Pango format).
 -d, --dream=FILE	Display an image instead of text.
 -l, --left		Make the bubble appear to the left of cow.
     --think		Display a thought bubble rather than a speech bubble.
     --daemon		Run xcowsay in daemon mode.
     --cow-size=SIZE	Size of the cow (small, med, large).
     --image=FILE	Use a different image instead of the cow.
     --monitor=N	Display cow on monitor N.
     --at=X,Y		Force the cow to appear at screen location (X,Y).
     --bubble-at=X,Y	Change relative position of bubble.
     --no-wrap		Disable wrapping if text cannot fit on screen.
     --config=FILE	Specify alternative config file.
     --debug		Keep daemon attached to terminal.
     --release		Close window on release event instead of press.
Default values for these options can be specified in the xcowsay config
file.  See the man page for more information.
If the display_time option is not set the display time will be calcuated
from the reading_speed parameter multiplied by the word count.  Set the
display time to zero to display the cow until it is clicked on.
Report bugs to nick@nickg.me.uk
```
### Fortune

Xcowsay supports the Fortune package.

First, edit your use flags for fortune-mod:

**`/etc/portage/repos.conf/tde.conf`**

Then, emerge fortune:

`root #``emerge --ask games-misc/fortune-mod`
Then, use the following command to have xcowsay write a fortune.

`user $``xcowfortune`
