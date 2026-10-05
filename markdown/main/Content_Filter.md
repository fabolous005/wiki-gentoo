<!-- source: https://wiki.gentoo.org/wiki/Content_Filter | group: Gentoo Wiki (Main) | wiki-title: Content Filter -->
---
title: Content Filter
author: Authentication; Authorization
url: https://wiki.gentoo.org/wiki/Content_Filter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: f600934367203871
license: CC BY-SA 4.0
---

# Content Filter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Planning

In order to build a successful content filter, some questions need answering. How many users will be filtered? What will be filtered? How will it be filtered?

The DansGuardian wiki has a good over view of [filter architecture](http://contentfilter.futuragts.com/wiki/doku.php?id=two_configuration_families).

## Single machines and small networks

For single machines and small networks I find that Tinyproxy and Dansguardian work best.

Tinyproxy is a very lightweight and easy to configure proxy server. Dansguardian actually does all the filtering work, but is unable to fetch pages by itself, this is where Tinyproxy comes in.

A user will open their web browser and request a web page, this request typically travels over port 80, we'll redirect port 80 to a port that Dansguardian is listening on, then Dansguardian will forward the request onto Tinyproxy, who will then actually go out and fetch the web page.

### Installing and configuring tinyproxy

By default a few USE flags are set for Tinyproxy, most are not needed for this guide but don't hurt either. However be sure that the **transparent-proxy** USE flag is **not** set. It is not compatible with this guide.

First, install Tinyproxy:

`root #``emerge --ask tinyproxy`
Next, back up the default configuration in case things go astray:

`root #``cp /etc/tinyproxy.conf /etc/tinyproxy.conf.vanilla`
Finally, open /etc/tinyproxy.conf in your favorite text editor, make sure you have suid root privileged. Make sure it looks something like this:

**`/etc/tinyproxy.conf`**

```
User tinyproxy
Group tinyproxy
Port 3128
Timeout 1200
DefaultErrorFile "/usr/share/tinyproxy/default.html"
LogFile "/var/log/tinyproxy/tinyproxy.log"
# change this to Info when debugging issues for more verbose output
LogLevel Error
# these settings can significantly effect the performance of the proxy
MaxClients 100
MinSpareServers 5
MaxSpareServers 20
StartServers 10
MaxRequestsPerChild 0
# for a single computer, chose this, otherwise remove it
Allow 127.0.0.1
# for a small network, chose the subnet in use, otherwise remove it
Allow 10.0.0.0/24
# you can leave this as is or change it to whatever you like, its a privacy thing
ViaProxyName "tinyproxy"
# the easiest, but least secure option for ConnectPort is to leave it commented
# this way all ports are allowed. each port you wish to allow should be listed on its own line
ConnectPort 445 # SSL
ConnectPort 563 # TLS
```
Start Tinyproxy by issuing this command:

`root #` `/etc/init.d/tinyproxy start`
Finally, add Tinyproxy to the start up list:

`root #` `rc-update add tinyproxy default`
### Installing and configuring DansGuardian

One thing you need to understand about DansGuardian is that it supports multiple filter groups, if you are interested you can read more about them in the large network section below. For the single machine or small network we'll set up a single filter group that everyone will be a member of.

First lets install DansGuardian:

`root #``emerge --ask dansguardian`
You may want/need to enable the pcre USE flag, this enables Perl Compatible Regular Expressions.

First, lets back up the default configuration, just to be safe:

`root #``cp /etc/dansguardian/dansguardian.conf /etc/dansguardian/dansguardian.conf.vanilla`
Now lets start configuring the global settings, for now just change the parameters here, leave everything else, it's a big file:

**`/etc/dansguardian/dansguardian.conf`**

Now that the global configuration has been taken care of, lets configure the per-group settings. If there is a conflict between per-group and global settings, per-group settings will always win; just keep that in mind while configuring and troubleshooting.

Again, lets make a backup of the defaults, just in case:

`root #``cp /etc/dansguardian/dansguardianf1.conf /etc/dansguardian/dansguardianf1.conf.vanilla`
Lets configure the first filter group. Again, this is a big file with lots going on, if a parameter isn't mentioned in this outline, usually its ok to leave it at default settings:

**`/etc/dansguardian/dansguardianf1.conf`**

Under /etc/dansguardian/lists are a set of files that control the mechanics of filtering. There is a lot going on here, usually the defaults are ok. Although a few files are worth noting individually.

- bannedsitelist - here, there is a section to explicitly list sites to be blocked.
- exceptionsitelist - similar to bannedsitelist, except for allowing sites rather then blocking.
- contentregexlist - this is a rather hairy file, especially if you have never worked with regular expressions before, enabling too many of these options often causes over filter problems and performance problems.

Unfortunately, the only official documentation on how to use these lists are the comments in the list files themselves.

Start DansGuardian by issuing this command:

`root #` `/etc/init.d/dansguardian start`
Finally, add DansGuardian to the start up list:

`root #` `rc-update add dansguardian default`
## Large networks

Larger networks pose a few challenges to administrators; the most prevalent being the sheer size of the network. With average enterprise class networks having thousands of users and thousands of machines with many subgroups requiring different levels of service, and the new wave of *Bring Your Own Device* or BYOD networks, the big question is; how to handle all this traffic without making compromises in what is filtered?

DansGuardian answers this question with filter groups. With filter groups you'll need some external method of authentication so that you can apply different levels of authorization with filter groups, which is unfortunately outside of the scope of this guide. In this guide we'll look at some of the ideas and concepts behind authentication with DansGuardian but the implementation details will be left up to you or another guide.

As you read this guide pay special attention to the default group information located under the Filter Group section. It is a powerful tool that catches everyone who hasn't authenticated.

### Installing and configuring squid

Make sure that the **tproxy** USE flag is **not** set.

You may wish to conciser setting these USE flags:

- logrotate - if you are already using logrotate to clean up old logs, you can include Squid's logs in the process via USE flag
- kerberos, ldap, nis, radius, samba, and sasl - these flags make integrating Squid with existing authentication schemes a little easier, not all are required

To install Squid, execute this command:

`root #``emerge --ask squid`
The Squid config is just as (if not more so) big and scary as the DansGuardian config. Each network has its own set of needs and generalizing all of those into one perfect configuration is impossible. Instead, this configuration is meant to give you the most basic set up to get you running while explaining some core Squid configuration topics. Fine tuning the Squid configuration to suit a specific requirement is up to the individual administrator. For a much closer look at all the configuration parameters that Squid accepts have a look at [the official documentation](http://www.squid-cache.org/Doc/config/) and for some (probably more helpful then raw configuration parameters) example configs check out the [Squid wiki](http://wiki.squid-cache.org/ConfigExamples).

As per usual, lets back up the default configuration, lest we find ourselves in need of it.

`root #` `mv /etc/squid/squid.conf /etc/squid/squid.conf.vanilla`
Keep in mind, this is a very minimal config to get you started:

**`/etc/squid/squid.conf`**

You can check your configuration file for syntax errors by running:

`root #``squid -k check`
Anything it marks as a warning, should be looked into but is not critical to getting Squid to run. Anything marked as an error will need fixed before Squid will even start.

Add Squid to the start up list:

`root #` `rc-update add squid default`
And then start Squid:

`root #` `/etc/init.d/squid start`
### Tweaking DansGuardian for heavier use

The basic global configuration for DansGuardian is pretty much the same as in the [previous small networks example](https://wiki.gentoo.org#Installing_and_configuring_DansGuardian). So make sure you read over that section first and then come back here.



#### Overview of large network filtering

This is essentially a recap of what was presented in the single machine/small network section, with extra content. The real meat and potatoes to enterprise class, per-user filtering is based on authentication methods and filter groups. Basically you sub-divide your users into groups that need different levels of filtering, users (either in the background or directly) authenticate and are placed into the appropriate filter group. So for example, a very basic and minimal set up for a K-12 school might have a filter group for staff, and another for students, and yet another for technical staff who might need access to technical forums and documentation that regular users shouldn't have. Finally, there is a default group. The default group is a powerful tool, a catch all that handles unauthenticated users.

The DansGuardian wiki makes a distinction between [authentication and authorization](http://contentfilter.futuragts.com/wiki/doku.php?id=user_identification_methods). The long and the short of it is that; authentication is verifying who someone is, and authorization is checking that a given person is allowed to perform some action.

If you'd rather not read the wiki, there is one **important gotcha** you need to be aware of, that is that DansGuardian itself only cares about a username. The password could be incorrect and DansGuardian wouldn't care. That means to effectively authenticate users, you need to offload the task to another process like Squid.

There are three general categories of authentication processes supported by both DansGuardian and Squid:

- BASIC - sends credentials in plain text, very insecure (not recomended)
- Digest - hashes credentials before sending them over the network
- NTLM - New Technology Lan Manager; A proprietary protocol that also hashes credentials, really only useful if you have a Microsoft network

To use Squid as your authentication process checkout some of their [examples](http://wiki.squid-cache.org/ConfigExamples#Authentication) and pick one that matches the architecture of your existing network.

Now that you decided how to authenticate your users, you'll need to uncomment one of these lines in your global config:

**`/etc/dansguardian/dansguardian.conf`**

#### Filter groups

Before you can have multiple filter groups, you'll need to let DansGuardian know how many you'll have by changing this parameter to however many you want, in this example there will be 3:

**`/etc/dansguardian/dansguardian.conf`**

Each filter group will have its own configuration file named /etc/dansguardian/dansguardianfN.conf

Where *N* is a number assigned to the group, typically you'll need at least a first group or dansguardianf1.conf and by default this is the default group. You should probably make a copy of it to act as a template for further groups:

`root #` `cp -av /etc/dansguardian/dansguardianf1.conf /etc/dansguardian/dansguardian_filtergroup_vanilla.conf`
Lets also copy the lists directory as well:

`root #` `cp -aR /etc/dansguardian/lists /etc/dansguardian/filter_lists_vanilla`
Now that you have a filter group template made open the first group with your favorite text editor, but first realize that this touches on some of the basic configuration options and is not an all inclusive list:

**`/etc/dansguaridanf1.conf`**

## Transparent filtering with Iptables

So you've decided you'd like to setup your machine to transparently filter web content. The most obvious solution for single computer is to use IPTables to redirect traffic. If you don't already have experience with IPTables have a look at the [Gentoo Wiki](https://wiki.gentoo.org/wiki/Iptables) on the subject before proceeding.

Let's recap: our proxy is listening on port 3128 and address 127.0.0.1, DansGuardian is listening on port 8080 and address 127.0.0.1. We only need to point our web traffic to DansGaurdian which is configured to route traffic through Squid and back. It helps to know that standard HTTP travels over port 80.

Next, make sure your kernel is configured with [netfilter support for clients](https://wiki.gentoo.org/wiki/Iptables#Client). Also, select the following options:

**.config-4.4.6**

**.config-3.7.3**

**.config-3.12.13**

We'll also need to find the UID of whatever user your proxy is running under, which should be its own separate UID from both DansGaurdian and any human users:

`root #``grep squid /etc/passwd | awk -F: {'print $3'}` Finally append your IPTables rule list with something like this (put the number you got from the previous grep of /etc/passwd in place of $SQUID):

`root #``iptables -t nat -A OUTPUT -p tcp -m owner ! --uid-owner $SQUID --dport 80 -j REDIRECT --to-port 8080`
Once you are sure everything is working as it should, save the iptables state with this command:

`root #` `/etc/init.d/iptables save`
