<!-- source: https://wiki.gentoo.org/wiki/Ripgrep | group: Gentoo Wiki (Main) | wiki-title: Ripgrep -->
---
title: ripgrep
url: https://wiki.gentoo.org/wiki/Ripgrep
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-29"
fingerprint: "1d00b27902a70f03"
license: CC BY-SA 4.0
---

# ripgrep

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ripgrep** is search tool that can recursively search directories for regex search patterns. ripgrep can replicate much of the functionality of the [grep](https://wiki.gentoo.org/wiki/Grep) command, but has a generally wider scope.

## Installation

### USE flags


### Emerge

Install [sys-apps/ripgrep](https://packages.gentoo.org/packages/sys-apps/ripgrep):

`root #``emerge --ask sys-apps/ripgrep`
## Usage

### Invocation

To view ripgrep usage and options:

`user $``rg --help`
### Search for a string in current directory

To recursively search files in the current directory for a string:

`user $``rg <string>`
### Find a string in a file

Use ripgrep to find a string in a file:

`user $``rg <string> <file>`
### Prettified Output for JSON Results

Use this option to provide a more readable format for JSON search results:

`user $``rg --pretty`
### Suppress Error Messages

Suppress all error messages:

`user $``rg --quiet`
### Set Maximum Filesize for Search

Limit the search to files of a specific size:

`user $``rg --max-filesize N`
Example: Limit search to files smaller than 1MB

`user $``rg --max-filesize 1M "pattern"`
### Replace Each Match

Replace each matched instance with a specified string:

`user $``rg --replace REPLACEMENT "pattern"`
Example: Replace "foo" with "bar" in the search results

`user $``rg --replace bar foo`
### Disable Searching Inside Compressed Files

Avoid searching within compressed files:

`user $``rg --no-search-zip`
### Enable Searching Inside Compressed Files

Search within compressed files such as .zip and .gzip:

`user $``rg --search-zip`
### Display Statistics After Search

Show detailed statistics after completing the search:

`user $``rg --stats`
### Search Both Binary and Text Files

Include both binary and text files in the search:

`user $``rg --binary "pattern"`
### List Available File Types

Display a list of all file types recognized by ripgrep:

`user $``rg --type-list`
### Output Results in Vim-Compatible Format

Format the search results in a manner compatible with Vim:

`user $``rg --vimgrep "pattern"`
### Read Ignore Patterns from a File

Exclude patterns specified in a particular file:

`user $``rg --ignore-file FILE_PATH`
Example: Ignore patterns listed in ".rgignore"

`user $``rg --ignore-file .rgignore "pattern"`
### Disable Memory-Mapped I/O

Turn off memory-mapped I/O during file search:

`user $``rg --no-mmap "pattern"`
## File Listing

While **ripgrep** is primarily a text-searching tool, it also offers the capability to list files, similar to the [find](https://wiki.gentoo.org/wiki/Find) command, though with its unique set of advantages.

### Listing Files

Print the files that ripgrep would search, but don't actually search them.

`user $``rg --files /path/to/directory`
### Searching for Specific Filenames

Combining the file listing feature with another ripgrep command allows for matching specific filenames. For instance, to find all filenames with a certain match:

`user $``rg --files /path/to/directory | rg -i <filename_pattern>`
This command first lists all files ripgrep would search in the specified path and then filters them based on the provided filename match.

## Sorting Results

**ripgrep** offers the flexibility to sort search results based on different criteria. The sorting modes available are:

### Sorting in Ascending Order

- Do not sort results. This is the default behavior, offering the fastest performance and the potential for multi-threading.

`user $``rg --sort none`
- Sort results by file path.

`user $``rg --sort path`
- Sort results based on the last modified time of files.

`user $``rg --sort modified`
- Sort results based on the last accessed time of files.

`user $``rg --sort accessed`
- Sort results based on the file creation time.

`user $``rg --sort created`
### Sorting in Descending Order

- Do not sort results (same as the default behavior). Offers the fastest performance with potential multi-threading.

`user $``rg --sortr none`
- Sort results in reverse order by file path.

`user $``rg --sortr path`
- Sort results in reverse order based on the last modified time of files.

`user $``rg --sortr modified`
- Sort results in reverse order based on the last accessed time of files.

`user $``rg --sortr accessed`
- Sort results in reverse order based on the file creation time. This mode is single-threaded.

`user $``rg --sortr created`
## Configuration

**ripgrep** offers customization capabilities through a configuration file that enables the setting of default flags read every time it's invoked. The path to the file must be set using the RIPGREP\_CONFIG\_PATH environment variable. A common convention is to use \~/.ripgreprc, but ripgrep will not look at any predetermined config file path:

**`~/.ripgreprc`**

## Integration with Other Tools

**ripgrep**  integrates seamlessly with various tools to enhance workflow:

- **Vim**: Integration of ripgrep with [Vim](https://wiki.gentoo.org/wiki/Vim) allows for searching patterns across projects. Plugins such as [vim-ripgrep](https://github.com/jremmen/vim-ripgrep) and [fzf.vim](https://github.com/junegunn/fzf.vim) provide this capability.

- **Less**: Combining ripgrep's output with [sys-apps/less](https://packages.gentoo.org/packages/sys-apps/less) offers paginated viewing.

`user $``rg 'pattern' | less`
## Removal

### Unmerge

Remove [sys-apps/ripgrep](https://packages.gentoo.org/packages/sys-apps/ripgrep):

`root #``emerge --ask --depclean --verbose sys-apps/ripgrep`
## Debug

Shows ripgrep's debug output. This is useful for understanding why a particular file might be ignored from search, or what kinds of configuration ripgrep is loading from the environment.

`user $``rg --debug "pattern"`
## See also

- [grep](https://wiki.gentoo.org/wiki/Grep) — a tool for searching text files with regular expressions
