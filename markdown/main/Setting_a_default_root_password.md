<!-- source: https://wiki.gentoo.org/wiki/Setting_a_default_root_password | group: Gentoo Wiki (Main) | wiki-title: Setting a default root password -->
---
title: Setting a default root password
url: https://wiki.gentoo.org/wiki/Setting_a_default_root_password
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-06"
fingerprint: b790f1cb4a4163dc
license: CC BY-SA 4.0
---

# Setting a default root password

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Under certain circumstances it might be convenient to set a default root password. For example when deploying Gentoo cross-platform trying to [chroot in the common way](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Chrooting) may return an exec format error. If the [respective entry in the Knowledge Base](https://wiki.gentoo.org/wiki/Knowledge_Base:Chrooting_returns_exec_format_error) does not apply chances are that using a [QEMU user chroot](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_qemu_user_chroot) instead of the standard procedure is indicated. In that case circumventing the necessity of chrooting may spare a lot of efforts. Setting a default root password is integral to that circumvention.

To set a default root password the file TARGET/etc/shadow needs to be manipulated.

## Hash the password

Since passwords may not be stored in plaintext use openssl to convert the password:

`user $``openssl passwd -6`
Password:
Verifying - Password:
$6$I9Q9AyTL$Z76H7wD8mT9JAyrp/vaYyFwyA5wRVN0tze8pvM.MqScC7BBm2PU7pLL0h5nSxueqUpYAlZTox4Ag2Dp5vchjJ0

In this example the string corresponds to the password "gentoo".

## Option 1: Edit shadow by hand

The resulting string needs to be placed in TARGET/etc/shadow. In that file replace the line beginning with `root:` with the line shown below, substituting SHADOW\_COMMAND\_OUTPUT with the string obtained before.

**`/etc/shadow`**

In case of the example above it would look like that:

**`/etc/shadow`**

## Option 2: Use sed to manipulate shadow

The resulting string needs to be placed in TARGET/etc/shadow. First escape [Basic Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended) in the string provided by the openssl command above, that is precede each of the characters `$.*[\^/&` by `\`.

In the following command substitute MODIFIED\_SHADOW\_COMMAND\_OUTPUT with that modified string.

`root #````
sed -i 's/root\:\*/root\:MODIFIED_SHADOW_COMMAND_OUTPUT/' TARGET/etc/shadow
```
In case of the example above this would look like that:

`root #````
sed -i 's/root\:\*/root\:\$6\$I9Q9AyTL\$Z76H7wD8mT9JAyrp\/vaYyFwyA5wRVN0tze8pvM\.MqScC7BBm2PU7pLL0h5nSxueqUpYAlZTox4Ag2Dp5vchjJ0/' TARGET/etc/shadow
```
