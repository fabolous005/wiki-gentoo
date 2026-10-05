<!-- source: https://wiki.gentoo.org/wiki/Synapse | group: Gentoo Wiki (Main) | wiki-title: Synapse -->
---
title: Synapse
url: https://wiki.gentoo.org/wiki/Synapse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-28"
fingerprint: "823bf15c9986bc86"
license: CC BY-SA 4.0
---

# Synapse

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- Correct handling of TLS certificates

**Synapse** is the official reference implementation of [Matrix](https://matrix.org/) homeserver.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask net-im/synapse`
### Additional software

#### Coturn

Coturn is software which enables a standard home user to allow their Matrix server to communicate over a Network Address Traversal (NAT) using either [STUN](https://en.wikipedia.org/wiki/STUN) or [TURN](https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT) techniques. NAT is used both as a measure to counteract IPv4 address exhaustion and as a security zone. It is a standard and important component of [SOHO networks](https://en.wikipedia.org/wiki/Small_office/home_office), and subsequently adds a filter for inbound connections to a home server inside a private network.

`root #``emerge --ask net-im/coturn`
## Configuration

Synapse will not work using an IP address, so even with a static IP it's necessary to have a valid internet hostname. DynuDNS [https://www.dynu.com/en-US/](https://www.dynu.com/en-US/) is free, but any domain name service can be used. Changing the server name is not straightforward once Synapse is installed, and therefore can be configured after a domain name has been obtained.

### Set the home server name

As root, the `--server-name` option must be changed to the actual domain name:

`root #````
cd /var/lib/synapse
```
`root #``sudo -u synapse python -m synapse.app.homeserver --server-name my.domain.name --config-path  /etc/synapse/homeserver.yaml --generate-config --report-stats=yes`
### Files

### Service

#### OpenRC

`root #``rc-service synapse start`
Or the corresponding systemd command...

### Determine the operational status of Synapse

`user $``ps aux|grep synapse`
Synapse can fail to start without showing an error message. If so check the /etc/synapse/homeserver.yaml config file.

### Add users

Once Synapse is set up with TLS and TURN trying to add a user gives Python errors; commenting out all TLS and TURN entries and setting tlsːfalse in /etc/synapse/homeserver.yaml then restarting synapse will allow users to be added; then the homeserver.yaml file can then be restored and synapse restarted again.

`user $``register_new_matrix_user -c /etc/synapse/homeserver.yaml http://127.0.0.1:8008`
### Install Let's Encrypt / Certbot

Instructions are hereː [Let's Encrypt](https://wiki.gentoo.org/wiki/Let%27s_Encrypt)

The reverse proxy is probably best set up after Synapse is up and running. These steps assume one has not been set up.

The certificates as installed do not have read permissions required for Synapse, and require copying and permissions changing. Edits are welcome for a better way of handling this, that has been fully tested and works with voice and video calls over different networks. Modifications to the synapse group and symlinking (or directly specifying the certificate locations in homeserver.yaml) may be a start, but the privkey1.pem is set 0600 and owned by root. Using a unique domain name and cert for synapse, and different domain name and certs for other applications may improve security:

`root #````
/bin/cp /etc/letsencrypt/archive/your.domain.name/fullchain1.pem /etc/synapse/your.domain.name.tls.crt
```
`root #````
/bin/cp /etc/letsencrypt/archive/your.domain.name/privkey1.pem /etc/synapse/your.domain.name.tls.key
```
`root #````
/bin/chown synapse:synapse /etc/synapse/your.domain.name.tls.*
```
`root #````
/bin/chmod 640 /etc/synapse/your.domain.name.tls.key
```
`root #``/bin/chmod 644 /etc/synapse/your.domain.name.tls.crt`
### Configuration

Generate secret key:

`user $``openssl rand -hex 16`
Edit /etc/turnserver.conf:

**`/etc/turnserver.conf`**

An example config file is at /etc/turnserver.conf.default Edit /etc/synapse/homeserver.yaml with your domain name and keys. To add new users after TLS has been set up disable TLS and TURN as comments. It may not be neccessary to change all the lines noted:

**`/etc/synapse/homeserver.yaml`**

```
# Configuration file for Synapse.
#
# This is a YAML file: see [1] for a quick introduction. Note in particular
# that *indentation is important*: all the elements of a list or dictionary
# should have the same indentation.
#
# [1] https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html
#
# For more information on how to configure Synapse, including a complete accounting of
# each option, go to docs/usage/configuration/config_documentation.md or
# https://matrix-org.github.io/synapse/latest/usage/configuration/config_documentation.html
server_name: "your.domain.name"
pid_file: /var/lib/synapse/homeserver.pid
listeners:
  - port: 8008
#CHANGE TO tls:false TO ADD NEW USER
    tls: true
    type: http
    x_forwarded: true
# Bind to the server IP address the modem will port forward to 
    bind_addresses: ['192.168.x.xxx']
    resources:
      - names: [client, federation]
        compress: false
database:
  name: sqlite3
  args:
    database: /var/lib/synapse/homeserver.db
log_config: "/etc/synapse/your.domain.name.log.config"
media_store_path: /var/lib/synapse/media_store
registration_shared_secret: "0Q&*NQ74K0qq*V:zK:7dEKnEHMTjdavK3y:K@.bf^ljhH*BHw&"
report_stats: true
macaroon_secret_key: "Default entry will work"
form_secret: "ODk^^*yaABCDEFGHpXqw7_lndv2LA6hR0fyxiqWpz+dVBOk~"
signing_key_path: "/etc/synapse/your.domain.name.signing.key"
trusted_key_servers:
# CHANGE TO   - server_name: "matrix.org" TO ADD NEW USER
  - server_name: "your.doamin.name"
# COMMENT ALL TLS AND TURN OUT TO ADD NEW USER
tls_private_key_path: "/etc/synapse/your.domain.name.tls.key"
tls_certificate_path: "/etc/synapse/your.domain.name.tls.crt"
turn_uris: [ "turn:your.domain.name?transport=udp", "turn:your.domain.name?transport=tcp" ]
turn_shared_secret: "SharedSecretInEtcTurnserver.conf"
turn_user_lifetime: 86400000
turn_allow_guests: true
# vim:ft=yaml
```
#### Set router port forwarding

For correct operations, the following ports need to be forwarded to the serverː

UDP 49152 to 65535

TCP 8008

UDP/TCP 3478

UDP/TCP 5349

TCP 8448

Restart Synapse:

`root #``rc-service synapse restart`
Start TURN server:

`root #``rc-service turnserver restart`
Add to default runlevel:

`root #````
rc-update add synapse default
```
`root #``rc-update add turnserver default`
### Client software

This has been tested with the Element Linux and Android client. Specify the port as ː8008 when logging in.

## Troubleshooting

### Service will not start

Check the homeserver.yaml and turnserver.conf files for any "your.domain.name" entries which have not been updated to the instance's domain name.

### Text messaging works, but video and voice calls only work within the same network

This is normally an issue with the TURN server setup.

## See also

- [User:Maffblaster/Projects/Matrix](https://wiki.gentoo.org/wiki/User:Maffblaster/Projects/Matrix) — maff's landing page for setting up a dendrite-based Matrix server on Gentoo.

## External resources

- [https://matrix-org.github.io/synapse/latest/welcome\_and\_overview.html](https://matrix-org.github.io/synapse/latest/welcome_and_overview.html)
- [https://python-docs.synapse.org/build/html/CommandLineClient.html#](https://python-docs.synapse.org/build/html/CommandLineClient.html#)
- [https://wiki.archlinux.org/title/Matrix](https://wiki.archlinux.org/title/Matrix)
