<!-- source: https://wiki.gentoo.org/wiki/SELinux/Unconfined_domains | group: Gentoo Wiki (Main) | wiki-title: SELinux/Unconfined domains -->
---
title: SELinux/Unconfined domains
url: https://wiki.gentoo.org/wiki/SELinux/Unconfined_domains
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-27"
fingerprint: e4f905770612edc0
license: CC BY-SA 4.0
---

# SELinux/Unconfined domains

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux uses a *deny by default* approach for its mandatory access control rules. That means that any use of SELinux requires the entire system to be modeled in the SELinux policy. That might be difficult to obtain if a multitude of systems need to have SELinux deployed, even though the protection measures are only needed for a small set of "domains". To enable such policies, SELinux has introduced the concept of *unconfined domains*.

## Introduction

An *unconfined domain* is a regular SELinux domain, but with massive privileges assigned to it. So although it is called *unconfined*, it is still managed by SELinux - however, almost all possible privileges are assigned to the unconfined domain, effectively having SELinux grant all access done through the unconfined domain.

### Unconfined users

An *unconfined user* is a regular user who is mapped to a SELinux user (usually `unconfined_u`) with only one role (`unconfined_r`) and has `unconfined_t` as the default login type.

Every action performed by the user is done in the `unconfined_t`, which is granted all the privileges ever needed (and more).

In the following example, we map the *john* Linux account to the *unconfined\_u* SElinux user, placing their user session in the unconfined domain:

`root #``semanage login -a -s unconfined_u john`
All Linux users mapped to the *unconfined\_u* SELinux user can be listed using **seinfo**:

`root #``seinfo -u unconfined_u -x`
While less secure than a confined user session, a user session in the unconfined domain is more much more flexible, as policy for each application run as that user (which does not exist for every application out there) is likely not needed.

### Unconfined application and daemon domains

However, it isn't sufficient to just consider `unconfined_t` as being very "liberal" in the allowed privileges. Domains can be extended with the `unconfined_domain` interface (which, amongst various other additional privileges, also "tags" the domains with the `unconfined_domain_type` attribute.

Querying which domains are assigned this attribute helps in identifying which services are, even though still SELinux-managed, very widely privileged.

`root #``seinfo -aunconfined_domain_type -x`
