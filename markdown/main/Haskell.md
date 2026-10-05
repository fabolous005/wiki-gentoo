<!-- source: https://wiki.gentoo.org/wiki/Haskell | group: Gentoo Wiki (Main) | wiki-title: Haskell -->
---
title: Haskell
url: https://wiki.gentoo.org/wiki/Haskell
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-22"
fingerprint: "6b99876e1fac96cb"
license: CC BY-SA 4.0
---

# Haskell

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Haskell** is a purely-functional programming language.

## Getting started

First, put the following entries into /etc/portage/package.accept\_keywords:

**`/etc/portage/package.accept_keywords`**

```
# Haskell has no stable keywords in Gentoo
dev-haskell/*
dev-lang/ghc
# Only needed if using ::haskell, but harmless if not.
# May as well put it in, just in case for future use.
*/*::haskell
```
If interested in doing Haskell development, or if the needed packages required are not in the main [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), please enable and configure the [haskell](https://repos.gentoo.org/#haskell) repository. The Haskell repository (::haskell) has its own [instructions](https://github.com/gentoo-haskell/gentoo-haskell/blob/master/README.rst) too. The most important part is that this repository requires a specific unmasking procedure to prevent blockers.

Install git to fetch the overlay and eselect-repository to setup the overlay:

`root #``emerge --ask app-eselect/eselect-repository dev-vcs/git`
Add the overlay:

`root #````
eselect repository enable haskell
```
Update system:

`root #````
emerge --sync
```
`root #````
emerge -uvDU @world
```
## Compiler and interpreter

The most important and up-to-date Haskell-implementation is the [**Glasgow Haskell Compiler**](https://www.haskell.org/ghc/) (GHC). Install it with:

`root #``emerge --ask dev-lang/ghc`
The package also includes an interpreter called GHCI (except on the ARM architecture).

Furthermore, there's [**Hugs**](https://www.haskell.org/hugs/), an (now outdated) interpreter for Haskell98. The ecosystem has moved on with both a newer Haskell specification being published (Haskell 2010) and packages often relying on GHC extensions to Haskell. However, Hugs can still be fun to play with. Install it with:

`root #``emerge --ask dev-lang/hugs98`
## cabal tool

With [**cabal**](https://www.haskell.org/cabal/) tool, it is possible to package and build libraries and programs. Install it with:

`root #``emerge --ask dev-haskell/cabal-install`
## Updating Haskell packages

Sometimes:

`root #``emerge -auvDN --keep-going @world`
has trouble figuring out how to update Haskell packages. Providing emerge with the full list of dev-haskell packages that have upgrades available can sometimes help:

`root #``eix-update``root #``` emerge -av --oneshot --keep-going `eix --only-names --upgrade -C dev-haskell` ```root #``haskell-updater`
Sometimes, if there are sub-slot blockers (when updating ghc or some specific package there are a list of blockers), this issue could be solved via running:

`root #``haskell-updater --all -- =dev-lang/ghc-`*<latest.version>*
## Hoogle with local installation

The Hoogle ebuild is currently only available in the official *gentoo-haskell* [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository). So add that first.

`root #````
eselect repository enable haskell
```
`root #``emerge --sync haskell`
In order to get the an offline installation of all hoogle data, enable the `doc`, `hscolour`, and `hoogle` USE flag values.

`root #``echo "dev-haskell/* doc hoogle hscolour" >> /etc/portage/package.use``root #``emerge --ask dev-util/hoogle`
After emerging haskell packages, the hoogle database of the locally installed packages is updated by running:

`user $``hoogle generate --local`
At this point, Hoogle should work from the command-line. For example, the following will search for the `splitOn` function:

`user $``$ hoogle splitOn`
Data.Text.Lazy splitOn :: Text -> Text -> \[Text\]
Data.Text splitOn :: Text -> Text -> \[Text\]
Data.List.Extra splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Extra splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split.Internals splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split.Internals splitOneOf :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split splitOneOf :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]

### Integration with GHCi

To use Hoogle within GHCi, make a little modification to local \~/.ghci file. Add the following:

**`~/.ghci`**

```
-- Surround a string in single quotes.
let single_quote s = concat ["'", s, "'"]
-- Escape a single quote in the shell. (This mess actually works.)
let escape_single_quote c = if c == '\'' then "'\"'\"'" else [c]
-- Simple heuristic to escape shell command arguments.
let simple_shell_escape = single_quote . (concatMap escape_single_quote)
:def hoogle \x -> return $ ":!hoogle --color " ++ (simple_shell_escape x)
:def doc \x -> return $ ":!hoogle --info --color " ++ (simple_shell_escape x)
```
Now, within GHCi, there should be access to two new commands, `:hoogle` and `:doc`. The first will perform a normal Hoogle search and print the output:

`user $``ghci`
ghci> :hoogle splitOn
Searching for: splitOn
Data.Text.Lazy splitOn :: Text -> Text -> \[Text\]
Data.Text splitOn :: Text -> Text -> \[Text\]
Data.List.Extra splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Extra splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split.Internals splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split splitOn :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split.Internals splitOneOf :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]
Data.List.Split splitOneOf :: Eq a => \[a\] -> \[a\] -> \[\[a\]\]

The second will display the Haddock documentation for the method:

`user $``ghci`
ghci> :doc splitOn
Searching for: splitOn
Data.Text.Lazy splitOn :: Text -> Text -> \[Text\]
O(m+n) Break a Text into pieces separated by the first Text argument (which cannot be an empty string), consuming the delimiter. An empty delimiter is invalid, and will cause an error to be raised.
Examples:
> splitOn "\r\n" "a\r\nb\r\nd\r\ne" == \["a","b","d","e"\]
> splitOn "aaa"  "aaaXaaaXaaaXaaa"  == \["","X","X","X",""\]
> splitOn "x"    "x"                == \["",""\]
and
> intercalate s . splitOn s         == id
> splitOn (singleton c)             == split (==c)
(Note: the string s to split on above cannot be empty.)
This function is strict in its first argument, and lazy in its second.
In (unlikely) bad cases, this function's time complexity degrades towards O(n\*m). 
From package text
splitOn :: Text -> Text -> \[Text\]

## HLint

[**HLint**](http://community.haskell.org/~ndm/hlint/) checks and simplifies the Haskell source code! Install it with:

`root #````
eselect repository enable haskell
```
`root #``emerge --sync haskell``root #``emerge --ask dev-haskell/hlint`
## Editor plugins

### Emacs Haskell Mode

The [Haskell Mode for Emacs](https://haskell.github.io/haskell-mode/) improves the experience of developing and debugging Haskell programs in Emacs. It can be installed with:

`root #``emerge --ask app-emacs/haskell-mode`
Now it can be configured with `M-x customize-group RET haskell RET`.

### Haskell-Mode for Vim

[There](http://projects.haskell.org/haskellmode-vim/)'s also a Haskell-Mode for [Vim](https://wiki.gentoo.org/wiki/Vim).

## Troubleshooting

### Haskell ebuilds failing with out of memory error

When [MAKEOPTS](https://wiki.gentoo.org/wiki//etc/portage/make.conf#MAKEOPTS) is set to allow parallel jobs, ghc may fail in Haskell ebuilds with `ghc: failed to create OS thread: Cannot allocate memory`. To fix this, lower the amount of jobs set in `MAKEOPTS`, or do not allow parallel jobs at all. `MAKEOPTS` can be overridden for failing ebuilds as described in [Overriding environment variables per package](https://wiki.gentoo.org/wiki/Knowledge_Base:Overriding_environment_variables_per_package).

## External resources

- The [#haskell](ircs://irc.libera.chat/#haskell) ([webchat](https://web.libera.chat/#haskell)) and [#gentoo-haskell](ircs://irc.libera.chat/#gentoo-haskell) ([webchat](https://web.libera.chat/#gentoo-haskell)) channels on irc.libera.chat.
