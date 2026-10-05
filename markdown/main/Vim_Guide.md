<!-- source: https://wiki.gentoo.org/wiki/Vim/Guide | group: Gentoo Wiki (Main) | wiki-title: Vim/Guide -->
---
title: Vim/Guide
url: https://wiki.gentoo.org/wiki/Vim/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-23"
fingerprint: "16b9dd73282dbbb0"
license: CC BY-SA 4.0
---

# Vim/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**[vim](https://wiki.gentoo.org/wiki/Vim)** is a powerful, extendable, versatile, *text editor*. It has a long history and wide usage on many platforms.

The power of [vi](https://wiki.gentoo.org/wiki/Vi) comes from the interface, which uses a form of text-editing command language. All interaction is through the keyboard, where specific modes direct what command each key will perform. This interface permits significant changes across a file with just a few keystrokes, keeping the fingers on the keyboard, and making light work of sometimes complex edits. Investing in learning a tool such as vim should be rewarding for years to come, for anyone spending any consequent amount of time editing text files.

This tutorial will explain basic usage for users new to [vi-like](https://en.wikipedia.org/wiki/vi) text editors in general, and vim in particular. Following this guide, it should be possible to become a proficient vi user, with just a short time commitment. It will show how to move around, edit text, use insert mode, copy and paste, and use important vim extensions like visual mode and multi-window editing. It is aimed at anyone not yet comfortable using vi, to get up to speed with one of the most popular and powerful text editors.

## vi-like editors and Vim

vi was written in 1977 by computer engineer [Bill Joy](https://en.wikipedia.org/wiki/Bill_Joy), at [Berkeley](https://en.wikipedia.org/wiki/UC_Berkeley) (of [BSD](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution) fame). vi was not created in a vacuum, and to cut a long story short, it is a descendant of the [ed](https://wiki.gentoo.org/wiki/Ed) [line editor](https://en.wikipedia.org/wiki/Line_editor), but able to edit a whole screen of text.

There are many versions of vi, all more or less similar to the original version. This guide focuses on vim, a popular version of vi, but most of the information will apply to other vi-like editors. Any Vim-specific commands will be indicated as such.

There are [installation instructions for Gentoo](https://wiki.gentoo.org/wiki/Vim#Installation) on the wiki, but Vim can be downloaded from [www.vim.org](https://www.vim.org/) for users on other OSs, if it is not already installed, or available from a package manager.

In addition to an enhanced command-line editor, vim also comes with [Gvim](https://wiki.gentoo.org/wiki/Vim#Gvim), a graphical editor which can be configured to use the GTK+ library. Gvim  can make things a bit easier for some beginners, who would rather avoid the command line - but knowing how to use a [shell](https://wiki.gentoo.org/wiki/Shell) is also a valuable skill.

## Opening a file

Launch Vim with the vim command. If only Vim is installed, the vi command should launch Vim (see the [vi](https://wiki.gentoo.org/wiki/Vi#.2Fusr.2Fbin.2Fvi_symlink) article for details of how this is set up in Gentoo).

To open a text file in Vim, simply pass it the name of the file on the command line.

`user $``vim myfile.txt`
Using a name for a non existent file will create a new file with that name on saving.

To use gvim, type gvim myfile.txt.

After vi loads, the contents of the text file will be shown, or just the beginning if it is too long to fit.

## Quitting

Surely this shouldn't be the first thing to learn ? Well, because vi isn't self discoverable (this does not mean that it is not ergonomic or easy, just that it must be learned), how to quit can be a very useful thing to know starting out. If anything is to be retained from this tutorial, *this* is the sort of knowledge that can help around the office, to liberate an overly curious colleague from an incomprehensible black and white screen :).

From the command mode, type `:q` then press `Enter` (Return) to quit.

If the file was modified in some way, vi will refuse to exit. To quit while throwing away any changes, type `:q!` then press the `Enter` (Return) key.

See [I'm trapped in vim!](https://wiki.gentoo.org/wiki/Vim#I.27m_trapped_in_vim.21) for more details.

## Introducing modes

Unlike many editors, when vi starts up, it is in a special mode called *command mode*. In this mode, pressing a letter key will not insert that letter into the document, but will execute a command associated with that key. The current mode will be indicated at the bottom left of the screen (or it will be blank for command mode).

Command mode is one of the things that makes vi such an efficient editor, that still has appeal over forty years after it's invention. This mode is used for navigating around files, and for high level editing operations, such as copying, pasting, or deleting whole sections of text; for searching; for recording macros etc.

If one thing has made vi so popular, it is that command mode is like an efficient ***document editing language***, with functions represented by single characters, **providing huge power to the user, right under the fingertips**.

Certain commands will enter different modes, sometimes while simultaneously performing an action. The main modes are the all important *command mode*, the *insert mode* for entering text, *visual mode* for text selection, and what is called *ex-mode*, which is a little like a command line.

Don't worry about understanding what all these are for, yet. How to enter or exit the appropriate modes will be indicated in the tutorial, when needed. If the wrong mode is entered by accident, press `esc` several times to return to command mode.

## Introducing the ex-mode

Pressing `:` while in the command mode will open the *ex-mode*, which is a little like a command line, in some ways. As just seen, quitting using `:q`, uses the ex-mode.

It is called *ex-mode* because vi has a built-in *non-visual* editor called *ex*. This is a descendant of the old Unix *ed* command, which is at the very origins of vi.

This mode has specific commands, different to command mode, and allows composing of more complex commands, when needed.

*ex-mode* can be used similarly to sed to perform line-based editing operations. In some configurations (default vim doesn't seem to do this at the moment) hitting the `Q` key while in command mode will stick in ex mode. In this case, the ":" prompt will remain on screen, and hitting enter will scroll the entire screen upwards. To get back to vi mode, type "vi" (or "visual") and hit `Enter` .

## Moving around

Before any actual editing can take place, the first thing to learn is how to get around a file in command mode, and vi has a lot of movement commands. For this part of the tutorial, find an unimportant text file and load it into vi as explained in the [opening a file](https://wiki.gentoo.org#opening_a_file) section.

### Up, down, left, right

When in command mode, use the `h`, `j`, `k`, and `l` keys to move the cursor left, down, up and right respectively. For modern versions of vi, the arrow keys will also work for this purpose - but it is advised to use "hjkl", as they are right under the finger tips. vi is very advantageous to [touch typists](https://en.wikipedia.org/wiki/Typing#Touch_typing), but less so if [hunting and pecking](https://en.wikipedia.org/wiki/Typing#Hunt_and_peck).

Try using the "hjkl" keys to move around in the text file. Try pressing `h` until the cursor gets to the beginning of a line. Notice that vi doesn't allow the cursor to "wrap around" to the previous line by hitting `h` while on the first character, or to "wrap around" to the next line by hitting `l` at the end of a line.

Use `Ctrl`+`F` and `Ctrl`+`B` (remember as "foreword" and "back") to move forwards and backwards a whole page at a time. Modern versions of vi (like vim) will also allow  use of the `Page Up` and `Page Down` keys for this purpose.

### Go to the beginning or the end of a line

Press `0` (zero - easy to remember) to jump to the first character of a line, and `$` to jump to the last character of a line. `^` (caret) will go to the first non blank character of a line. These are keys that are often closer to the fingertips than the "home" or "end" keys, on most keyboards.

Since vi has handy movement commands close to the fingertips, it makes a great [pager](https://wiki.gentoo.org/wiki/Recommended_tools#Pagers) (like the more or less commands). Using vi as the default system pager can help learn the movement commands.

### Word moves

vi provides commands to move to the left or right by word increments. To move to the *first* character of the next word, press `w` (remember as "word"). To move to the *last* character of the next word, press `e` (remember as "word End"). To move to the first character of the *previous* word, press `b` (remember as "word Back").

After playing around with the word movement commands, it may have become clear that vi considers words like `foo-bar-oni` as five separate words! This is because by default, vi delimits words by spaces *or* punctuation. `foo-bar-oni` is therefore considered five separate words.

Sometimes, this is desirable behavior, but sometimes it isn't. Vi also has the concept of a "bigword". vi delimits bigwords by *spaces or newlines only*. This means that while `foo-bar-oni` is considered five vi words, it's considered only one vi bigword.

To jump around to the next or the previous bigword, simply use a *capitalized* word move command. i.e. `W` to jump to the first character of the next bigword, `E` to jump to the last character of the next bigword, and `B` to jump to the first character of the previous bigword. Compare the matching word and bigword movement commands until the differences are clear.

### Move by sentence or paragraph

Use the `(` and `)` characters to move to the beginning of the previous and next sentence.

Hit `{` or `}` to jump to the beginning of the current paragraph, or the beginning of the next.

### Jumping about

In command-mode, jump to a particular line by typing `<number>G`. To jump to the first line of a file, type `1G`, or `gg`. Note that `G` is capitalized.

To go to the end of a file, type `G`.

### Searching

To jump to the next occurrence of a particular text pattern, type `/<regexp>` and hit `Enter`. Replace \<regexp> with the [regular expression](https://en.wikipedia.org/wiki/Regular_expression) matching the text to be found.

Regular expression knowledge is not needed to search for simple strings. Just typing `/foo` will move to the next occurrence of *foo*.

To repeat the search forwards, hit `n`. To repeat the search backwards, type `N`. Type `//` to repeat the last search.

## Save, and save as

To save changes to the current text file, type `:w`. To save changes to another file, type `:w filename.txt` to save as filename.txt. To save and quit simultaneously, type `:x` or `:wq`.

## Buffers (windows)

In vim (and other advanced vi editors, like elvis), it is possible to have multiple buffers open at once. To open a file into a new window, type `:sp filename.txt`. filename.txt will appear open for editing in a new split window. To switch between windows, type `Ctrl`+`w`+`Ctrl`+`w` (control-w twice). Any `:q`, `:q!`, `:w` and `:x` commands will only be applied to the currently-active window.

## Editing from command mode

Editing commands in command mode tend to be simple. The more complex editing commands will automatically enter insert mode, but this will be covered later.

### Deleting and replacing

Pressing `x` will delete the character under the cursor. Now, move to the middle of a the paragraph somewhere in a text file, and hit `J` (capitalized), this will join the next line to the end of the current line.

To replace a *single character'*, move over a it and hit `r`, and then type in a new character - this will replace the original character while remaining in command mode.

Move to any line in the file and type `dd` to delete the current line of text.

### Repeating and combining commands with a movement

Repeat any editing command by hitting the `.` key. For example, typing `dd...` will delete 4 lines, and `J..` will join four lines.

Some commands, such as the `d` command to delete, can be combined with a movement command, using some of the movements previously discussed. For example, `dw` will delete from the current position to the beginning of the next word; `d)` will delete up until the end of the next sentence, and `d}` will delete the remainder of the paragraph.

### Undo

Vim maintains a history of actions, to allow to undo them. By pressing `u`, the original version of vi allowed to undo the last edit only. Modern versions of vi, like vim, will allow to repeatedly press `u` to continue to undo changes. Try combining some `d` and `u` commands.

## Insert mode

After covering how to move around in vi, perform file I/O, and basic editing operations, it is time to type in free-form text! vi's insert mode may seem a little complicated at first. However, after becoming comfortable with insert mode, its complexity (and flexibility) will become an asset.

In vi *insert mode*, enter text directly to the screen just like in many other visual editors. Once modifications are complete, hit escape to return to *command mode*.

Enter insert mode by pressing `i` or `a`. `i`, will *insert* before the current character, and `a` will *append* after the current character. Remember, after entering text, to hit `Esc` to return to command mode.

Try using the arrow keys and the `Del` key to get a feel for how insert mode works. By using the arrow keys and `Del` key, it is possible to perform significant editing steps without repeatedly entering and leaving insert mode.

### Insert options

Here are some other handy ways to enter insert mode, from command mode. Press `A` (capital) to begin appending to the *end* of the current line, regardless of the current position on the line. Likewise, press `I` (capital) to begin inserting text at the *beginning* of the current line.

Press `o` from command mode to create a new blank line below the current line into which to insert text, and press `O` (capital) to create a new line above the current line.

To replace the entire current line with a new line, press the `c` key twice in a row. To replace everything from the current position to the end of the line, type `c$`. To replace everything from the current position to the beginning of the line, type `c0`.

In addition to performing a special operation, every one of these commands will enter insert mode. After typing in text, hit `Esc` to return to command mode.

### Changing text

The `c` (change) command was just introduced when typing `cc`, `c0` and `c$`. `cc` is a special form of the change command, similar to `dd`. The `c0` and `c$` commands are examples of using the change command in combination with a movement command. In this form, `c` works similarly to `d`, except that it enters into insert mode to enter replacement text for the deleted region. Try combining some movement commands with `c` and test them out (hint: `cW`, `ce`, `c(`).

These "composite" commands that change mode may not seem groundbreaking, but usage will show that these slight gains in having a command perform two steps not only speeds things up, but with practice will put less of a cognitive load on the user.

## Compound commands

vi *really* becomes powerful when using compound ("combo") commands, like `d{` and `cw`. In addition to these commands, it is possible to combine a number with any movement command, such as `3w`, which will tell vi to jump three words to the right. Here are some more movement "combo" command examples: `12b`, `4j`.

vi, in addition to allowing (number)(movement command) combinations, also allows `d` or `c` to be combined with a number or movement command. So, `d3w` will delete the next three words, `d2j` will delete the current and next two lines, etc. Test out some `c` and `d` combo moves to get a feel for how powerful and concise vi editing can be. Once these commands are learned, it will be second-nature to edit files at blazing speed.

## Productivity features

Now that how to move, save and quit, perform simple edits and deletions, and use insert mode has been covered, it should be possible to use vi to perform almost any task.

However, vi also has many more powerful commands. This section will cover how to *cut*, *copy*, and *paste*, *search* and *replace*, and use *autoindent* features. These commands will help make vi even more fun and productive.

### Visual mode

The best way to cut and paste is to use *visual mode*, a special mode that has been added to modern versions of vi, like vim and elvis. Think of visual mode as a "highlight text" mode. Once the text is highlighted, it can be copied or deleted, and then pasted. In gvim, highlight by simply dragging the left mouse button over a particular region:

Enter visual mode by hitting `v` (this may be the only option if using vi from the console.) Then, by moving the cursor using movement commands (typically the arrow keys), highlight a region of text.

To copy highlighted text, hit `y` (which stands for "yank"), to cut the text, hit `d`, then vi will return to command mode. Now, move to the position to insert the cut or copied text, and hit `P` to insert before the cursor, or `p` to insert after the cursor.

Voila, the cut/copy and paste is complete! Test out several copy/cut and paste operations before advancing to the next section.

### Replacing text

To replace patterns of text, use *ex* mode. To replace the first pattern that appears on the current line, type `:s/<regexp>/<replacement>/` and press the `Enter` key, where \<regexp> is the pattern to match, and \<replacement> is the replacement string.

To replace all matches on the current line, type `:s/<regexp>/<replacement>/g` and hit enter.

To replace every occurrence of this pattern in the file (which is often the case), type `:%s/<regexp>/<replacement>/g`. To do a global replace, but have vi prompt for each change, type `:%s/<regexp>/<replacement>/gc` (stands for "confirm") and hit `Enter`.

### Indentation

vi supports autoindentation, for editing source code. Most modern versions of vi (like vim) will auto-enable autoindent mode editing a source file (like a .c file, for example). When autoindent is enabled, use `Ctrl`+`d` (control-d) to move one indent level to the left, and `Ctrl`+`t` (control-t) to move one indent level to the right.

If autoindent wasn't enabled automatically, manually enable it by typing in `:set autoindent`. The tab size to can be set to user preference by using the `:set tabstop` command; `:set tabstop=4` is quite popular.

## Finishing off

To use vi as the default editor in Gentoo, use eselect:

`root #``eselect editor list`
Available targets for the EDITOR variable:
  \[1\]   /bin/nano
  \[2\]   /bin/ed
  \[3\]   /usr/bin/ex
  \[4\]   /usr/bin/vi
  \[ \]   (free form)

`root #``eselect editor set 4`
Setting EDITOR to /usr/bin/vi ...
>>> Regenerating /etc/ld.so.cache...
Run ". /etc/profile" to update the variable in your shell.

To set this for a user instead of system wide, set the `EDITOR` variable in \~/.bashrc:

`user $``vi ~/.bashrc`
**`~/.bashrc`**

```
(...)
export EDITOR="/usr/bin/vi"
```
## External resources

- [A byte of Vim](https://github.com/swaroopch/byte-of-vim) - a popular vim primer.
- [The vi Lovers Home Page](https://thomer.com/vi/vi.html) - An excellent resource for all things vi.
- [The vim homepage](https://www.vim.org/) - The place to go for all your vim needs.
- [Learning the vi and Vim Editors, 7th Edition](http://shop.oreilly.com/product/9780596529833.do) - An excellent choice for those who are looking for a good, old-fashioned book. Contains good coverage of vi and vi clones.
