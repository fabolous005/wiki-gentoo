<!-- source: https://wiki.gentoo.org/wiki/IRC/Guide | group: Gentoo Wiki (Main) | wiki-title: IRC/Guide -->
---
title: IRC/Guide
url: https://wiki.gentoo.org/wiki/IRC/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-20"
fingerprint: d79adf58d80963c9
license: CC BY-SA 4.0
---

# IRC/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide provides a broad overview of concepts related to IRC. It intends to be a handy resource for those who want to go deeper than the [IRC](https://wiki.gentoo.org/wiki/IRC) article. Topics such as IRC clients, convenience, and hardening will be covered in this guide.

## What is Internet Relay Chat (IRC)?

**I**nternet **R**elay **C**hat (IRC) is a way to communicate in real time over the internet with other people. Think of an IRC *server* as a huge room with people sitting around tables, each table having a focused topic of discussion - each table would correspond to a *channel* in IRC terminology.

In a channel, some of the people will be actively engaged in conversation, others just listening (reading), and some may even be ignoring the conversation entirely. Note that often a `#` is used to denote a channel name.

### Terminology

Using correct terminology is helpful to both readers and writers because distinctions must be made in order to accurately transmit information. Several IRC related terms are used in the next sections; unless the reader is completely comfortable with the terminology of IRC it is wise to briefly scan through them to gain an understanding before proceeding:

