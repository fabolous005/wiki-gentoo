<!-- source: https://wiki.gentoo.org/wiki/Localization/Guide | group: Gentoo Wiki (Main) | wiki-title: Localization/Guide -->
---
title: Localization/Guide
url: https://wiki.gentoo.org/wiki/Localization/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-23"
fingerprint: "9e0df5ce2d011b92"
license: CC BY-SA 4.0
---

# Localization/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Time zone

In order to keep the system time properly according to the present location, the timezone needs to be set.  Instructions how to do this for [OpenRC based systems](https://wiki.gentoo.org/wiki/System_time#OpenRC) and [systemd based systems](https://wiki.gentoo.org/wiki/System_time#systemd) can be found in the [system time](https://wiki.gentoo.org/wiki/System_time) article.

## Locale system

### What are locales?

A locale is a set of information that most programs use for determining country and language specific settings. The locales and their data are part of the system library and can be found under the /usr/share/i18n/locales/ directory on most systems. A locale name is generally named `ab_CD` where `ab` is the two (or three) letter language code (as specified in [ISO-639](https://en.wikipedia.org/wiki/ISO_639)) and `CD` is the two letter country code (as specified in [ISO-3166](https://en.wikipedia.org/wiki/ISO_3166)). Variants like `@euro` or `@latin` are often appended to locale names, e.g. `de_DE@euro` or `nan_TW@latin`. Please explore [Wikipedia](<https://en.wikipedia.org/wiki/Locale_(computer_software)>) to read more about locales and related articles.

### Environment variables for locales

The variables controlling different aspects of locale settings are given in the table below. All of them take one name of a locale in `ab_CD` format given above.

| Variable name | Explanation | 
|---|---|
| `LANG` | Defines all locale settings at once, while allowing further individual customization via the LC\_\* settings below. | 
| `LC_COLLATE` | Define alphabetical ordering of strings. This affects e.g. output of sorted directory listings. | 
| `LC_CTYPE` | Define the character-handling properties for the system. This determines which characters are seen as alphabetic, numeric, and so on. This also determines the character set used, if applicable. | 
| `LC_MESSAGES` | Programs' localizations stored in /usr/share/locale/ for applications that use a message-based localization scheme (the majority of GNU programs). | 
| `LC_MONETARY` | Defines currency units and formatting of currency-type numeric values. | 
| `LC_NUMERIC` | Defines formatting of numeric values which aren't monetary. Affects things such as thousand separator and decimal separator. | 
| `LC_TIME` | Defines formatting of dates and times. | 
| `LC_PAPER` | Defines default paper size. | 
| `LC_ALL` | Overrides all other settings. | 

Most typically, users only set the `LANG` variable globally.

### Generating specific locales

Most users will probably only use one or maybe two locales on their system. How additional locales can be specified is explained in the /etc/locale.gen file.

The next step is to run locale-gen. It will generate all the locales specified in the /etc/locale.gen file and write them to the locale-archive (/usr/lib/locale/locale-archive).

`root #``locale-gen`
Found 2 locale declarations in '/etc/locale.gen'.
Compiling 3 locales with 3 workers ...
\[1/3\] Compiling locale: C.UTF-8
\[2/3\] Compiling locale: de\_DE.UTF-8
\[3/3\] Compiling locale: en\_US.UTF-8
Waiting for active workers to finish their jobs ...
The location of the archive shall be '/usr/lib/locale/locale-archive'.
Adding 3 locales to the locale archive ...
Successfully installed an archive containing 3 locales, of 2.93 MiB in size.

Verify that the selected locales are available by running locale -a.

`user $``locale -a`
C
C.utf8
en\_US.utf8
POSIX
de\_DE.utf8

The /usr/lib/locale/locale-archive file can be shown by localedef.

`user $``localedef --list-archive`
Its raw content can be displayed using the strings command.

`user $``strings /usr/lib/locale/locale-archive | less`
### Setting a locale

#### OpenRC

When using OpenRC locale settings are stored in environment variables. These are typically set in /etc/env.d/02locale (for system-wide settings) and \~/.bashrc (for user-specific settings).  More details can be found in the [UTF-8](https://wiki.gentoo.org/wiki/UTF-8#Setting_the_locale) article.  The system wide settings (/etc/env.d/02locale) can be managed through eselect locale. For example, to set the preferred locale:

`root #``eselect locale list`
Available targets for the LANG variable:
  \[1\]   C
  \[2\]   C.UTF-8
  \[3\]   en\_US.UTF-8 \*
  \[4\]   POSIX
  \[5\]   de\_DE.UTF-8
  \[ \]   (free form)

`root #``eselect locale set 5`
Of course, editing the file manually is possible as well to diversify the locale variables.

The command above lists the suffix in lower case without any hyphens, glibc understands both forms of the suffix, many other programs don't. The most common example of which is X. So it is best to always use UTF-8 in preference to utf8.

**`/etc/env.d/02locale`**

**Setting the default system locale in /etc/env.d/02locale**

```
LANG="de_DE.UTF-8"
LC_COLLATE="C.UTF-8"
```
In some cases users may notice glitchy non-English representation in some applications like Krusader ([https://bugs.kde.org/show\_bug.cgi?id=371582](https://bugs.kde.org/show_bug.cgi?id=371582)). Removing or commenting the `LC_ALL=""` line from /etc/env.d/02locale should fix the problem.

It's also possible, and pretty common especially in a more traditional UNIX environment, to leave the global settings unchanged, i.e. in the `C` locale. Users can still specify their preferred locale in their own shell configuration file:

**`~/.bashrc`**

**Setting the user locale**

```
export LANG="de_DE.UTF-8"
export LC_COLLATE="C.UTF-8"
```
**`~/.profile`**

**Setting the user locale for X applications**

```
export LANG="de_DE.UTF-8"
export LC_COLLATE="C.UTF-8"
```
Another way of configuring system is to leave it in the default `C` locale, but enable UTF-8 character representation at the same time. This option is achieved using the following settings in /etc/env.d/02locale:

Using the above snippet, users will be able to see localized file names properly, while not being forced to completely use the selected language.

Once the right locale is set up, be sure to update the environment variables to make the system aware of the change.

For a system-wide default locale:

`root #``env-update && source /etc/profile`
For a user-specific locale:

`user $``source ~/.bashrc`
After this, kill the X server by pressing `Ctrl`+`Alt`+`Backspace`, log out, then log in as a user.

Now, verify that the changes have taken effect:

`user $``locale`
The values of locale environment variables that have been explicitly set e.g. in an export statement (if using bash) are listed without double quotes. Those whose value has been inherited from other locale environment variables have their values in double quotes.

#### systemd

With systemd set the locale with the localectl command. Check the list of available locales with:

`root #``localectl list-locales`
Then set the desired locale:

`root #``localectl set-locale LANG=de_DE.utf8`
Finally check if the result is good:

`root #``localectl | grep "System Locale"`
System Locale: LANG=de\_DE.utf8

## Keyboard layout for the console

### OpenRC

The keyboard layout used by the console is set in /etc/conf.d/keymaps by the `keymap` variable. Valid values can be found in /usr/share/keymaps/YOUR\_ARCH/. i386 has further subdivisions into layout (qwerty/, azerty/, etc.). Some languages have multiple options - experiment with the various options to decide which one fits your needs best.

**`/etc/conf.d/keymaps`**

**Setting the console keymap**

```
keymap="de"
#keymap="de-latin1"
#keymap="de-latin1-nodeadkeys"
```
### systemd

With systemd the keymap layout used for the console can be set using the localectl command. First check the available keymap layouts:

`root #``localectl list-keymaps`
Then set the requested console keymap layout:

`root #``localectl set-keymap it`
Finally check if the console keymap layout was set correctly:

`root #``localectl | grep "VC Keymap"`
VC Keymap: it

## Keyboard layout for the X server

### OpenRC

The keyboard layout to be used by the X server is specified in /etc/X11/xorg.conf.d/30-keyboard.conf by the `XkbLayout` option. For details visit the [Xorg guide](https://wiki.gentoo.org/wiki/Xorg/Guide#Configuring_the_keyboard) and the article about [Keyboard layout switching](https://wiki.gentoo.org/wiki/Keyboard_layout_switching#X11).

### systemd

With systemd the keymap layout for the X11 server can be set using the localectl command. First check the available X11 keymap layouts:

`root #``localectl list-x11-keymap-layouts`
Then set the requested X11 keymap layout:

`root #``localectl set-x11-keymap it`
Finally check if the X11 keymap layout was set correctly:

`root #``localectl | grep "X11 Layout"`
X11 Layout: it

## Native Language Support

For message based localization to work in programs that support it and have the `nls` [USE flag](https://packages.gentoo.org/useflags/nls), compile the programs with this flag set. Message strings are installed in /usr/share/locale/\<locale>/LC\_MESSAGES/\<package>.mo files. Most of the programs using the *Native Language Support (NLS)* also need the gettext library to extract and use localized messages. Of course, Portage will automatically install it when needed.

After enabling the `nls` USE flag some packages might need to be re-emerged:

`root #``emerge --ask --changed-use --deep --with-bdeps=y @world`
## LINGUAS

There is also an additional `LINGUAS` variable that is used by some gettext-based build systems to control which localization files are built and installed. The variable takes in *space*-separated list of language codes, and a suggested place to set it is [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf):

`root #``nano -w /etc/portage/package.use/00-localization`
\## (Add in the LINGUAS variable. For instance, for German, Finnish and English:)
\*/\* LINGUAS: de fi en

Note that there is a large difference between `LINGUAS` being unset and being set to an empty value: Unset `LINGUAS` means to install all available languages. By contrast, with `LINGUAS=""`, most ebuilds would install only the packages' default language but none of the `LC_MESSAGES` files.

## L10N

A `USE_EXPAND` variable called `L10N` decides which extra localization support will be installed. This is commonly used for downloads of additional language packs by packages. Similar to `LINGUAS`, the variable takes a space separated list of language tags, and it can be set in /etc/portage/ global USE file:

`root #``nano -w /etc/portage/package.use/localization`
\## (Add in the L10N variable. For instance, for German and Brazilian Portuguese:)
\*/\* L10N: de pt-BR

To set it per-package, edit [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) and prefix the requested language packs with "l10n\_", as shown in the next example:

**`/etc/portage/package.use`**

```
 l10n_de l10n_pt-BR
```
Note that while the common two letter language codes (like `de` or `fr`) are identical in `LINGUAS` and `L10N`, more complex entries have a different syntax because `L10N` uses [IETF language tags](https://www.w3.org/International/articles/language-tags/) (aka BCP 47). For example, `pt_BR` and `sr@latin` in `LINGUAS` become `pt-BR` and `sr-Latn` in `L10N`, respectively.

A list of `L10N` values that can be used is provided as /var/db/repos/gentoo/profiles/desc/l10n.desc:

`user $``grep -i portuguese /var/db/repos/gentoo/profiles/desc/l10n.desc`
pt - Portuguese
pt-BR - Portuguese (Brazil)
pt-PT - Portuguese (Portugal)

After setting the `L10N USE_EXPAND` variable it may be necessary to re-emerge some packages:

`root #``emerge --ask --changed-use --deep --with-bdeps=y @world`
## See also

- [Configuring locales](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Configure_locales) (Gentoo Handbook)
- [Musl#Locales](https://wiki.gentoo.org/wiki/Musl#Locales)
- [Keyboard layout](https://wiki.gentoo.org/wiki/Evdev#Keyboard_layout) inside the Evdev article
- [X resources](https://wiki.gentoo.org/wiki/X_resources)
- [Localization/Guide/The Euro symbol](https://wiki.gentoo.org/wiki/Localization/Guide/The_Euro_symbol) — how to display the Euro symbol (€) for the console and in X.
