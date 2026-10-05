<!-- source: https://wiki.gentoo.org/wiki/Lolcat | group: Gentoo Wiki (Main) | wiki-title: Lolcat -->
---
title: Lolcat
url: https://wiki.gentoo.org/wiki/Lolcat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-11"
fingerprint: e9de02550ca8cf97
license: CC BY-SA 4.0
---

# Lolcat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Lolcat** is a funny terminal colorizer written in [Ruby](https://wiki.gentoo.org/wiki/Ruby).

## Installation

[Accept](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS) the unstable keyword.

**`/etc/portage/package.accept_keywords`**

### Emerge

Install [games-misc/lolcat](https://packages.gentoo.org/packages/games-misc/lolcat):

`root #``emerge --ask games-misc/lolcat`
### Tweaking

It is funny to add this to your \~/.bashrc, if you wish to lolcat all the time:

**`~/.bashrc`**

```
alias cat="lolcat"
```
## Usage

There are many use cases; Here are a few ways to take advantage of it:

`user $````
find /usr/portage -type f -exec lolcat '{}' \;
```
`user $````
find /var/cache/man -type f -exec lolcat '{}' \;
```
`user $````
fortune | cowsay | lolcat
```
`user $````
tcpdump | lolcat
```
`user $````
emerge --info | lolcat
```
`user $````
cat /dev/urandom | base64 -w $COLUMNS | lolcat
```
You can have a nice fortune every time you open a shell with a random cow:

**`~/.bashrc`**

```
[1]="-b"
cow_mode[2]="-d"
cow_mode[3]="" # default
cow_mode[4]="-g"
cow_mode[5]="-p"
cow_mode[6]="-s"
cow_mode[7]="-t"
cow_mode[8]="-w"
cow_mode[9]="-y"
rng=$(( $RANDOM % 9 + 1))
IFS=' '
cowfiles=(`cowsay -l | sed 1d | paste -sd " "`)
num_files=${#cowfiles[*]}
cowfile=${cowfiles[$((RANDOM % num_files))]}
fortune | cowsay -W 35 ${cow_mode[$rng]} -f $cowfile | lolcat
```
