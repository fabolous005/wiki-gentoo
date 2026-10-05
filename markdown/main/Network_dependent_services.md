<!-- source: https://wiki.gentoo.org/wiki/Network_dependent_services | group: Gentoo Wiki (Main) | wiki-title: Network dependent services -->
---
title: Network dependent services
url: https://wiki.gentoo.org/wiki/Network_dependent_services
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-27"
fingerprint: "925131991d08abe9"
license: CC BY-SA 4.0
---

# Network dependent services

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Services like netmount or fetchmail which depend on an available network connection could be started/stopped with the following  setup:[\[1\]](https://wiki.gentoo.org#cite_note-bug522206-1)

### Precondition

Precondition is that [network management](https://wiki.gentoo.org/wiki/Network_management) is done with [dhcpcd](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD).

### Implementation

Get the patch for dhcpcd:[\[2\]](https://wiki.gentoo.org#cite_note-2)

`root #``mkdir -p /etc/portage/patches/net-misc/dhcpcd-6.9.0`
Get the patch for openrc:[\[3\]](https://wiki.gentoo.org#cite_note-3)

`root #``mkdir -p /etc/portage/patches/sys-apps/openrc-0.13.11`
Re-emerge both dhcpcd and openrc:

`root #``emerge -1avt =net-misc/dhcpcd-6.6.7 =sys-apps/openrc-0.13.11`
Add one line **start\_inactive=true** to the init script:[\[4\]](https://wiki.gentoo.org#cite_note-4)

`root #``patch /etc/init.d/dhcpcd < attachment.cgi?id=384410`
Restart dhcpcd:

`root #``/etc/init.d/dhcpcd restart`
Later versions than the presently stable of net-misc/dhcpcd and sys-apps/openrc have not been tested with these patches.  It does not work for [sys-apps/openrc-0.16.4](https://bugs.gentoo.org/show_bug.cgi?id=522206#c38).

### Result

Services having "need net" in their init.d scripts like fetchmail would then start after dhcpcd is started.

**`/etc/init.d/fetchmail`**

```
#!/sbin/runscript
piddir=${pid_dir:-/var/run/fetchmail}
pid_file=${piddir}/${RC_SVCNAME}.pid
rcfile=/etc/${RC_SVCNAME}rc
depend() {
        need net
        use mta
}
```
`root #````
eselect rc start fetchmail
```
Starting init script
dhcpcd          \* Starting DHCP Client Daemon ...
fetchmail       \* WARNING: fetchmail is scheduled to start when dhcpcd has started            \[ ok \]

They will be stopped when dhcpcd turns inactive and will be restarted when dhcpcd is back.

`user $``rc-config show default`
dhcpcd                    \[inactive\]
  fetchmail                 \[stopped\]

This should be sufficient for most end user computers. For more complex requirements in dependency behaviour see [OpenRC#Dependency\_behaviour](https://wiki.gentoo.org/wiki/OpenRC#Dependency_behaviour).



## References

1. [↑](https://wiki.gentoo.org#cite_ref-bug522206_1-0) [Bug 522206 – net-misc/dhcpcd-6.4.3 fails to start/stop network dependant services like ntpd, sshd, fetchmai](https://bugs.gentoo.org/show_bug.cgi?id=522206), [Gentoo's Bugzilla Main Page](https://bugs.gentoo.org/), (Last modified) April 9th, 2015. Retrieved on May 7th, 2015.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [99-openrc\_dhcpcd\_hook.patch, updated for epatch\_user](https://bugs.gentoo.org/show_bug.cgi?id=522206#c37), [Gentoo's Bugzilla Main Page](https://bugs.gentoo.org/), May 8th, 2015. Retrieved on May 10th, 2015.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [runscript-background.patch, updated for epatch\_user](https://bugs.gentoo.org/show_bug.cgi?id=522206#c36), [Gentoo's Bugzilla Main Page](https://bugs.gentoo.org/), May 8th, 2015. Retrieved on May 10th, 2015.
4. [↑](https://wiki.gentoo.org#cite_ref-4) Roy Marples. [Mark the dhcpcd service as starting inactive](https://bugs.gentoo.org/show_bug.cgi?id=522206#c3), [Gentoo's Bugzilla Main Page](https://bugs.gentoo.org/), September 8th, 2014. Retrieved on May 7th, 2015.
