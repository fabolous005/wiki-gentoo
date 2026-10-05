<!-- source: https://wiki.gentoo.org/wiki/Printing/Standards | group: Gentoo Wiki (Main) | wiki-title: Printing/Standards -->
---
title: Printing/Standards
url: https://wiki.gentoo.org/wiki/Printing/Standards
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-21"
tags: ['CUPS 2.4b1']
fingerprint: "9288a555a1816bce"
license: CC BY-SA 4.0
---

# Printing/Standards

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page aims to be a brief summary of various standards in the printing space.

## Driver-based printing

### IPP

IPP is the Internet Printing Protocol. IPP/1.0 was published as a series of experimental [IETF](https://en.wikipedia.org/wiki/IETF) documents in 1999; IPP/1.1 became a draft standard in 2000, a proposed standard in January 2017, and [Internet Standard 92](https://www.rfc-editor.org/info/std92) in June 2018.

IPP 2.0 was published in 2009 as a [Printer Working Group](https://www.pwg.org/about.html) (PWG) Candidate Standard, defining two new IPP versions (2.0 for printers and 2.1 for print servers) with additional conformance requirements beyond IPP 1.1. In 2011, it was replaced by a subsequent Candidate Standard which defined an additional 2.2 version for production printers; this specification was updated and approved as a full PWG Standard in 2015.

## Driverless printing

### IPP Everywhere

[IPP Everywhere](https://www.pwg.org/ipp/everywhere.html) is a 'driverless printing' extension to the IPP/2.0 series<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

OpenPrinting provides a [database of printers supporting IPP Everywhere](https://openprinting.github.io/printers/).

### AirPrint

[AirPrint](https://en.wikipedia.org/wiki/AirPrint) is Apple's 'driverless printing' extension to IPP. AirPrint support has been automatic in CUPS since version 1.4.6 (2011-01-06). However, CUPS servers earlier than version 1.4.6 with DNS-Based Service Discovery<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> can be configured manually, by adding DNS-SD printer service discovery records to a name server. Support for AirPrint *clients* (e.g. clients running iOS) was added in [CUPS 2.4b1](https://github.com/OpenPrinting/cups/releases/tag/v2.4b1) (2021-10-27).

OpenPrinting provides a [database of printers supporting AirPrint](https://openprinting.github.io/printers/).

### Mopria

**Mopria** is a proprietary profile of IPP used for printing from Android and Microsoft Windows.

### Wi-Fi Direct Print Services

**Wi-Fi Direct** printers work as a Wi-Fi access point so that mobile devices can print without an existing Wi-Fi network.
