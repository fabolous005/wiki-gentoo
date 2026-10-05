<!-- source: https://wiki.gentoo.org/wiki/Portage/Profiles/Switching_profiles | group: Gentoo Wiki (Main) | wiki-title: Portage/Profiles/Switching profiles -->
---
title: Portage/Profiles/Switching profiles
url: https://wiki.gentoo.org/wiki/Portage/Profiles/Switching_profiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
fingerprint: "13407a065d9786"
license: CC BY-SA 4.0
---

# Portage/Profiles/Switching profiles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

Sometimes when the usage of a system changes, or when realizing that another profile is a better fit, it can be necessary to switch profiles.

After the release of a new profile, profiles can need to be upgraded - see [upgrading profiles](https://wiki.gentoo.org/wiki/Portage/Profiles#Upgrading_profiles).

## Possible profile changes

Some profile changes are simple, but others can be non-trivial:

- Changing to or from [Hardened](https://wiki.gentoo.org/wiki/Hardened_Gentoo) or [SELinux](https://wiki.gentoo.org/wiki/SELinux/Installation) profiles, for example, requires specific steps, as described in the [relevant](https://wiki.gentoo.org/wiki/Hardened_Gentoo#Switching_to_a_Hardened_profile) [documentation](https://wiki.gentoo.org/wiki/SELinux/Installation#Change_the_Gentoo_profile).
- Changing to or from [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) or [systemd](https://wiki.gentoo.org/wiki/Systemd) profiles, is another case for which it is important to follow the appropriate [documentation](https://wiki.gentoo.org/wiki/Systemd#Installation).
- It is not possible to change to profiles with a different ABI (e.g. pure LLVM or musl) without a reinstall.

## Profile-switch example procedure

Each profile change will have specificities, as stated above, these instructions are provided for illustrative purposes only, and should be adapted for the actual task.

### Perform a system update

First, [update the system](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) (make sure the *depclean* step is not trying to remove required packages):

`root #``emaint --auto sync``root #``emerge --ask --verbose --update --deep --newuse @world``root #``emerge --ask --depclean`
The `profile` module of [the eselect tool](https://wiki.gentoo.org/wiki/Eselect) allows users to switch profiles.

### Select new profile

List available profiles, then choose the appropriate profile to switch to:

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]   default/linux/amd64/23.0 \*
  \[2\]   default/linux/amd64/23.0/desktop
  \[3\]   default/linux/amd64/23.0/desktop/gnome
  \[4\]   default/linux/amd64/23.0/desktop/kde

Set the new profile:

`root #``eselect profile set 2`
### Toolchain change, when required

If new profile has big changes (like update [from 17.1 to 23.0 profile](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#Updating_to_23.0_profile)) or requires new toolchain (any normal profile to hardened and vice versa), consider updating whole toolchain and removing existing binary packages:

`root #``rm -r /var/cache/binpkgs/*`
Then fix/remove/comment lines at files at /etc/portage/binrepos.conf/ in any way.

Update binutils:

`root #``emerge --ask --verbose --oneshot binutils`
Check if the new binutils are used:

`root #``binutils-config -l`
If not correct binutils are set now, set them and update environment:

`root #``binutils-config #put_number_from_the_previous_list#``root #``env-update && source /etc/profile`
Update GCC:

`root #``emerge --ask --verbose --oneshot sys-devel/gcc`
Check if the new gcc is used:

`root #``gcc-config -l`
If not correct gcc is set now, set it and update environment:

`root #``gcc-config #put_number_from_the_previous_list#``root #``env-update && source /etc/profile`
Update libc (glibc or musl):

`root #``emerge --ask --verbose --oneshot sys-libs/glibc`
Or:

`root #``emerge --ask --verbose --oneshot sys-libs/musl`
Update environment once more to be sure:

`root #``env-update && source /etc/profile`
Re-emerge libtool:

`root #``emerge --ask --oneshot libtool`
Just for safety, delete the contents of the binary package cache at ${PKGDIR} again:

`root #` `rm -r /var/cache/binpkgs/*`
### Update the system to use the new profile

If it was required to update toolchain, add --emptytree option to emerge to rebuild all packages with new toolchain:

`root #``emerge --ask --verbose --update --deep --newuse --emptytree @world`
If it wasn't necessary update toolchain, just apply changes to the system:

`root #``emerge --ask --verbose --update --deep --newuse @world`
Remove unneeded packages, be sure to check the output first that no important packages are listed to be removed:

`root #``emerge --ask --depclean`
