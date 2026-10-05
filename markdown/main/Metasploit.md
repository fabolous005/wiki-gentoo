<!-- source: https://wiki.gentoo.org/wiki/Metasploit | group: Gentoo Wiki (Main) | wiki-title: Metasploit -->
---
title: Metasploit
url: https://wiki.gentoo.org/wiki/Metasploit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-08"
fingerprint: f332d36314959506
license: CC BY-SA 4.0
---

# Metasploit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Metasploit Project is a computer security project that provides information about security vulnerabilities and aids in penetration testing and IDS signature development. The framework is maintained by Rapid7 and the community. Its best-known sub-project is the open source Metasploit Framework, a tool for developing and executing exploit code against a remote target machine. Other important sub-projects include the Opcode Database, shellcode archive and related research. The Metasploit Project is well known for its anti-forensic and evasion tools, some of which are built into the Metasploit Framework.

## Installation

Rapid7 recommends using the [binary installer](https://www.rapid7.com/products/metasploit/download/editions/) for the desired version. The installer comes with a [guide](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) that aims to help during the installation process. If someone wants to develop and contribute, there's a [guide](https://github.com/rapid7/metasploit-framework/wiki/Setting-Up-a-Metasploit-Development-Environment) to set up a development environment.

### Overlay

Metasploit is available from the [pentoo](https://repos.gentoo.org/#pentoo) overlay: [https://github.com/pentoo/pentoo-overlay](https://github.com/pentoo/pentoo-overlay)

### Emerge

`root #``emerge --ask net-analyzer/metasploit`
## Usage

Metasploit comes with its own CLI. For a detailed list of available commands refer to [Offensive Security guide](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/).

For a GUI, refer to [Armitage project](http://www.fastandeasyhacking.com/).

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-analyzer/metasploit`
## See also

- [Wireshark](https://wiki.gentoo.org/wiki/Wireshark) — a free and open-source packet analyzer.
- [Nmap](https://wiki.gentoo.org/wiki/Nmap) — an open source recon tool used to check for open ports, what is running on those ports, and metadata about the daemons servicing those ports.

## External resources

- [Downloads by version (official)](https://github.com/rapid7/metasploit-framework/wiki/Downloads-by-Version) - Metasploit's versions.
- [Metasploit unleashed](https://www.offensive-security.com/metasploit-unleashed/) - Offensive security's Metasploit course.
- [Rapid7 free tools](https://www.rapid7.com/free-tools) - some free tools from Rapid7
- [Community's documentation](https://community.rapid7.com/community/metasploit/content?filterID=contentstatus%5Bpublished%5D~category%5Bdocumentation%5D) - some comunnity's documentation
- [Armitage](http://www.fastandeasyhacking.com/) - Metasploit front-end UI
