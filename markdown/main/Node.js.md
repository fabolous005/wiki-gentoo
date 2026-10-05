<!-- source: https://wiki.gentoo.org/wiki/Node.js | group: Gentoo Wiki (Main) | wiki-title: Node.js -->
---
title: Node.js
url: https://wiki.gentoo.org/wiki/Node.js
hostname: gentoo.org
sitename: Node.js
date: "2025-10-03"
fingerprint: ce86b24868833d81
license: CC BY-SA 4.0
---

# Node.js

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Node.js** is a cross platform, open source, JavaScript server environment.

## Installation

### USE flags


| [+icu](https://packages.gentoo.org/useflags/+icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [+inspector](https://packages.gentoo.org/useflags/+inspector) | Enable V8 inspector | 
| [+npm](https://packages.gentoo.org/useflags/+npm) | Enable NPM package manager | 
| [+snapshot](https://packages.gentoo.org/useflags/+snapshot) | Enable snapshot creation for faster startup | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [+system-icu](https://packages.gentoo.org/useflags/+system-icu) | Use system dev-libs/icu instead of the bundled version | 
| [+system-ssl](https://packages.gentoo.org/useflags/+system-ssl) | Use system OpenSSL instead of the bundled one | 
| [corepack](https://packages.gentoo.org/useflags/corepack) | Enable the experimental corepack package management tool | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [lto](https://packages.gentoo.org/useflags/lto) | Enable Link-Time Optimization (LTO) to optimize the build | 
| [pax-kernel](https://packages.gentoo.org/useflags/pax-kernel) | Enable building under a PaX enabled kernel | 
| [temporal](https://packages.gentoo.org/useflags/temporal) | Enable the Temporal API for handling dates and times | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### npm

Node.js has a [USE flag](https://wiki.gentoo.org/wiki/USE_flag) to include npm, the Node.js package manager. npm is necessary to install a Node.js application's dependencies, which are defined in a file named `package.json`. The USE can be disabled if npm is not necessary locally, or prefer to only install an alternative, for example, [sys-apps/yarn](https://packages.gentoo.org/packages/sys-apps/yarn).

npm and [sys-apps/yarn](https://packages.gentoo.org/packages/sys-apps/yarn) are what's known as [application-level package managers](https://wiki.gentoo.org/wiki/Application_level_package_management). They can install packages in one of two modes:

- Local (the default). Packages are installed in the working directory of the Node.js project being worked on. This is the generally preferred way of working with Node.js projects and dependencies.
- Global (enabled by the `--global` option). Packages are installed in a system-wide location and available for all projects and from the command line outside of a Node.js project.

As a workaround, the [environment variable](https://wiki.gentoo.org/index.php?title=Environment_variable&action=edit&redlink=1) `NPM_CONFIG_PREFIX` can be used to override to install "global" packages in the user's home directory:

**`~/.config/bash/bashrc`**

```
export NPM_CONFIG_PREFIX=$HOME/.local/
```
## Standalone Node.js server

Node.js can be run as a standalone HTTP server. It does not require root privileges and can be accessed from the Internet, for example on port 3000.

To launch the [official example](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs), the `hostname` (a variable in the example) must be set to a public IPv6 or IPv4 address (`localhost` will not work). The modified example can be executed from the user space as following:

`user $``node modified-downloaded-example.js`
The only problem is the inability to connect to [well-known ports](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports) (e.g. 80) from unprivileged user space. But this problem can be solved with [port redirection](https://wiki.gentoo.org#Port_redirection).

### SELinux policy

As of December 2, 2024, the Node.js package does not come with a [SELinux](https://wiki.gentoo.org/wiki/SELinux) policy, so creating a custom policy is required. The following custom policy assumes that Node.js will be executed from unprivileged user space. The policy was tested with Nodejs v. 22.7.0 on the `default/linux/arm64/23.0/musl/hardened/selinux` profile.

**`nodejs.te`**

**`nodejs.fc`**

#### Policy installation

To compile and install the policy module, run the commands:

`root #``make -f /usr/share/selinux/strict/include/Makefile nodejs.pp``root #``semodule --install nodejs.pp`
#### Policy usage

The policy requires that all files that should be accessible to Node.js be labeled as `nodejs_www_t`.

For example, if the project is stored in the /opt/website directory, the following command can be used:

`root #``chcon --recursive --type nodejs_www_t /opt/website`
#### Policy removal

To remove the policy, run the command:

`root #``semodule --remove nodejs`
### Port redirection

This section describes a way to redirect ports using the legacy [iptables](https://wiki.gentoo.org/wiki/Iptables) approach or the modern [nftables](https://wiki.gentoo.org/wiki/Nftables) approach. Choose one.

#### iptables

The redirection requires the following option to be enabled in the kernel:

**Enable redirections**

Assuming the Node.js server is running on port 3000, run the following command to redirect port 80 to 3000:

`root #``ip6tables --table nat --append PREROUTING --protocol tcp --dport 80 --jump REDIRECT --to-port 3000`
The server should be immediately accessible via port 80.

To see the modified NAT table, run the command:

`root #``ip6tables --table nat --list`
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination
REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:http redir ports 3000
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination

To remove the added rule (to change the port or because of a mistake), run the command:

`root #``ip6tables --table nat --delete PREROUTING 1`
#### nftables

The redirection requires the following options to be enabled in the kernel:

**Enable redirections**

**IPv6**

**IPv4**

Create the NAT table and chain:

`root #``nft add table ip6 nat``root #``nft add chain ip6 nat prerouting '{ type nat hook prerouting priority 0; }'`
Redirect port `80` to port `3000`:

`root #``nft add rule ip6 nat prerouting tcp dport 80 counter redirect to 3000`
The server should be immediately accessible via port 80.

To see the prerouting chain, run the command:

`root #``nft --handle list chain ip6 nat prerouting````
table ip6 nat {
	chain prerouting { # handle 1
		type nat hook prerouting priority filter; policy accept;
		tcp dport 80 counter packets 0 bytes 0 redirect to :3000 # handle 2
	}
}
```
To remove the added rule (to change the port or because of a mistake), run the command:

`root #``nft delete rule ip6 nat prerouting handle 2`
### HTTPS

This section relies on [Express.js](https://expressjs.com/en/starter/installing.html) because it provides a simple way to host static files that appear dynamically. All paths match the [acme-tiny configuration guide](https://wiki.gentoo.org/wiki/Let%27s_Encrypt#acme-tiny), but there are no strict requirements, the files can be anywhere.

#### Certificate issuance (Let's Encrypt)

First, it is necessary to create and run a server script that will host the Let's Encrypt token for the [HTTP-01 challenge](https://letsencrypt.org/docs/challenge-types/#http-01-challenge):

**`http-server.js`**

```
const express = require('express');
const PORT = 3000;
const ACME_CHALLENGE_PATH = '/var/www/localhost/acme-challenge';
const app = express();
app.use('/.well-known/acme-challenge', express.static(ACME_CHALLENGE_PATH));
app.listen(PORT);
```
Then redirect port `80` to port `3000` as described [above](https://wiki.gentoo.org#Port_redirection). Install acme-tiny as described [here](https://wiki.gentoo.org/wiki/Let%27s_Encrypt#acme-tiny_.28optional.29) and issue the certificate as described [here](https://wiki.gentoo.org/wiki/Let%27s_Encrypt#acme-tiny).

#### Certificate usage

Once the certificate has been issued, the server script needs to be replaced with this one:

**`https-server.js`**

```
const fs = require('node:fs');
const https = require('node:https');
const express = require('express');
const PORT = 3000;
const CERTIFICATE_PATH = '/var/lib/letsencrypt/chained.pem';
const PRIVATE_KEY_PATH = '/var/lib/letsencrypt/domain.key';
const app = express();
const options = {
  cert: fs.readFileSync(CERTIFICATE_PATH),
  key: fs.readFileSync(PRIVATE_KEY_PATH)
};
https.createServer(options, app).listen(PORT);
```
Redirect port `443` to `3000` as described [above](https://wiki.gentoo.org#Port_redirection). The connection should now be encrypted. The above script doesn't actually require Express.js anymore, but it's left as an example, an example of pure Node.js can be found [here](https://nodejs.org/api/https.html#httpscreateserveroptions-requestlistener). The ACME challenge is not required either, even for renewals.

### Node.js as a reverse proxy for Forgejo\Gitea (or anything else)

The simplest way to set up a reverse proxy is to use [Express.js](https://expressjs.com/en/starter/installing.html) with [express-http-proxy](https://github.com/villadora/express-http-proxy).

The following example shows a way to redirect all requests coming to `http://<DOMAIN>/projects` to [Forgejo](https://wiki.gentoo.org/wiki/Forgejo) (or [Gitea](https://wiki.gentoo.org/wiki/Gitea)). The example assumes that port `80` is redirected to port `3000` as described [above](https://wiki.gentoo.org#Port_redirection).

The minimal Forgejo [configuration](https://forgejo.org/docs/latest/admin/config-cheat-sheet/):

**`<Forgejo\Gitea root directory>/custom/conf/app.ini`**

```
[server]
ROOT_URL = http://<DOMAIN GOES HERE>/projects/
HTTP_PORT = 3001
```
The minimal HTTP server:

**`server.js`**

```
const express = require('express');
const proxy = require('express-http-proxy');
const PORT = 3000;
const app = express();
app.use('/projects', proxy('localhost:3001'));
app.listen(PORT);
```
To use HTTPS, just inject the following lines in the script provided [here](https://wiki.gentoo.org#Certificate_usage):

## Web application daemons with nginx and monit

This section will walk through installing Node.js behind nginx and using Monit to keep Node instances alive. Since Node.js is a single-process application, the goal is to launch multiple instances of the application and load balance using nginx.

### Packages

Use [app-admin/monit](https://packages.gentoo.org/packages/app-admin/monit) for spawning Node.js servers.

`root #``emerge --ask monit nginx nodejs`
### Configure Monit

**`/etc/monit.d/nodejs-server`**

**Auto restart NodeJS App**

### Configure Nginx

**`/etc/nginx/nginx.conf`**

**Nginx Config**

## Web application with openrc runscript

**`/etc/init.d/nodejs-server`**

**Sample init.d file for a Node.js daemon**

```
#!/sbin/openrc-run
 
user="nobody"
group="nobody"
command="/usr/bin/node"
directory="/opt/${RC_SVCNAME}"
command_args="httpd.js"
command_user="${user}:${group}"
command_background="yes"
pidfile="/run/${RC_SVCNAME}.pid"
output_log="/var/log/${RC_SVCNAME}.log"
error_log="${output_log}"
 
depend() {
	use net
}
```
