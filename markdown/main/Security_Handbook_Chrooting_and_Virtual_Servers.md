<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Chrooting_and_Virtual_Servers | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Chrooting and Virtual Servers -->
---
title: Security Handbook/Chrooting and Virtual Servers
url: https://wiki.gentoo.org/wiki/Security_Handbook/Chrooting_and_Virtual_Servers
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-29"
fingerprint: de1b98397d278095
license: CC BY-SA 4.0
---

# Security Handbook/Chrooting and Virtual Servers

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Chrooting

Chrooting a service is a way of limiting a service (or user) environment to only accessing what it should and not gaining access (or information) that could lead to root access. By running the service as another user than root (nobody, apache, named) an attacker can only access files with the permissions of this user. This means that an attacker cannot gain root access even if the services has a security flaw.

Some services like pure-ftpd and bind have features for chrooting, and other services do not. If the service supports it, use it, otherwise you have to figure out how to create your own. Lets see how to create a chroot, for a basic understanding of how chroots work, we will test it with bash (easy way of learning).

Create the /chroot directory with mkdir /chroot. And find what dynamic libraries that bash is compiled with (if it is compiled with -static this step is not necessary):

The following command will create a list of libraries used by bash.

Now let's create the environment for bash.

Next copy the files used by bash (/lib) to the chrooted lib and copy the bash command to the chrooted bin directory. This will create the exact same environment, just with less functionality. After copying try it out: chroot /chroot/bash /bin/bash. If you get an prompt saying / it works! Otherwise it will properly tell you what a file is missing. Some shared libraries depend on each other.

You will notice that inside the chroot nothing works except echo. This is because we have no other commands in out chroot environment than bash and echo is a build-in functionality.

This is basically the same way you would create a chrooted service. The only difference is that services sometimes rely on devices and configuration files in /etc. Simply copy them (devices can be copied with cp -a) to the chrooted environment, edit the init script to use chroot before executing. It can be difficult to find what devices and configuration files a services need. This is where the strace command becomes handy. Start the service with /usr/bin/strace bash and look for open, read, stat and maybe connect. This will give you a clue on what files to copy. But in most cases just copy the passwd file (edit the copy and remove users that has nothing to do with the service), /dev/zero, /dev/log and /dev/random.

### User Mode Linux

Another way of creating a more secure environment is by running a virtual machine. A virtual machine, as the name implies, is a process that runs on top of your real operating system providing a hardware and operating system environment that appears to be its own unique machine. The security benefit is that if the server running on the virtual machine is compromised, only the virtual server is affected and not the parent installation.

For more information about how to setup User Mode Linux consult the [User Mode Linux Guide](https://wiki.gentoo.org/wiki/User-mode_Linux/System_testing_with_UML).
