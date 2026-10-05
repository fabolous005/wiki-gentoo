<!-- source: https://wiki.gentoo.org/wiki/Upgrading_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Upgrading Gentoo -->
---
title: Upgrading Gentoo
url: https://wiki.gentoo.org/wiki/Upgrading_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-30"
fingerprint: c41d5a2e0e47e704
license: CC BY-SA 4.0
---

# Upgrading Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo has its own approach to updates, this document explains how to **upgrade (update)** Gentoo, as well as how to proceed for a well maintained system.

It is important to keep Gentoo up to date. In addition to the need to have the latest security patches, Gentoo installations can get too out of sync with the current version and sometimes become complex to update.

Gentoo differs from most other Linux distributions when it comes to updates. Most distributions have regular releases, every few months or years, which can be anticipated events for some distributions.

Gentoo, by contrast, is a ***[rolling release](https://en.wikipedia.org/wiki/Rolling_release)*** distribution. There is no need to wait for a release to come out in order to get the latest version of packages - software can be installed as soon as it becomes stable. Gentoo was designed from the beginning around this concept of **fast, incremental updates**, and there are frequent updates to Gentoo software, with new and updated packages most days. Further information on Gentoo specifics can be found in the [FAQ](https://wiki.gentoo.org/wiki/FAQ), about [releases and Gentoo](https://wiki.gentoo.org/wiki/FAQ#Can_I_upgrade_Gentoo_from_one_release_to_another_without_reinstalling.3F) and [what makes Gentoo different](https://wiki.gentoo.org/wiki/FAQ#What_makes_Gentoo_different.3F).

Once software is installed, **[regular updates]** will keep all packages on the latest available version.

In rare cases, changes made to the core system, to certain packages, 
[profile changes](<https://wiki.gentoo.org/wiki/Profile_(Portage)>), or certain updates of [Portage](https://wiki.gentoo.org/wiki/Portage), may require manual intervention during or after an update. A [news item](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items) will be published in such critical cases and/or will be signaled after a [Gentoo repository synchronization](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization). Always read and follow the news items and Portage messages.

Profiles are central to a Gentoo system because they can define core system functionality, and new profiles are made available when there are fundamental changes to the way Gentoo works. A profile is [selected at install time](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Choosing_the_right_profile), according to the intended use of the system and is usually only changed if necessary, or for an update.

The [Handbook](https://wiki.gentoo.org/wiki/Handbook) has detailed information on [updating the Gentoo repository](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Updating_the_Gentoo_repository) and on [updating the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Updating_the_system). See man emerge for more detailed information. See [repository synchronization](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization) for complete information on how to use emaint to synchronize repositories.

To update all installed packages to the latest available versions, first [update the Gentoo repository](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization) with [emaint](https://wiki.gentoo.org/wiki/Project:Portage/Sync#Operation):

`root #``emaint --auto sync`
Or, for short:

`root #``emaint -a sync`
This may output messages that should be read and followed, notably the [news items](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items) previously mentioned. If config file updates are pending from a previous update, it may remind to [update the config files](https://wiki.gentoo.org/wiki/Dispatch-conf).

Run [emerge](https://wiki.gentoo.org/wiki/Emerge) to update the whole system, with dependencies:

`root #``emerge --ask --verbose --update --deep --newuse @world`
Or, with short options:

`root #``emerge -avuDN @world`
`--changed-use` may be used in place of `--newuse`, but only if you do not build binary packages. `--changed-use` will not trigger reinstallation when disabled USE flags are added or removed from a package. See the [Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide#Updating_packages_on_the_binary_package_host).

`--with-bdeps=y` can be used to update build time dependencies also.

If Portage reports dependency issues, sometimes using the `--backtrack=30` (or an even higher number) can help. By default, Portage has a relatively low limit on how far it tries to resolve dependencies (for performance reasons), occasionally it is not enough.

When the output of portage contain pages over pages of unresolved dependency issues, in addition to `--backtrack=1000`, it can be useful to try `--emptytree`. When emerge presents the list of packages to update, pay attention to any information about remaining issues like skipped updates. When the output of emerge is fine, as `--emptytree` will be overkill in most cases and take a very long time, refuse the update and run the same command without that option.

Any configuration file changes should be addressed, this can be managed by [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf):

`root #``dispatch-conf`
After the update, Portage recommends running emerge --depclean. Be very careful running emerge --depclean, it can remove important packages (e.g. kernel sources or [virtual package](https://wiki.gentoo.org/wiki/Project:Portage/FAQ#Why_does_emerge_--depclean_sometimes_remove_system_packages.3F) optional dependencies when an alternative gets merged).

When a new profile is available, Portage will inform the user with a [news item](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items) (recent news items are listed on [the website](https://www.gentoo.org/support/news-items/)).

If a system is too old, it may be non trivial to [get the system up to date](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#Updating_old_systems), it may even be easier to start from scratch.

Generally, it is not mandatory to switch to a new profile when one comes out. Systems can continue to use their old profile, they won't stop working by staying on an old profile. However, Gentoo strongly recommends updating the profile if it becomes deprecated, as this means that Gentoo developers no longer plan on supporting it.

Profile updates are executed manually. The way to update may vary significantly from profile to profile; it depends on how deep the modifications introduced in the new profile are. In the simplest case, users only have to use the eselect tool to change the /etc/portage/make.profile symlink, the worst case could be having to recompile the entire system from scratch, with significant reconfiguration (though changes are usually not too hard, and are well explained - the main thing to remember is to properly follow the instructions).

Exactly what is required to migrate to a new profile is detailed in the relevant news item.

This is a generic outline of what is done to update a profile. As stated previously, specific instructions will be provided in a [news item](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items), for each new profile. A profile update often requires manual intervention, beyond just switching the profile version.

Make sure [warnings in parent section](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#General_instructions) have been read.

To switch profiles with the automatic tool, [app-admin/eselect](https://packages.gentoo.org/packages/app-admin/eselect) must be installed. The eselect utility makes viewing and selecting profiles easy, without needing to create or remove symlinks by hand:

`root #````
eselect profile list
```
`root #``eselect profile set <number>`
Make sure [warnings in parent section](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#General_instructions) have been read.

Changing profiles manually is still supported:

`root #````
rm /etc/portage/make.profile
```
`root #````
cd /etc/portage
```
`root #````
ln -s ../../var/db/repos/gentoo/profiles/<selected profile> make.profile
```
See the appropriate [news item](https://www.gentoo.org/support/news-items/2024-03-22-new-23-profiles.html). At this point all installations should already be using the 23.0 profile, and migration could be difficult.

See the appropriate [news item](https://www.gentoo.org/support/news-items/2019-06-05-amd64-17-1-profiles-are-now-stable.html). At this point all installations should already be using the 17.1 profile, and migration could be difficult.

See the appropriate [news item](https://www.gentoo.org/support/news-items/2017-11-30-new-17-profiles.html). At this point all installations should be using the 17.1 profile, and migration could be difficult.

Sometimes, systems are too old to easily upgrade. It may be possible to manually update a very old system, but it may be better to start from scratch and copy system configuration and files from the old system to the new one.

Here is a rough guide to updating an old system. Another method can be found [here](https://wiki.gentoo.org/wiki/User:NeddySeagoon/HOWTO_Update_Old_Gentoo).

The idea with this upgrade approach is that we create an intermediate build chroot in which a recent stage3 is extracted. Then, using the tools available in the stage3 chroot we upgrade the packages on the live system.

Let's first create the intermediate build chroot location, say /mnt/build, and extract a recent stage3 archive into it.

`root #````
mkdir -p /mnt/build
```
`root #````
tar -xf /path/to/stage3-somearch-somedate.tar.bz2 -C /mnt/build
```
`root #````
mount --rbind /dev /mnt/build/dev
```
`root #````
mount --rbind /proc /mnt/build/proc
```
`root #````
mount --rbind /sys /mnt/build/sys
```
Next, we create a mount point inside this chroot environment, on which we then bind-mount the live (old) environment.

`root #````
mkdir -p /mnt/build/mnt/host
```
`root #````
mount --rbind / /mnt/build/mnt/host
```
So now the live (old) system is also reachable within /mnt/build/mnt/host. This will allow us to reach the live (old) system and update the packages even when chrooted inside the intermediate build chroot.

The new install needs to access the network, so copy over the network related information:

`root #``cp -L /etc/resolv.conf /mnt/build/etc/`
Now chroot into the intermediate build location, and start updating vital packages on the live system, until the live system can be updated from within the live system (rather than through the intermediate build chroot):

`root #````
chroot /mnt/build
```
`root #````
source /etc/profile
```
`root #``export PS1="(chroot) ${PS1}"``(chroot) root #``emerge --sync`
It may be a good idea to check that the profile and Portage configuration are compatible between the (old) live system and the chroot.

Now start building packages into the (old) live system. If Portage is old or missing, it is a good idea to start with that:

`(chroot) root #``emerge --root=/mnt/host --config-root=/mnt/host --verbose --oneshot sys-apps/portage`
Keep this chrooted session open and try to update the (old) live system. When failures occur, use this chrooted session to update packages using the build tools available in the intermediate build chroot (which includes recent [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc), [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc), etc.). Tools can be added as needed to the build chroot.

For some installations it may be necessary to update configuration files in order to install new software. Make the changes in the chroot environment.

To get the system fully up-to-date before exiting the root, build the `@world` set (all packages) into the (old) live system:

`(chroot) root #``emerge --root=/mnt/host --config-root=/mnt/host --update --newuse --deep --ask @world`
Once finished the system should now be up to date!

- [Updating old Gentoo installations](https://wiki.gentoo.org/wiki/User:NeddySeagoon/HOWTO_Update_Old_Gentoo)
- [Can I upgrade Gentoo from one release to another without reinstalling?](https://wiki.gentoo.org/wiki/FAQ#Can_I_upgrade_Gentoo_from_one_release_to_another_without_reinstalling.3F)
- [Cheat Sheet on updates](https://wiki.gentoo.org/wiki/Gentoo_Cheat_Sheet#Package_upgrades)
- [Handbook on updates](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Updating_the_system)
- [Installation](https://wiki.gentoo.org/wiki/Installation) — an overview of the principles and practices of installing Gentoo on a running system.
