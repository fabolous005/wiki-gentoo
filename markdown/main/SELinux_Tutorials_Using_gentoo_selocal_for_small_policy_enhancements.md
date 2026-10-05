<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Using_gentoo_selocal_for_small_policy_enhancements | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Using gentoo selocal for small policy enhancements -->
---
title: SELinux/Tutorials/Using gentoo selocal for small policy enhancements
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Using_gentoo_selocal_for_small_policy_enhancements
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: f2be6c6744cf083a
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Using gentoo selocal for small policy enhancements

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Using Gentoo selocal for small policy enhancements

In a previous tutorial, we explained how to create your own policy module. However, when you want to make small changes here and there, this might seem like too much overhead. To allow for small changes to be made to the policy, Gentoo introduced the **selocal** command which manages a local policy module (called "selocal") where you can add and remove SELinux policy rules to.

### Using selocal

As the name implies, **selocal** aims to provide an interface for handling local policy updates that do not need to be packaged or distributed otherwise. It is a command-line application that you feed single policy rules at one at a time. Each rule can be accompanied with a single-line comment, making it obvious for the user to know why he added the rule in the first place.

`root #``selocal --help`
Usage: selocal \[\<command>\] \[\<options>\] \<rule|line> 
Command can be one of:
  -l, --list            List the content of a SELinux module
  -a, --add             Add an entry to a SELinux module
  -d, --delete          Remove an entry from a SELinux module
  -M, --list-modules    List the modules currently known by selocal
  -u, --update-dep      Update the dependencies for the rules
  -b, --build           Build the SELinux module (.pp) file (requires privs)
  -L, --load            Load the SELinux module (.pp) file (requires privs)
Options can be one of:
  -m, --module \<module>         Module name to use (default: selocal)
  -c, --comment \<comment>       Comment (with --add)
The option -a requires that a rule is given, like so:
  selocal -a "dbadm\_role\_change(staff\_r)"
The option -d requires that a line number, as shown by the --list, is given, like so:
  selocal -d 12

Let’s say that you need to launch a small script you written as a daemon, but you want this to run while you are still in the staff\_t domain (it is a user-sided daemon you use personally). As regular staff\_t isn’t allowed to have processes bind on generic ports/nodes, you need to enhance the SELinux policy a bit. With **selocal**, you can do so as follows:

`root #````
selocal --add "corenet_tcp_bind_generic_node(staff_t)" --comment "Launch local webserv.py daemon"
```
`root #````
selocal --add "corenet_tcp_bind_generic_port(staff_t)" --comment "Launch local webserv.my daemon"
```
`root #``selocal --build --load`
When finished, the local policy is enhanced with the two mentioned rules. You can query which rules are currently stored in the policy:

`root #``selocal --list`
12: corenet\_tcp\_bind\_generic\_node(staff\_t) # Launch local webserv.py daemon
13: corenet\_tcp\_bind\_generic\_port(staff\_t) # Launch local webserv.py daemon

If you need to delete a rule, just pass the line number:

`root #``selocal --delete 13`
Having this tool around also makes it easier to test out changes suggested through bugreports as well.

Underlyingly, selocal creates and maintains the necessary policy file in \~/.selocal and by default uses the selocal policy module name.

### What you need to remember

What you should remember from this tutorial is that

- Gentoo provides a **selocal** script to easily manage small enhancements
- The **selocal** script by default manages the *selocal* policy module
