<!-- source: https://wiki.gentoo.org/wiki/Mosquitto | group: Gentoo Wiki (Main) | wiki-title: Mosquitto -->
---
title: Mosquitto
url: https://wiki.gentoo.org/wiki/Mosquitto
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-02"
fingerprint: a219c459ad0f7081
license: CC BY-SA 4.0
---

# Mosquitto

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Mosquitto is an open source MQTT message broker provided by the Eclipse foundation.

## Installation

### Emerge

`root #``emerge --ask app-misc/mosquitto`
### Additional software

Libraries/ integration, e.g. Eclipse Paho.

## Configuration

### Files

- /etc/mosquitto/mosquitto.conf - Global (system wide) configuration file.
- \~/.config/mosquitto\_sub - per user defaults for command mosquitto\_sub
- \~/.config/mosquitto\_pub - per user defaults for command mosquitto\_pub

Listeners:

- have at least a single *listener* so remote connections are possible
- specify the network interface with *bind\_interface* to if only one out of many is allowed
- configure multiple listeners with enabled per-listener-configuration to separate contexts or shard traffic

Security:

- memory\_limit to avoid resource exhaustion
- message\_size\_limit so the broker rejects payloads being too large
- persistent\_client\_expiration to allow cleaning stale clients

Monitoring

- *log\_dest*, preferrably /var/log/mosquitto.log, in conjunction with *log\_type* and optionally *connection\_messages*

### TLS (X509)

This section illustrates basic steps:

1. create a private key
2. create a certificate signing request (CSR) for the private key
3. signing the CSR as your own CA to yield a server certificate

For other options and how to let the system trust your own CA see [Certificates](https://wiki.gentoo.org/wiki/Certificates) and [Certificates/Become your own CA](https://wiki.gentoo.org/wiki/Certificates/Become_your_own_CA).

First create a directory tls under Mosquitto's configuration and create a broker key. Shown here an elliptic curve key with non-NIST algorithm:

`root #``cd /etc/mosquitto` `root #``mkdir tls` `root #``cd tls` `root #``openssl genpkey -algorithm ED25519 >broker.key` `root #``chown mosquitto:mosquitto broker.key`
Certificates have limited validity and need to be re-created. It is much easier to do this with a configuration file (no alternative names/ certificate for the MQTT broker only):

**`/etc/mosquitto/tls/openssl-25519.conf`**

Create the CSR:

`root #``openssl req -new -out mosquitto_yourserver.csr -key broker.key -config openssl-25519.conf`
With your own root/ intermediate CA issue a certificate valid for 365 days:

`user $``openssl x509 -req -in mosquitto_yourserver.csr -days 365 -out broker-yourserver.crt -CA root.cer -CAkey root.key -sha256 -CAcreateserial`
Finally store *broker-yourserver.crt* in */etc/mosquitto/tls* and configure *mosquitto.conf* accordingly:

**`/etc/mosquitto/mosquitto.conf`**

Finally secure all files by revoking permissions/ limiting access to user mosquitto only:

`root #``chown -R mosquitto:mosquitto /etc/mosquitto/tls` `root #``chmod 400 /etc/mosquitto/tls/*`
Improvements:

- broker key with password, requires unlocking upon start/ restart
- monitoring of certificate expiration, e.g. [Icinga2](https://wiki.gentoo.org/wiki/Icinga2)
- use key management, e.g. an external device or partition that is only available when starting the service

### Service

#### OpenRC

`root #``/etc/init.d/mosquitto start`
#### systemd

`root #``systemctl start mosquitto`
## Usage

The package provides the broker and tools to directly interact with it. The following command subscribes to a topic *announce/info* on a given host with port 8883 – assuming the broker was configured with a TLS listener (process runs until stopped):

`user $``mosquitto_sub -h mqtt.example.com -p 8883 -u mqtt-consumer-12 -P secret -t announce/info`
To publish the message *This broker is up and running* to the same topic on the same host with a different user:

`user $``mosquitto_pub -h mqtt.example.com -p 8883 -u mqtt-publisher -P othersecret -t announce/info -m 'This broker is up and running'`
This message now shows up in the output of the first command.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-misc/mosquitto`
