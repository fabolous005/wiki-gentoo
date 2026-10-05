<!-- source: https://wiki.gentoo.org/wiki/Syslog-ng | group: Gentoo Wiki (Main) | wiki-title: Syslog-ng -->
---
title: Syslog-ng
url: https://wiki.gentoo.org/wiki/Syslog-ng
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-31"
fingerprint: ec1313132ea539c0
license: CC BY-SA 4.0
---

# Syslog-ng

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**syslog-ng** is a powerful, highly configurable monitoring and logging daemon.

## Installation

### USE flags


| [amqp](https://packages.gentoo.org/useflags/amqp) | Enable support for AMQP destinations | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [dbi](https://packages.gentoo.org/useflags/dbi) | Enable dev-db/libdbi (database-independent abstraction layer) support | 
| [geoip2](https://packages.gentoo.org/useflags/geoip2) | Add support for geo lookup based on IPs via dev-libs/libmaxminddb | 
| [grpc](https://packages.gentoo.org/useflags/grpc) | Enable GRPC based driver support (OpenTelemetry) via net-libs/grpc | 
| [http](https://packages.gentoo.org/useflags/http) | Enable support for HTTP destinations | 
| [json](https://packages.gentoo.org/useflags/json) | Enable support for JSON template formatting via dev-libs/json-c | 
| [kafka](https://packages.gentoo.org/useflags/kafka) | Enable support for Kafka destinations | 
| [mongodb](https://packages.gentoo.org/useflags/mongodb) | Enable support for mongodb destinations | 
| [mqtt](https://packages.gentoo.org/useflags/mqtt) | Enable MQTT support via net-libs/paho-mqtt-c | 
| [pacct](https://packages.gentoo.org/useflags/pacct) | Enable support for reading Process Accounting files (EXPERIMENTAL, Linux only) | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [redis](https://packages.gentoo.org/useflags/redis) | Enable support for Redis destinations | 
| [smtp](https://packages.gentoo.org/useflags/smtp) | Enable support for SMTP destinations | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Add support for the Simple Network Management Protocol if available | 
| [spoof-source](https://packages.gentoo.org/useflags/spoof-source) | Enable support for spoofed source addresses | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Install [app-admin/syslog-ng](https://packages.gentoo.org/packages/app-admin/syslog-ng):

`root #``emerge --ask app-admin/syslog-ng`
### Additional software

When using a system logger such as syslog-ng, it is a wise idea to install log rotation software to appropriately trim the logs as they consume more disk space. [Logrotate](https://wiki.gentoo.org/wiki/Logrotate) is a fine option:

`root #``emerge --ask app-admin/logrotate`
## Configuration

syslog-ng is configured using [C](https://wiki.gentoo.org/wiki/C) syntax. Directives are defined with an **object type**, **identifier** (name), and **parameters**.

The default configuration provided by the ebuild is located at /etc/syslog-ng/syslog-ng.conf and contains the following:

**`/etc/syslog-ng/syslog-ng.conf`**

**syslog-ng.conf version 4.0**

### Network Server

When it comes to syslog, most people still think about [RFC3164](https://datatracker.ietf.org/doc/html/rfc3164), which is also often called legacy syslog. It is old, not really well-standardized, as it just tries to describe existing practice. Still, most syslog messages arrive in this format.

[RFC5424](https://datatracker.ietf.org/doc/html/rfc5424) is a well-standardized format for syslog messages, right from the beginning. It has a more precise timestamp, and can forward name-value pairs. However, it is not widely used.

What we can see a lot more often is that if someone wants to forward name-value pairs between syslog servers, they use a legacy [RFC3164](https://datatracker.ietf.org/doc/html/rfc3164) syslog header, and a JSON formatted message part.

When it comes to collecting log messages over a network, syslog-ng has three modes of operation:

- **Client mode** - syslog-ng is collecting logs from the client and sending them to the central server directly or through a relay.
- **Relay mode** -  syslog-ng is collecting logs from clients through the network and sending them to the central server directly or through another relay.
- **Server mode** -   syslog-ng is collecting logs from clients and / or relays and storing them either locally or in a non-syslog destination-driver.

Of course, in the real world, these modes are not so strictly separated. Some relays might both store and forward log messages.

#### Relays

Relays have many important roles in a logging infrastructure. Many devices use UDP for log transport. UDP is an unreliable protocol, so you want to collect these log messages as close to the source as possible.

Using relays can give structure and additional security to your logging infrastructure. You can install a relay for each department or site. This is especially important when the central server is at a remote location. Relays ensure that log messages leave clients immediately even if the central server is unavailable due to maintenance or network problems.

**`/etc/syslog-ng/netsource.conf`**

**netsource.conf version 4.0**

Stop the running syslog implementation and start syslog-ng with this configuration in the foreground with debug information enabled:

`root #``syslog-ng -Fvde -f /etc/syslog-ng/netsource.conf`
With syslog-ng started in the foreground and with debugging enabled, you should see the incoming log message on the screen. The file /var/log/fromnet should show your test message at the end.

#### Using logger with a network source

From another terminal we can use the logger command to generate test messages.

- **-T** - TCP
- **-n** - Hostname or IP
- **-P** - Port
- **test message** - Log message

`root #``logger -T -n 127.0.0.1 -P 514 test message`
### Global Options

Global options, specified with the **options** statement are applied to applicable objects, some include:

- **threaded** - Used to enable threading.
- **chain\_hostnames** - Used to enable hostname chaining, enabling this can interfere with how syslog-ng counts hosts.
- **stats\_freq** - Adjusts how often syslog-ng prints a stats line to the logs.
- **mark\_freq** - Adjusts how often syslog-ng prints a mark line in the logs.

### Object types

- **source**<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> - Defined with a driver, a ingress tool.
- **destination** - Egress point for log information. Log records received by any source can be sent to one or more destinations.
- **log** - Defines the path between a **source** and **destination**, without **log** directives, **source**s and **destination**s are independent objects.
- **filter** - Filters can be added to **log** directives to change which records will be sent to the **destination**.
- **parser** - Segments log contents into different fields to assist in log processing and handling.
- **rewrite rule** - Used to rewrite the contents of a log record.
- **template**<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> - Templates can be used to define formats using macros or parameters.

#### Source

The default source is defined as:

**`/etc/syslog-ng/syslog-ng.conf`**

**Define the default**source** as being the *system* logger**

##### Receive logs networked to localhost

**`/etc/syslog-ng/syslog-ng.conf`**

**Define*src* to receive logs on udp://localhost:514**

##### Create a separate source for traffic on a LAN interface

**`/etc/syslog-ng/syslog-ng.conf`**

**Define*lan\_src* to receive logs on {tcp,udp}://192.168.0.2:514**

#### Destination

To define a **destination** log file, *test\_log*, which writes to /var/log/test.log, the following configuration can be used:

**`/etc/syslog-ng/syslog-ng.conf`**

**Define*test\_log* to write to /var/log/test.log**

#### Filter

To add a **filter** which matches log messages starting with "Test string|", the following configuration can be used:

**`/etc/syslog-ng/syslog-ng.conf`**

**Define*f\_test* to match messages starting with "Test string|"**

To add a **filter** which matches logs coming from hosts "10.10.10.1" "10.10.10.3" and "10.10.10.5":

**`/etc/syslog-ng/syslog-ng.conf`**

**Define*f\_test2* to match messages from 10.10.10.{1,3,5}"**

#### Templates

The syslog-ng application allows you to define message templates, and reference them from every object that can use a template. Templates can include strings, macros (for example date, the hostname, and so on), and template functions. For example, you can use templates to create standard message formats or filenames. For a list of macros available in syslog-ng Open Source Edition, see Macros of syslog-ng Fields from the structured data (SD) part of messages using the new IETF-syslog standard can also be used as macros.

**`/etc/syslog-ng/syslog-ng.conf`**

**Example how to use filters on a destination source**

Template objects have a single option called template-escape(), which is disabled by default (template-escape(no)). This behavior is useful when the messages are passed to an application that cannot handle escaped characters properly. Enabling template escaping (template-escape(yes)) causes syslog-ng to escape the ', ", and backslash characters from the messages.

If you do not want to enable the template-escape() option (which is rarely needed), you can define the template without the enclosing braces.

Templates can also be used inline, if they are used only at a single location.

**`/etc/syslog-ng/syslog-ng.conf`**

The following file destination uses macros to daily create separate logfiles for every client host.

**`/etc/syslog-ng/syslog-ng.conf`**

The following example shows how to use this template function to store log messages in JSON format:

**`/etc/syslog-ng/syslog-ng.conf`**

##### Hash

Truncate the hash to the first N characters. Calculates a hash of the string or macro received as argument using the specified hashing method. If you specify multiple arguments, effectively you receive the hash of the first argument salted with the subsequent arguments.

This template function can be used for anonymizing sensitive parts of the log message (for example, username) that were parsed out using PatternDB before storing or forwarding the message. This way, the ability of correlating messages along this value is retained. Also, using this template, quasi-unique IDs can be generated for data, using the `--length` option. This way, IDs will be shorter than a regular hash, but there is a very small possibility of them not being as unique as a non-truncated hash.

The following example calculates the SHA256 hash of the hostname of the message:

**`/etc/syslog-ng/syslog-ng.conf`**

The following example calculates the SHA256 hash of the hostname, using the salted string to salt the result:

**`/etc/syslog-ng/syslog-ng.conf`**

To replace the hostname with its hash, use a rewrite rule:

**`/etc/syslog-ng/syslog-ng.conf`**

##### Anonymizing IP addresses

The following example replaces every IPv4 address in the MESSAGE part with its SHA256 hash:

**`/etc/syslog-ng/syslog-ng.conf`**

##### Format messages in Python

Getting log messages into the desired format can sometimes be a problem, but with syslog-ng you can use Python to get exactly the format that is neededed. The syslog-ng Python template function allows you to write custom templates for syslog-ng in Python. The Python template function can work on the whole log message which is passed on to it automatically as an object or on the data received as argument.

Here is a complete working configuration customized to test the Python template function and replace /etc/syslog/syslog-ng.conf with it:

**`/etc/syslog-ng/syslog-ng.conf`**

**syslog-ng.conf version 4.0**

Restart syslog-ng and open a second terminal and send a message using the logger command to the port defined in the configuration:

`user $``logger -n 127.0.0.1 -T -P 1234 --rfc3164 8.8.8.8`
A new file resolve.txt should be created in the /var/log directory. Should it possibly not appear, make sure syslog-ng has the python useflag enabled.

`2023-05-20T21:46:26+02:00 Host: dns.google IP: 8.8.8.8`
### Security Handbook

The Gentoo Security Handbook [Logging](https://wiki.gentoo.org/wiki/Security_Handbook/Logging) provides more information on the security aspects of logging.

For a more comprehensive configuration see the configuration provided by [Security Handbook: Hardened Syslog-ng logging](https://wiki.gentoo.org/wiki/Security_Handbook/Logging#Syslog-ng).

Alternatively, this configuration can be viewed using bzcat, part of [app-arch/bzip2](https://packages.gentoo.org/packages/app-arch/bzip2), and is available at:

/usr/share/doc/syslog-ng-\*/syslog-ng.conf.gentoo.hardened.bz2

### Example configuration

The default **source** for system messages, *src*, can be defined with:

**`/etc/syslog-ng/syslog-ng.conf`**

**Define the default**source** as being the *system* logger**

A **destination** must be configured, otherwise nothing can be logged:

**`/etc/syslog-ng/syslog-ng.conf`**

**Define the**destination** *messages* as a file at /var/log/message**

To direct logs being received through *src* to *messages*:

**`/etc/syslog-ng/syslog-ng.conf`**

**Log everything received by*src* to *messages***

### Service

#### OpenRC

To add the syslog-ng daemon to the *default* runlevel, so that logging starts with the system:

`root #``rc-update add syslog-ng default`
To start the syslog-ng daemon:

`root #``rc-service syslog-ng start`
#### systemd

To start the syslog-ng daemon with the system, the service can be enabled with:

`root #``systemctl enable syslog-ng@default`
To start the daemon:

`root #``systemctl start syslog-ng@default`
### Docker Image

Your central log server can also run in a Docker container. If you wish to deploy your log server running syslog-ng in a Docker container, it is available as a ready-to-use image from the Docker Hub, already passing 500K pulls. The image is based on the latest Debian and the latest stable version of syslog-ng. It has all modules, including Java modules and experimental modules from the incubator.

It is possible to to issue a single command to download the image and run it in a container on your host machine. In addition to, of course, sharing that command with you, my goal in this post is to explain how it is made up and what it does.

To be able to use them, we need enable these ports both in the syslog-ng configuration (syslog-ng.conf) and in the command line starting the Docker container.

#### Starting for the first time

If you do not have a configuration file at hand for testing, create one. Here is a simple syslog-ng.conf, which listens to the legacy syslog protocol on UDP port 514, the new syslog protocol on TCP port 601, and stores any incoming log messages in a file called /var/log/syslog.

**`/data/syslog-ng/conf/syslog-ng.conf:/etc/syslog-ng/syslog-ng.conf`**

**syslog-ng.conf version 4.1**

You can map files or directories from your host into the container. In the case of syslog-ng.conf, you have a simple configuration file with all the settings in a single file and you do not have any encryption keys, so mapping the configuration file is the easiest. In any other scenario, you should map a complete directory for the configuration.

If you store all your log messages in a database, there is not much need for persistent storage for your container. If your central log server also stores data, there is a good chance that you will want to have access to those logs even if you switch to another syslog-ng image. In this case, you should map a directory from the host machine, so your log storage is independent from your Docker containers.

If you have your syslog-ng.conf under /data/syslog-ng/conf and plan to store your logs in the /data/syslog-ng/logs directory, you can use the following command line to get started. It 1) starts the container in interactive mode, 2) maps two network ports from the host to the container, 3) maps the configuration file and log directory, and 4) adds some debug options to syslog-ng. The name of the container will be “syslog-ng” and Docker will use the latest available syslog-ng image from the Docker Hub.

`user $``docker run -it -v /data/syslog-ng/conf/syslog-ng.conf:/etc/syslog-ng/syslog-ng.conf -v /data/syslog-ng/logs:/var/log -p 514:514 -p 601:601 -name syslog-ng balabit/syslog-ng:latest -edv`
When you first execute this command, it can take a few minutes until syslog-ng is up and running, as the image is downloaded over the Internet. On subsequent executions, Docker will use the local copy and start immediately.

#### Testing

Using the **docker ps** command from an other terminal you can check, that your container is up and running. You can see many information about the image, including the opened network ports.

`user $``docker ps`
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
b947a3411c1a balabit/syslog-ng:latest “/usr/sbin/syslog-ng” 18 seconds ago Up 17 seconds 0.0.0.0:514->514/tcp, 514/udp, 0.0.0.0:601->601/tcp, 6514/tcp syslog-ng

The loggen command can generate you a few sample logs. If you used the above configuration and directories, you should see a flood of messages on the screen where you started the container and also new messages in the file /data/syslog-ng/logs/syslog

`user $``loggen -i -S -P localhost 601`
average rate = 1006.53 msg/sec, count=10066, time=10.000, (average) msg size=260, bandwidth=255.42 kB/sec

### Incorrect timestamps on musl-based systems

If the time zone is set according to [the musl usage guide](https://wiki.gentoo.org/wiki/Musl#Timezones), the system will still send messages in UTC format. If the logger is additionally configured to read messages from a file (e.g. /proc/kmsg), the messages will be in the specified time zone. This causes the logger to output a log file with incorrectly written timestamps. To fix this problem, it is possible to specify the same time zone that is used in the system:

**`/etc/syslog-ng/syslog-ng.conf`**

## See also

- [syslog-ng (Security Handbook)](https://wiki.gentoo.org/wiki/Security_Handbook/Logging#Syslog-ng) - The system logging with syslog-ng is covered in the [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook).
- [Metalog](https://wiki.gentoo.org/wiki/Metalog) — an alternative syslog daemon
- [Rsyslog](https://wiki.gentoo.org/wiki/Rsyslog) — open source system for high performance log processing.
- [Sysklogd](https://wiki.gentoo.org/wiki/Sysklogd) — utility that reads and logs messages to the system console, logs files, other machines and/or users as specified by its configuration file.

## External resources

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.37/administration-guide/17#TOPIC-1828968](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.37/administration-guide/17#TOPIC-1828968)
2. [↑](https://wiki.gentoo.org#cite_ref-2) [https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.30/administration-guide/67](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.30/administration-guide/67)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.37/administration-guide/28#TOPIC-1829013](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.37/administration-guide/28#TOPIC-1829013)
4. [↑](https://wiki.gentoo.org#cite_ref-4) Balabit. [Collecting messages from the systemd-journal system log storage](https://www.balabit.com/sites/default/files/documents/syslog-ng-ose-latest-guides/en/syslog-ng-ose-guide-admin/html/configuring-sources-journal.html), [The syslog-ng Open Source Edition 3.7 Administrator Guide](https://www.balabit.com/sites/default/files/documents/syslog-ng-ose-latest-guides/en/syslog-ng-ose-guide-admin/html/index.html), January 22nd, 2016. Retrieved on January 30th, 2016.
5. [↑](https://wiki.gentoo.org#cite_ref-5) [https://syslog-ng.github.io/admin-guide/060\_Sources/030\_Wildcard-file/000\_Wildcard-file\_options#time-zone](https://syslog-ng.github.io/admin-guide/060_Sources/030_Wildcard-file/000_Wildcard-file_options#time-zone)
