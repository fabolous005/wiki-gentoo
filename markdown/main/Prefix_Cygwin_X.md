<!-- source: https://wiki.gentoo.org/wiki/Prefix/Cygwin/X | group: Gentoo Wiki (Main) | wiki-title: Prefix/Cygwin/X -->
---
title: Prefix/Cygwin/X
url: https://wiki.gentoo.org/wiki/Prefix/Cygwin/X
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-24"
fingerprint: b28dbb560d262bb5
license: CC BY-SA 4.0
---

# Prefix/Cygwin/X

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to get Portage to build usable Qt/Gtk-apps on Cygwin.

## Preface

Before reading this, you should be familiar with "[Gentoo Prefix](https://wiki.gentoo.org/wiki/Prefix)" and "[Prefix on Cygwin](https://wiki.gentoo.org/wiki/Prefix/Cygwin)".

It would be useful, if you already set up a Cygwin/X system and bootstrapped a Gentoo Prefix on it. However, reading about the theory might be enough, as you'd have to throw it away later anyway. ;-)

## Cygwin/X

Before thinking about bootstrapping, a working Cygwin/X system should be at hand. This is not as trivial as one might think. For best results some things should run as daemons, like they do on any GNU/Linux system. But as you are on Windows, this means you have to set up some of the Daemons as Windows Services.

Don't panic, that one is easier than you think.

### Basic installation

I am using the cygwin setup.exe directly using a windows command prompt. That is quicker than searching&clicking every package together.

The following commands can be used to install a Cygwin/X system, that already enables Audio, adds a tray icon, dbus support, Kerberos ticketing, private key management, postfix setup, ssh access, system message logging and an xdg-menu-icon.

I bet you didn't know half of that was possible, right? Me neither until a couple of weeks ago...

Important: The upcoming \*-devel packages are chosen so the later Gentoo prefix does not need concurrent packages to what is running anyway, or what is almost impossible to build on cygwin using portage. We'll tell portage via package.provided what's what.

You can combine the package lists, of course, and install everything at once.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

- Basic system
Install a rudimentary system that let's you start an X-Server and bootstrap Gentoo:`C:\cygwin>``setup-x86_64.exe -P gcc-g++,git-svn,libdconf-devel,libfam-devel,libgcr-base3-devel,libgcr-ui3-devel,libusb-devel,nano,wget,xinit,xorg-server-devel,xwin-xdg-menu`
- Avahi Support`C:\cygwin>``setup-x86_64.exe -P avahi,libavahi-client-devel,libavahi-common-devel,libavahi-core-devel,libavahi-glib-devel`
- dbus Daemon`C:\cygwin>``setup-x86_64.exe -P dbus,libdbus-glib_1-devel,libdbus1-devel,libdbusmenu-glib-devel,libdbusmenu-gtk-devel,libdbusmenu-gtk3-devel`
- Tray Indicator`C:\cygwin>``setup-x86_64.exe -P libindicator-devel,libnotify-devel,notification-daemon,trayer`
- Pulse Audio`C:\cygwin>``setup-x86_64.exe -P libpulse-devel,pasystray,pulseaudio-module-x11`
- Kerberos`C:\cygwin>``setup-x86_64.exe -P krb5-auth-dialog,krb5-workstation,libblkid-devel,libss-devel`
- Extras`C:\cygwin>``setup-x86_64.exe -P gnome-keyring,openssh,postfix,syslog-ng`

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Switching the user context without password

Daemons must be able to switch user context. Either to gain special privileges or to drop unneeded privileges.

