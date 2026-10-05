<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Managing_network_port_labels | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Managing network port labels -->
---
title: SELinux/Tutorials/Managing network port labels
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Managing_network_port_labels
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: "723c8871841481a1"
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Managing network port labels

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Managing network port labels

We have discussed access controls on files and directories before, but there are numerous other resources that SELinux governs. Some of these are network related resources, such as network ports. This becomes relevant when you, for instance, change the default port that a daemon listens on.

### Example: the SSH daemon

Let's consider the SSH daemon, moving it from port 22 to 1122 through the /etc/sshd/sshd\_config file. When we then restart the SSH daemon, it fails to bind on the port, citing a *Permission denied*. In the SELinux logs, we notice the following:

```
type=AVC msg=audit(1364718726.658:74): avc: denied { name_bind } for pid=17021 comm="sshd"
src=1122 scontext=system_u:system_r:sshd_t tcontext=system_u:object_r:unreserved_port_t tclass=tcp_socket
```
So what is this all about? Isn't *sshd\_t* allowed to bind on a tcp port?

That's not it. The *unreserved\_port\_t* type is a domain assigned to ports that are not reserved for particular services in the SELinux policy. And that implies that ports that are assigned for a particular service are named differently.

### SELinux port labels

Within SELinux, ports are labeled, just like other resources. The **semanage port** command displays the rules for port assignment.

`root #``semanage port -l`
SELinux Port Type              Proto    Port Number
afs3\_callback\_port\_t           tcp      7001
afs3\_callback\_port\_t           udp      7001
afs\_bos\_port\_t                 udp      7007
...
ssh\_port\_t                     tcp      22
...
unreserved\_port\_t              tcp      1024-65535
unreserved\_port\_t              udp      1024-65535

The third column gives the port number, or port number range. If it is a range, it has a lower priority than a specific port number (so in case of port 7001, the port type will be *afs3\_callback\_port\_t* and not *unreserved\_port\_t*). Also, if none of the rules match, then the port falls back to *port\_t*.

You'll also notice that the second column reveals the protocol for which the rule takes effect. In case of SSH, if a daemon would like to bind to port 22, but with the UDP protocol (and not TCP) then SELinux will look for an allow on *port\_t*, not *ssh\_port\_t*.

### Assigning names on other ports

When we update daemons to run on different ports, there are two things we can do in order for SELinux to allow this.

1. Either we allow the domain additional access rules (on other port labels), or
2. we assign the label to the new port.

The first choice requires a policy update (which we will discuss as part of the second series of tutorials) and is most of the time not what you need. After all, allowing the domain rights on other ports is actually increasing the rights of the domain, whereas we don't want to increase the rights - only change the port.

The second choice is often the preferred - and most simple. With **semanage**, we can map a port label on a different port. Let's assign *ssh\_port\_t* to port 1122.

`root #``semanage port -a -t ssh_port_t -p tcp 1122`
This is exactly what the previous example needed to get SSH to work again.

### What you need to remember

What you should remember from this tutorial is that

- SELinux also labels udp/tcp ports as it is one of the many resources it governs
- you can assign existing labels to other ports using **semanage port**
