<!-- source: https://wiki.gentoo.org/wiki/KeePassXC/cli | group: Gentoo Wiki (Main) | wiki-title: KeePassXC/cli -->
---
title: KeePassXC/cli
url: https://wiki.gentoo.org/wiki/KeePassXC/cli
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-10"
fingerprint: a31d2dbb054036d1
license: CC BY-SA 4.0
---

# KeePassXC/cli

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**keepassxc-cli** is a command line interface for the KeePassXC password manager.

## Configuration

### Files

- \~/Passwords.kdbx - Local (default) database name.
- \~/keepass.keyx - (optional) key file for unlocking the database.

## Usage

keepassxc-cli provides the ability to query and modify the entries of a KeePass database, directly from the command line. This document shows a simple set of commands. Presented example commands cover *most common* use cases needed to create new db, add entries, list and remove entries from a example database.

Specific database entries need to be set using the KeePassXC GUI. Generating of (OTP) one-time passwords works.

### db-create

Create a password database named Passwords.kdbx, the KeePassXC default database name, ask to set a database password:

`user $``keepassxc-cli db-create -p Passwords.kdbx`
keepassxc-cli db-create -p Passwords.kdbx
Enter password to encrypt database (optional):
Do you want to create a database with an empty password? \[y/N\]: y
Successfully created new database.

### db-info

Show technical information about the created database \~/Passwords.kdbx:

`user $``keepassxc-cli db-info Passwords.kdbx````
Enter password to unlock Passwords.kdbx:
UUID: {cb79ec89-5f80-43da-8ed6-0ae6a1c79baa}
Name:
Description:
Cipher: AES 256-bit
KDF: AES (1000000 rounds)
Recycle bin is enabled.
Location: Passwords.kdbx
Database created: 15/01/2024 09:08
Last saved: 15/01/2024 10:08
Unsaved changes: no
Number of groups: 1
Number of entries: 0
Number of expired entries: 0
Unique passwords: 0
Non-unique passwords: 0
Maximum password reuse: 0
Number of short passwords: 0
Number of weak passwords: 0
Entries excluded from reports: 0
Average password length: 0 character(s)
```
2 database entries are empty using shown syntax.

- `Name:`
- `Description:`

Find and use additional keepassxc-cli commands to setup both (optional) entries.

### open

Open passwords database, authenticate to the database, show ready prompt:

`user $``keepassxc-cli open Passwords.kdbx`
Enter password to unlock Passwords.kdbx:
Passwords.kdbx>

Use the quit command to exit.

`Passwords.kdbx>``exit`
### add

Add user `-u larry`, generate random password `-g`, name this database entry `wiki.gentoo.org`:

`user $``keepassxc-cli add Passwords.kdbx -u larry -g wiki.gentoo.org`
Enter password to unlock Passwords.kdbx: 
Successfully added entry wiki.gentoo.org.

### ls

list database entries:

`user $``keepassxc-cli ls Passwords.kdbx`
Enter password to unlock Passwords.kdbx:
wiki.gentoo.org

### show

Show entry name `wiki.gentoo.org`, show the protected `show -s` password entry

`user $``keepassxc-cli show -s Passwords.kdbx wiki.gentoo.org````
Enter password to unlock Passwords.kdbx:
Title: wiki.gentoo.org
UserName: larry
Password: r2xTqE47DYkWzdhyPuyjEZVvUvyusyMu
URL: 
Notes: 
Uuid: {bd849456-108d-43ee-a43c-b1c37354f7cc}
Tags: 
```
### show (TOTP)

Generate a TOTP, a **T**ime-Based **O**ne-**T**ime **P**assword from the the `wiki.gentoo.org` database entry using the `show -t` *-t token* command:

`user $``keepassxc-cli show -t Passwords.kdbx wiki.gentoo.org`
Enter password to unlock Passwords.kdbx:
975956

### clip

Copy passsword from the `wiki.gentoo.org` entry name to clipboard. A timer is running at command line output `[...]` skipped for brevity:

`user $``keepassxc-cli clip Passwords.kdbx wiki.gentoo.org`
Enter password to unlock Passwords.kdbx: 
Entry's "Password" attribute copied to the clipboard!
Clearing the clipboard in 10 seconds... 
\[...\]
Clearing the clipboard in 1 seconds... 
Clipboard cleared!