| Term | Meaning | 
|---|---|
| **(IRC) Network** | The system of interconnected servers that transmit conversation (and other) information. For the [Gentoo project](https://wiki.gentoo.org/wiki/Project:Gentoo) (and its related sub-projects) the [irc://irc.gentoo.org](irc://irc.gentoo.org) URL can be used in an IRC client in order to connect to the correct location. [irc://irc.gentoo.org](irc://irc.gentoo.org), for now, is simply a redirect to irc.libera.chat. Either one of these URLs can be used, although in the event that the Gentoo project should move to another network in the future [irc://irc.gentoo.org](irc://irc.gentoo.org) is a safer URL to use. | 
| **(IRC) Server** | One of the many machines hosting the IRC *service* software. | 
| **(IRC) Client** | A program that connects a user to IRC Networks available in many variations. | 
| **User** | A person who has connected to a network (you!) having standard permissions. They can generally read and write messages inside the channels they have entered. | 
| **(IRC) Channels** | Channels are locations where communications occur; they are the "chat rooms" of IRC. In the bullet point list below, the hashtag (`#`) and a short string with the prefix `gentoo-` is used for most of the entries, referencing some of the Gentoo channels available on IRC. For a full list of official Gentoo related IRC channels, see the [All IRC Channels page](https://www.gentoo.org/get-involved/irc-channels/all-channels.html) on the main site. | 
| **(IRC) Nick name** | IRC users have historically followed a convention of creating a nick name, used as their primary handle, to refer to themselves. Each developer in the Gentoo project has a nick name. When a user or another developer wants to get the attention of a specific developer, mentioning their nick name will (generally) trigger some kind of notification event. Notification events are specific to a given IRC client, however all good clients provide some kind of notification when the user's nick name is mentioned in the client's open IRC channels. | 
| **(Channel) Operator** | A special type of user who can perform actions to the channel itself or on other users. Operators have full permissions and should be consulted when authority is needed. | 
| **Voiced (User)** | A user who has some level of official recognition in the channel. They have a level of authority in between Operators and (regular) users. | 
| [**WebChat**](https://webchat.freenode.net/) | A method of connecting to an IRC network using a web browser client. This approach is useful when a user has a simple question or merely wants to experiment with IRC. | 
| **Network Policies** | A set of rules defining what is expected from the Operators and Users of the IRC network infrastructure. | 
| **Channel Guidelines** | A set of rules a user should follow while communicating in channels. Guidelines can change based on the channel and should be reviewed prior to engaging in conversation. Typical guidelines include restrictions on the use of profanity or how many lines of text can be pasted into a buffer. | 
| **Buffer** | The chat window that displays the text from an IRC channel is called a buffer. This term is more commonly used on command-line IRC clients such as [Irssi](https://wiki.gentoo.org/wiki/Irssi) and [WeeChat](https://wiki.gentoo.org/wiki/WeeChat). | 
| **Message of the Day (MOTD)** | In general, each channel a user visits will have a Message of the Day (MOTD). Most IRC clients can be configured to automatically display or not display the MOTD upon joining a channel. Reading the MOTD can be helpful to determine the topic at hand, the rules of the channel, and other relevant information. For the most part MOTD text is not updated regularly, so do not be surprised to find the information a bit dated. | 
| **Bots** | In the IRC world [bots lurk in various channels](https://wiki.gentoo.org/wiki/Category:IRC-Bot), watching channel logs and storing up information for a time when it is needed by a human. | 

### Uses

IRC is for *communication*. In the Gentoo project it is among the primary means of:

- Getting support ([#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)))
- Providing support to others ([#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)))
- Fulfilling the desire for a community ([#gentoo-chat](ircs://irc.libera.chat/#gentoo-chat) ([webchat](https://web.libera.chat/#gentoo-chat)))
- Tracking or discussing bugs on Bugzilla ([#gentoo-bugs](ircs://irc.libera.chat/#gentoo-bugs) ([webchat](https://web.libera.chat/#gentoo-bugs)))
- Discussing Wiki-related content or changes ([#gentoo-wiki](ircs://irc.libera.chat/#gentoo-wiki) ([webchat](https://web.libera.chat/#gentoo-wiki)))
- Tracking Wiki changes ([#gentoo-wiki](ircs://irc.libera.chat/#gentoo-wiki) ([webchat](https://web.libera.chat/#gentoo-wiki)))
- Discussing Gentoo website related changes ([#gentoo-www](ircs://irc.libera.chat/#gentoo-www) ([webchat](https://web.libera.chat/#gentoo-www)))
- Making a complaint, feature request, or suggestion (any project specific channel)

## Installation

IRC clients enable users to connect to an IRC server. After connection to the server, channels can be entered, they are where the action takes place.

## Configuration

### Nick names

Like on websites and forums, nick names are used on IRC. Choose a nick name and setup the IRC client to use the nick name each time it connects to the network. This process is different for each client.

If any extended length of time is to be spent on IRC, it is important to register a nick name for personal use so that the nick name will be available on each connection. Without registering a nick name, other users may claim the nick name or even pretend to be you.

After connecting to the Libera Chat Network, [follow these steps](https://libera.chat/guides/registration) to register a nick name.

Upon successful identification of a nick name the following message should appear:

`unaffiliated/username is now your hidden host (set by services.)`

#### Cloaks

Cloaks help users stay secure on IRC by hiding the user's IP address. Without a cloak, all users on the channel can ask for an `/info` or a `whois` on the uncloaked user and see the public IP address their client is using to connect.

If the user is connected from home via their primary internet connection, which is more than likely the case, this can potentially have negative consequences. There are exceptions to this rule. Some users connect to the IRC server through proxy connections, onion router networks (Tor), or other means in order to conceal their actual IP address. In these cases (without a cloak) an IP address will still be visible, but it will not be the *actual* (original) IP address of the client.

If the reader of this guide will be doing any long-term communication over IRC, obtaining a cloak should be a high priority.

Two types of cloaks exist:

- **Affiliated cloaks** - These kind of cloaks are given out by various projects. They are to associate the user's nick name to a specific organization on the IRC network.
- **Unaffiliated cloaks** - Unaffiliated cloaks exist for all users who are *not* part of an organization with a presence on the server. These can be obtained by asking an Admin on the network.

#### Tor

For Libera Chat, see [https://libera.chat/guides/connect#accessing-liberachat-via-tor](https://libera.chat/guides/connect#accessing-liberachat-via-tor)

## Usage

### Commands

#### Client

Client commands will change based upon the client in use. Visit the respective software article or see the client's man page for information on commands specific to the client in use.

#### Server

When corrected to a server, the server software will dictate what commands are or are not acceptable. In this section the Libera Chat IRC network will be used as an example for server commands.

Message the ChanServ user in order to gain a list of available server commands. This can be done by issuing a `/msg` command:

`/msg ChanServ help`
Or by opening a new buffer/chat window directly to ChanServ.

#### Channel

These kinds of commands narrow in scope to the channel they are executed in.

#### Bot

There are many IRC bots that are available for general use in IRC channels. Since each of them are separate projects, Bots do not have a unified command interfaces. Research the bot software or ask a user in the channel for help interacting with the bot.

### Channel guidelines

It is typical for each IRC channel to have guidelines. Gentoo's official IRC guidelines can be [seen on the main site](https://www.gentoo.org/get-involved/irc-channels/index.html), however they will be repeated here for the reader's convenience:

- Please act sensibly and maturely, abiding by the [Code of Conduct](https://wiki.gentoo.org/wiki/Project:Council/Code_of_conduct).
- Please read the topic when entering a channel, it contains valuable information!
- Bots or scripts that talk or create public logs are not welcome in most channels. If in doubt, please ask.
- Please do not use `CTCP VERSION` or the like on users/channels without their consent.

It is important for each potential IRC user to read the [Behaviour and consequences section](https://wiki.gentoo.org/wiki/Project:Council/Code_of_conduct#Behaviour_and_consequences) of the Code of Conduct.

## External resources

- [https://www.irchelp.org/](https://www.irchelp.org/) - A site dedicated to helping users understand IRC.
- [http://tldp.org/HOWTO/IRC/index.html](http://tldp.org/HOWTO/IRC/index.html) - The Linux Documentation Project's IRC HOWTO.
