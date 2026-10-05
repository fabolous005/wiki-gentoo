<!-- source: https://wiki.gentoo.org/wiki/Traefik | group: Gentoo Wiki (Main) | wiki-title: Traefik -->
---
title: Traefik
url: https://wiki.gentoo.org/wiki/Traefik
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: b7e7d7cf3ba5b8c1
license: CC BY-SA 4.0
---

# Traefik

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Traefik** provides a proxy that is container aware. 
This is a level 7 proxy, that is it operates in the application layer in the OSI model, that can only do connection termination.
One needs a level 4 proxy, that is it operate in the transport layer in the OSI model, to do connection routing as required by SSL-SNI for example.

Traefik will configure a virtual host for each container in ones container cluster (Mesos/docker/kubernetes and other supported), allowing it to serve from each container. It will even go one further and issue SSL certificates for each service.

It may be run from within a container or upon the host. This article is concerned with the latter.

## Installation

Ebuilds for traefik are presently supplied from overlay.

`root #``eselect repository enable r7l``root #``emerge --sync r7l`
### USE flags

Traefik is a go based project and does not seem to need any specific use flags.

### Emerge

Traefik is readily emerged via the usual means once the overlay has been configured.

`root #``emerge --ask net-proxy/traefik`
### Additional software

Traefik is run as a compliment to ones container manager (Docker/Mesos). One may configure it to be run as the web facing proxy if there are no host services or behind another proxy (Haproxy/NginX/Apache) should one be serving content from another web server(Nginx/Apache). Additionally Traefik supports analytics/monitoring which can be connected to packages like Prometheus.

## Configuration

As traefik is quite modular it is easiest to copy the relevant pre-configured sections out of their documentation, stitching the parts together as necessary for ones own system.

### Environment variables

Traefik does not have any environment variables that it looks out for, there is supposedly certain configurations that may make it use environment variables (Although the author can not recall the packages name at this time).

### Files

#### Configuration

Traefik has one main configuration file which controls its operation. It can either be written in TOML or YAML language.

- /etc/traefik/traefik.toml - Global configuration file for traefik using TOML.
- /etc/traefik/traefik.yml - Global configuration file for traefik using YAML.

#### Additional Configuration

One may configure Traefik to use a further folder/file for different frontend/backend configurations.

- /etc/traefik/conf.d/ - Path for additional routing configurations.

#### Logs

The following paths should be used for logging purposes.

- /var/log/traefik/traefik.log - Traefik log for recording program, container and certification events .
- /var/log/traefik/access.log - Access log for recording web traffic through Traefik.

### Service

Typically the service needs access to the docker socket, /var/run/docker.sock, to generate configurations for the containers it proxies. Traefik should be really be started as root so that it can connect to the socket and then restrict it's own permissions to that of the Traefik user. The present init script starts the service as the traefik user which prevents it from seeing the socket. One may add Traefik to the docker group to enable this permission again.

`root #``gpasswd -a traefik docker`
One one has configured Traefik to their satisfaction they may enable and start the service.

#### OpenRC

`root #``rc-update add traefik default`
Subsequently start the service or restart the machine:

`root #``rc-service traefik start`
Use the following command to disable it:

`root #``rc-update del traefik`
#### systemd

`root #``systemctl enable --now traefik`
Use the following command to disable it:

`root #``systemctl disable traefik`
## Usage

While one is configuring Traefik it is convenient to run it directly.

### Invocation

Traefik may be run in debug mode, allowing one verify their configuration and to watch it's responses to various requests.

`user $``traefik -d --configFile=/etc/traefik/traefik.toml`
One may use `ctrl`+`c` to terminate this process.

## Troubleshooting

### Ignoring Frontend/backend configuration

Traefik is not told to explicitly watch it's own configuration file for frontend/backend configuration(s). This is resolved by simply adding the file heading into the configuration.

### 404

If traefik is hosted behind a proxy and the proxy is not forwarding the Host header then traefik cannot know which container to serve and returns a 404. To fix this ensure that one has correctly configured the proxy protocol in both the frontend proxy and the Traefik service itself.

## Removal

Removal of Traefik is as simple as unmerging the package, one may also wish to remove the overlay thereafter.

### Unmerge

`root #``emerge --ask --depclean --verbose net-proxy/traefik`
