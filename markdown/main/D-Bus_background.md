<!-- source: https://wiki.gentoo.org/wiki/D-Bus/background | group: Gentoo Wiki (Main) | wiki-title: D-Bus/background -->
---
title: D-Bus/background
url: https://wiki.gentoo.org/wiki/D-Bus/background
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: "851ac2f56ea189c4"
license: CC BY-SA 4.0
---

# D-Bus/background

[D-Bus](https://wiki.gentoo.org/wiki/D-Bus)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page provides a brief overview of D-Bus, to save people from having to distill the core concepts from more detailed documents elsewhere.

This document is purely descriptive; nothing in it should be taken as implying either support or criticism of D-Bus and/or its design.


## Introduction

D-Bus is a *protocol*, and should not be confused with specific [implementations](https://wiki.gentoo.org/wiki/D-Bus/background#Implementations) of the protocol. In particular, the [D-Bus specification](https://dbus.freedesktop.org/doc/dbus-specification.html) provides [buses](https://en.wikipedia.org/wiki/Software_bus) to facilitate [Inter-Process
Communication](https://en.wikipedia.org/wiki/Inter-process_communication) (IPC) and [Remote Procedure Calls](https://en.wikipedia.org/wiki/Remote_procedure_call) (RPC).

The name "D-Bus" is short for "Desktop Bus". The intent of D-Bus was to try to standardise communication for desktop services: before D-Bus, the [GNOME](https://wiki.gentoo.org/wiki/GNOME) project used [CORBA](https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture) for communication and [KDE](https://wiki.gentoo.org/wiki/KDE) used [DCOP](https://en.wikipedia.org/wiki/DCOP), hindering interoperability. The design of D-Bus was heavily influenced by DCOP. KDE moved to using D-Bus in version 4, released in 2008.

There seems to be a common misconception that D-Bus is part of the [systemd](https://wiki.gentoo.org/wiki/Systemd) project. This is incorrect; although systemd makes extensive use of D-Bus, the two projects are distinct. In fact, the initial release of the D-Bus reference implementation was in November 2006; the initial release of systemd was in March 2010.


## Core concepts

There are two primary D-Bus buses: a *system bus*, and a *session bus*. It's important to note that *these are distinct buses, used for different purposes, and are not simply the same bus being run in two different ways*. The two buses can be viewed in a GUI environment via qdbusviewer6, provided by [dev-qt/qttools](https://packages.gentoo.org/packages/dev-qt/qttools), or d-Spy, provided by [dev-debug/d-spy](https://packages.gentoo.org/packages/dev-debug/d-spy).

A *system bus* relates to services provided by the OS and system daemons, and communication between different user sessions. The system bus is used for communication about system-wide events: storage being added, network connectivity changes, printer status, and so on. There is usually only one system bus.

A *session bus* relates to a particular user login session, and is used by processes wishing to communicate with each other within that session.

Within each of these, there are various bus *names*. The D-Bus specification glossary says that a  name:

is simply an identifier used to locate connections ... An application is said to own a name if the message bus has associated the application's connection with the name.


A name can roughly be considered as "a service accessible via D-Bus".

Each name has objects specified by an *object path*, e.g. `/org/freedesktop/DBus`, and each of those objects supports one or more *interfaces*, which are collections of messages that can be handled by an object.

Every D-Bus connection has a *unique connection name*, e.g. `:1.1553`; there is no special meaning to the identifier.

D-Bus can be used for [service activation](https://dbus.freedesktop.org/doc/dbus-specification.html#message-bus-starting-services) / auto-starting. For example, instead of manually starting a [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) server, one can be automatically started when an application requires it.

Programs can register as being interested in messages to particular bus names via service files in the /usr/share/dbus-1/services/ and /usr/share/dbus-1/system-services\` directories. For example, org.knopwob.dunst.service in /usr/share/dbus-1/services/ contains:

\[D-BUS Service\]
 Name=org.freedesktop.Notifications
 Exec=/usr/bin/dunst
 SystemdService=dunst.service

This file says that messages for `org.freedesktop.Notifications` can be handled by /usr/bin/dunst.

Thus, desktop applications and components can send messages to particular bus names, without having to explicitly specify which applications/components should respond to those messages. For example, applications can send notifications via
`org.freedesktop.Notifications`, and users can choose which notification daemon handles those notifications, and how.


## Implementations

The reference implementation is "dbus", [sys-apps/dbus](https://packages.gentoo.org/packages/sys-apps/dbus)), but other implementations include [GDBus](https://developer.gnome.org/gio/stable/gdbus.html), provided by [dev-libs/glib](https://packages.gentoo.org/packages/dev-libs/glib), and the systemd implementation, "sd-bus".


## Well-known bus names and interfaces

For an overview of well-known bus names and interfaces, refer to [D-Bus/reference](https://wiki.gentoo.org/wiki/D-Bus/reference), which provides links to further details.


## Querying and messaging

Refer to [the "Usage" section of the "D-Bus" page](https://wiki.gentoo.org/wiki/D-Bus#Usage).


## External resources

- [D-Bus tutorial](https://dbus.freedesktop.org/doc/dbus-tutorial.html) - a more detailed overview of the D-Bus architecture.
