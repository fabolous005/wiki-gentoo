<!-- source: https://wiki.gentoo.org/wiki/Newsboat | group: Gentoo Wiki (Main) | wiki-title: Newsboat -->
---
title: Newsboat
url: https://wiki.gentoo.org/wiki/Newsboat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-21"
fingerprint: d5b25aae9fd0f0f5
license: CC BY-SA 4.0
---

# Newsboat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Newsboat is an RSS/Atom feed reader for the text console. It is an actively maintained fork of Newsbeuter.

## Installation

### USE flags


### Emerge

`root #``emerge --ask net-news/newsboat`
## Configuration

### Feeds management

Add feed URLs to the \~/.newsboat/urls file with any text editor one per line. This example uses the gentoo.org news feed XML:

**`~/.newsboat/urls`**

**urls**

### Formatting / Naming links

To have names appear instead of the long URL configure the links like this:

**`~/.newsboat/urls`**

**urls**

This statement will show \_better\_name\_for\_url\_here\_ instead of the URL link address in newsboat.

### Tags

It is possible to have various feeds under the same "tag". These can later be filtered by pressing the `t` key inside Newsboat. This example will add the gentoo home page to the `gentoo` tag and the linux kernel homepage to the `kernel` tag:

**`~/.newsboat/urls`**

**urls**

## Usage

### Invocation

`user $``newsboat --help````
Newsboat 2.23
usage: newsboat [-i <file>|-e] [-u <urlfile>] [-c <cachefile>] [-x <command> ...] [-h]
    -e, --export-to-opml            export OPML feed to stdout
    -r, --refresh-on-start          refresh feeds on start
    -i, --import-from-opml=<file>   import OPML file
    -u, --url-file=<urlfile>        read RSS feed URLs from <urlfile>
    -c, --cache-file=<cachefile>    use <cachefile> as cache file
    -C, --config-file=<configfile>  read configuration from <configfile>
    -X, --vacuum                    compact the cache
    -x, --execute=<command>...      execute list of commands
    -q, --quiet                     quiet startup
    -v, --version                   get version information
    -l, --log-level=<loglevel>      write a log with a certain loglevel (valid values: 1 to 6)
    -d, --log-file=<logfile>        use <logfile> as output log file
    -E, --export-to-file=<file>     export list of read articles to <file>
    -I, --import-from-file=<file>   import list of read articles from <file>
    -h, --help                      this help
        --cleanup                   remove unreferenced items from cache
```
## Tips

### Displaying rss feeds as a timeline

It is possible to use Newsboat's [queries](https://newsboat.org/releases/2.21/docs/newsboat.html#_query_feeds) to replicate a timeline-like view of rss feeds. This could be used to replicate YouTube's Subscription feed. This example uses two YouTube channel rss feeds and planet gentoo's.

**`~/.newsboat/urls`**

**urls**

This will create a feed named "Subscriptions" that will display all the entries from feeds belonging to the `timeline` tag (in this example 2 YouTube channels and the planet gentoo entries).
