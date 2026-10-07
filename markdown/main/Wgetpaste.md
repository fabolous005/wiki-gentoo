<!-- source: https://wiki.gentoo.org/wiki/Wgetpaste | group: Gentoo Wiki (Main) | wiki-title: Wgetpaste -->
---
title: wgetpaste
url: https://wiki.gentoo.org/wiki/Wgetpaste
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-06"
fingerprint: "35a8895d03a79ac1"
license: CC BY-SA 4.0
---

# wgetpaste

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**wgetpaste** is a [command-line](https://wiki.gentoo.org/wiki/Shell) tool for posting snippets of text to various online [pastebin](https://en.wikipedia.org/wiki/pastebin) services.

wgetpaste allows posting text directly to a pastebin from the command line or from a script, returning a link to allow the post to be shared.

See the [service selection](https://wiki.gentoo.org/wiki/Wgetpaste#Service_selection) section for a list of available pastebin services. Visit a service's website for usage-information specific to that service.

The default service, [bpaste](http://bpa.st), is well suited for posting to Gentoo IRC or requesting support. Services that require *javascript* to view the posts can be impractical.

wgetpaste is written in [bash](https://wiki.gentoo.org/wiki/Bash) and only requires [sed](https://wiki.gentoo.org/wiki/Sed) and [wget](https://wiki.gentoo.org/wiki/Wget), so it is very [portable](https://en.wikipedia.org/wiki/Software_portability).

## Installation

### USE flags

wgetpaste currently has just one use flag, for using SSL/TLS or not:


### USE flags for
            [app-text/wgetpaste](https://packages.gentoo.org/packages/app-text/wgetpaste)
            
            Command-line interface to various pastebins

| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 

### Emerge

Install [app-text/wgetpaste](https://packages.gentoo.org/packages/app-text/wgetpaste):

`root #``emerge --ask app-text/wgetpaste`
## Configuration

### Files

- /etc/wgetpaste.conf - Global (system wide) configuration file.
- /etc/wgetpaste.d/ - Global configuration directory, for \*.conf files.
- \~/.wgetpaste.conf - Local (per user) configuration file.
- \~/.wgetpaste.d/ - Local configuration directory, for \*.conf files.

### Example configurations

**`~/.wgetpaste.d/main.conf`**

```
# Always pass pastes through app-text/ansifilter
NOANSI=1
# Give raw links which can immediately be used for patches, etc
RAW=1
# Optionally default to gists
#DEFAULT_SERVICE=gists
# Default gists to secret
PUBLIC_gists='false'
# Provide github gist authorization token
HEADER_gists="Authorization: token XXXX"
```
There is also an [example configuration file](http://wgetpaste.zlin.dk/wgetpaste.example) available from upstream, and an [advanced configuration example](https://wgetpaste.zlin.dk/zlin.conf) showing how to add a new service.

### Github gists

The gists service requires a valid API token. Generate it on the [Github website](https://github.com/settings/tokens) and paste it into a config snippet:

**`~/.wgetpaste.d/gists.conf`**

```
HEADER_gists="Authorization: token abcdef..."
```
A gist **must** be set to public or private by setting the `PUBLIC_gists` variable either in the config file, or on the command-line, thus (for Bash):

`user $``PUBLIC_gists=false wgetpaste -s gists <path-to-file>`
## Usage

### Invocation

Invoke wgetpaste with the `--help` option for useful information on usage:

`user $``wgetpaste --help````
Usage: /usr/bin/wgetpaste [options] [file[s]]
Options:
    -l, --language LANG           set language (defaults to "Plain Text")
    -d, --description DESCRIPTION set description (defaults to "stdin" or filename)
    -n, --nick NICK               set nick (defaults to your username)
    -s, --service SERVICE         set service to use (defaults to "dpaste")
    -e, --expiration EXPIRATION   set when it should expire (defaults to "1")
    -S, --list-services           list supported pastebin services
    -L, --list-languages          list languages supported by the specified service
    -E, --list-expiration         list expiration setting supported by the specified service
    -u, --tinyurl URL             convert input url to tinyurl
    -c, --command COMMAND         paste COMMAND and the output of COMMAND
    -i, --info                    append the output of `emerge --info`
    -I, --info-only               paste the output of `emerge --info` only
    -x, --xcut                    read input from clipboard (requires x11-misc/xclip)
    -X, --xpaste                  write resulting url to the X primary selection buffer (requires x11-misc/xclip)
    -C, --xclippaste              write resulting url to the X clipboard selection buffer (requires x11-misc/xclip)
    -r, --raw                     show url for the raw paste (no syntax highlighting or html)
    -t, --tee                     use tee to show what is being pasted
    -v, --verbose                 show wget stderr output if no url is received
        --completions             emit output suitable for shell completions (only affects --list-*)
        --debug                   be *very* verbose (implies -v)
    -h, --help                    show this help
    -g, --ignore-configs          ignore ""/etc/wgetpaste.conf, ~/.wgetpaste.conf etc.
        --version                 show version information
Defaults (DEFAULT_{NICK,LANGUAGE,EXPIRATION}[_${SERVICE}] and DEFAULT_SERVICE)
can be overridden globally in ""/etc/wgetpaste.conf or ""/etc/wgetpaste.d/*.conf or
per user in any of ~/.wgetpaste.conf or ~/.wgetpaste.d/*.conf.
An additional http header can be passed by setting HEADER_${SERVICE} in any of the
configuration files mentioned above. For example, authenticating with github gist:
HEADER_gists="Authorization: token 1234abc56789...", or with gitlab snippets:
HEADER_snippets="PRIVATE-TOKEN: 1234abc56789..."
You can also set PUBLIC_gists='false' if you want to default to secret instead of
public github gists. In the case of gitlab, you can set VISIBILITY_snippets= to
'public', 'private' or 'internal'"
To change your gitlab server, you can override the default API URL setting
URL_snippets='https://gitlab.[server].com/api/v4/snippets'
```
### Service selection

Before posting a snippet, care should be taken to select the desired service to post to.

To show which is the current default service, and list available services, use the `--list-services` (`-S` for short) option:

`user $``wgetpaste --list-services````
                                                                                                                                                                                                                       
Services supported: (case sensitive):
   Name:     | Url:
   ==========|=================
    0x0      | http://0x0.st
    bpaste   | https://bpa.st/api/v1/paste
    codepad  | http://codepad.org/
    dpaste   | http://dpaste.com/api/v2/
    gists    | https://api.github.com/gists
    ix_io    | http://ix.io
    snippets | https://gitlab.com/api/v4/snippets
   *pgz      | https://paste.gentoo.zip
```
The default service is marked with an asterisk, this service will be used unless a different service is selected with the `--service` (`-s` for short) option, when pasting. For example:

`user $``wgetpaste --service codepad file.txt`
Different paste services have different constraints, such as allowable size, retention period etc. Go to the service website for full information. For larger posts, 0x0 may be useful.

### Posting a file

To post a file, simply run wgetpaste followed by the filename, not forgetting to specify a paste service if something other than the default is required.

For example, run the following command to create a paste of the system's Xorg configuration:

`user $``wgetpaste /etc/X11/xorg.conf`
To create a paste of xorg.conf using the tiny URL service use the `--tinyurl` option (`-u` for short):

`user $``wgetpaste --tinyurl /etc/X11/xorg.conf`
#### Binary files

Most paste services cannot handle binary files, e.g. gzip archives. However, there is a simple core utility to help with that base64.

If a binary file needs to be posted, run this command first to prepare the file and post the result instead:

`user $``base64 hugefile.log.gz > hugefile.log.gz.base64`
After someone receives the paste, the file can be decoded like:

`user $``base64 -d hugefile.log.gz.base64 > hugefile.log.gz`
This encoding will inflate the original size so files on the brink of maximum size may be rejected.

### Post command output

A command's output may be directly posted to a snippet service, though it is recommended to write output to a file and check the contents before pasting, to avoid any possible security issues.

To paste the entire output of a command, use the `--command` (`-c` for short) option. Remember to quote the command:

`user $``wgetpaste --command 'emerge -vp musique'`
Output can also be piped to wgetpaste, but this will only include stdout by default. Use `|&` to include stderr as well:

`user $``{ echo "Hello, stdout!"; echo "Hello, stderr!" >&2; } |& wgetpaste`
### Advanced options

To set a language for syntax highlighting use the `--language` (`-l` for short) option:

`user $``wgetpaste --language Bash /etc/bash/bashrc`
Use the `--list-languages` (`-L` for short) option to list all available languages, these depend on the selected paste service:

`user $``wgetpaste --list-languages`
### Removing ANSI sequences with ansifilter

For getting color on terminals, ANSI sequences are used. When uploading a logfile these can be annoying (in particular it makes it hard to search for error messages), e.g.:

�\[32m \* �\[39;49;00mPackage:    x11-wm/dwm-6.2
�\[32m \* �\[39;49;00mRepository: gentoo
�\[32m \* �\[39;49;00mMaintainer: gyakovlev@gentoo.org
�\[32m \* �\[39;49;00mUSE:        abi\_x86\_64 amd64 elibc\_glibc kernel\_linux savedconfig userland\_GNU xinerama
�\[32m \* �\[39;49;00mFEATURES:   network-sandbox preserve-libs sandbox userpriv usersandbox

[app-text/ansifilter](https://packages.gentoo.org/packages/app-text/ansifilter) removes these control characters. To install:

`root #``emerge --ask app-text/ansifilter`
Filter the build.log and upload with wgetpaste:

`user $``ansifilter /var/tmp/portage/cat/package-1.23/temp/build.log | wgetpaste`
You can also use ansifilter in the middle of pipes. For example:

`user $``ls --color / | ansifilter | wgetpaste`
### Interacting with the clipboard

wgetpaste can read from the system clipboard as content input or write the resulting url to the system clipboard. You need to install [x11-misc/xclip](https://packages.gentoo.org/packages/x11-misc/xclip) as an optional dependency.

`root #``emerge --ask x11-misc/xclip`
Read input from clipboard:

`user $``wgetpaste --xcut`
Your paste can be seen here: https://paste.gentoo.zip/85vuQg0X

Write resulting url to the X clipboard selection buffer:

`user $``wgetpaste -c "emerge --info" --xclippaste`
Your paste can be seen here: https://paste.gentoo.zip/4HnpeZvu

You can also use `--xpaste` to write resulting url to the X primary selection buffer.

## External resources

- [app-text/pastebinit](https://packages.gentoo.org/packages/app-text/pastebinit) - Another similar tool available through Portage.
- [Pastebin](https://en.wikipedia.org/wiki/Pastebin) - Wikipedia article about pastebin services.

## See also

- [Support](https://wiki.gentoo.org/wiki/Support) — provide **support** for technical issues encountered when installing or using Gentoo Linux
- [Troubleshooting](https://wiki.gentoo.org/wiki/Troubleshooting) — provide users with a set of techniques and tools to troubleshoot and fix problems with their Gentoo setups.
