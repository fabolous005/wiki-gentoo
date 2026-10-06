<!-- source: https://wiki.gentoo.org/wiki/Docker/Compose | group: Gentoo Wiki (Main) | wiki-title: Docker/Compose -->
---
title: Docker/Compose
url: https://wiki.gentoo.org/wiki/Docker/Compose
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-01"
fingerprint: "78b734692da57999"
license: CC BY-SA 4.0
---

# Docker/Compose

From Gentoo Wiki

\< [Docker](https://wiki.gentoo.org/wiki/Docker)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Docker Compose** is  a tool for running multi-container applications on [Docker](https://wiki.gentoo.org/wiki/Docker) defined using the Compose file format.

A Compose file is used to define how one or more containers that make up how the application is configured. Once a Compose file has been created, the application can be started with a single command: `docker compose up`.

## Installation

### USE flags


### USE flags for
            [app-containers/docker-compose](https://packages.gentoo.org/packages/app-containers/docker-compose)
            
            Multi-container orchestration for Docker

### Emerge

`root #``emerge --ask app-containers/docker-compose`
## Usage

### Starting Docker Compose

To start Docker Compose with a file named docker-compose.yml in the current directory, run:

`user $``docker compose up`
