<!-- source: https://wiki.gentoo.org/wiki/Ledger | group: Gentoo Wiki (Main) | wiki-title: Ledger -->
---
title: ledger
url: https://wiki.gentoo.org/wiki/Ledger
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-15"
fingerprint: e2414e74ac8ca852
license: CC BY-SA 4.0
---

# ledger

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**ledger** is a scriptable double-entry accounting system for the command-line. Unlike most other accounting software, ledger eschews opaque binary file formats and embraces a simple but well structured text file as its file format of choice. The file format ledger uses has the advantage of being trivial for both computers and humans to parse. So much so that ledger single-handedly spawned the [Plain Text Accounting](https://plaintextaccounting.org/) movement. This has lead to dozens of clones and support tools that all embrace ledger's file format, the most prominent of which are [hledger](https://wiki.gentoo.org/index.php?title=Hledger&action=edit&redlink=1) ([Haskell](https://wiki.gentoo.org/wiki/Haskell)) and [Beancount](https://wiki.gentoo.org/index.php?title=Beancount&action=edit&redlink=1) ([Python](https://wiki.gentoo.org/wiki/Python)).

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gpg](https://packages.gentoo.org/useflags/gpg) | Enable support for encrypted journals | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 

### Emerge

`root #``emerge --ask app-office/ledger`
### Environment variables

ledger has an extensive list of command-line options detailed in its man page, nearly any one of which can be turned into an environment variable. When an environment variable conflicts with a command-line option, the command-line option takes precedence.

### Files

- \~/.ledgerrc - user specific configuration file.

## Usage

### Invocation

## Troubleshooting

### How do I import a CSV file into ledger?

ledger comes with a *convert* command that readily accepts a CSV file as input. The only issue is that it must be told how to parse and date codes it encounters as there are several standards in use internationally:

ledger convert download.csv --input-date-format "%Y/%m/%d"

### How do I convert files from Quickbooks?

This is not directly supported, but there are third party tools that will do this, such as:

- [outofit](https://github.com/rcaputo/outofit) (Perl)
- [qb2ledger](https://gist.github.com/genegoykhman/3765100) (Ruby)
- [QIFtoLedger](https://github.com/Kolomona/QIFtoLedger) (Ruby)

### How do I import files from GNUCash?

This isn't directly supported either but simple third party scripts can do this, such as:

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-office/ledger`
## See also

- [hledger](https://wiki.gentoo.org/index.php?title=Hledger&action=edit&redlink=1)
- [Beancount](https://wiki.gentoo.org/index.php?title=Beancount&action=edit&redlink=1)
- [bc](https://wiki.gentoo.org/wiki/Bc) — arbitrary-precision fixed-point mathematical scripting language
- [sc-im](https://wiki.gentoo.org/wiki/Sc-im) — a terminal-based spreadsheet and calculator with [vim](https://wiki.gentoo.org/wiki/Vim)-like key bindings
