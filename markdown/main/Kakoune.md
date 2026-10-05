<!-- source: https://wiki.gentoo.org/wiki/Kakoune | group: Gentoo Wiki (Main) | wiki-title: Kakoune -->
---
title: Kakoune
url: https://wiki.gentoo.org/wiki/Kakoune
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-22"
fingerprint: f219fd7b951391cd
license: CC BY-SA 4.0
---

# Kakoune

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Kakoune** is a modern, actively developed [editor](https://wiki.gentoo.org/wiki/Text_editor) for the [command line](https://wiki.gentoo.org/wiki/Shell), inspired by [vi](https://wiki.gentoo.org/wiki/Vi).

A modal editor, Kakoune uses keystrokes as a text editing language. Multiple selections allow for sweeping changes with very few commands.

Modal editors generally have a steep learning curve; Kakoune helps level the leaning curve with strong focus on interactivity, showing immediate and incremental results.


## Installation


### Emerge

`root #``emerge --ask app-editors/kakoune`

## Configuration


### System Clipboard Integration

Kakoune can be configured such that yanking within the editor will also synchronize the copied text with the system clipboard. The following snippet uses an OSC52 escape sequence to set the system clipboard via the parent [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator). OSC52 has the advantage of working even when Kakoune is running remotely, such as over [ssh](https://wiki.gentoo.org/wiki/Ssh).

**`~/.config/kak/kakrc`**

**System Clipboard Integration**

```
 global RegisterModified '"' %{
    nop %sh{
        # Concat selections line-by-line
        eval set -- "$kak_quoted_selections"
        reg=$1; shift
        for selection; do
            reg=$(printf '%s\n%s' "$reg" "$selection")
        done
        # Convert to OSC52 and send to client's tty
        client_tty=/proc/$kak_client_pid/fd/0
        encoded=$(printf %s "$reg" | base64 | tr -d '\n')
        printf "\e]52;;%s\e\\" "$encoded" > $client_tty
    }
}
```

## See also

- [Knowledge Base:Edit a configuration file](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file)
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
