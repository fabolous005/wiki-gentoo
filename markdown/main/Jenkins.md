<!-- source: https://wiki.gentoo.org/wiki/Jenkins | group: Gentoo Wiki (Main) | wiki-title: Jenkins -->
---
title: Jenkins
url: https://wiki.gentoo.org/wiki/Jenkins
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-06-30"
fingerprint: "945857785f1d13db"
license: CC BY-SA 4.0
---

# Jenkins

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Jenkins** is an open source automation server written in [Java](https://wiki.gentoo.org/wiki/Java). The project was forked from [Hudson](<https://en.wikipedia.org/wiki/Hudson_(software)>) after a dispute with [Oracle](https://en.wikipedia.org/wiki/Oracle_Corporation)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

Common use case of Jenkins is automation of [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) (CI) and [continuous delivery](https://en.wikipedia.org/wiki/Continuous_delivery) (CD) related tasks.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-util/jenkins-bin`
## Configuration

### First configuration

Open [http://localhost:8080](http://localhost:8080) with a [web browser](https://wiki.gentoo.org/wiki/Category:Web_browser) and follow the first configuration steps.

### General configuration

The main configuration dashboard is at [http://localhost:8080/manage](http://localhost:8080/manage).

### Security

The security configuration page is accessible on the configuration dashboard, or directly on [http://localhost:8080/configureSecurity/](http://localhost:8080/configureSecurity/).

#### Allow remote command-line interface

To allow anyone to connect with [Command-line interface](https://en.wikipedia.org/wiki/Command-line_interface) (CLI),  activate two options:

1. "Allow anonymous read access" in "Authorisations" section,
2. "Enable CLI over Remoting" in the "CLI" section.

## Usage

### Service

#### systemd

For a oneshot start:

`root #``systemctl start jenkins`
To enable the service at each startup:

`root #``systemctl enable jenkins`
### Access from the command-line

You must have remote CLI option activated.

Download [http://localhost:8080/jnlpJars/jenkins-cli.jar](http://localhost:8080/jnlpJars/jenkins-cli.jar).

To obtain the list of possible commands, open a console or a terminal and enter:

`user $``java -jar jenkins-cli.jar -s http://localhost:8080/ help`
