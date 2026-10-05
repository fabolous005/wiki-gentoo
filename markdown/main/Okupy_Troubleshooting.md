<!-- source: https://wiki.gentoo.org/wiki/Okupy/Troubleshooting | group: Gentoo Wiki (Main) | wiki-title: Okupy/Troubleshooting -->
---
title: Okupy/Troubleshooting
url: https://wiki.gentoo.org/wiki/Okupy/Troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: "50d286bc5ab26adb"
license: CC BY-SA 4.0
---

# Okupy/Troubleshooting

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## LDAP connection

In a *python* terminal:

```
import ldap
l = ldap.initialize('ldap://evidence.tampakrap.gr')
l.start_tls_s()
l.simple_bind_s()
l.search_s('ou=users,dc=tampakrap,dc=gr', ldap.SCOPE_SUBTREE)
```
In a *./manage.py shell* terminal:

from okupy.accounts.models import LDAPUser
users = LDAPUser.objects.all()
wolverine = LDAPUser.objects.get(username='wolverine')