For the full details please see the corresponding [part in the cygwin ntsec guide](https://www.cygwin.com/cygwin-ug-net/ntsec.html#ntsec-nopasswd1).

For the daemons you see above, we need two methods. The first is to configure [cyglsa](https://cygwin.com/cygwin-ug-net/ntsec.html#ntsec-nopasswd2), the second is to add some dummy users to Windows.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Enabling LSA authentication

Start a Cygwin64 Terminal with Administrator privileges and issue the following command:

`user $``/usr/bin/cyglsa-config` administrator privileges! You also have to reboot the machine to

activate the change.

Are you sure you want to continue? (yes/no) yes

Cygwin LSA authentication package registered.

Activating Cygwin's LSA authentication package requires to reboot.

But before rebooting the machine, you can add the dummy users mentioned above.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Adding dummy users

Actually this is only one group and two users.

If you want to use postfix, do the following:

1. Right click on the Windows start button.
2. Select "*Computer Management*"
3. Expand "*Local Users and Groups*"
4. Select "*Users*"
5. Add a new user named "*postfix*", any password, with "*User cannot change password*" and "*Password never expires*" checked. Make it a member of your general "*Users*" group.
6. Select "*Groups*"
7. Add a new group named "*postdrop*"
8. Add "*postfix*" and your local user to the group.

At least you need the "*nobody*" user:

1. Repeat Steps 1-4 above
2. Add a new user named "*nobody*" , any password, with "*User cannot change password*" and "*Password never expires*" checked. Make it a member of your general "*Users*" group.

That's it. The installation of openssh daemon will add a sshd user, and cygserver will add a cyg\_server user. Everything else will use the SYSTEM account.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Making the daemons work

In order for the system daemons to work, Windows must be told how to start them, and when. Here are the steps needed for the various daemons. Please make sure that [LSA authentication](https://wiki.gentoo.org#Enabling_LSA_authentication) is installed (see above) and Windows was rebooted. Further the dummy users mentioned above should be set up already.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Cygwin Server

This is needed for IPC message queues, semaphores and shared memory support. More information is available on the [Cygserver page](https://cygwin.com/cygwin-ug-net/using-cygserver.html).

1. `user $``/usr/bin/cygserver-config` Just follow the on-screen information.
2. Go into the windows services panel and search for "*CYGWIN cygserver*".
3. In its properties Change "*Startup type*" to "*Automatic (Delayed Start)*". The normal automatic start may be too fast. If it is, you get weird UID->Name resolutions.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### dbus system bus

It is good to have a system bus. Most applications run fine with a session bus only, but a system bus makes live much easier.

1. `user $``/usr/bin/messagebus-config` Just follow the on-screen information
2. Go into the windows services panel and search for "*CYGWIN D-Bus system service*".
3. In its properties Change "*Startup type*" to "*Automatic (Delayed Start)*".
4. Open a command (CMD) prompt with administrator privileges and add cygserver to the dependencies: `C:\WINDOWS\system32>``sc config messagebus depend= cygserver`The depend= is a keyword, there really must be a space between it and the dependency list!

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### syslog-ng system logger

With this you get /var/log/messages back.

1. `user $``/usr/bin/syslog-ng-config` Just follow the on-screen information.
2. Go into the windows services panel and search for "*CYGWIN syslog-ng*".
3. In its properties Change "*Startup type*" to "*Automatic (Delayed Start)*".
4. Open a command (CMD) prompt with administrator privileges and add cygserver to the dependencies: `C:\WINDOWS\system32>``sc config syslog-ng depend= cygserver`The depend= is a keyword, there really must be a space between it and the dependency list!

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Avahi Daemon

Well... yeah... You must decide whether you need this, or not...

1. `user $``/usr/sbin/avahi-daemon-config` Just follow the on-screen information.
2. Go into the windows services panel and search for "*CYGWIN Avahi service*".
3. In its properties Change "*Startup type*" to "*Automatic (Delayed Start)*".
4. Open a command (CMD) prompt with administrator privileges and add dbus to the dependencies: `C:\WINDOWS\system32>``sc config avahi-daemon depend= messagebus`The depend= is a key... well... you got it, right?

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Postfix Mail service

If you want to send mails from the console, or any application that uses postfix, you must install its windows service.

1. `user $``/usr/bin/postfix-config` Just follow the on-screen information.
2. Go into the windows services panel and search for "*CYGWIN postfix*".
3. In its properties Change "*Startup type*" to "*Automatic (Delayed Start)*".
4. Open a command (CMD) prompt with administrator privileges and add syslog-ng to the dependencies: `C:\WINDOWS\system32>``sc config postfix depend= syslog-ng`

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### ssh daemon

If you want to use SSH to log into your cygwin, then please, go ahead! :-)

1. `user $``/usr/bin/ssh-host-config` Just follow the on-screen information.
2. Go into the windows services panel and search for "*CYGWIN sshd*".
3. In its properties Change "*Startup type*" to "*Automatic (Delayed Start)*".
4. Open a command (CMD) prompt with administrator privileges and add syslog-ng to the dependencies: `C:\WINDOWS\system32>``sc config sshd depend= syslog-ng`

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

## newlib-cygwin - compiling yourself

In the [Prefix on Cygwin guide](https://wiki.gentoo.org/wiki/Prefix/Cygwin) you have read that you need a [cygwin1.dll with the "forkables" patches applied](http://dev.gentoo.org/~haubi/cygwin-gentoo/).

It should be sufficient to use the pre-compiled one, at least as far as I know. However, if you want to compile your own cygwin1.dll, then this is the section for you.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Needed packages

To be able to successfully configure and compile the project, you will need a few extra packages:

`C:\cygwin>``setup-x86_64.exe -P autoconf2.5,automake1.9,bison,cocom,dejagnu,docbook2X,docbook-xml45,expect,flex,gcc-ada,gcc-fortran,gcc-g++,git-svn,libiconv-devel,libisl-devel,libmpc-devel,libmpfr-devel,make,mingw64-x86_64-gcc-g++,mingw64-x86_64-zlib,nano,tcl-devel,tcl-tk-devel,texinfo,wget,xmlto,zlib-devel`
⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Clone the tree

Which tree you use is up to you and your discretion

- [https://github.com/haubi/newlib-cygwin.git](https://github.com/haubi/newlib-cygwin.git) (Branch: Gentoo)

These are what the pre-compiled cygwin1.dll mentioned above is made off.

- [https://github.com/Yamakuzure/newlib-cygwin.git](https://github.com/Yamakuzure/newlib-cygwin.git) (Branch: master)

These are the same Forkables and Gentoo patches, but rebased on the official master repository, thus including all official fixes.

If you need advice, go with the first. I can not guarantee, that my sources do more than compile cleanly, as I do not have the time to check whether all rebased patches are still in whole, and whether the automatic resolving of any conflicts hasn't caused any damage. However, I am always grateful for fixes and patches. ;-)

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Configure and build

As you now have all the packages you need, you can configure and build the whole tree.

`newlib-cygwin $``./configure --prefix=/usr --build=x86_64-pc-cygwin CFLAGS="$CFLAGS -march=native -O2 -pipe" CXXFLAGS="$CXXFLAGS -march=native -O2 -pipe" LDFLAGS="$LDFLAGS -L/usr/lib -L/usr/bin"`
Whether you put in these FLAGS or not is up to you, unless you already are in your Gentoo prefix, it should *just work* without them. But --prefix and --build are important.

When the configure stage is finished, you can build everything. It is best not to push too hard against the windows kernel. The following is well tested on Windows 10 with an 8-core-CPU:

`newlib-cygwin $``make -j5 -l4`
⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Building from inside the Gentoo prefix

If you build newlib-cygwin from inside you Gentoo prefix, there are two things to take care of:

1. sys-devel/gcc-6.4.0 implicitly sets \_FORTIFY\_SOURCE to 1 or 2. Both are incompatible with libc.
2. The default on Gentoo is to build with -fstack-protector, which is incompatible with both libc and winsup.

The solution is to configure with:

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Install

Yes, you read right. Install this. But not in the root folder, that wouldn't work, because cygwin1.dll is in use. Install it in an image path like this:

`newlib-cygwin $``make install DESTDIR=$(pwd -P)/image`
The true installation can only happen if absolutely no cygwin process is running. So quit the xdg-menu, close all consoles/windows, and stop all Cygwin services. On Windows 10 you can do the latter using the Task Manager, it has a *Services* tab. Under *Processes* you might want to check whether there is no dangling dbus process left behind.

Once everything is stopped, you can use the Windows Explorer or any other file manager to copy the contents of your image directory to your cygwin root directory like this:

1. Copy image/usr/bin to the root directory (like C:\Cygwin) ; This directory will be mounted as /usr/bin, so do **not** copy your image/usr/bin into C:\Cygwin\usr!

1. Copy image/usr/{sbin,share,x86\_64-pc-cygwin} into the usr directory (like C:\Cygwin\usr)

1. If you have enabled LSA authentication, you must run cyglsa-config again from a MinTTY with administrator privileges and reboot your machine.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

## Re-Basing DLLs (fix fork() failures)

The dreaded **fork() failure** can happen from time to time. There is much potential for windows software to interfere with Cygwin. Please read "[Fixing fork()failures](https://cygwin.com/faq/faq.html#faq.using.fixing-fork-failures)" in the Cygwin FAQ for general information.

However, triggering a full rebase by the Cygwin setup program is meaningless for Gentoo, we are somewhere else. The solution is simple, we do it ourselves.

### Find all relevant libraries

There is more to this than just re-basing. There is also the possibility, that a DLL lacks the executable bit. But DLLs must be executable, or it can not be used. Static libraries on the other hand, do not need that bit set.

I have written this little helper script to beat both issues with one bat. Create a list of DLLs to rebase, and fix the executable bit where appropriate. Just adapt the EPREFIX setting to your need.

This will re-create /rebase.lst, which lists all possible re-basable libraries.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Do the rebase

The rebase is done in these simple steps:

1. Close all Cygwin processes and log out of your Cygwin X session.
2. End all running cygwin services. The Task Manager in Windows 10 has a "Services" tab you can use.
3. Start a Command Prompt (CMD) with administrator privileges.
4. `C:\WINDOWS\system32>``C:\Cygwin\bin\dash.exe /bin/rebaseall -T /rebase.lst`
5. If the above command tells you that only dash.exe must be running when rebasing, you have some Cygwin processes left. You can kill them (bash.exe, dbus, etc.) using the Task Manager.

You are done. Start all Cygwin Services, a new X Session, open a console using the xdg-menu and continue with your bootstrapping.

## Bootstrapping the Prefix

Just do the bootstrap like described on the [Project:Prefix/Bootstrap](https://wiki.gentoo.org/wiki/Project:Prefix/Bootstrap) page, unless you do want to start of with some pre-configuration. I prefer the EPREFIX to be /gentoo, and export this in my \~/.bashrc. However, I'll refer to the prefix location as $EPREFIX here, so my preferences shouldn't give you a hard time.

If you want to make your life easier, you can prepare your prefix by cloning the [Cygwin On Steroids Repository](https://wiki.gentoo.org#CygwinOnSteroids).

If you'd like to use the latest portage tree, you can tell the bootstrap script to do so by issuing:

`user $` `LATEST_TREE_YES=1 ./bootstrap-prefix.sh`
However, I have already had fails due to that download being not what I wanted. It really downloads portage-latest.tar.bz2, which might or might not work. The prefix tree, however, **will** work.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

Many of the packages used here need to be patched. Some need adapted ebuilds, as they do not pull in user patches.

To make this easier, and to help me (and you) to reproduce everything without much typing, copying and pasting, I have created a GitHub repository named [Cygwin On Steroids](https://github.com/Yamakuzure/CygwinOnSteroids).

You can use it with the following simple steps:

1. Use Cygwins git to clone the repository where you want to have your prefix. I prefer /gentoo, and refer to $EPREFIX as /gentoo. However, for any (exported!) EPREFIX this will do:
2. Copy make.conf.bootstrap to be your new make.conf:`user $``cp $EPREFIX/etc/portage/make.conf{.boostrap,}` 
3. Adapt the new make.conf to your needs. Be sure that the CPU flags are correct for your system!
4. Do the boostrapping.
5. Once finished, replace your make.conf with make.conf.system:`user $``cp $EPREFIX/etc/portage/make.conf{.system,}` 
6. Adapt the new make.conf to your needs. Again look at the CPU flags, and adapt the locale and mirror settings.
7. Make sure that the entries in $EPREFIX/etc/portage/repos.conf/gentoo.conf point to your actual prefix location.

That's it, there isn't anything more to do. :-)

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Things to note down

In case of failure, you can often use the ebuild command to finish a merge. But when you are outside the bootstrap script and back on your terminal, the environment is back to normal.

When you have told the script what your EPREFIX should be, it'll print the important environment variables for you. You may want to copy them somewhere safe and use them to prefix a later possible manual ebuild run.

These are what I have on my machine:

EPREFIX=/gentoo
 CHOST=x86\_64-pc-cygwin
 PATH=/gentoo/usr/bin:/gentoo/bin:/gentoo/tmp/usr/bin:/gentoo/tmp/bin:/gentoo/tmp/usr/local/bin:/usr/bin:/bin:/cygdrive/c/Windows/System32:/cygdrive/c/Windows
 MAKEOPTS=-j5

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Custom ebuild script for fixing issues

If an ebuild does not finish merging, like gcc always halting at the end of the compile stage, at least on my machine, it may be useful to have a custom ebuild script that takes care of the above environment values.

The following is such a script, that I have in \~/sys/bin, and it works pretty well. At least for me. Adapt and use it if you like.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Possible Preparations

There are a few things you can do before bootstrapping your gentoo prefix. Not much, really, and everything here is purely optional.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Build in RAM

If you have enough RAM, you can have portage do the builds there.
Windows does not know tmpfs, but there is a nice freeware tool you can use to setup RAM disks.
[ImDisk Virtual Disk Driver for Windows](https://sourceforge.net/projects/imdisk-toolkit/) can be used to set up a RAM DISK and mount it in /build, or wherever you want.
Just do not forget to tell portage about it by adding

export PORTAGE\_TMPDIR=/build

(or wherever you mount your RAM Disk) to your \~/.bashrc. Alternatively you can set up a make.conf to start with, and add the PORTAGE\_TMPDIR entry there.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Pre-Setup a make.conf

I'll post here my make.conf for bootstrapping. It adds -march=native to the C\[XX\]FLAGS, sets CPU\_FLAGS\_X86 and enables split logs in $EPREFIX/var/log/portage. Further portage is made cute and rather silent. Maybe you want to remove that part.

If you ***really*** want to use this, put it in $EPREFIX/etc/portage and be **damn** sure to check CPU\_FLAGS\_X86 against your CPU!

Oh, and have a look at the localization settings at the bottom! ;-)

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Portage tree too old

If any of the problems below signals that your portage tree is too old, either because using LATEST\_TREE\_YES didn't do what you expected, or because your boostrap-prefix.sh script is too old, you do not have to start over. Just fix this and continue.

1. Download the latest [boostrap-prefix.sh](https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh) script.
2. `user $``rm $EPREFIX/tmp/etc/make.globals $EPREFIX/usr/portage/.unpacked $EPREFIX/tmp/usr/lib/portage/bin/emerge`
3. Restart the script, but kill it with Ctrl+C after it has synced in the latest prefix tree, and starts to continue the bootstrap.
4. If you have binutils-2.28.1 installed, do the following steps:
  1. `user $``do_ebuild $EPREFIX/usr/portage/sys-devel/binutils/binutils-2.29.1-r1.ebuild merge`
  2. `user $``PATH=$EPREFIX/usr/bin:$EPREFIX/bin:$EPREFIX/tmp/usr/lib/portage/bin:$PATH binutils-config x86_64-pc-cygwin-2.29.1`
  3. Source $EPREFIX/etc/profile and \~/.bashrc
5. If you have gcc-5.3.0 installed, do the following steps:
  1. `user $``do_ebuild $EPREFIX/usr/portage/sys-devel/gcc/gcc-6.4.0.ebuild merge`(If the merge hangs at the end of the compilation stage, see [gcc: Never ending merge](https://wiki.gentoo.org#gcc:_Never_ending_merge).)
  2. Make sure that the following command shows that gcc-6.4.0 is selected: `user $``CHOST="x86_64-pc-cygwin" PATH=$EPREFIX/usr/bin:$PATH gcc-config -l`\[1\] x86\_64-pc-cygwin-5.3.0
\[2\] x86\_64-pc-cygwin-6.4.0 \*
  3. Source $EPREFIX/etc/profile and \~/.bashrc

After this you can continue your bootstrap process.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

## Problems, Breaks, Failures when bootstrapping

There are some things that might fail. Here are the common ones that were experienced so far.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### gcc: Never ending merge

On some rare occasions the merge of sys-devel/gcc will halt after the compilation stage is over.

Luckily this is easy to fix, as long as you noted down the values mentioned above, that were displayed by the bootstrap script. Just substitute the values here with your values.

**Or even better**: Use my do\_ebuild script above, it'll make things easy for you. ;-)

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Send Ctrl+C to break the script.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Fix in **Stage 2**

In *Stage 2* use the fixing script above like

`user $``do_ebuild $EPREFIX/usr/portage/sys-devel/gcc/gcc-6.4.0.ebuild merge`
**or** issue:

$ EPREFIX=$EPREFIX/tmp \
 CHOST=x86\_64-pc-cygwin \
 PATH=$EPREFIX/usr/bin:$EPREFIX/bin:$EPREFIX/tmp/usr/bin:$EPREFIX/tmp/bin:$EPREFIX/tmp/usr/local/bin:/usr/bin:/bin:/cygdrive/c/Windows/System32:/cygdrive/c/windows \
 $EPREFIX/tmp/usr/bin/ebuild $EPREFIX/usr/portage/sys-devel/gcc/gcc-6.4.0.ebuild merge

to finish the merge.

Afterwards you should see:

\* Switching native-compiler to x86\_64-pc-cygwin-6.4.0 ...
\* Updating LTO plugin symlink in /.$EPREFIX/tmp/usr/x86\_64-pc-cygwin/binutils-bin/lib/bfd-plugins     \[ ok \]

As this ends stage 2, you can do like portage suggests, before restarting the bootstrap.

$ . $EPREFIX/tmp/etc/profile
 $ . \~/.bashrc

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Fix in **Stage 3**

If you use the default PORTAGE\_TMPDIR, or if you changed that but exported the value in your .bashrc, you can proceed like in *Stage 2*, but do not change EPREFIX.

Otherwise, if you changed PORTAGE\_TMPDIR in your make.conf, but did not export the value, you have to fix the build like in *Stage 2*, but set PORTAGE\_TMPDIR to $EPREFIX/var/tmp like this:

In *Stage 3* use the fixing script above like

`user $``PORTAGE_TMPDIR=$EPREFIX/var/tmp do_ebuild $EPREFIX/usr/portage/sys-devel/gcc/gcc-6.4.0.ebuild merge`
**or** issue:

$ CHOST=x86\_64-pc-cygwin \
 PATH=$EPREFIX/usr/bin:$EPREFIX/bin:$EPREFIX/tmp/usr/bin:$EPREFIX/tmp/bin:$EPREFIX/tmp/usr/local/bin:/usr/bin:/bin:/cygdrive/c/Windows/System32:/cygdrive/c/Windows \
 PORTAGE\_TMPDIR=$EPREFIX/var/tmp \
 $EPREFIX/tmp/usr/bin/ebuild $EPREFIX/usr/portage/sys-devel/gcc/gcc-6.4.0.ebuild merge

and it will (hopefully) be finished.

This always did the trick for me. This halt happened for me not only in stage2 **and** stage3, but also after the bootstrapping in regular merges.

After the merge finishes, you should see something like:

\* Switching native-compiler to x86\_64-pc-cygwin-6.4.0 ...        \[ ok \]

After that you can source $EPREFIX/etc/profile and your \~/.bashrc and just restart the bootstrap script. It will continue where it left off.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### app-arch/xz-utils fail in Stage 3

In stage 2, "app-arch/xz-utils-5.2.3" merges just fine, using x86\_64-pc-cygwin-windres from binutils-2.29.1 provided by cygwin. But in stage 3 the x86\_64-pc-cygwin-windres provided by the available binutils-2.28.1 breaks. It does not recognize the -isystem parameter in its call from libtool.

To fix this we are going to force the xz-utils build system to use the cygwin version of x86\_64-pc-cygwin-windres.

\~ $ EXTRA\_ECONF="ac\_cv\_prog\_RC=/usr/bin/windres.exe" \~/sys/bin/do\_ebuild $EPREFIX/usr/portage/app-arch/xz-utils/xz-utils-5.2.3.ebuild clean configure
 (...)
 checking for x86\_64-pc-cygwin-windres... (cached) /usr/bin/windres.exe
 (...)

This looks good, so call "merge" instead of "clean configure" to install a working xz-utils. This time it should work.

Afterwards just restart the bootstrap script, it should continue where it left off.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### sys-apps/coreutils-8.29 fails in Stage 3

As of writing this, sys-apps/coreutils-8.29 fails with the message:

mv: cannot stat '/build/portage/sys-apps/coreutils-8.29/image/gentoo//usr/libexec/coreutils/libstdbuf.dll.a.exe': No such file or directory

Unfortunately the patches and methods to emerge coreutils-8.28 do not work with 8.29 and the only way to get around this, right now, is to use

`user $``do_ebuild $EPREFIX/usr/portage/sys-apps/coreutils/coreutils-8.28-r1.ebuild merge`
to merge the older version.

Further you need to mask the new coreutils version, at least for now.

$ mkdir -p $EPREFIX/etc/portage/package.mask
 $ echo ">=sys-apps/coreutils-8.29" >> $EPREFIX/etc/portage/package.mask/coreutils\_limit

### child\_info\_fork::abort: \<path to dll>: Loaded to different address

The re-basing did not work as it should have. Before continuing, you should rebase everything.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### child\_info\_fork::abort *still* happens from time to time!

Yes. I had that, too. This is extremely irritating when the final emerge -e system is running, as it has to be started over and over again.

On my system I found that the antivirus program (Sophos) interfered with the re-basing portage performed. Not always, just from time to time.

Unfortunately I am in a company domain and have no control over the antivirus software.

If you take a look at the [Cygwin list of interfering software](https://www.cygwin.com/faq.html#faq.using.bloda), our Sophos version is listed, you might find what is sabotaging you here.

The page has a telling paragraph reading:

  - Random fork() failures
- Caused by hook DLLs that load themselves into every process in the system. POSIX fork() semantics require that the memory map of the child process must be an exact duplicate of the parent process' layout. If one of these DLLs loads itself at a different base address in the child's memory space as compared to the address it was loaded at in the parent, it can end up taking the space that belonged to a different DLL in the parent. When Cygwin can't load the original DLL at that same address in the child, the fork() call has to fail.

However, if you, like me, can not turn off the offending software, here is what to do:

1. Complete the failed build using do\_ebuild provided above or from the [Cygwin On Steroids Repository](https://wiki.gentoo.org#CygwinOnSteroids).
2. Continue the merge with:
3. The final package to merge will most probably be gcc, which might or might not hang. See above
4. That's it, your system rebuild is finished. The final steps the script would have done, which you have to do now manually, are:
  1. `user $``rm -Rf $EPREFIX/tmp`
  2. `user $``mkdir $EPREFIX/tmp`
  3. `user $``hash -r`
  4. `user $``SHELL=$EPREFIX/bin/bash PATH=$EPREFIX/bin:$EPREFIX/usr/bin:$PATH bootstrap-prefix.sh $EPREFIX startscript`

Congratulations, your bootstrap is finished!

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

## After the bootstrap

Once your bootstrapping is finished, it is time to update your configuration a bit and install some extra tools. The first tool I always start with, is app-portage/eix ; it makes life so much easier...

However, before we start with anything, please add your EPREFIX to the prefixes environment:

### Know where you are

I like it to have the prompt telling me what's what. Just like when installing Gentoo using a chroot. Therefore I like to add a little extra to my \~/.bashrc:

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Update PKG\_CONFIG\_PATH

As we need to use some of the Cygwin provided devel packages, and as some packages' build systems' configuration tools use pkg-config to search for their dependencies, we must add the Cygwin paths to pkg-config:

Otherwise merges will break where the configuration of the package uses pkg-config, and the cygwin devel package shall be used.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Update make.conf

Apart from what you might want to add, here are some information about what I add to it.

1. USE flags
  - I tend to always set almost all flags I want at once. The only flags I do not add right now are gnome-keyring, qt5 and X, as these would pull in a myriad of packages. Too much right now.
  - So basically I substitute my basic USE flag settings with:
2. Hardware, provided by cygwin
  - If you ask what video card cygwin uses, it is:
3. Python targets
  - Python 3.x is too much pain to upgrade to, so I explicitly limit it to 2.7:

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Generate your repos.conf

If you must or want to use webrsync, maybe because your companies firewall blocks rsync, you have to create your own repos.conf, please adapt the locations to your needs.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Install app-portage/eix

As I said, eix makes your life much easier. But we need to prepare some things first.

For eix to be installable, you have to accept some keywords for it:

Now you should be ably to simply emerge eix, followed by a eix-update to build your first database.

If you stumbled over sys-apps/opentmpfiles, don't worry. It just installs three shell scripts. Two are in $EPREFIX/etc/init.d and ignored (not usable anyway) in a prefix. The third is $EPREFIX/usr/bin/tmpfiles and eix uses it to safely create $EPREFIX/var/cache/eix.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Add eix-update to postsync.d

Unfortunately it is not a good idea to use eix-update in a cygwin prefix. It just doesn't work as expected, so we simply call eix-update and eix-diff after every emerge --sync:

Now, whenever you have synced, but you should **not** do this yet, eix will update itself.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Fix eclasses after each sync

You may find this curious, but this page is about getting your Gentoo prefix to let you emerge Qt apps.

- To use Qt, you need cmake. And unfortunately the cmake.eclass is [broken for Cygwin](https://bugs.gentoo.org/644926).
- To actually build Qt, you need an [awful lot of patches and a fix to the qt5-build.eclass](https://bugs.gentoo.org/644748) as well.
- And if you plan to build any KDE apps, a [fix to kde5-functions.eclass](https://bugs.gentoo.org/648594) is also needed.

Luckily I will show you a way how this happens automatically after each emerge --sync, so you do not have to do this by hand each time.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Prepare the postsync.d script

After each emerge --sync is completed, the scripts in the postsync.d folder are executed. We already use this for eix, let's use this for our eclass fixes as well.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Fix cmake-utils.eclass for Cygwin

CMake on Cygwin prefix does not understand CMAKE\_PLATFORM\_REQUIRED\_RUNTIME\_PATH, so it does not add the supplied paths as intended, but simply dumps them to the command line, which leads to the configure stage to fail.

The following patch fixes that: 
[fix\_cmake-utils.eclass\_for\_cygwin.patch](https://644926.bugs.gentoo.org/attachment.cgi?id=515224)
Just place the file into your prefix path.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Fix qt5-build.eclass for Cygwin

For Qt5 there is more to do, because we have two problems:

1. The qt5-build.eclass imposes strict but sane defaults, and makes the Qt5 build system to heed the \*FLAGS we set in our make.conf.
2. The Qt5 build systems Cygwin support is utterly broken and must be patched.

Unfortunately the latter is futile, thanks to the first. This is because all the settings needed for Cygwin are ignored, thanks to the qt5-build.eclass.

So first, place [fix\_qt5-build.eclass\_for\_cygwin.patch](https://644748.bugs.gentoo.org/attachment.cgi?id=516538) into your prefix directory.

Second, we have to place the patches:

And finally, a few Qt5 packages need some extra patching:

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Sync your tree

Yes. Now is the time to sync your tree. And after you did an emerge --sync, you should have seen eix updating, and the process finishing with:

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Preparations before your first @world update

Before going on and updating your @world, you just synced your tree, you have to do some preparations.

First, you might want to install $app-portage/gentoolkit, which will provide the formidable equery tool.

Then we have to tell Portage, that it should use some of the packages Cygwin provides. Generally speaking there are three reasons to do so:

1. Packages like sys-apps/dbus, that need running Windows services, have to be provided by Cygwin.
2. Some packages are just not easy to emerge, or provoke future emerge failurs, like dev-libs/icu.
3. A few Packages make no sense or even can not be installed, like sys-auth/consolekit or sys-auth/elogind.

The resulting $EPREFIX/etc/portage/profile/package.provided file looks like this: (At the moment. Likely to change!)

As you can see this lists some packages we do not have installed yet using Cygwin. So here we go:

`C:\cygwin>``setup-x86_64.exe -P e2fsprogs,gobject-introspection,gtk2.0-atk-bridge,libOSMesa-devel,libgnome-keyring-devel,mesa,mingw64-x86_64-freeglut`
Before your can really do your first @world update, a few more packages need to be un-keyworded:

- If you have USE="kerberos"`user $````
cat >>$EPREFIX/etc/portage/package.accept_keywords/for_kerberos <<_EOF
dev-perl/GSSAPI::gentoo_prefix *
app-crypt/mit-krb5::gentoo_prefix ~*
dev-libs/libev::gentoo_prefix ~*
dev-libs/libverto::local_overlay ~*
virtual/krb5::gentoo_prefix *
_EOF
```
However, to be able to actually merge mit-krb5 (at least openssl can not use Heimdal) you have to add a line to /usr/bin/compile\_et, which belongs to libcom\_err-devel:`user $``echo "exit 0" >> /usr/bin/compile_et`The reason is, that mit-krb5 configure will fail, as compile\_et will exit with code 1 due to a warning from the last chmod it calls. The missing exit 0 is just an oversight and only needed here.
- If you have USE="git"`user $``echo "net-libs/gnutls::gentoo_prefix ~*" >> $EPREFIX/etc/portage/package.accept_keywords/for_git`

If you have done everything like I have recorded above, \~80 packages should now be listed by your emerge -auUD @world update. A lot of these will be from USE flag changes, so it is good to get things straight now, before we carry on.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

Like in bootstrapping, this can happen again, if you have interfering software installed.

Something hooked into a dll that was just installed by portage (like cygncurses6.dll) and sabotaged the rebase.

1. Try to finish the merge by calling ebuild \<path to failed ebuild> merge
2. If this worked, fine. If not, you have to do a [full rebase](https://wiki.gentoo.org#Re-Basing_DLLs_.28fix_fork.28.29_failures.29).

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

## Adding Qt5 and X support

The next step is to install Qt5. This could be done as a series dependent packages to any application that uses Qt, but as this part might be a bit trickier, I prefer to go forward step by step.

It just isn't nice to break in the middle of a 120+ packages merge...

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Installing cmake

If you have experience with running a Gentoo prefix on Cygwin, and have already tried a simple emerge -1 cmake, then you know that you have no chance getting cmake to even bootstrap.

#### Prerequisite: rhash

Unfortunately the rhash build system insists on suffixing libraries with '.so', while they have to be suffixed '.dll' (dynamic) or '.dll.a' (static).

You therefore have to patch the rhash ebuild, best if you put it into an overlay, with this patch:

And in the files subdirectory in your overlay, you need [rhash-1.3.5-fix\_install\_on\_cygwin.patch](https://raw.githubusercontent.com/Yamakuzure/CygwinOnSteroids/master/overlay/app-crypt/rhash/files/rhash-1.3.5-fix_install_on_cygwin.patch). This patch changes the installation to put cygrhash.dll into the bin directory with its executable bit set.
(*Without this cmake will be successfully built, but not installed.*)

Further you need to unkeyword a few packages:

Now you should be able to simply emerge -a1 cmake.

#### Patching cmake

I hope you have patched cmake-utils.eclass like described above? If not, you have to do so now.

As a next step, you should place [cmake-3.10.1-fix\_build\_on\_cygwin.patch](https://raw.githubusercontent.com/Yamakuzure/CygwinOnSteroids/master/etc/portage/patches/dev-util/cmake/cmake-3.10.1-fix_build_on_cygwin.patch) into $EPREFIX/etc/portage/patches/dev-util/cmake/, this will fix some installed auxiliary files which are otherwise causing problems with other packages later.

Now you should be able to simply emerge -a1 cmake.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Installing qtcore

Now that cmake is working, we'll emerge qtcore. It is the basis and a good test whether what we are trying to do actually works.

Before we can start, we have to unkeyword a few packages. The simplest would be to just place [CygwinOnSteroids::for\_qt](https://raw.githubusercontent.com/Yamakuzure/CygwinOnSteroids/master/etc/portage/package.accept_keywords/for_qt) into $EPREFIX/etc/portage/package.accept\_keywords/.

#### Prerequisite: double-conversion

Unfortunately this package can not be emerged on Cygwin. The ebuild simply isn't up to scratch and needs to be changed slightly. Otherwise it would install an .so file instead of a cyg\*.dll file.

After copying dev-libs/double-conversion to your overlay, apply the following patch:

If you have copied the relevant patches like described in [Fix qt5-build.eclass](https://wiki.gentoo.org#Fix_qt5-build.eclass_for_Cygwin), you can now do emerge -1 qtcore and it should work.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### After installation: Enable qt5

If you now try out qmake, you will see, that it is not working. You will most probably see a message like: qmake: could not find a Qt installation of "

We have to tell qtchooser what to do, first:

Now you are set up to move forward.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

### Enabling X and qt5

Now it is time to enable both the "X" and the "qt5" USE flags in your $EPREFIX/etc/portage/make.conf and do another @world update.

I suggest you do: emerge -DNu --with-bdeps y @world, but first read on.

These flags will pull in a lot of packages that need to be unkeyworded, so I suggest you put [CygwinOnSteroids::for\_X\_Qt5](https://raw.githubusercontent.com/Yamakuzure/CygwinOnSteroids/master/etc/portage/package.accept_keywords/for_X_Qt5) into $EPREFIX/etc/portage/package.accept\_keywords/.

#### Patching Graphite2

Unfortunately media-gfx/graphite2 has to be patched to get it to emerge successfully on Cygwin.

When you take a look at the [fix\_graphite2.patch](https://raw.githubusercontent.com/Yamakuzure/CygwinOnSteroids/master/etc/portage/patches/media-gfx/graphite2/fix_graphite2.patch) you need to place in $$EPREFIX/etc/portage/patches/media-gfx/graphite2/, you might wonder, but it is true.

The patch really only adds two characters.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑

#### Patching libX11

Yes, x11-libs/libX11 must be patched also, but apart from these two, all other packages merge right out of the box now.

The [fix\_locale\_t\_on\_cygwin.patch](https://raw.githubusercontent.com/Yamakuzure/CygwinOnSteroids/master/etc/portage/patches/x11-libs/libX11/fix_locale_t_on_cygwin.patch) is neccessary, because the build system gets confused with Cygwins own includes. The culprit here are the libX11 developers, who were so brillant as to add an X11/Xlocale.h include file. So when the system include xlocale.h is wanted, and I remind you that we are on a case insensitive filesystem, then... well, I guess you know what happens...

Now update @world, everything should work out just fine.

⇑ [Back to top](https://wiki.gentoo.org#top) ⇑
