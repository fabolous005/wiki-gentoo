<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Working_with_customizable_types | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Working with customizable types -->
---
title: SELinux/Tutorials/Working with customizable types
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Working_with_customizable_types
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: "744bd1a04ee1250"
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Working with customizable types

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Working with customizable types

In a previous tutorial, we introduced **restorecon** and noted that it will not reset the context of a file whose context has a *customizable type*, unless the **-F** option (*force*) is given. In this tutorial we will cover the purpose and uses of *customizable types*.

### What are customizable types used for

SELinux *customizable types* are types which are meant to persist through a standard relabel operation (whether through **restorecon** or through a complete system relabel operation). Because of this behavior, such contexts are most frequently used on files whose location is not fixed on the system. Because their location is not fixed,   the policy writer cannot use a context mapping definition to manage the file context.

On SELinux systems, a list of customizable types is kept in /etc/selinux/\*/contexts/customizable\_types:

`user $``cat /etc/selinux/targeted/contexts/customizable_types`
sandbox\_file\_t
svirt\_image\_t
virt\_content\_t
httpd\_user\_htaccess\_t
httpd\_user\_script\_exec\_t
httpd\_user\_content\_ra\_t
httpd\_user\_content\_rw\_t
httpd\_user\_content\_t
git\_session\_content\_t
home\_bin\_t

So if you have a script in a home directory (currently labeled *user\_home\_t*) and you change the context of this file towards *home\_bin\_t*, then a relabel of this file (be it directly or through a recursive relabeling operation against the entire home directory) will not change the context back from *home\_bin\_t* to *user\_home\_t*.

`user $``ls -Z getinfo.sh`
-rwxr-x---. swift users user\_u:object\_r:user\_home\_t:s0 getinfo.sh

`user $````
chcon -t home_bin_t getinfo.sh
```
`user $``ls -Z getinfo.sh`
-rwxr-x---. swift users user\_u:object\_r:home\_bin\_t:s0 getinfo.sh

`user $````
restorecon -v getinfo.sh
```
`user $``ls -Z getinfo.sh`
-rwxr-x---. swift users user\_u:object\_r:home\_bin\_t:s0 getinfo.sh

### Marking types as customizable

The list of customizable types is considered part of the SELinux policy provided by your distribution. So, even though you can edit the aforementioned customizable\_types yourself, these changes will be overwritten the next time the system policy package is updated. If you really need to get a type marked as customizable, contact your distribution's policy developers to discuss the issue.

### Allowing users to change the security context of files

We've said that *customizable types* were created in order to deal with the issue of security contexts on files which do not live in predetermined locations. The obvious example for such files are those that live in user's home directories. Policy writers generally have no way to know which files a user will have, and what context they should have. Instead, they make *customizable types* available to users.

But this raises a new issue. So far, all context changes we've seen were performed by the root user. Indeed, the security context of a file, like setting file ownership, is considered a privileged operation. However, unsurprisingly, SELinux also provides fine-grained control (in the form of permission) about who gets to relabel which files to what context. Thus, you will find that the system policy usually contains some which allow regular users to set the security context for certain files, such as files in their home directory. Of course, users aren't to make changes to the context mappings on the system using **semanage fcontext**, and instead they must use **chcon**. Since *Customizable Types* persist through relabeling (with **restorecon**, for example), if the user sets the context of a file to one such *customizable type*, the change is effectively permanent (at least until the user changes it).

The *relabelfrom* and *relabelto* permissions are those which control the ability to perform relabeling.
You will likely find that your system's Policy grants these permissions to the *user\_t* domain (the domain in which regular user processes usually run) in relation to types such as *user\_home\_t* or *home\_bin\_t* files ( more generally, types with the *user\_home\_type* attribute). It's these permissions that allow users to use '*chcon* to set certain types (customizable or not) on files they control.

To see examples of relabel permission granted to users on your system's policy, examine the output of the following commands:

`user $````
sesearch -s user_t -t user_home_t -c file -p relabelfrom -A
```
`user $``sesearch -s user_t -t home_bin_t -c file -p relabelto -A`
### Overriding customizable contexts (hard resets)

When you need to override the customizable contexts, you can use the **-F** (which stands for *force*) option with **restorecon**. This not only resets the type back to the context mapping definition, but it also resets all the other fields of the context accordingly. The purpose of context fields other than the *type* will be explained in a later tutorial.

`root #``restorecon -RF /home/swift`
It is *not recommended* to perform a force relabel against all user home directories on a system, where other users' files are. These users will have made use of customizable types on files in their home directories (such as marking files as *httpd\_user\_content\_t* so that the web server can display the content), and resetting the contexts for these files will undo their changes and will likely provoke ire.

### What you need to remember

- customizable types exist for files and resources that have no fixed location on a file system
- the list of current customizable types can be found in /etc/selinux/\*/contexts/customizable\_types
- the context of files with a customizable type context can be reset if you use the *force* (**-F**) option during relabel operations
