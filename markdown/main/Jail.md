<!-- source: https://wiki.gentoo.org/wiki/Jail | group: Gentoo Wiki (Main) | wiki-title: Jail -->
---
title: jail
url: https://wiki.gentoo.org/wiki/Jail
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-14"
fingerprint: a1ac18caeb2333cd
license: CC BY-SA 4.0
---

# jail

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This guide will go through how to use the **jail** tool to set up a [chroot](https://wiki.gentoo.org/wiki/Chroot).

## Installation

### Emerge

`root #``emerge --ask app-misc/jail`
## Configuration

### Create directories

Create the root directory for jail:

`root #``mkjailenv /var/chroot````
mkjailenv
A component of Jail (version 1.9 for linux)
http://www.gsyc.inf.uc3m.es/~assman/jail/
Juan M. Casillas <assman@gsyc.inf.uc3m.es>
Making chrooted environment into /var/jail
        Doing preinstall()
        Doing special_devices()
        Doing gen_template_password()
        Doing postinstall()
Done.
```
### Add a new user account in the main system

This account should have the chroot as its home directory and the jail binary as the shell:

`root #``useradd -g users --home /var/chroot --shell /usr/bin/jail larry`
### Add a new user account in the chroot

This account should have the same name as the account in the main system:

`root #``addjailuser /var/chroot /home/larry /bin/bash larry`
addjailuser
A component of Jail (version 2.0 for linux)
http://www.gsyc.inf.uc3m.es/\~assman/jail/
Juan M. Casillas \<assman@gsyc.inf.uc3m.es>
Adding user larry in chrooted environment /var/chroot
Done

The home directory and shell paths in the above command refer to paths within the chroot.

### Adding software

Add the set of basic programs to the jail:

`root #``addjailsw /var/chroot`
A component of Jail (version 2.0 for linux)
http://www.gsyc.inf.uc3m.es/\~assman/jail/
Juan M. Casillas \<assman@gsyc.inf.uc3m.es>
Guessing head args()
Guessing sh args()
Guessing vi args(-c q)
Guessing pwd args()
Guessing mv args()
Guessing rmdir args()
Guessing ls args()
Guessing ln args()
Guessing tail args()
Guessing id args()
Guessing mkdir args()
Guessing touch args()
Guessing grep args()
Guessing cp args()
Guessing rm args()
Guessing more args()
Guessing cat args()
Warning: not allowed to overwrite /var/chroot//etc/passwd 
Warning: not allowed to overwrite /var/chroot//etc/group 
Warning: can't create /proc/meminfo from the /proc filesystem
Done.

Next we need to also add the login shell:

`root #``addjailsw /var/chroot -P /bin/bash --version`
It may be necessary to pass an argument (`--version` as in the above example) to help jail figure out the libraries that are necessary for the program to run in the jail.

The command `addjailsw -P` can be used to add any programs to the chroot.

### Finishing touches

Copy the shell startup scripts to the jail:

`root #````
mkdir -p /var/chroot/etc/bash
```
`root #````
cp /etc/bash/bashrc /var/chroot/etc/bash
```
`root #````
cp /etc/profile /var/chroot/etc
```
`root #````
cp /etc/DIR_COLORS /var/chroot/etc
```
## Activating jail

Everytime you switch to the larry user you will be logged into the jail:

`root #``su - larry`
The first time you run this command it will probably fail, see below.

## Troubleshooting

If it is not possible to su into target jail system and the following error message appears:

jail: execve() : No such file or directory

it means that the dynamic linker is missing. Copy it from the host lib64 directory -

`root #` `cp -L /lib64/ld-linux-x86-64.so.2 /var/chroot/lib64`
The -L switch is very important as ld-linux-x86-64.so.2 is actually a symlink that points to ld-2.25.so, which is the dynamic linker. The -L dereferences the symlink and copies the file that it points to instead of copying the symlink itself. The copied file inherits the name of the symlink.
