<!-- source: https://wiki.gentoo.org/wiki/Procmail | group: Gentoo Wiki (Main) | wiki-title: Procmail -->
---
title: procmail
url: https://wiki.gentoo.org/wiki/Procmail
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-10"
fingerprint: "20b227ba0253bb8a"
license: CC BY-SA 4.0
---

# procmail

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


## Tips

Here a few tricks which could be used by Gentoo developers and other procmail users to clean up / sort excessive mail.

Please note that in order for any of the listed tips to work, procmail has to be enabled first. On dev.gentoo.org, this is done through setting the *\~/.forward* file:

**`~/.forward`**

**Enabling procmail**

### Removing duplicate (CC) mails from mailing lists

When replies to the mailing list CC people that are subscribed, they often get multiple copies of the same mail. To filter out the duplicates the following procmail rule can be used:

**`~/.procmailrc`**

**Filtering out direct mails addressed to various mailing lists**

In this case, mails which were addressed to the particular mailing list but didn't come from the mailing list directly are moved directly to *Trash* folder. Alternatively, */dev/null* can be used to discard them completely.

### Watching commits to chosen packages

Often it is useful to watch the commits done to your packages and/or eclasses, or any other CVS location relevant to you. In order to achieve that, you can subscribe to the gentoo-commits [mailing list](https://www.gentoo.org/get-involved/mailing-lists/) and then set up procmail rules to filter out irrelevant paths:

**`~/.procmailrc`**

**Filtering out gentoo-commits by paths**
