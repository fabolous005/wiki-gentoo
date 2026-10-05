<!-- source: https://wiki.gentoo.org/wiki/Set_Keyboard_Repeat_Delay_and_Rate | group: Gentoo Wiki (Main) | wiki-title: Set Keyboard Repeat Delay and Rate -->
---
title: Set Keyboard Repeat Delay and Rate
url: https://wiki.gentoo.org/wiki/Set_Keyboard_Repeat_Delay_and_Rate
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-07-10"
fingerprint: "73b42c96adaa3390"
license: CC BY-SA 4.0
---

# Set Keyboard Repeat Delay and Rate

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

If you are working with [Vim](https://wiki.gentoo.org/wiki/Vim) for example, navigating through text can be much faster with a higher autorepeat rate.

### Generic X11

You can use the [xset](https://wiki.gentoo.org/index.php?title=Xset&action=edit&redlink=1) command to set 400 milliseconds delay and 100 repeat rate.

`user $``xset r rate 400 100`
### KDE Plasma

Open **System Settings** and navigate to **Input Devices** module, then **Keyboard**. Set **Delay** and **Rate** to your preferred values.
