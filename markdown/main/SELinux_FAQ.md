<!-- source: https://wiki.gentoo.org/wiki/SELinux/FAQ | group: Gentoo Wiki (Main) | wiki-title: SELinux/FAQ -->
---
title: SELinux/FAQ
url: https://wiki.gentoo.org/wiki/SELinux/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: e7af8d6f06d20bc0
license: CC BY-SA 4.0
---

# SELinux/FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux is a system of mandatory access controls that can enforce the security policy over all processes and objects in the system.

## Introduction

Using SELinux requires administrators a more thorough knowledge of their system and a good idea on how processes should behave. Next to the [SELinux](https://wiki.gentoo.org/wiki/SELinux) resources, a proper FAQ allows us to inform and help users in their day-to-day SELinux experience.

The FAQ is an aggregation of solutions found on IRC, mailinglists, forums and elsewhere. It focuses on SELinux integration on Gentoo Hardened, but general SELinux questions that are popping up regularly will be incorporated as well.

## General SELinux support questions

### Does SELinux enforce resource limits?

No, resource limits are outside the scope of an access control system. If this type of support is required, take a look at technologies like [cgroups](https://wiki.gentoo.org/wiki/Cgroups), [PAM](https://wiki.gentoo.org/wiki/PAM) and the like.

### Can I use SELinux and the hardened compiler (with PIE-SSP)?

Definitely. It is recommended to do so.

### Can I use SELinux with any file system?

SELinux requires access to a file's security context to operate properly. To do so, SELinux uses *extended file attributes* which needs to be properly supported by the underlying file system. If the file system supports extended file attributes and you have configured your kernel to enable this support, then SELinux will work on those file systems.

General Linux file systems, such as ext2, ext3, ext4, jfs, xfs and btrfs support extended attributes (but don't forget to enable it in the kernel configuration) as well as tmpfs (for instance used by udev). If your file system collection is limited to this set, then you should have no issues.

Ancillary file systems such as vfat and iso9660 are supported too, but with an important caveat: all files in each file system will have the same SELinux security context information since these file systems do not support extended file attributes.

Network file systems can be supported in the same manner as ancillary file systems (all files share the same security context). However, some development has been made in supported extended file attributes on the more popular file systems such as NFS. Although this is far from production-ready, it does look like we will eventually support these file systems on SELinux fully as well.

### Can I use SELinux with **amd64** no-multilib?

Yes, just use the hardened/linux/amd64/no-multilib/selinux profile and you're all set.

### Can I use SELinux with systemd?

Yes, support is available with the *systemd/selinux* profile. Users are encouraged to try it and report bugs.

### What is UBAC exactly?

UBAC, or *User Based Access Control*, introduces additional constraints when using SELinux policy. Participating domains / types that are *both* marked as a `ubac_constrained_type` (which is an attribute) will only have the allowed privileges in effect if they both run with the same SELinux user context.

Of course, this is not always the case. Besides the earlier mentioned requirement that both types are `ubac_constrained_type`, if the source domain is `sysadm_t`, then the constraint will not be in effect (the `sysadm_t` domain is exempt from UBAC constraints). Also, if the source or destination SELinux user is `system_u` then the constraint will also not be in effect.

See the dedicated [SELinux User-based access control](https://wiki.gentoo.org/wiki/SELinux/User-based_access_control) article too.

### Python: global name 'audit' is not defined

If `semanage` fails with `NameError: global name 'audit' is not defined`, you need to ensure that `sys-process/audit` has the `python` use flag enabled.

## Using SELinux

### How do I enable SELinux?

This is explained in the [SELinux installation instructions](https://wiki.gentoo.org/wiki/SELinux/Installation).

### How do I switch between permissive and enforcing?

The easiest way is to use the `setenforce` command. With `setenforce 0` SELinux is told to run in permissive mode. Similarly, with `setenforce 1` SELinux is told to run in enforcing mode.

It is also possible to add a kernel option `enforcing=0` or `enforcing=1` in the bootloader configuration (or during the startup routine of the system). This allows users to run SELinux in permissive or enforcing mode from the start of the system.

The default state of the system is kept in /etc/selinux/config.

### How do I disable SELinux completely?

It might be possible that running SELinux in permissive mode is not sufficient to properly fix any issue you have. To disable SELinux completely, you need to edit /etc/selinux/config and set `SELINUX=disabled`. Next, reboot the system.

### How can I see SELinux denials?

### How do I know which file context rule is used for a particular file?

If you use the `matchpathcon` command, it will tell you what the security context for the given path (file or directory) should be, but it doesn't tell you which rule it used to deduce this. To do that, you can use grep on the policy repository.

When the SELinux utilities try to apply a context, they try to match the rule that is the most specific. The most specific means, in order of tests:

1. If line A has a regular expression, and line B doesn't, then line B is more specific.
2. If the number of characters before the first regular expression in line A is less than the number of characters before the first regular expression in line B, then line B is more specific
3. If the number of characters in line A is less than in line B, then line B is more specific
4. If line A does not map to a specific SELinux type, and line B does, then line B is more specific

If there are specifications that match inside both the policy-provided contexts, the homedir generated contexts and locally-defined contexts, then the following precedence rules apply:

1. Rules (regardless of their origin) that do not have any regular expressions take precedence over any other expressions
2. Expressions inside the locally-defined contexts (file\_contexts.local) as generated by `semanage fcontext` commands take precedence over the homedirs- and policy-provided ones
3. Expressions inside the homedirs generated contexts (file\_contexts.homedirs) take precedence over the policy-provided ones (file\_contexts)

### How do I make small changes (additions) to the policy?

If you are interested in the Gentoo Hardened SELinux development itself, please have a look at the [SELinux development resources](https://wiki.gentoo.org/wiki/SELinux#Development).

However, you will eventually need to keep some changes on your policy, due to how you have configured your system or when you need to allow something that is not going to be accepted as a distribution-wide policy change. In that case, read on.

Updates on the policy are only possible as long as you need to *allow* additional privileges. It is not possible to easily remove rules from the policy, only enhance it. To maintain your own set of additional rules, create a file in which you will keep your changes. In the next example, I will use the term fixlocal, substitute with whatever name you like - but keep it consistent. In the file (fixlocal.te) put in the following text (again, substitute fixlocal with your chosen name):

In this file, you can add rules as you like. In the next example, we add three rules:

1. Allow `mozilla_t` the `execmem` privilege (based on a denial that occurs when mozilla fails to start)
2. Allow `ssh_t` to connect to any port rather than just the SSH port
3. Allows the `user_t` domain to send messages directly to the system logger

If you need to provide raw allow statements (like the one above for the `mozilla_t` domain), make sure that the type (`mozilla_t`), class (`process`) and privilege (`execmem`) are mentioned in the `require { ... }` paragraph.

When using interface names, make sure that the types (`ssh_t` and `user_t`) are mentioned in the `require { ... }` paragraph.

To find the proper interface name (like `corenet_tcp_connect_all_ports` above), you can either look for it in the [SELinux Reference Policy API](http://oss.tresys.com/docs/refpolicy/api/) online or, if [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base) is built with the `doc` USE flag, in /usr/share/doc/selinux-base-policy-.\*/html. Of course, you can also ask for help in [#gentoo-hardened](ircs://irc.libera.chat/#gentoo-hardened) ([webchat](https://web.libera.chat/#gentoo-hardened)), the mailinglist, forums, etc. to find the proper rules and statements for your case.

With the policy file created, you can then build it using the Makefile provided by the system:

`root #``make -f /usr/share/selinux/mcs/include/Makefile fixlocal.pp`
Then, if the builds succeeds, you can load it in memory. Once loaded, it will be loaded after every boot as well, so you do not need to repeat this over and over again.

`root #``semodule -i fixlocal.pp`
### How to I load an entire policy set?

Usually, it is sufficient to go into the /usr/share/selinux/mcs location and execute `semodule` like so:

`root #``semodule -b base.pp -i $(ls *.pp | grep -v base.pp)`
With the 2.4 userspace, that command can be simplified:

`root #``semodule -i *.pp`
However, there is a downside. With the 2.4 userspace onwards, modules can have a certain priority. Modules that were loaded in through the 2.3 userspace utilities are at priority 100, whereas new modules are at priority 400. When a new set of policy modules is loaded, but it does not contain every previous module, then weird things can happen.

Consider the following situation where there no longer is a `storage` module:

`root #``semodule -i *.pp`
Re-declaration of typeattribute fixed\_disk\_raw\_read
Failed to create node
Bad typeattribute declaration at line 74 of /var/lib/selinux/mcs/tmp/modules/100/storage/cil
Failed to build ast
semodule:  Failed!

However removing that module is not possible, because the currently loaded modules depend on objects defined by it:

`root #``semodule -X 100 -r storage`
libsemanage.semanage\_direct\_remove\_key: Removing last storage module (no other storage module exists at another priority).
Failed to resolve typeattributeset statement at 193 of /var/lib/selinux/mcs/tmp/modules/400/authlogin/cil
Failed to resolve ast
semodule:  Failed!

As it is not possible to load in a set of new policies on a new priority (400) and remove an older one from another priority (100) we seem to be in a deadlock.

To resolve this, make an empty policy module named `storage` and load it (at the higher priority):

`root #``semodule -i storage.pp -i /usr/share/selinux/mcs/*.pp`
libsemanage.semanage\_direct\_install\_info: Overriding storage module at lower priority 100 with module at priority 400.

With that in place, there is now an empty `storage` module active at priority 400, allowing the administrator to remove the older one, followed by the empty one.

`root #````
semodule -X 100 -r storage
```
`root #``semodule -r storage`
libsemanage.semanage\_direct\_remove\_key: Removing last storage module (no other storage module exists at another priority).

## SELinux kernel error messages

### I get a register\_security error message when booting

During boot-up, the following message pops up:

This is nothing to worry about (and perfectly normal).

This means that the Capability LSM module couldn't register as the primary module, since SELinux is the primary module. The third message means that it registers with SELinux as a secondary module.

### I get a 'Permission ... in class ... not defined' message during booting

During boot-up, the following message is shown:

This means that the Linux kernel that you are booting supports permissions that are not defined in the policy (as offered through the [sec-policy/selinux-base-policy](https://packages.gentoo.org/packages/sec-policy/selinux-base-policy) package). If you do not notice any errors during regular operations, then this can be ignored (the permissions will be made part of upcoming policy definitions).

## SELinux and Gentoo

### I get a missing SELinux module error when using emerge

When trying to use `emerge`, the following error message is displayed:

This indicates that the portage SELinux module is missing or damaged. Recent Portage versions provide this module out-of-the-box, but the security contexts of the necessary files might be wrong on your system. Try relabeling the files of the portage package:

`root #``rlpkg portage`
### During rlpkg I get 'conflicting specifications for ... and ..., using ...'

When trying to relabel a package (`rlpkg packagename`) or system (`rlpkg -a -r`) you get a message similar to the following:

This is most likely caused by hard linked files. Remember, SELinux uses the extended attributes in the file system to store the security context of a file. If two separate paths point to the same file using hard links (i.e. the files share the same inode) then both files will have the same security context.

The solution depends on the particular case; in order of most likely to happen and resolve:

1. Although both files are the same, they are not used in the same context. In such cases, it is recommended to remove one of the files and then copy the other file back to the first (`rm B; cp A B`). This way, both files have different inodes and can be labeled accordingly.
2. Both files are used for the same purpose; in this case, it might be better to label the file which would not be labeled correctly (say a binary somewhere in a /usr/lib64/ location) using `semanage` (`semanage fcontext -a -t correct_domain_t /usr/lib64/path/to/file`)

It is also not a bad idea to report (after verifying if it hasn't been reported first) this on [Gentoo's bugzilla](https://bugs.gentoo.org) so that the default policies are updated accordingly.

### During package installation, ld.so complains 'object 'libsandbox.so' from LD\_PRELOAD cannot be preloaded: ignored'

During installation of a package, you might see the following error message:

This message should *only* occur after the *Setting SELinux security labels* message. It happens because SELinux tells glibc to disable `LD_PRELOAD` (and other environment variables that are considered potentially harmful) during domain transitions. Here, portage calls the `setfiles` command (part of a SELinux installation) and as such transitions from `portage_t` to `setfiles_t`, which clears the environment variable.

We believe that it is safer to trust the SELinux policy here (as setfiles runs in its own confined domain anyhow) rather than updating the policy to allow transitioning between `portage_t` to `setfiles_t` without clearing these environment variables. Note that *libsandbox.so is not disabled during builds and merges*, only during the activity where Portage labels the files it just merged.

So the error is in our opinion cosmetic and can be ignored (but sadly not hidden).

### Emerge does not work, giving 'Permission denied: /etc/portage/make.conf'

This is to be expected if you are not using the `sysadm_r` role. Any Portage related activity requires that you are in the `sysadm_r` role. To transition to the role, first validate if you are currently known as `staff_u` (or, if you added your own SELinux identities, a user that has the permission to transition to the `sysadm_r` role). Then run `newrole -r sysadm_r` to transition.

`user $``emerge --info`
Permission denied: '/etc/portage/make.conf'

`user $``id -Z`
staff\_u:staff\_r:staff\_t

`user $``newrole -r sysadm_r`
Password:

This is also necessary if you logged on to your system as root but through SSH. The default behavior is that SSH sets the lowest role for the particular user when logged on. And you shouldn't allow remote root logins anyhow.

### Cron fails to load in root's crontab with message '(root) ENTRYPOINT FAILED (crontabs/root)'

When you hit the mentioned error with a root crontab or an administrative users' crontab, but not with a regular users' crontab, then check the context of the crontab file:

`root #``ls -Z /var/spool/cron/crontabs/root`
staff\_u:object\_r:user\_cron\_spool\_t /var/spool/cron/crontabs/root

Next, check what the default context is for the given user (in this case, root) when originating from the `crond_t` domain:

`root #``getseuser root system_u:system_r:crond_t`
seuser:  root, level (null)
Context 0       root:sysadm\_r:cronjob\_t
Context 1       root:staff\_r:cronjob\_t

As you can see, the default context is always for the `root` SELinux user. However, the /var/spool/cron/crontabs/root file context in the above example is for the SELinux user `staff_u`. Hence, cron will not be able to read this file (the `user_cron_spool_t` type is a UBAC constrained one).

To fix this, change the user of the file to root:

`root #``chcon -u root /var/spool/cron/crontabs/root`
Another workaround would be to disable UBAC completely. This is accomplished with `USE="-ubac"`.

### When querying the policy, I get 'ERROR: could not find datum for type ...'

When using `seinfo` or `sesearch` to query the policy on the system, you get errors similar to:

`root #``seinfo -tasterisk_t`
ERROR: could not find datum for type asterisk\_t

This is most likely because your tools are using a newer binary policy to enforce policy, but an older binary for querying. You can verify if this is the case by listing the last modification time on the files:

`root #``ls -ltr /etc/selinux/mcs/policy/policy.*`
The file modified last should be the same one as returned by checking /sys/fs/selinux/policyvers:

`root #``cat /sys/fs/selinux/policyvers; echo`
24

If this is not the case (which is very likely since you are reading this FAQ entry) then try forcing the utilities policy version to the correct version:

**`/etc/selinux/semanage.conf`**

**Force the SELinux policy version**

### Portage fails to label files because "setfiles" does not work anymore

Portage uses the `setfiles` command to set the labels of the files it installs. However, that command is a dynamically linked executable, so any update in its depending libraries (libselinux.so, libsepol.so, libaudit.so and of course libc.so) might cause for the application to fail. Gentoo's standard solution (`revdep-rebuild`) will not work, since the tool will try to rebuild [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils), which will fail to install because Portage cannot set the file labels.

The solution is to rebuild [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils) while disabling Portage's SELinux support, then label the installed files manually using `chcon`, based on the feedback received from `matchpathcon`.

`root #````
FEATURES="-selinux" emerge --oneshot policycoreutils
```
`root #``for FILE in $(qlist policycoreutils); do CONTEXT=$(matchpathcon -n ${FILE}); chcon ${CONTEXT} ${FILE}; done`
Now Portage will function properly again, labeling files as they should.

### Applications do not transition on a nosuid-mounted partition

If you have file systems mounted with the `nosuid` option, then applications started from these file systems will not transition into their appropriate domain. This is intentional.

So, a `passwd` binary, although correctly labeled `passwd_exec_t`, will not transition into the `passwd_t` domain if the binary is stored on a file system mounted with `nosuid`.

### Why do I always need to re-authenticate when operating init scripts?

When you, as an administrator, want to launch or stop daemons, these activities need to be done as `system_u:system_r`. Switching to this context set is a highly privileged operation (since you are effectively leaving the user context and entering a system context) and hence the default setup requires the user to re-authenticate.

You can ask not to re-authenticate if you use PAM by editing /etc/pam.d/run\_init and adding the following line on top:

**`/etc/pam.d/run_init`**

**Allowing password-less execution if the user is root**

With this in place, you can now prepend your init script activities with `run_init` and it will not ask for your password anymore:

`root #``run_init rc-service local status`
Authenticating swift.
 \* status: started

### How do I use SELinux with initramfs?

We fully support in enforcing mode with an initramfs image. Once the real init loads after the initramfs pivots, it will load the SELinux policy, and if /etc/selinux/config is set to enforcing, the kernel will automatically go into enforcing mode.

If you run SELinux on a production system and would not like to have attackers be able to switch back to permissive mode (even when they would have the necessary privileges otherwise), set the `secure_mode_policyload` boolean. When enabled, enforcing mode cannot be disabled anymore (until you reboot).

`root #``setsebool secure_mode_policyload on`
### Logons through xdm (or similar) fail

If you log on through sddm, gdm, lightdm, xdm, kdm, slim or any other graphical display/login manager, you might notice in permissive mode that your context is off, and in enforcing mode that you just cannot log on.

The reason of this is that PAM needs to be configured to include SELinux awareness in your session handling:

**`/etc/pam.d/somesession`**

**Enable SELinux support through PAM sessions**

Replicate the calls towards pam\_selinux.so in the various /etc/pam.d/gdm\* files (or similar depending on your graphical logon manager).

### What is SELinuxfs and where should it be?

The selinuxfs is, as the name suggest, the SELinux File System. It is a pseudo-filesystem, which means that it is represented through files and directories, but those files or directories are not on your disk, but generated by the Linux kernel every time you query it.

The file system is used by the SELinux utilities as an interface to query the SELinux state, maintained by the Linux kernel.

It lives at /sys/fs/selinux.

Historically (before libselinux-2.1.9), the mount point for the SELinux file system *had to be* /selinux. From libselinux-2.1.9 onwards, the default location where the file system is looked for is /sys/fs/selinux, although the library still falls back to the original /selinux location if it cannot find it at the new place.

### How do I reload all SELinux policy modules?

By default, Gentoo incrementally updates the SELinux policy. This is because the SELinux policy is modular, starting with a base.pp setting and then several individual SELinux policy modules. When you install a SELinux policy package, it first tries to load the individual SELinux policy module. If that fails however, it will try to (re)load the entire policy (base with all installed policy modules), akin to the following:

`root #````
cd /usr/share/selinux/mcs
```
`root #``semodule -b base.pp -i $(ls *.pp | grep -v unconfined | grep -v base.pp)`
Or, if you have unconfined domains:

`root #``semodule -b base.pp -i $(ls *.pp | grep -v base.pp)`
Failures that occur now usually mean that not all SELinux policy modules have been upgraded yet, or that there are locally created policies loaded which cannot coexist with the newly defined SELinux policies.

### Why can't I just './service start' when I am inside /etc/init.d?

For the more verbose explanation, see [bug #448292](https://bugs.gentoo.org/show_bug.cgi?id=448292). Basically, running services requires a few domain transitions, assisted by the `run_init` command. This command first changes the working directory to / before actually running the requested service. As a result, it tries to execute `./service start` from within / instead of /etc/init.d, resulting in an execution failure.

`root #``cd /etc/init.d; ./sshd status`
Authenticating root.
Exec:: No such file or directory

### What is the run\_init\_t domain for?

When user domains, such as sysadm\_t, want to execute a service script, the script itself should not only run in the initrc\_t domain, but also with the full system context (so system\_u:system\_r:initrc\_t).

Now, switching the SELinux user and role to this (privileged) context should only be done by well controlled steps. Hence, we introduce the run\_init\_t domain, as target for executing any init script, which still runs in the user context (sysadm\_u:sysadm\_r:run\_init\_t. Then, the OpenRC code (through the openrc-run command) will transition towards the proper initrc\_t domain.

Only the run\_init\_t domain is capable of transitioning towards the initrc\_t domain (and change SELinux user and role). This way, we don't need to grant this privilege to the wide user domain sysadm\_t.

In the audit logs, a reference can occur about the open\_init\_pty. That is to be expected, it is the openrc-run command which invokes either open\_init\_pty to transition to initrc\_t, or the service script file itself.
