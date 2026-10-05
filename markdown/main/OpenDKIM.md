<!-- source: https://wiki.gentoo.org/wiki/OpenDKIM | group: Gentoo Wiki (Main) | wiki-title: OpenDKIM -->
---
title: OpenDKIM
url: https://wiki.gentoo.org/wiki/OpenDKIM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-21"
fingerprint: e2382dd2bd0c9e44
license: CC BY-SA 4.0
---

# OpenDKIM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Sharing a local socket with the MTA

### Background

Using a local socket to communicate *securely* with an MTA requires some subtle configuration. The four security goals achieved are:

1. Allow OpenDKIM to read the DKIM signing keys.
2. Allow the MTA to read and write to a shared socket file.
3. Do **not** allow the MTA to read the DKIM signing keys.
4. Don't allow anyone other than root to modify the signing keys.

In recent versions of [mail-filter/opendkim](https://packages.gentoo.org/packages/mail-filter/opendkim), the opendkim daemon runs as the opendkim user and group. The signing keys should be,

1. Located under /var/lib/opendkim
2. Owned by root, with group opendkim
3. Have mode 640

Taken together, these imply that the opendkim group (including the daemon) can read the DKIM signing keys, but not write to them. The problem of the socket is now, essentially: how to share the socket file between the opendkim user and the MTA, without allowing the MTA to read the opendkim group's files? The usual approach here would be to add the MTA to the opendkim group, and then allow that group to write to the socket file. However, doing so would allow the MTA to read the DKIM signing keys in this case, and that violates one of our security goals.

### Solution

The solution to this problem is to create a new, dedicated group that is used only to control access to the socket. For example, you might

1. Create a new dkimsocket group.
2. Add the opendkim user to the dkimsocket group.
3. Add the MTA to the dkimsocket group.
4. Change the umask in /etc/opendkim/opendkim.conf to allow group-write.

This **almost** does what we want, except for one critical pitfall: the socket gets created by the opendkim user, and as a result, it gets created with that user's primary group, opendkim. Since the socket's group isn't dkimsocket, our trick has failed! However, all is not lost: we can **change the primary group of the opendkim user to dkimsocket**, after which the socket will be created with the correct group. With this one crucial modification, everything works as desired.

### Example

Below is a step-by-step example of sharing a local socket with Postfix, running as the postfix user.

First, edit /etc/opendkim/opendkim.conf to specify the name of a local socket. On Gentoo, the local socket should be located under /run/opendkim, because the permissions on that directory are set correctly at boot time.

**`/etc/opendkim/opendkim.conf`**

Then, ensure that the UMask is set correctly in /etc/opendkim/opendkim.conf (the ebuild does this on install) so that the socket gets created with group-writable permissions:

**`/etc/opendkim/opendkim.conf`**

Finally, create and configure the dedicated dkimsocket group. [GLEP 81](https://www.gentoo.org/glep/glep-0081.html) changed the way that users and groups are managed on Gentoo.

#### The new way

In a local overlay, create an ebuild for the dkimsocket group:

**`acct-group/dkimsocket/dkimsocket-0.ebuild`**

Then, create newer revisions of the postfix and opendkim user ebuilds that make use of the new group:

**`acct-user/postfix/postfix-0-r4.ebuild`**

**`acct-user/opendkim/opendkim-0-r4.ebuild`**

Now those two packages will take precedence over the ones in ::gentoo, ensuring that the users on the system are a member of dkimsocket.

#### The old way

Create the dkimsocket that will control access to the socket, and add the postfix user to it:

`root #````
groupadd dkimsocket
```
`root #````
usermod --append --groups dkimsocket postfix
```
Next, **switch** the primary group of the opendkim user to dkimsocket, and then *append* the opendkim group back:

`root #````
usermod --gid dkimsocket opendkim
```
`root #````
usermod --append --groups opendkim opendkim
```
With that, OpenDKIM is ready to start.
