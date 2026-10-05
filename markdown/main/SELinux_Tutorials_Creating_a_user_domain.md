<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_a_user_domain | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Creating a user domain -->
---
title: SELinux/Tutorials/Creating a user domain
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_a_user_domain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-26"
fingerprint: e4b80c1b08d73bc4
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Creating a user domain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Creating a user domain

In this tutorial the focus is on creating a user domain. By default, SELinux on Gentoo comes with a number of SELinux users and roles, but more can be added to tailor the SELinux permissions to the purpose of the system. In this tutorial, a mail administrator user will be created that has administrative rights on the Postfix infrastructure.

### Creating the user module

The first step is to create a user module, in this case mailadm.te. This module will contain the permissions to be granted to the user:

**`mailadm.te`**

Build and load the module and verify that the user domain and role now exists:

`root #``make mailadm.pp`
Compiling mcs mailadm module
/usr/bin/checkmodule:  loading policy configuration from tmp/mailadm.tmp
/usr/bin/checkmodule:  policy configuration loaded
/usr/bin/checkmodule:  writing binary representation (version 17) to tmp/mailadm.mod
Creating mcs mailadm.pp policy package
rm tmp/mailadm.mod.fc tmp/mailadm.mod

`root #``seinfo -rmailadm_r`
mailadm\_r

`root #``seinfo -tmailadm_t`
mailadm\_t

### Update context and type information

Before assigning the new user domain to a real Linux account, it is important to first configure SELinux contexts and types for it.

The first file we need to update is /etc/selinux/mcs/contexts/default\_type (substitute *mcs* with the policy store name that is active on the system). In it, SELinux is told what the default type is when a role is selected. In this case, when `mailadm_r` is selected, `mailadm_t` should be the default type:

**`/etc/selinux/mcs/contexts/default_type`**

The second file is one to create inside /etc/selinux/mcs/contexts/users. The simplest method is to copy an existing user file (this is for SELinux users) and modify it for the new role.

`root #``sed -e 's|user|mailadm|g' user_u > mailadm_u`
### Assign mailadm to the right Linux account

Finally, assign the newly created role to a Linux account.

First, create a SELinux user on which the role and type are mapped. This user is `mailadm_u` (which is also the name of the file created in /etc/selinux/mcs/contexts/users previously.

`root #``semanage user -a -R "mailadm_r system_r"`
Next, map this user to the Linux account. For instance, if the Linux account is `user1234` then this is accomplished like so:

`root #``semanage login -a -s mailadm_u user1234`
Finally, reset the contexts of this users' home directory:

`root #``restorecon -RvF /home/user1234`
### What to remember

Creating a SELinux user is a matter of the following simple steps:

1. Create the module which adds in the rights
2. Create the SELinux user which is allowed the role(s) previously created
3. Update the SELinux contexts to recognize the new user
4. Map the Linux account(s) to the new SELinux user
