<!-- source: https://wiki.gentoo.org/wiki/PyCharm_Community_Edition | group: Gentoo Wiki (Main) | wiki-title: PyCharm Community Edition -->
---
title: PyCharm Community Edition
url: https://wiki.gentoo.org/wiki/PyCharm_Community_Edition
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-23"
fingerprint: "50171faae5aef050"
license: CC BY-SA 4.0
---

# PyCharm Community Edition

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**PyCharm Community Edition** is an open source, single-language integrated developer environment (IDE) for [Python](https://wiki.gentoo.org/wiki/Python) projects created by JetBrains. PyCharm features syntax and error highlighting, auto-indentation, code completion, spell check, and a built-in Python debugger.

## Installation

### USE flags


### Emerge

Install PyCharm:

`root #``emerge --ask dev-util/pycharm-community`
## Troubleshooting

### Partition is mounted with no exec

The IDE cannot execute a test script in the directory. Possible reason: the partition is mounted with 'no exec' option.

`user $``ln -s /tmp /home/pych/.cache/JetBrains/PyCharmCE2023.1/tmp`
### No JDK found

If getting the following error message:

That means the `JDK` environment variables are not set properly.

If using `bash`:

**`~/.bashrc`**

If using `fish`:

**`~/.config/fish/conf.d/pycharm.fish`**

## See also

- [Vim](https://wiki.gentoo.org/wiki/Vim) — a [vi](https://wiki.gentoo.org/wiki/Vi)-like [text editor](https://wiki.gentoo.org/wiki/Text_editor), originally descended from the [Stevie](<https://en.wikipedia.org/wiki/Stevie_(text_editor)>) vi clone.
- [Emacs](https://wiki.gentoo.org/wiki/Emacs) — a class of powerful, extensible, self-documenting text editors.
