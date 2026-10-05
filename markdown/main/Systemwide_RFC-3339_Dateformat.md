<!-- source: https://wiki.gentoo.org/wiki/Systemwide_RFC-3339_Dateformat | group: Gentoo Wiki (Main) | wiki-title: Systemwide RFC-3339 Dateformat -->
---
title: Systemwide RFC-3339 Dateformat
url: https://wiki.gentoo.org/wiki/Systemwide_RFC-3339_Dateformat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-14"
fingerprint: "68a15078a6e5ee24"
license: CC BY-SA 4.0
---

# Systemwide RFC-3339 Dateformat

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### RFC-3339 date format and its friends

The [RFC-3339 standard](https://www.rfc-editor.org/rfc/rfc3339) is available for free in the public and widely used for software. The 
[ISO 8601 standard](https://en.wikipedia.org/wiki/ISO_8601) is very similar, but requires the 'T' between the day and the hours.
RFC-3339 allows to use a space or the 'T' respectively.



| Comparison of date standards |  |  | 
|---|---|---|
| Example | Allowed in Standard | date command | 
|---|---|---|
| 2023-08-13 | RFC-3339, ISO\_8601 | `user $``date --rfc-3339='date'` | 
| 2023-08-13T16:07:54+02:00 | RFC-3339, ISO\_8601 | `user $``date --iso-8601='seconds'` | 
| 2023-08-13 16:08:44+02:00 | RFC-3339 | `user $``date --rfc-3339='seconds'` | 
| 2023-08-13 16:08:44 +02:00 | RFC-3339 | `user $``date "+%F %T %:z"` | 
| 2023-08-13 16:08:44 | - | `user $``date "+%F %T"` | 



- [stackoverflow: What's the difference between ISO 8601 and RFC 3339 Date Formats?](https://stackoverflow.com/questions/522251/whats-the-difference-between-iso-8601-and-rfc-3339-date-formats)
- comparison between RFC 3339 and ISO 8601 [https://ijmacd.github.io/rfc3339-iso8601/](https://ijmacd.github.io/rfc3339-iso8601/)

## System wide RFC-3339 date

Several configuration files need adjustments to achieve a system wide RFC-3339 date representation. Here we focus on the representation with space instead of 'T'.

### .profile

**`~/.profile`**

**.profile**

```
# Localization 
TIME_STYLE=long-iso #for ISO Date in ls"
```
### zsh

**`~/.zshrc`**

**.zshrc**

```
# Localization 
TIME_STYLE=long-iso #for ISO Date in ls"
```


### xfce panel clock

**`.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-panel.xml`**

**xfce panel clock with T**

```
<property name="digital-format" type="string" value="%Y-%m-%dT%H:%M %:z"/>
<property name="digital-time-format" type="string" value="%Y-%m-%dT%H:%M %:z"/>
```
**`.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-panel.xml`**

**xfce panel clock with space**

```
<property name="digital-format" type="string" value="%Y-%m-%d %H:%M %:z"/>
<property name="digital-time-format" type="string" value="%Y-%m-%d %H:%M %:z"/>
```
