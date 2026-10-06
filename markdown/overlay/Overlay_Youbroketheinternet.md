<!-- source: https://wiki.gentoo.org/wiki/Overlay:Youbroketheinternet | group: Gentoo Overlay | wiki-title: Overlay:Youbroketheinternet -->
---
title: Overlay:youbroketheinternet
url: https://wiki.gentoo.org/wiki/Overlay:Youbroketheinternet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-16"
fingerprint: f93996df5b7637eb
license: CC BY-SA 4.0
---

# Overlay:youbroketheinternet

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The youbroketheinternet overlay (*short: ybti*) is a collaborative maintained Gentoo developer overlay.  
Some of us are involved with [SecuShare](http://secushare.org), but we try out all sorts of XKEYSCORE-resistant technology.

## Access URLs

You may find our [Gentoo overlay](http://youbroketheinternet.org/#overlay) accessible at one of the following URLs:

- git://cheettyiapsyciew.onion/youbroketheinternet-overlay (primary)
- https://gnunet.org/git/youbroketheinternet-overlay.git
- git://gnunet.org/youbroketheinternet-overlay.git

We are working on making our developer overlay accessible via [GNUnet](https://gnunet.org)

## Using the overlay

### eselect repository

Add the overlay:

`root #````
eselect repository enable youbroketheinternet
```
`root #``emerge --sync youbroketheinternet`
## Statement from the README

FILE **`README`**

```
We produce original ebuilds for some items but we also borrow and  
build from other sources, then republish them via a secure authen-
tication medium, a Tor onion. Since all of git, rsync and https  
protocols can be man in the middled, using a self-authenticating  
onion is the way to go. Makes you nervous? Why.. this is not about  
anonymity. All the known problems related to onion services are  
about de-anonymization. There are no known cases of authenticity  
failure, which is what we expect from this. So we copy other  
people's ebuilds because we don't want to integrate insecure  
overlays into our operating system build procedures. So, if your  
ebuild is in here, it's an honor for us to be hosting a copy.  
Please drop by
            irc://loupsycedyglgamf.onion:67/welcome
         or http://loupsycedyglgamf.onion/PSYC/
         or torify telnet loupsycedyglgamf.onion
for feedback and 'git pull' offers.
Git was intended for everyone to run their own little git server  
and pull from each other. Git was NOT invented for centralized  
commercial social networking clouds such as github! If you want  
to submit a patch to this overlay, pass it in form of a textual  
patch or make your copy of this git available on your own onion.  
Github is not a safe infrastructure for a GNU Internet.
```
