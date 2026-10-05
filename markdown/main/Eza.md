<!-- source: https://wiki.gentoo.org/wiki/Eza | group: Gentoo Wiki (Main) | wiki-title: Eza -->
---
title: eza
url: https://wiki.gentoo.org/wiki/Eza
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-28"
fingerprint: f690111a97e6c663
license: CC BY-SA 4.0
---

# eza

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**eza** is a modern ls clone written in Rust that uses colorful output to distinguish file types and metadata.

## Installation

### USE flags


### Emerge

`root #``emerge --ask sys-apps/eza`
## Configuration

### Replacing ls

For more comfortable usage, the shell can be configured - e.g. via \~/.zshrc or \~/.bashrc - to make `eza` an alias for `ls`:

**`~/.zshrc`**

**Replace ls with eza**

Passing flags to eza is not strictly required,since it uses the same flags as ls. For instance, when `ls -la` is entered, it will be transformed to `eza -la` by the shell. However, it's possible to e.g. specify flags such as `--icons` and `--color=always` for more graphical output:

**`~/.zshrc`**

**Replace ls with eza**

### Colors

In order to customize colors of eza's output, the theme.yml file can be created in the \~/.config/eza directory.

First, create the directory:

`user $``mkdir -p ~/.config/eza`
Then, create a theme.yml file in that directory:

`user $``touch ~/.config/eza/theme.yml`
Finally, use a text editor to modify the created theme.yml configuration file, e.g.:

`user $``nano ~/.config/eza/theme.yml`
Refer to [the eza\_colors-explanation(5) man page](https://github.com/eza-community/eza/blob/main/man/eza_colors-explanation.5.md) for details. Additionally, refer to [the theme.yml file in the sources](https://github.com/eza-community/eza/blob/main/docs/theme.yml) for the default configuration, and to the [the eza-themes repository](https://github.com/eza-community/eza-themes?tab=readme-ov-file) for official eza themes.

## Usage

### Invocation

`user $``eza --help````
Usage:
  eza [options] [files...]
META OPTIONS
  --help                     show list of command-line options
  -v, --version              show version of eza
DISPLAY OPTIONS
  -1, --oneline              display one entry per line
  -l, --long                 display extended file metadata as a table
  -G, --grid                 display entries as a grid (default)
  -x, --across               sort the grid across, rather than downwards
  -R, --recurse              recurse into directories
  -T, --tree                 recurse into directories as a tree
  -X, --dereference          dereference symbolic links when displaying information
  -F, --classify=WHEN        display type indicator by file names (always, auto, never)
  --colo[u]r=WHEN            when to use terminal colours (always, auto, never)
  --colo[u]r-scale           highlight levels of 'field' distinctly(all, age, size)
  --colo[u]r-scale-mode      use gradient or fixed colors in --color-scale (fixed, gradient)
  --icons=WHEN               when to display icons (always, auto, never)
  --no-quotes                don't quote file names with spaces
  --hyperlink                display entries as hyperlinks
  --absolute                 display entries with their absolute path (on, follow, off)
  -w, --width COLS           set screen width in columns
FILTERING AND SORTING OPTIONS
  -a, --all                  show hidden and 'dot' files. Use this twice to also
                             show the '.' and '..' directories
  -A, --almost-all           equivalent to --all; included for compatibility with `ls -A`
  -d, --list-dirs            list directories as files; don't list their contents
  -L, --level DEPTH          limit the depth of recursion
  -r, --reverse              reverse the sort order
  -s, --sort SORT_FIELD      which field to sort by
  --group-directories-first  list directories before other files
  -D, --only-dirs            list only directories
  -f, --only-files           list only files
  -I, --ignore-glob GLOBS    glob patterns (pipe-separated) of files to ignore
  --git-ignore               ignore files mentioned in '.gitignore'
  Valid sort fields:         name, Name, extension, Extension, size, type,
                             modified, accessed, created, inode, and none.
                             date, time, old, and new all refer to modified.
LONG VIEW OPTIONS
  -b, --binary               list file sizes with binary prefixes
  -B, --bytes                list file sizes in bytes, without any prefixes
  -g, --group                list each file's group
  --smart-group              only show group if it has a different name from owner
  -h, --header               add a header row to each column
  -H, --links                list each file's number of hard links
  -i, --inode                list each file's inode number
  -m, --modified             use the modified timestamp field
  -M, --mounts               show mount details (Linux and Mac only)
  -n, --numeric              list numeric user and group IDs
  -O, --flags                list file flags (Mac, BSD, and Windows only)
  -S, --blocksize            show size of allocated file system blocks
  -t, --time FIELD           which timestamp field to list (modified, accessed, created)
  -u, --accessed             use the accessed timestamp field
  -U, --created              use the created timestamp field
  --changed                  use the changed timestamp field
  --time-style               how to format timestamps (default, iso, long-iso,
                             full-iso, relative, or a custom style '+<FORMAT>'
                             like '+%Y-%m-%d %H:%M')
  --total-size               show the size of a directory as the size of all
                             files and directories inside (unix only)
  --no-permissions           suppress the permissions field
  -o, --octal-permissions    list each file's permission in octal format
  --no-filesize              suppress the filesize field
  --no-user                  suppress the user field
  --no-time                  suppress the time field
  --stdin                    read file names from stdin, one per line or other separator 
                             specified in environment
  --git                      list each file's Git status, if tracked or ignored
  --no-git                   suppress Git status (always overrides --git,
                             --git-repos, --git-repos-no-status)
  --git-repos                list root of git-tree status
  -@, --extended             list each file's extended attributes and sizes
  -Z, --context              list each file's security context
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-apps/eza`
