<!-- source: https://wiki.gentoo.org/wiki/Puppet | group: Gentoo Wiki (Main) | wiki-title: Puppet -->
---
title: Puppet
url: https://wiki.gentoo.org/wiki/Puppet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-04"
fingerprint: f510ee5b79919bd9
license: CC BY-SA 4.0
---

# Puppet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Puppet** is a configuration management system written in [Ruby](https://wiki.gentoo.org/wiki/Ruby). It can be used for automating machine deployments.

## Installation

Currently, there is no distinction between server and client, so the basic installation procedure is the same for both.

### USE flags


| [augeas](https://packages.gentoo.org/useflags/augeas) | Enable augeas support | 
| [diff](https://packages.gentoo.org/useflags/diff) | Enable diff support | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [hiera](https://packages.gentoo.org/useflags/hiera) | Enable hiera support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [rrdtool](https://packages.gentoo.org/useflags/rrdtool) | Enable rrdtool support | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [shadow](https://packages.gentoo.org/useflags/shadow) | Enable shadow support | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vim-syntax](https://packages.gentoo.org/useflags/vim-syntax) | Pulls in related vim syntax scripts | 

### Emerge

Install [app-admin/puppet](https://packages.gentoo.org/packages/app-admin/puppet):

`root #``emerge --ask app-admin/puppet`
## Configuration and setup

Puppet is mainly configured through /etc/puppet/puppet.conf in an INI-style format. Comments are marked with a hash sign (`#`).

The configuration file is separated into several sections, or blocks:

- `[main]` contains settings that act as a default for all parts of Puppet, unless overridden by settings in any of the following sections:
  - `[master]` is used for settings applying to the Puppetmaster (puppet master), or CA tool (puppet cert)
  - `[agent]` is used for settings applying to the Puppet agent (puppet agent)

A more in-depth explanation, as well as a list of further blocks used is available in the [official Puppet documentation](http://docs.puppetlabs.com/guides/configuring.html).
Also, there is a [list of all configuration](http://docs.puppetlabs.com/references/stable/configuration.html) options, some of which of course make only sense when applied to either server or client.

### Server (Puppetmaster) setup

The default configuration put by the ebuild into puppet.conf can be used as-is. For Puppet 2.7.3, the server-related parts look like this:

**`/etc/puppet/puppet.conf`**

**Server-related default configuration**

```
[main]
    # The Puppet log directory.
    # The default value is '$vardir/log'.
    logdir = /var/log/puppet
  
    # Where Puppet PID files are kept.
    # The default value is '$vardir/run'.
    rundir = /var/run/puppet
  
    # Where SSL certificates are kept.
    # The default value is '$confdir/ssl'.
    ssldir = $vardir/ssl
```
#### Setting up the file server

To be able to send files to the clients, the file server has to be configured. This is done in /etc/puppet/fileserver.conf. By default, there are no files being served.

**`/etc/puppet/fileserver.conf`**

**Setting the`files` share**

```
[files]
    path /var/lib/puppet/files
    allow 192.168.0.0/24
```
The snippet above sets up a share called `files` (remember this identifier, as it will need to be referenced later), looking for files in /var/lib/puppet/files and only available for hosts with an IP from the 192.168.0.0/24 network. Any of the IP addresses, CIDR notation, and host names (including wildcards like `*.domain.invalid`) can be used here. The `deny` command can be used to explicitly deny access to certain hosts or IP ranges.

#### Starting the puppetmaster daemon

With the basic configuration as well as an initial file server configuration, we can start the Puppetmaster daemon using its OpenRC init script:

`root #``rc-service puppetmaster start`
During the first start, Puppet generates an SSL certificate for the Puppetmaster host and places it into the directory configured through the `ssldir` variable, as configured above.

It listens on Port `8140/TCP`, make sure that there are no firewall rules obstructing access from the clients.

#### A simple manifest

Manifests, in Puppet's terminology, are the files in which the client configuration is specified.
The documentation contains a [comprehensive guide](http://docs.puppetlabs.com/guides/language_guide.html) about the manifest markup language.

As a simple example, let's create a *message of the day* (motd) file on the client. On the puppetmaster, create a file inside the `files` share created earlier:

**`/var/lib/puppet/files/motd`**

**MOTD file on the server**

Then, we have to create the main manifest file in the manifests directory. It is called `site.pp`:

**`/etc/puppet/manifests/site.pp`**

**Main manifest on the server**

```
node default {
  file { '/etc/motd':
    source => 'puppet:///puppet/files/motd'
  }
}
```
The `default` *node* (the name for a client) definition is used in case there is no specific `node` statement for the host.
We use a `file` resource and want the /etc/motd file on our clients to contain the same thing as the `motd` file in the `files` share on the host `puppet`. If the puppetmaster is only reachable using another host name, adapt the `source` URI accordingly.

### Client configuration

During the first execution of the Puppet agent, wait for the certificate to be signed by the puppetmaster. To request a certificate, and execute the first configuration run, execute:

`root@client #``puppet agent --test --waitforcert 60`
info: Creating a new certificate request for client
info: Creating a new SSL key at /var/lib/puppet/ssl/private\_keys/client.pem
notice: Did not receive certificate

Before the client can connect, authorize the certificate request on the server. The client should appear in the list of nodes requesting a certificate:

`root@server #````
puppet cert --list
```
client

Now, we grant the request:

`root@server #````
puppet cert --sign client
```
The client will check every 60 seconds whether its certificate has already been issued. After that, it continues with the first configuration run:

```
info: Caching catalog for client
info: Applying configuration version '1317317379'
notice: /Stage[main]//Node[default]/File[/etc/motd]/ensure: defined content as '{md5}30ed97991ad6f591b9995ad749b20b00'
notice: Finished catalog run in 0.05 seconds
```
When this message pops up, all went well. Now check the contents of the /etc/motd file on the client:

`user@client $````
cat /etc/motd
```
Welcome to this Puppet-managed machine!

#### OpenRC

Start the puppet agent as a daemon and have it launch on boot:

`root@client #````
rc-service puppet start
```
`root@client #````
rc-update add puppet default
```
#### systemd

Conversely, when running systemd:

`root@client #````
systemctl start puppet
```
`root@client #````
systemctl enable puppet
```
## Other topics

### Manually generating certificates

To manually generate a certificate, use the puppet cert utility.
It will place all generated certificates into the `ssldir` defined directory as set in the puppet configuration and will sign them with the key of the local Puppet Certificate Authority (CA).

An easy case is the generation of a certificate with **only one Common Name:**

`root #``puppet cert --generate host1`
If the certificate has to be valid for **multiple host names**, use the `--certdnsnames` parameter and separate the additional host names with a colon:

`root #``puppet cert --generate --certdnsnames puppet:puppet.domain.invalid host1.domain.invalid`
This example will generate a certificate valid for the three listed host names.

### Refreshing agent certificates

This is the process used to manually refresh agent certificates.

1. (on master) `root #``puppet cert clean ${AGENT_HOSTNAME}` 
2. (on agent) `root #``rm /etc/puppet/ssl/{certs,certificate_requests}/${AGENT_HOSTNAME}.pem`
  - This will cause the Puppet agent to regenerate the CSR with the existing SSL key.
  - The old certificate is no longer valid, as it was nuked on the master.
  - When one of the above steps is forgotten, an error will pop up about the certificate mis-matching between agent and master.
  - To replace the SSL keys (optional): `root #``rm /etc/puppet/ssl/{public,private}_keys/${AGENT_HOSTNAME}.pem`
3. (on agent) `root #``puppet agent --onetime --no-daemonize --verbose --test --waitforcert 30`
  - When using auto-signing, no further steps are needed.
4. (on master) `root #``puppet cert list ${AGENT_HOSTNAME}` 
5. Verify that the fingerprint listed in the previous two outputs matches
6. (on master) `root #``puppet cert sign ${AGENT_HOSTNAME}` 
7. (on agent) `root #``puppet agent --onetime --no-daemonize --verbose --test`

### Managing slots with puppet

While the default portage provider in puppet does support slots there are puppet modules available which also have this functionality.

For instance, with [app-admin/puppet](https://packages.gentoo.org/packages/app-admin/puppet) version 4.6.0 and higher, and/or [app-admin/puppet-agent](https://packages.gentoo.org/packages/app-admin/puppet-agent), the slot functionality is supported like to:

Additional modules are:
