<!-- source: https://wiki.gentoo.org/wiki/S6/File_descriptor_holder | group: Gentoo Wiki (Main) | wiki-title: S6/File descriptor holder -->
---
title: S6/File descriptor holder
url: https://wiki.gentoo.org/wiki/S6/File_descriptor_holder
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-02-13"
fingerprint: "1740695ff084cac0"
license: CC BY-SA 4.0
---

# S6/File descriptor holder

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Linux kernel allows one process to send a copy if its open file descriptors to a different process. This is done by transmitting `SCM_RIGHTS` control messages that contain an array of file descriptors from one process to the other, over a UNIX domain socket, as ancillary data (i.e. over a socket for address family `AF_UNIX` as the object pointed to by the `msg_control` field of a `struct msghdr` object) using the POSIX `sendmsg()` and `recvmsg()` calls. This works like POSIX `dup2()` does for a single process, and in s6's documentation, this is called *fd-passing*. A *file descriptor holder* (of *fd-holder*) is a process that receives file descriptors from others via fd-passing and *holds* them, for later retrieval either from the same process or from a different one. The fd-holder doesn't do anything with the file descriptors, it only keeps them open.

This article describes [s6](https://wiki.gentoo.org/wiki/S6)'s file descriptor holder and related tools.

## Configuration

### Environment variables

- `S6_FD#` — Number of file descriptors transferred to an s6-fdholderd process by s6-fdholder-setdump.
- `S6_FD_0`, `S6_FD_1`, ... — File descriptors transferred to an s6-fdholderd process by s6-fdholder-setdump.
- `S6_FDID_0`, `S6_FDID_1`, ... — Identifiers of the file descriptors transferred to an s6-fdholderd process by s6-fdholder-setdump.
- `S6_FDLIMIT_0`, `S6_FDLIMIT_1`, ... — Expiration times of the file descriptors transferred to an s6-fdholderd process by s6-fdholder-setdump, as a timestamp in [external TAI64N format](https://cr.yp.to/libtai/tai64.html).

## Usage


### The fd-holder daemon

#### s6-fdholderd

s6 provides a daemon that implements fd-holder functionality, named s6-fdholderd. It must have its standard input redirected to a bound and listening stream mode UNIX domain socket (i.e. a `SOCK_STREAM` socket), and accepts a set of options that control its behaviour. It has builtin access control features, and all operations requested to it must be explicitly granted to the requesting client. For this purpose, it accepts either an `-i` option specifying the pathname of a *rules directory*, or an `-x` option specifying the pathname of a *rules file*, [just like s6-ipcserver-access](https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server#rulesdir). s6-fdholderd will exit with an error status if neither of these options is supplied. The environment specified via appropriate {uid/\*,gid/\*,default}/env subdirectories of the rules directory controls which operations supported by s6-fdholderd are allowed to clients. s6-fdholderd ignores the {uid/\*,gid/\*,default}/exec files of the rules directory.

s6-fdholderd runs until told to exit with a `SIGTERM` signal; after that, it can keep running for limited time to allow connected clients to finish their ongoing operations. This is called the *lame duck timeout*, which can be specified with a `-T` option followed by a time value in milliseconds. If s6-fdholderd is invoked with a `-T 0` option, or no `-T` option, the lame duck timeout is infinite: after receiving the signal, it will wait until all clients disconnect before exiting. s6-fdholderd supports the [s6 readiness protocol](https://wiki.gentoo.org/wiki/S6#s6readiness), and if it is invoked with a `-1` option, it will turn readiness notification on, with file descriptor 1 (i.e. its standard output) as the notification channel's file descriptor. If s6-fdholderd was invoked with a rules file (`-x`) and receives a `SIGHUP` signal, it will re-read it. If s6-fdholderd was invoked with a rules directory (`-i`), changes are picked up automatically so `SIGHUP` isn't needed.

s6-fdholder-daemon is a helper program that accepts options and a UNIX domain socket pathname, and invokes s6-ipcserver-socketbinder chained to s6-fdholderd, or s6-ipcserver-socketbinder chained to s6-applyuidgid, chained to s6-fdholderd, depending on the options. The socket pathname is passed to s6-ipcserver-socketbinder. s6-fdholder-daemon options specify corresponding s6-ipcserver-socketbinder, s6-applyuidgid and s6-fdholderd options.

For further information about s6-fdholder-daemon or s6-fdholderd please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.

#### fd-holder operations

The operations supported by s6-fdholderd are *store*, *retrieve*, *list*, *delete*, *get dump* and *set dump*. To store, retrieve or delete a file descriptor, s6-fdholderd uses *identifiers* to refer to them. An identifier is a character string containing 1 to 255 characters; any non-null character can be used in an identifier, but it is recommended to only use reasonable ones. For file descriptor identifiers associated with UNIX domain sockets, it is conventional to adhere to the `unix:$path` format, where *$path* is a placeholder for the socket's absoulte pathname. For file descriptor identifiers associated with TCP or UDP sockets, it is conventional to adhere to the `$protocol:$address:$port` or `$protocol:$host:$port` format, where *$protocol* is 'tcp' or 'udp', *$address* is is a placeholder for an IPv4 or IPv6 address, *$host* is is a placeholder for a domain name, and *$port* is is a placeholder for the TCP or UDP port number. If an identifier is currently in use by s6-fdholderd, it cannot be reused to store a new file descriptor until the one currently using the identifier is deleted or has expired.

- A store operation transfers a file descriptor from a client to the fd-holder, specifying an identifier for it, and optionally an expiration time. When a file descriptor has been held for a period of time equal to the expiration time, the fd-holder closes it and frees its identifier. To allow store operations, a file named S6\_FDHOLDER\_STORE\_REGEX must exist in the appropriate {uid/\*,gid/\*,default}/env subdirectory of s6-fdholderd's rules directory, or rules file created from a rules directory, containing a POSIX extended regular expression (i.e those accepted by the grep -E command). A store operation must specify an identifier that matches the regular expression.
- A retrieve operation transfers a file descriptor from the fd-holder to a client, specifying its corresponding identifier. To allow retrieve operations, a file named S6\_FDHOLDER\_RETRIEVE\_REGEX must exist in the appropriate {uid/\*,gid/\*,default}/env subdirectory of s6-fdholderd's rules directory, or rules file created from a rules directory, containing a POSIX extended regular expression. A retrieve operation must specify an identifier that matches the regular expression.
- A delete operation requests the fd-holder to close a currently held file descriptor, specifying its corresponding identifier, which is then freed. A delete operation is allowed if a store operation specifying the same identifier would be allowed.
- A list operation requests the fd-holder a list of the identifiers of all currently held file descriptors. To allow list operations, a nonempty file named S6\_FDHOLDER\_LIST must exist in the appropriate {uid/\*,gid/\*,default}/env subdirectory of s6-fdholderd's rules directory, or rules file created from a rules directory.
- A get dump operation requests the fd-holder to transfer to a client all currently held file descriptors. The transfered file descriptors are not deleted. To allow get dump operations, a nonempty file named S6\_FDHOLDER\_GETDUMP must exist in the appropriate {uid/\*,gid/\*,default}/env subdirectory of s6-fdholderd's rules directory, or rules file created from a rules directory.
- A set dump operation transfers a subset of a client's file descriptors to the fd-holder, under the control of environment variables. To allow set dump operations, a nonempty file named S6\_FDHOLDER\_SETDUMP must exist in the appropriate {uid/\*,gid/\*,default}/envsubdirectory of s6-fdholderd's rules directory, or rules file created from a rules directory.

### fd-holder clients

s6 provides a client program for each supported fd-holder operation. They all accept options, a UNIX domain socket pathname, and client-specific arguments. A `-t` option followed by a time value in millisecond makes the client program fail with an error message if the requested operation is not processed after the specified time elapses.

#### Store, retrieve, list and delete operations

The client program that performs store operations is s6-fdholder-store. It accepts options, a UNIX domain socket pathname and a file descriptor identifier, and makes the store request to the fd-holder listening on the specified socket, transferring the specified identifier and file descriptor to it via fd-passing. A `-d` option followed by an unsigned integer value specifies the file descriptor to store; no `-d` option is equivalent to `-d 0`, i.e. s6-fdholder-store will store its standard input. If s6-fdholder-store is invoked with a `-T` option followed by a time value in milliseconds, the file descriptor's expiration time is set to that value in the fd-holder. If it is invoked with a `-T 0` option or no `-T` option, the held file descriptor does not expire.

The client program that performs retrieve operations is s6-fdholder-retrieve. It is a [a chain loading program](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) that accepts options, a UNIX domain socket pathname and a file descriptor identifier, and makes the retrieve request to the fd-holder listening on the specified socket, transferring the specified identifier to it, receiving the corresponding file descriptor via fd-passing, and executing the next program in the chain. This program's standard input will be a copy of the received descriptor. If s6-fdholder-retrieve is invoked with a `-D` option, it will also request a delete operation for the file descriptor after retrieving it.

The client program that performs delete operations is s6-fdholder-delete. It accepts options, a UNIX domain socket pathname and a file descriptor identifier, and makes the delete request to the fd-holder listening on the specified socket, transferring the specified identifier to it.

The client program that performs list operations is s6-fdholder-list. It accepts options and a UNIX domain socket pathname, and makes the list request to the fd-holder listening on the specified socket, printing the received identifier list to its standard output.

For further information about all described fd-holder clients please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.

#### Dump operations

Dump operations are controlled by [environment variables](https://wiki.gentoo.org#Environment_variables). For each file descriptor transfered to or from an s6-fdholderd process, there are three corresponding variables: `S6_FD_${n}`, `S6_FDID_${n}` and `S6_FDLIMIT_${n}`, where *${n}* is a placeholder for an unsigned integer value between 0 and N - 1, and **N** represents the value of environment variable `S6_FD#`. The value of `S6_FD_${n}` specifies the file descriptor as an unsigned integer value, the value of `S6_FDID_${n}` specifies the file descriptor's identifier, and the value of `S6_FDLIMIT_${n}` specifies the file descriptor's expiration time for set dump operations, or the file descriptor's remaining time until expiration for get dump operations.

The client program that performs get dump operations is s6-fdholder-getdump. It is [a chain loading program](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) that accepts options and a UNIX domain socket pathname, and makes the get dump request to the fd-holder listening on the specified socket, receiving all file descriptors held by it via fd-passing, and executing the next program in the chain. This program will have a copy of all the received descriptors, and its environment will have the `S6_FD*` variables set to appropriate values.

The client program that performs set dump operations is s6-fdholder-setdump. It accepts options and a UNIX domain socket pathname, and makes the set dump request to the fd-holder listening on the specified socket, transferring the subset of its file descriptors specified by the `S6_FD*` environment variables —which are expected to be set to appropriate values— to the fd-holder, via fd-passing.

For further information about all described fd-holder clients please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.

### File descriptor transfers

s6 also provides an s6-fdholder-transferdump program, that allows transferring all currently held file descriptors in one s6-fdholderd process to another one. The transferred set of file descriptors are added to the destination process' currently held ones. This is implemented by performing a get dump operation with the source process, followed by a set dump operation with the destination process, so these operations must be allowed by the corresponding fd-holder's access control policy. A typical use case of s6-fdholder-transferdump is when an s6-fdholder process needs to upgrade or restart for some reason. Transferring the file descriptors to another, temporary fd-holder allows it to restart without losing all descriptors.

s6-fdholder-transferdump accepts options and the UNIX domain socket pathnames of the source and destinations fd-holders, and performs the transfer between them. If a `-t` option is specified followed by two time values in milliseconds separated by a colon (':'), they will be used as timeouts for the get dump and set dump operations, working just like the `-t` option does for s6-fdholder-getdump and s6-fdholder-setdump.

For further information about s6-fdholder-transferdump please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.

### Historical note

The s6 package used to have programs named s6-fdholder-storec, s6-fdholder-retrievec, s6-fdholder-deletec, s6-fdholder-listc, s6-fdholder-getdumpc, s6-fdholder-setdumpc and s6-fdholder-transferdumpc, which where applications written to be run by an [IPC UCSPI client](https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server#ipcserverd).

s6-fdholder-store, s6-fdholder-retrieve, s6-fdholder-delete, s6-fdholder-list, s6-fdholder-getdump, s6-fdholder-setdump and s6-fdholder-transferdump used to simply call one of these programs, using [chain loading](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) with s6-ipcclient and programs fdmove and fdclose from [dev-lang/execline](https://packages.gentoo.org/packages/dev-lang/execline).

These programs where removed in s6 version 2.9.0.0.

### Examples

Example [s6-rc](https://wiki.gentoo.org/wiki/S6-rc) service definitions in s6-rc-compile source format for holding the reading end of a FIFO:

`user $``ls -l *`
fifo-reader:
total 8
-rwxr-xr-x 1 user user 117 Aug  1 12:00 run
-rw-r--r-- 1 user user   8 Aug  1 12:00 type
fifo-reader-heldfd:
total 12
-rw-r--r-- 1 user user  18 Aug  1 12:00 dependencies
-rwxr-xr-x 1 user user 157 Aug  1 12:00 run
-rw-r--r-- 1 user user   8 Aug  1 12:00 type
fifo-reader-setup:
total 16
-rw-r--r-- 1 user user  14 Aug  1 12:00 dependencies
-rw-r--r-- 1 user user  90 Aug  1 12:00 down
-rw-r--r-- 1 user user   8 Aug  1 12:00 type
-rw-r--r-- 1 user user 143 Aug  1 12:00 up
fifo-writer:
total 8
-rwxr-xr-x 1 user user 84 Aug  1 12:00 run
-rw-r--r-- 1 user user  8 Aug  1 12:00 type
test-fdholder:
total 16
drwxr-xr-x 2 user user 4096 Aug  1 12:00 data
-rw-r--r-- 1 user user    2 Aug  1 12:00 notification-fd
-rwxr-xr-x 1 user user  101 Aug  1 12:00 run
-rw-r--r-- 1 user user    8 Aug  1 12:00 type

**`fifo-writer/type`**

**`fifo-writer/run`**

```
#!/bin/execlineb -P
redirfd -w 1 /home/user/test-fifo
test-daemon
```
test-daemon is assumed to be a program that prints to its standard output a message of the form "Message #n", with an incrementing number **n** between 0 and 9, each time it receives a `SIGHUP` signal. Service fifo-writer, a longrun, is a test-daemon process with its standard output redirected to the /home/user/test-fifo FIFO.

**`fifo-reader/type`**

**`fifo-reader/run`**

```
#!/bin/execlineb -P
redirfd -r 0 /home/user/test-fifo
s6-log t /home/user/logdir
```
Service fifo-reader, a longrun, is an s6-log process that reads from the /home/user/test-fifo FIFO and logs to the /home/user/logdir logging directory. Creating the FIFO:

`user $``mkfifo -m ug=rw,o= test-fifo``user $``ls -l test-fifo`
prw-rw---- 1 user user 0 Aug  5 22:00 test-fifo

Starting both services, assuming that s6-rc-compile has been called on the service definitions set to create a compiled services database, that s6-rc-init has been called after s6-svscan, and that the s6 scan directory and the s6-rc live state directory (named live) are both in the same directory:

`user $``s6-rc -l ../live -v 2 -u change fifo-writer fifo-reader`
s6-rc: info: processing service fifo-writer: starting
s6-rc: info: processing service fifo-reader: starting
s6-rc: info: service fifo-writer started successfully
s6-rc: info: service fifo-reader started successfully

s6-rc's `-v 2` argument increments its verbosity level. Sending three `SIGHUP` signals to test-daemon:

`user $``for i in 1 2 3; do s6-svc -h fifo-writer; done``user $``cat ../logdir/current | s6-tai64nlocal`
2017-08-05 22:06:37.556779100 Message #1
2017-08-05 22:06:37.557858265 Message #2
2017-08-05 22:06:37.558856806 Message #3

This shows that test-daemon's messages were sent over the FIFO to s6-log and logged. Stopping momentarily the fifo-reader service:

`user $``s6-rc -l ../live -v 2 -d change fifo-reader`
s6-rc: info: processing service fifo-reader: stopping
s6-rc: info: service fifo-reader stopped successfully

Sending three more `SIGHUP` signals to test-daemon:

`user $``for i in 4 5 6; do s6-svc -h fifo-writer; done`
test-daemon: warning: Got SIGPIPE
test-daemon: warning: Got SIGPIPE
test-daemon: warning: Got SIGPIPE

Since the FIFO no longer has any reader, writing to it makes the kernel send a `SIGPIPE` signal to the test-daemon process. Restarting the fifo-reader service and sending three final `SIGHUP` signals to test-daemon:

`user $``s6-rc -l ../live -u change fifo-reader``user $``for i in 7 8 9; do s6-svc -h fifo-writer; done``user $``cat ../logdir/current | s6-tai64nlocal`
2017-08-05 22:06:37.556779100 Message #1
2017-08-05 22:06:37.557858265 Message #2
2017-08-05 22:06:37.558856806 Message #3
2017-08-05 22:10:58.036594559 Message #7
2017-08-05 22:10:58.037747723 Message #8
2017-08-05 22:10:58.038749083 Message #9

This shows that the messages sent by test-daemon when s6-log was not running are effectively lost. This can be avoided by modifying the services so that the reading end of the FIFO is held by an fd-holder process.

**`test-fdholder/type`**

**`test-fdholder/run`**

```
#!/bin/execlineb -P
s6-fdholder-daemon -1 -x data/rules /home/user/fdholder-socket
```
**`test-fdholder/notification-fd`**

Service test-fdholder, a longrun, is an s6-fdholderd process bound to socket /home/user/fdholder-socket, with readiness notification enabled (`-1`), and using rules file rules in the data subdirectory of its compiled s6 service directory. `s6-fdholder-daemon -1 -x data/rules /home/user/fdholder-socket` is equivalent to `s6-ipcserver-socketbinder /home/user/fdholder-socket s6-fdholderd -1 -x data/rules`, but shorter. The rules file is created from the following rules directory using a `s6-accessrules-cdb-from-fs test-fdholder/data/rules ../rules.d` command:

`user $``ls -l ../rules.d/*/*`
../rules.d/uid/1000:
total 4
-rw-r--r-- 1 user user    0 Aug  1 12:00 allow
drwxr-xr-x 2 user user 4096 Aug  1 12:00 env
../rules.d/uid/default:
total 0
-rw-r--r-- 1 user user 0 Aug  1 12:00 deny

`user $``ls -l ../rules.d/uid/1000/env`
total 8
-rw-r--r-- 1 user user  1 Aug  1 12:00 S6\_FDHOLDER\_LIST
lrwxrwxrwx 1 user user 23 Aug  1 12:00 S6\_FDHOLDER\_RETRIEVE\_REGEX -> S6\_FDHOLDER\_STORE\_REGEX
-rw-r--r-- 1 user user 32 Aug  1 12:00 S6\_FDHOLDER\_STORE\_REGEX

**`S6_FDHOLDER_STORE_REGEX`**

This rules directory allows *user* (assumed to have user ID 1000) to perform store (and therefore delete), retrieve and list operations. The S6\_FDHOLDER\_LIST file contains a single empty line.

**`fifo-reader-setup/type`**

**`fifo-reader-setup/dependencies`**

**`fifo-reader-setup/up`**

```
 -rnb 0 /home/user/test-fifo
s6-fdholder-store /home/user/fdholder-socket fifo:/home/user/test-fifo
```
**`fifo-reader-setup/down`**

```
 /home/user/fdholder-socket fifo:/home/user/test-fifo
```
Service fifo-reader-setup, a oneshot, opens FIFO /home/user/test-fifo for reading (in non-blocking mode that is changed to blocking afterwards, using execline's redirfd program with options `-n` and `-b`) and stores the corresponding file descriptor in the fd-holder using identifier *fifo:/home/user/test-fifo*. The dependency on service test-fdholder ensures that s6-rc -u change will start s6-fdholderd before trying to store to it. When fifo-reader-setup transitions to the down state, it will delete the held file descriptor.

**`fifo-reader-heldfd/type`**

**`fifo-reader-heldfd/dependencies`**

**`fifo-reader-heldfd/run`**

```
#!/bin/execlineb -P
s6-fdholder-retrieve /home/user/fdholder-socket fifo:/home/user/test-fifo
s6-log t /home/user/logdir
```
Service fifo-reader-heldfd, a longrun, is a modified version of fifo-reader that retrieves the FIFO's reading end from the fd-holder. The dependency on service fifo-reader-setup ensures that s6-rc -u change will have stored the FIFO's reading end first in the fd-holder with the appropriate identifier. Starting the fifo-writer and fifo-reader-heldfd services:

`user $``s6-rc -l ../live -v 2 -u change fifo-writer fifo-reader-heldfd`
s6-rc -l ../live -v 2 -u change fifo-writer fifo-reader-heldfd
s6-rc: info: processing service fifo-writer: starting
s6-rc: info: processing service test-fdholder: starting
s6-rc: info: processing service s6rc-oneshot-runner: starting
s6-rc: info: service fifo-writer started successfully
s6-rc: info: service test-fdholder started successfully
s6-rc: info: service s6rc-oneshot-runner started successfully
s6-rc: info: processing service fifo-reader-setup: starting
s6-rc: info: service fifo-reader-setup started successfully
s6-rc: info: processing service fifo-reader-heldfd: starting
s6-rc: info: service fifo-reader-heldfd started successfully

Sending three `SIGHUP` signals to test-daemon, stopping momentarily the fifo-reader-heldfd service, sending three more `SIGHUP` signals to test-daemon:

`user $``for i in 1 2 3; do s6-svc -h fifo-writer; done``user $``s6-rc -l ../live -v 2 -d change fifo-reader-heldfd`
s6-rc: info: processing service fifo-reader-heldfd: stopping
s6-rc: info: service fifo-reader-heldfd stopped successfully

`user $``for i in 4 5 6; do s6-svc -h fifo-writer; done``user $``cat ../logdir/current | s6-tai64nlocal`
2017-08-05 22:29:45.042414497 Message #1
2017-08-05 22:29:45.043441185 Message #2
2017-08-05 22:29:45.044699928 Message #3

`user $``s6-fdholder-list ../fdholder-socket`
fifo:/home/user/test-fifo

This shows that no more messages are being logged as a consequence, but because there is still an open file descriptor held by s6-fdholderd), there is no `SIGPIPE` signal this time. Restarting the fifo-reader-heldfd service:

`user $``s6-rc -l ../live -u change fifo-reader-heldfd``user $``cat ../logdir/current | s6-tai64nlocal`
2017-08-05 22:29:45.042414497 Message #1
2017-08-05 22:29:45.043441185 Message #2
2017-08-05 22:29:45.044699928 Message #3
2017-08-05 22:33:07.907057008 Message #4
2017-08-05 22:33:07.907113023 Message #5
2017-08-05 22:33:07.907114734 Message #6

This shows that once the file descriptor is retrieved for s6-log, all pending messages are delivered and logged. Sending three final `SIGHUP` signals to test-daemon:

`user $``for i in 7 8 9; do s6-svc -h fifo-writer; done``user $``cat ../logdir/current | s6-tai64nlocal`
2017-08-05 22:29:45.042414497 Message #1
2017-08-05 22:29:45.043441185 Message #2
2017-08-05 22:29:45.044699928 Message #3
2017-08-05 22:33:07.907057008 Message #4
2017-08-05 22:33:07.907113023 Message #5
2017-08-05 22:33:07.907114734 Message #6
2017-08-05 22:33:43.821315726 Message #7
2017-08-05 22:33:43.822098686 Message #8
2017-08-05 22:33:43.823644164 Message #9

This shows that no messages are lost even if the FIFO reader momentarily stops.

### Pre-opening sockets before their servers are running

[systemd](https://wiki.gentoo.org/wiki/Systemd) supports a mechanism it calls *socket activation*, that makes a systemd process pre-open UNIX domain, TCP/IP or Netlink sockets, FIFOs and other special files, and pass them to a child process when a connection is made, a writer opens the FIFO, new data is available for reading, etc. Socket activation is performed when [a socket unit file](https://www.freedesktop.org/software/systemd/man/systemd.socket.html) is provided with an accompanying service unit file. This mechanism combines superserver and fd-holder functionality implemented in process 1, file descriptor inheritance from parent process to child process, and programs written to communicate using pre-opened file descriptors. Similar behaviour can be achieved, for services that want it, by a combination of individual s6 programs like [s6-ipcserverd](https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server#ipcserverd), s6-fdholderd and the fd-holder client programs, without involving process 1. The software package's author notes, however, that, since read operations, and write operations when buffers are full, on a file descriptor held by an fd-holder will block, speed gains might no be that significant, and that, dependending on the scenario (e.g. logging infrastructure), communicating with a service assuming it is ready when it actually isn't might hurt reliability<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

Example [s6-rc](https://wiki.gentoo.org/wiki/S6-rc) service definitions in s6-rc-compile source format for pre-opening a UNIX domain socket and pass it to a program written to be executed by a IPC UCSPI server:

`user $``ls -l *`
test-fdholder:
total 16
drwxr-xr-x 2 user user 4096 Aug  1 12:00 data
-rw-r--r-- 1 user user    2 Aug  1 12:00 notification-fd
-rwxr-xr-x 1 user user  101 Aug  1 12:00 run
-rw-r--r-- 1 user user    8 Aug  1 12:00 type
test-server-heldfd:
total 12
-rw-r--r-- 1 user user  18 Aug  1 12:00 dependencies
-rwxr-xr-x 1 user user 140 Aug  1 12:00 run
-rw-r--r-- 1 user user   8 Aug  1 12:00 type
test-server-setup:
total 16
-rw-r--r-- 1 user user  14 Aug  1 12:00 dependencies
-rw-r--r-- 1 user user  92 Aug  1 12:00 down
-rw-r--r-- 1 user user   8 Aug  1 12:00 type
-rw-r--r-- 1 user user 158 Aug  1 12:00 up

Service test-fdholder is the same as in the previous FIFO example.

**`test-server-setup/type`**

**`test-server-setup/dependencies`**

**`test-server-setup/up`**

```
 /home/user/test-socket
s6-fdholder-store /home/user/fdholder-socket unix:/home/user/test-socket
```
**`test-server-setup/down`**

```
 /home/user/fdholder-socket unix:/home/user/test-socket
```
Service test-server-setup, a oneshot, pre-opens a listening UNIX domain socket bound to /home/user/test-socket using s6-ipcserver-socketbinder, and stores the corresponding file descriptor in the fd-holder using identifier *unix:/home/user/test-socket*. The dependency on service test-fdholder ensures that s6-rc -u change will start s6-fdholderd before trying to store to it. When test-server-setup transitions to the down state, it will delete the held file descriptor.

**`test-server-heldfd/type`**

**`test-server-heldfd/dependencies`**

**`test-server-heldfd/run`**

```
#!/bin/execlineb -P
s6-fdholder-retrieve /home/user/fdholder-socket unix:/home/user/test-socket
s6-ipcserverd test-server
```
Service test-server-heldfd, a longrun, retrieves the listening socket's file descriptor from the fd-holder, and invokes super-server s6-ipcserverd to handle incoming connections, spawning a test-server process for each one. test-server is the same program used in the example contained [here](https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server#examples). The dependency on service test-server-setup ensures that s6-rc -u change will have stored the listening socket's file descriptor first in the fd-holder with the appropriate identifier. Starting the test-server-setup service:

`user $``s6-rc -l ../live -v 2 -u change test-server-setup`
s6-rc: info: processing service test-fdholder: starting
s6-rc: info: processing service s6rc-oneshot-runner: starting
s6-rc: info: service s6rc-oneshot-runner started successfully
s6-rc: info: service test-fdholder started successfully
s6-rc: info: processing service test-server-setup: starting
s6-rc: info: service test-server-setup started successfully

s6-rc's `-v 2` argument increments its verbosity level. Starting a UCSPI client in the background to connect to the server:

`user $``s6-ipcclient ../test-socket test-client &`
Connecting to server...

test-client is the same program used in the example contained [here](https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server#examples). This shows that test-client has been launched with a connection to the server's listening socket, but because its file descriptor is held by s6-fdholderd and no server program is currently running, it blocks on an I/O operation.

`user $``s6-fdholder-list ../fdholder-socket`
unix:/home/user/test-socket

This shows that s6-fdholderd is holding the listening socket's file descriptor. Starting the test-server-heldfd service:

`user $``s6-rc -l ../live -v 2 -u change test-server-heldfd`
s6-rc: info: processing service test-fdholder: already up
s6-rc: info: processing service s6rc-oneshot-runner: already up
s6-rc: info: processing service test-server-setup: already up
s6-rc: info: processing service test-server-heldfd: starting
s6-rc: info: service test-server-heldfd started successfully
Server process created with PID 2377, client is "user"

This shows that once s6-ipcserverd has started and retrieved the listening socket's file descriptor from s6-fdholderd, it accepts the connection and spawns test-server to handle it.