The default timer is set to`10 seconds`. Use the `Ctrl`+`V` keys to paste.

### rm

Remove the database entry named `wiki.gentoo.org`:

`user $``keepassxc-cli rm Passwords.kdbx wiki.gentoo.org`
Enter password to unlock Passwords.kdbx: 
Successfully recycled entry wiki.gentoo.org

The entry will be moved to the database *recycle bin*.

### generate

keepassxc-cli generate random password, password length `-L 64`

`user $``keepassxc-cli generate -L 64`
aUVEetihMzbFMeqV4CjxTaWEVJReqXQnEuFqNiL7P4nXCDh57bSbrT7Wfst9XU2H

### diceware

keepassxc-cli diceware a phrase using only 4 words `-W 4`. *Default* setting is `7`:

`user $``keepassxc-cli diceware -W 4`
casing handshake tank overdraft

Use a local wordlist by appending `-w example_wordlist.txt` at the end of the shown command example. Example wordlist are availble [here](https://www.eff.org/dice).

### man

For more commands and options use the manual pages:

`user $``man keepassxc-cli`
### Invocation

`user $``keepassxc-cli`
Usage: keepassxc-cli \[options\] command
KeePassXC command line interface.
Available commands:
add                 Add a new entry to a database.
analyze             Analyse passwords for weaknesses and problems.
attachment-export   Export an attachment of an entry.
attachment-import   Imports an attachment to an entry.
attachment-rm       Remove an attachment of an entry.
clip                Copy an entry's attribute to the clipboard.
close               Close the currently opened database.
db-create           Create a new database.
db-edit             Edit a database.
db-info             Show a database's information.
diceware            Generate a new random diceware passphrase.
edit                Edit an entry.
estimate            Estimate the entropy of a password.
export              Exports the content of a database to standard output in the specified format.
generate            Generate a new random password.
help                Display command help.
import              Import the contents of an XML database.
ls                  List database entries.
merge               Merge two databases.
mkdir               Adds a new group to a database.
mv                  Moves an entry to a new group.
open                Open a database.
rm                  Remove an entry from the database.
rmdir               Removes a group from a database.
search              Find entries quickly.
show                Show an entry's information.
Options:
  --debug-info   Displays debugging information.
  -h, --help     Displays help on commandline options.
  --help-all     Displays help including Qt specific options.
  -v, --version  Displays version information.
Arguments:
  command        Name of the command to execute.

## Tips

### Passwordless authentication

Depending on the given situation it might be useful to authenticate using only a \~/\*.keyx file, without the need of keyboard-interactive user authentication. For example, testing in automated environements.

Generate a random key file named `keepass.keyx` in current directory, generate using openssl:

`user $``openssl rand -out keepass.keyx 256`
Now keepassxc-cli db-create the database, and *set* keyfile to `--set-key-file keepass.keyx`, using applications default `Passwords.kdbx` database name:

`user $``keepassxc-cli db-create --set-key-file keepass.keyx Passwords.kdbx`
Successfully created new database.

Display database technical information using the keepassxc-cli db-info command, *use* `--key-file keepass.keyx` to authenticate and ask `--no-password`:

`user $``keepassxc-cli db-info Passwords.kdbx --key-file keepass.keyx --no-password````
UUID: {f4fba927-ac90-4452-8295-52469c515b5c}
Name: 
Description: 
Cipher: AES 256-bit
KDF: AES (1000000 rounds)
Recycle bin is enabled.
Location: Passwords.kdbx
Database created: 21/01/2024 08:57
Last saved: 21/01/2024 09:57
Unsaved changes: no
Number of groups: 1
Number of entries: 0
Number of expired entries: 0
Unique passwords: 0
Non-unique passwords: 0
Maximum password reuse: 0
Number of short passwords: 0
Number of weak passwords: 0
Entries excluded from reports: 0
Average password length: 0 character(s)
```
## See also

- [KeePassXC](https://wiki.gentoo.org/wiki/KeePassXC) — modern, secure, open-source, and cross-platform password manager
- [pass](https://wiki.gentoo.org/wiki/Pass) — a command-line password manager that stores, retrieves, generates, and synchronizes passwords securely.
- [Password management tools](https://wiki.gentoo.org/wiki/Password_management_tools) — This meta article is dedicated to secure password generation, auditing of generated passwords for security, and management of existing passwords.
