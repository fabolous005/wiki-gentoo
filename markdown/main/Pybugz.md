<!-- source: https://wiki.gentoo.org/wiki/Pybugz | group: Gentoo Wiki (Main) | wiki-title: Pybugz -->
---
title: Pybugz
url: https://wiki.gentoo.org/wiki/Pybugz
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-13"
fingerprint: "6c184e9348c6c6b0"
license: CC BY-SA 4.0
---

# Pybugz

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Pybugz is a command line interface to [Gentoo Bugzilla](https://bugs.gentoo.org/).
It is geared towards Bugzilla users that find the web interface inefficient.

It's features include:

- Searching bugzilla
- Listing details of a bug including comments and attachments
- Downloading/viewing attachments
- Posting bugs & comments
- Making changes to existing bugs
- Adding attachments to bugs

## Installation

### Emerge

Install the [www-client/pybugz](https://packages.gentoo.org/packages/www-client/pybugz) package:

`root #``emerge --ask www-client/pybugz`
## Documentation

Please read man 5 pybugz.d for configuring pybugz. The [README on GitHub](https://github.com/williamh/pybugz/blob/master/README) has general informations. Help about using bugz is available with:

`user $``bugz <subcommand> -h`
The subcommands are: attach, attachment, connections, get, modify, post, search.

## Easy configuration for bugs.gentoo.org

pybugz comes with a predefined profile for bugs.gentoo.org except for a required API key. Create an API key in the web interface and write it to the suitable place(s).

![screenshot from bugzilla.gentoo.org helping to navigate to the API Keys tab](https://wiki.gentoo.org/images/thumb/3/31/Bugz.g.o-preferences-numbered.png/300px-Bugz.g.o-preferences-numbered.png)

- Visit the [API keys tab](https://bugs.gentoo.org/userprefs.cgi?tab=apikey) and login if prompted
- Generate a new key and write an optional description
- Edit \~/.bugzrc and add:

**`~/.bugzrc`**

```
[Gentoo]
key=<api key>
```
- Do this for every user, e.g. root, \<user>

Check if everything works with:

![bugzilla.gentoo.org screenshot assisting in generating a new API key.](https://wiki.gentoo.org/images/thumb/3/32/Bugz.g.o-api-key-numbered.png/300px-Bugz.g.o-api-key-numbered.png)

![bugzilla.gentoo.org screenshot showing the generated key](https://wiki.gentoo.org/images/thumb/5/53/Bugz.g.o-api-key-generated-numbered.png/300px-Bugz.g.o-api-key-generated-numbered.png)

`user $``bugz search arm64`
## Examples

Attach plain text file `$log` to bug nr 12345:

`user $``bugz attach -c "text/plain" -d "build log" 12345 $log`
Search for version bump bugs assigned to `maintainer-needed`:

`user $``bugz search "version bump" --assigned-to maintainer-needed`
Get more info about a bug nr 12345:

`user $``bugz get 12345`
Mark bug 12345 as fixed and send a custom message:

`user $``bugz modify 12345 --fixed -c "This bug was closed in #<commit_hash>"`
Mark bug 12345 as invalid and send a custom message

`user $``bugz modify 12345 --invalid -c "Not reproducable"`
