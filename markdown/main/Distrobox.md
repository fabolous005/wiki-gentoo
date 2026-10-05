<!-- source: https://wiki.gentoo.org/wiki/Distrobox | group: Gentoo Wiki (Main) | wiki-title: Distrobox -->
---
title: distrobox
url: https://wiki.gentoo.org/wiki/Distrobox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-03"
fingerprint: "1402d154bfe57fd6"
license: CC BY-SA 4.0
---

# distrobox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**distrobox** is a program used to run a Linux Distribution in a terminal.

## Use Flags


## Installation

### Emerge

[app-containers/distrobox](https://packages.gentoo.org/packages/app-containers/distrobox) is in Gentoo's main repository:

`root #``emerge --ask app-containers/distrobox`
## Installing a container

### Podman

Emerge [Podman](https://wiki.gentoo.org/wiki/Podman):

`root #``emerge --ask app-containers/podman`
### Docker

Emerge [Docker](https://wiki.gentoo.org/wiki/Docker):

`root #``emerge --ask app-containers/docker`
## Usage

First, install an image (for this command [sudo](https://wiki.gentoo.org/wiki/Sudo) needs to be installed):

`user $``distrobox create --name gentoo -i docker.io/gentoo/stage3:latest --root`
Then, enter the Distrobox Container:

`user $``distrobox enter --root gentoo`
## Troubleshooting

### `Installing basic packages... Error: An error occured` While Entering Distrobox

A command line parameter `verbose` could be added to show more detailed information.

`user $``distrobox enter <container_name> --verbose`
From here, the issue will be categorized into container distribution and error messages.

#### Ubuntu: `update-locale: Error: invalid locale settings`

Invalid locale settings have been written into the `locale.gen` in the container and require manual intervention.

1\. Identify the docker image that the distrobox environment is based on :

`user $``docker images`
REPOSITORY                      TAG            IMAGE ID       CREATED          SIZE
ubuntu                          24.04          bbdabce66f1b   3 weeks ago      78.1MB
(...)

2\. Spawn a container based on the corresponding image:

`user $``docker run -dt <image_id> /bin/bash`
3\. Find the ID of the container created and entering it:

`user $``docker ps`
CONTAINER ID   IMAGE                COMMAND                   CREATED          STATUS          PORTS     NAMES
61f04b55d573   ubuntu:24.04         "/bin/bash"               8 seconds ago    Up 8 seconds              clever\_pasteur
(...)

4\. Entering the container:

`user $``docker exec -it <container_id> /bin/bash`
- Now the username and hostname section of the bash would change to indicate it's in the container.

5\. Update the repository and install the `locales` package:

`root@<container_id>:/#``apt update && apt install locales` 6\. Edit the `/etc/locale.gen`, removing the invalid entries and enable needed ones.

7\. Regenerate locales:

`root@<container_id>:/#``locale-gen` 8\. Exit the container by exectuting `exit`.

- Now the username and hostname section of the bash would change back.

9\. Save the changes to the container:

`user $``docker commit <container_id> <container_name>:<tag>`
10\. Stop the container spawned earlier.

`user $``docker stop <container_id>`
11\. Regenerate the distrobox container based on the fixed [Docker](https://wiki.gentoo.org/wiki/Docker) container and enter it.
