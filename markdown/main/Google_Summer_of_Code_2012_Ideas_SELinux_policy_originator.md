<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/SELinux_policy_originator | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/SELinux policy originator -->
---
title: Google Summer of Code/2012/Ideas/SELinux policy originator
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/SELinux_policy_originator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: ef68cdce93fe6ed2
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/SELinux policy originator

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo Hardened is maturing its SELinux support rapidly. In SELinux, policies are written in a higher abstract format (dictated by the reference policy) and converted to the SELinux-specific rules (like allow, dontaudit, type transitions, etc.). For troubleshooting rights however, it is a very daunting task to find out why a particular rule is set (in other words, to find which higher level rule is causing the SELinux rule to exist).

The rules are converted in M4 language from constructs like "corenet\_tcp\_bind\_http\_port" to rules like:

- allow $1 http\_port\_t:tcp\_socket name\_bind
- allow $1 self:capability net\_bind\_service

It is the latter that end users can easily see (with tools such as *sesearch*) but it is not easy to find that "allow openvpn\_t http\_port\_t:tcp\_socket name\_bind" comes from "corenet\_tcp\_bind\_http\_port(openvpn\_t)" defined in the openvpn.te definition.

In this idea, we would like to find a way to register where these lines come from to improve debugging and troubleshooting



| Contacts | Required Skills | 
|---|---|
|  |  |
