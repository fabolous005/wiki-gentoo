<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/Triggers | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/Triggers -->
---
title: Future EAPI/Triggers
url: https://wiki.gentoo.org/wiki/Future_EAPI/Triggers
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-10-28"
fingerprint: ce6dba55c2336220
license: CC BY-SA 4.0
---

# Future EAPI/Triggers

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Triggers** generally refer to various actions related to one package that are triggered by some event caused by another package. Examples of triggers in other distributions are [DpkgTriggers](https://wiki.debian.org/DpkgTriggers) in Debian and [alpm-hooks](https://www.archlinux.org/pacman/alpm-hooks.5.html) in Arch Linux. While those distributions support different kinds of triggers, the type most relevant to Gentoo are file triggers.

A **file trigger** gets activated when a package installs a file matching specific criteria. For example, if a package installs icons, a trigger can update the GTK icon cache.

## Current state in Gentoo

A trigger-like functionality is currently provided by a few eclasses in Gentoo. There are two ways of implementing this. Either:

- The developer can explicitly call the relevant cache update functions in `pkg_postinst` and `pkg_postrm` when they know that the package installs the specific files.
- The eclass can scan the installation image in `pkg_preinst`, and afterwards call the cache update in `pkg_postinst` and `pkg_postrm`.

The second method is similar to triggers, except they are manually implemented by eclass code.

## Current trigger candidates

The following table lists candidates for explicit triggers. Whenever possible, please provide as much detail as possible as to the conditions triggering them.

| Path | Filenames | Conditions | preinst | prerm | postrm | postinst | 
|---|---|---|---|---|---|---|
| [sys-apps/portage](https://packages.gentoo.org/packages/sys-apps/portage) builtin actions |  |  |  |  |  |  | 
| (unconditional after every package?) |  |  | - | - | env-update | env-update | 
| /usr/share/info | ? | ? | - | - | some high magic involving install-info |  | 
| xdg-utils.eclass (or automatic in xdg.eclass) |  |  |  |  |  |  | 
| /usr/share/applications | \*.desktop | if MimeType= is specified | - | - | xdg\_desktop\_database\_update | xdg\_desktop\_database\_update | 
| /usr/share/mime | \*.xml |  | - | - | xdg\_mimeinfo\_database\_update | xdg\_mimeinfo\_database\_update | 
| gnome2-utils.eclass (or automatic in gnome2.eclass) |  |  |  |  |  |  | 
| /etc/gconf/schemas/ | \*.schemas |  | gnome2\_gconf\_savelist | - | - | gnome2\_gconf\_install | 
| /usr/share/icons | \*.png \*.svg \*.xpm \*.icon |  | - | - | gnome2\_icon\_cache\_update | gnome2\_icon\_cache\_update | 
| /usr/share/glib-2.0/schemas | \*.gschema.xml |  | - | - | gnome2\_schemas\_update | gnome2\_schemas\_update | 
| /usr/share/omf | \*.omf |  | gnome2\_scrollkeeper\_savelist | - | gnome2\_scrollkeeper\_update | gnome2\_scrollkeeper\_update | 
| /usr/lib\*/gdk-pixbuf-2.0 | \*.so |  | gnome2\_gdk\_pixbuf\_savelist | - | - | gnome2\_gdk\_pixbuf\_update | 
| /usr/lib\*/gio/modules | \*.so |  | - | - | gnome2\_giomodule\_cache\_update | gnome2\_giomodule\_cache\_update | 
| elisp-common.eclass (or automatic in elisp.eclass) |  |  |  |  |  |  | 
| /usr/share/emacs/site-lisp/site-gentoo.d | \[0-9\]\[0-9\]\*.el |  | - | - | elisp-site-regen | elisp-site-regen | 

## Draft triggers design by mgorny

This is draft design by [Michał Górny](https://wiki.gentoo.org/wiki/User:MGorny) ([talk](https://wiki.gentoo.org/wiki/User_talk:MGorny)). It's subject to change. It tries to present the most complete possible form, with the assumption that we can strip extraneous stuff to reduce the complexity as necessary.

### metadata/triggers.desc

Trigger conditions are described in metadata/triggers.desc. The file uses the same whitespace-separated list format as profiles.desc. Example:

**`metadata/triggers.desc`**

```
# top-directory               filename-pattern   grep-condition   phase  exec-mode      trigger-name
/usr/share/applications       *.xml              ^MimeType=       *      after-package  xdg_desktop_database
/usr/share/icons              *.png              -                *      after-package  gtk_icon_cache
/usr/share/icons              *.svg              -                *      after-package  gtk_icon_cache
/usr/lib64/gdk-pixbuf-2.0     *                  -                *      after-package  gtk_pixbuf_cache
/usr/lib32/gdk-pixbuf-2.0     *                  -                *      after-package  gtk_pixbuf_cache
```
**top-directory**

- The top directory which is scanned *recursively* for files matching the trigger rules. For simplicity, we could avoid allowing patterns here. However, if we don't want to reiterate over all possible libdirs, we should probably allow patterns but then we need proper pattern handling and that might be PITA in Python.

**filename-pattern**

- Pattern against which files found in *top-directory* are matched. Repeating for multiple suffixes shouldn't be a problem as that's rather rare.

**grep-condition**

- Either a grep-compatible basic regexp against which files matching the previous two conditions are tested, or `-`.

Those are pretty much equivalent to:

```
find ${top_directory} -name ${filename_pattern} ${grep_condition:+-exec grep -l ${grep_condition} + {}}
```
run in `pkg_preinst`. The PM scans installed files for matches, and queues the triggers appropriately.

**phase**

- Running this ebuild phase triggers the trigger. Can be either `postinst` (triggered on install & upgrade), `postrm` (trigger on uninstall & upgrade) or `*` for both.

**exec-mode**

- Specifies when/how often the trigger needs to run. Can be either `after-package` to run after every package installing matching files, or `after-all` to run once after all packages have been processed (on exit).

**trigger-name**

- Name of the trigger to run.

Each trigger is run at most once per *exec-mode*, i.e. multiple matching entries do not trigger multiple runs. Triggers are unaware of files triggering them.

### metadata/triggers/\*.trigger

Those are the actual trigger code files. Each file is named ${name}.trigger.

**`xdg_desktop_database.trigger`**

```
xdg_desktop_database_trigger() {
  if type -P update-desktop-database &>/dev/null; then
    update-desktop-database -q "${EROOT}"/usr/share/applications || die
  else
    # remove stale file if present
    rm -f "${EROOT}"/usr/share/applications/mimeinfo.cache || die
  fi
}
```
The trigger defines a function named by itself so that we can easily include them in saved environment for `postrm`.
