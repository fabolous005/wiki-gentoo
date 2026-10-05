<!-- source: https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server | group: Gentoo Wiki (Main) | wiki-title: S6/UNIX domain super-server -->
---
title: S6/UNIX domain super-server
url: https://wiki.gentoo.org/wiki/S6/UNIX_domain_super-server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-08-20"
fingerprint: "8bb10818fe83ce8c"
license: CC BY-SA 4.0
---

# S6/UNIX domain super-server

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

s6 provides two programs, s6-ipcserver-socketbinder and s6-ipcserverd, that together implement a UNIX domain super-server. A *UNIX domain super-server* creates a listening stream mode UNIX domain socket (i.e. a `SOCK_STREAM` socket for address family `AF_UNIX`) and spawns a *server program* to handle each incoming connection after accepting it, so that from there on, the client communicates over the connection with the spawned server. This the UNIX domain equivalent of a TCP/IP super-server, such as xinetd ([sys-apps/xinetd](https://packages.gentoo.org/packages/sys-apps/xinetd)) or ipsvd ([net-misc/ipsvd](https://packages.gentoo.org/packages/net-misc/ipsvd)).

This article describes [s6](https://wiki.gentoo.org/wiki/S6)'s UNIX domain super-server and related tools.

## Configuration

### Environment variables

- `PROTO` — Set by s6-ipcclient and s6-ipcserverd to the value 'IPC', as per the IPC UCSPI specification, and used by s6-ipcserver-access and s6-connlimit to construct the names of other environment variables.
- `IPCREMOTEEUID` — Set by s6-ipcserverd to the effective user ID of the client, as per the IPC UCSPI specification, unless credentials lookups have been disabled. Read by s6-ipcserver-access (if the value of `PROTO` is 'IPC') to decide whether to allow or refuse access to the server.
- `IPCREMOTEEGID` — Set by s6-ipcserverd to the effective group ID of the client, as per the IPC UCSPI specification, unless credentials lookups have been disabled. Read by s6-ipcserver-access (if the value of `PROTO` is 'IPC') to decide whether to allow or refuse access to the server.
- `IPCCONNNUM` — Set by s6-ipcserverd to the number of connections originating from the same user (i.e. same user ID), and read by s6-connlimit (if the value of `PROTO` is 'IPC') to decide if the maximum number of connections originating from the same user will exceed the maximum allowed.
- `IPCCONNMAX` — Maximum number of connections originating from the same client allowed by s6-connlimit (if the value of `PROTO` is 'IPC').

## Usage


### s6-ipcserver-socketbinder and s6-ipcserverd

More specifically, what s6-ipcserver-socketbinder and s6-ipcserverd implement together is an *IPC UCSPI super-server*, i.e. a super-server that adheres to the server side of Daniel J. Bernstein's *UNIX Client-Server Program Interface* (UCSPI) and supports the IPC UCSPI protocol<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. The UCSPI defines an interface for client-server communications tools; *UCSPI tools* are executable programs that accept options, a protocol-specific *address*, an *application* name and its arguments. Tools can be either clients or servers, clients communicate with servers using a *connection*. If the UCSPI tool is a server, the application is invoked with the supplied arguments each time there is an incoming connection to the specified address, with file descriptor 0 opened for reading from the connection, file descriptor 1 opened for writing to the connection, and environment variables set to defined values. If the UCSPI tool is a client, a connection is made to the specified address and, if successful, the application is invoked with the supplied arguments, with file descriptor 6 opened for reading from the connection, file descriptor 7 opened for writing to the connection, and environment variables set to defined values. One of the application environment variables set by both UCSPI clients and servers is `PROTO`, with the name of the supported protocol as its value. The protocol implemented by the s6 programs is the *IPC UCSPI protocol*, for which the *address* specified to UCSPI tools is defined to be a UNIX domain socket pathname, and the value of `PROTO` is defined to be 'IPC'.

s6-ipcserver-socketbinder is [a chain loading program](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) that accepts options and a pathname. It creates a UNIX domain socket, binds it to the specified pathname and prepares it to accept connections with the POSIX `listen()` call. The next program in the chain will have its standard input connected to the listening socket, which will be **non-blocking** (`O_NONBLOCK`). The number of *backlog connections*, i.e. the number of outstanding connections in the socket's listen queue, can be set with the `-b` option; additional connection attempts will be rejected by the kernel. If s6-ipcserver-socketbinder is invoked with a `-b 0` option, the socket will be created but won't be listening, i.e. `listen()` won't be called. If it is invoked with the `-m` option, the created socket will be a datagram mode socket (`SOCK_DGRAM`), which also requires specifying a `-b 0` option. If it is invoked without the `-m` option, or with the `-M` option, the created socket will be a stream mode socket (`SOCK_STREAM`).

s6-ipcserverd is a program that must have its standard input redirected to a bound and listening stream mode UNIX domain socket, and accepts a program name and its arguments. For each connection made to the socket, s6-ipcserverd executes the program with the supplied arguments as a child process, that has its file descriptors 0 and 1 redirected, on Gentoo, to the socket returned by a Linux `accept4()` with the listening socket's file descriptor as an argument, and the following environment variables:

- `PROTO`, `IPCREMOTEEUID`, `IPCREMOTEEGID` and `IPCCONNNUM` — set as specified in ["Environment variables"](https://wiki.gentoo.org#Environment_variables).
- `IPCREMOTEPATH` — set to the pathname associated with the remote UNIX domain socket (on Gentoo, as contained in the `sun_path` field of the `struct sockaddr_un` object filled by the `accept4()` call), if any, as per the IPC UCSPI specification. Be aware that it may contain arbitrary characters.

On Gentoo, `IPCREMOTEEUID` and `IPCREMOTEEGID` are set with information obtained from a POSIX `getsockopt()` call with the Linux `SO_PEERCRED` option; this is called *credentials lookup*. If s6-ipcserverd is invoked with a `-P` option, credentials lookup will be disabled, and `IPCREMOTEEUID` and `IPCREMOTEEGID` will be unset. If it is invoked without a `-P` option, or with a `-p` option, credentials lookup will be enabled. s6-ipcserverd supports the [s6 readiness protocol](https://wiki.gentoo.org/wiki/S6#s6readiness), and if it is invoked with a `-1` option, it will turn readiness notification on, with file descriptor 1 (i.e. its standard output) as the notification channel's file descriptor.

If s6-ipcserverd receives a `SIGTERM` signal it will exit, but its children will continue running. If it receives a `SIGQUIT` signal, it will send its children a `SIGTERM` signal followed by a `SIGCONT` signal and then exit. If it receives a `SIGABRT` signal, it will send its children a `SIGKILL` signal and then exit. It is possible to make s6-ipcserverd kill its children without exiting (with a `SIGTERM` signal followed by a `SIGCONT` signal), by sending it a `SIGHUP` signal. s6-ipcserverd ignores the `SIGPIPE` signal, so at program startup, every child process will ignore it as well.

s6-ipcserver is a helper program that accepts options, a UNIX domain socket pathname, a program name and its arguments, and invokes s6-ipcserver-socketbinder chained to s6-ipcserverd, or s6-ipcserver-socketbinder chained to s6-applyuidgid, chained to s6-ipcserverd, depending on the options. The socket pathname is passed to s6-ipcserver-socketbinder, and the program name and its arguments, to s6-ipcserverd. s6-ipcserver options specify corresponding s6-ipcserver-socketbinder, s6-applyuidgid and s6-ipcserverd options. The created socket is a stream mode socket.

For further information about s6-ipcserver, s6-ipcserver-socketbinder or s6-ipcserverd please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.

### s6-ipcclient

s6-ipcclient is an IPC UCSPI client. It is a [chain loading program](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) that accepts options and a UNIX domain socket pathname. It creates a stream mode socket, makes a connection to the socket specified by the supplied pathname, and executes the next program in the chain with file descriptors 6 and 7 redirected to the local connected socket, and the following environment variables:

- `PROTO` — set as specified in ["Environment variables"](https://wiki.gentoo.org#Environment_variables).
- `IPCLOCALPATH` — set to the pathname associated with the local UNIX domain socket it is using for the connection, as per the IPC UCSPI specification, with information obtained from a POSIX `getsockname()` call.

If s6-ipcclient is invoked with a `-p pathname` argument, it will bind the created socket to *pathname* (using the POSIX `bind()` call) before initiating the connection to the remote socket. If it is invoked with a `-l value` argument, it will set `IPCLOCALPATH` to *value*, instead of using `getsockname()`.

For further information please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.


### s6-ipcserver-access and rules files and directories

s6-ipcserver-access is an access control tool for UNIX domain sockets. It is a [chain loading program](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) that must be spawned by a UCSPI server (like [s6-ipcserverd](https://wiki.gentoo.org#ipcserverd)) that appropriately sets the `PROTO`, `${PROTO}REMOTEEUID` and `${PROTO}REMOTEEGID` environment variables, where *${PROTO}* is the value of `PROTO`. It decides whether or not to execute the next program in the chain, or to execute a completely different program instead, based on either a *rules directory* or a *rules file*. The `-i` option specifies the pathname of a rules directory and the `-x` option specifies the pathname of a rules file.

If a rules directory **R** is specified with an `-i` option:

1. s6-ipcserver-access will first search the R/uid directory:
  1. (s6 version 2.8.0.0 or later) If it finds a subdirectory named self, and the value of the `${PROTO}REMOTEEUID` variable, which must be a numeric user ID, is equal to its effective user ID, and the subdirectory contains a file named allow, s6-ipcserver-access executes the next program in the chain. If the subdirectory does not contain a file named allow, but contains instead a file named deny, or if it does not contain any of those files, s6-ipcserver-access exits with code 1, and the next program in the chain is not executed.
  2. Otherwise, if it finds a subdirectory with a name that matches the value of the `${PROTO}REMOTEEUID` variable, and it contains a file named allow, s6-ipcserver-access executes the next program in the chain. If the subdirectory does not contain a file named allow, but contains instead a file named deny, or if it does not contain any of those files, s6-ipcserver-access exits with code 1, and the next program in the chain is not executed.
2. If R/uid search fails, or there is no such directory, s6-ipcserver-access will then search the R/gid directory:
  1. (s6 version 2.8.0.0 or later) If it finds a subdirectory named self, and the value of the `${PROTO}REMOTEEGID` variable, which must be a numeric group ID, is equal to its effective group ID, and the subdirectory contains a file named allow, s6-ipcserver-access executes the next program in the chain. If the subdirectory does not contain a file named allow, but contains instead a file named deny, or if it does not contain any of those files, s6-ipcserver-access exits with code 1, and the next program in the chain is not executed.
  2. Otherwise, if it finds a subdirectory with a name that matches the value of the `${PROTO}REMOTEEGID` variable, and it contains a file named allow, s6-ipcserver-access executes the next program in the chain. If the subdirectory does not contain a file named allow, but contains instead a file named deny, or if it does not contain any of those files, s6-ipcserver-access exits with code 1, and the next program in the chain is not executed.
3. If R/gid search fails, or there is no such directory, s6-ipcserver-access will finally search for an R/default directory. If it contains a file named allow, s6-ipcserver-access executes the next program in the chain. If the subdirectory does not contain a file named allow, but contains instead a file named deny, or if it does not contain any of those files, s6-ipcserver-access exits with code 1, and the next program in the chain is not executed.
4. If there is no R/default directory, s6-ipcserver-access exits with code 1, and the next program in the chain is not executed.

If in any of these steps s6-ipcserver-access finds an allow file in a matching directory **M** and executes the next program in the chain:

- Unless it was invoked with an `-E` option, it will add variable `${PROTO}LOCALPATH` to the program's environment, set to the pathname associated with the local UNIX domain socket (the one its standard input and output read from and write to, respectively), as reported by the POSIX `getsockname()` call.
- If it was invoked with an `-E` option, it will remove variables `PROTO`, `${PROTO}REMOTEPATH`, `${PROTO}REMOTEEUID`, `${PROTO}REMOTEEGID` and `${PROTO}CONNNUM` from the program's environment.
- If **M** contains a subdirectory named env, s6-ipcserver-access will modify the program's environment as if an `s6-envdir M/env` command had been used.
- If **M** contains a regular file named exec, s6-ipcserver-access will execute a different program instead of the one supplied as an argument, as if an `execlineb -c contents` command had been used, where *contents* is the contents of the M/exec file (e.g. the name of a program that can be found via `PATH` search plus its arguments), passed to execlineb as a single argument. execlineb is the script parser and launcher from the execline package ([dev-lang/execline](https://packages.gentoo.org/packages/dev-lang/execline)).

A rules file is a [constant database (CDB) file](https://cr.yp.to/cdb/cdb.txt) created from a rules directory using the s6-accessrules-cdb-from-fs program. If a rules file is specified with an `-x` option, s6-ipcserver-access will behave as if the corresponding rules directory had been specified with an `-i` option. A rules directory can be re-created from a rules CDB file using the s6-accessrules-fs-from-cdb program.

If s6-ipcserver-access is invoked with neither the `-i` option nor the `-x` option, it will execute the next program in the chain, i.e. it will unconditionally grant access.

For the full description of s6-ipcserver-access's, s6-accessrules-cdb-from-fs's and s6-accessrules-fs-from-cdb's functionality please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory. See also [suidless privilege gain tools](https://wiki.gentoo.org/wiki/S6#s6sudo) for s6-ipcserver-access usage examples.

### s6-connlimit and s6-ioconnect

s6-connlimit is a [chain loading program](https://wiki.gentoo.org/wiki/Daemontools-encore#changingstate) that limits connections from the same client to an UCSPI server based on the `PROTO`, `${PROTO}CONNNUM`, `${PROTO}CONNMAX` environment variables (see [environment variables](https://wiki.gentoo.org#Environment_variables)), where *${PROTO}* is the value of the `PROTO` variable.

s6-ioconnect is a program that performs data transmission from file descriptor 0 to file descriptor 7, and from file descriptor 6 to file descriptor 1, all of them assumed to be open at program startup. That is, s6-ioconnect performs full-duplex data transmission.

For the full description of s6-connlimit's and s6-ioconnect's functionality please consult the HTML documentation in the s6 package's /usr/share/doc subdirectory.


## Examples

**`test-server.cxx`**

**Example C++ aplication to be executed by a IPC UCSPI server**

```
#include <cerrno>
#include <cinttypes>
#include <cstring>
#include <iostream>
#include <pwd.h>
#include <sstream>
#include <string>
#include <unistd.h>
constexpr int ucspi_server_read = 0;
constexpr int ucspi_server_write = 1;
void read_from_socket(char *buffer, const size_t buffer_size) {
  size_t n = 0;
  while (ssize_t r = read(ucspi_server_read, buffer + n, buffer_size - n)) {
    if (r < 0) throw errno;
    n += r;
    if (n == buffer_size) break;
  }
  buffer[n] = 0;
}
void write_to_socket() {
  std::ostringstream out;
  const char *env_uid = getenv("IPCREMOTEEUID");
  const passwd *acct = env_uid? getpwuid(static_cast<uid_t>(std::strtoimax(env_uid, 0, 10))): nullptr;
  out << "Server process created with PID " << getpid() <<
    ", client is \"" << (acct? acct->pw_name: "<unavailable>") << "\"\n";
  if (write(ucspi_server_write, out.str().data(), out.str().length()) < 0) throw errno;
}
int main(){
try {
  char buffer[] = "Hello!";
  const std::string greeting(buffer);
  read_from_socket(buffer, greeting.length());
  if (greeting == buffer) write_to_socket();
  sleep(10);
  return 0;
}
catch (int err) {
  std::cerr << "test-server: fatal: " << std::strerror(err) << '\n';
  return 1;
}
catch (...) {
  return 1;
}
}
```
The application reads from the open file descriptor supplied by the UCSPI server, expecting to receive a "Hello!" message from the client, and if it does, it sends a response that contains the application's process ID and the account database username corresponding to the client's user ID, supplied by the UCSPI server via the `IPCREMOTEEUID` environment variable. The application then waits for 10 seconds, and finally exits.

**`test-client.cxx`**

**Example C++ aplication to be executed by a IPC UCSPI client**

```
#include <cerrno>
#include <cstring>
#include <iostream>
#include <string>
#include <unistd.h>
constexpr int ucspi_client_read = 6;
constexpr int ucspi_client_write = 7;
void read_from_socket(std::string &in) {
  const int buffer_size(100);
  char buffer[buffer_size];
  while (ssize_t r = read(ucspi_client_read, buffer, buffer_size)) {
    if (r < 0) throw errno;
    char *p = static_cast<char *>(std::memchr(buffer, '\n', buffer_size));
    in.append(buffer, p? p + 1: buffer + r);
    if (p) break;
  }
}
inline void write_to_socket(const char *greeting) {
  if (write(ucspi_client_write, greeting, std::strlen(greeting)) < 0) throw errno;
}
int main() {
try {
  std::cout << "Connecting to server...\n";
  std::cout.flush();
  const char greeting[] = "Hello!";
  write_to_socket(greeting);
  std::string in;
  read_from_socket(in);
  if (!in.empty()) std::cout << in;
  return 0;
}
catch (int err) {
  std::cerr << "test-client: fatal: " << std::strerror(err) << '\n';
  return 1;
}
catch (...) {
  return 1;
}
}
```
The application prints "Connecting to server..." to its standard output, sends a "Hello!" message to the server using the open file descriptor supplied by the UCSPI client, and waits for a reply, which is printed to its standard output. The application then exits.

Starting the UCSPI super-server:

`user1 $``s6-ipcserver test-socket ./test-server &``user1 $``ls -l test-socket`
srwxrwxrwx 1 user1 user1 0 Aug  4 12:00 test-socket

This shows that a socket named test-socket has been created in the current working directory. Starting a UCSPI client to connect to the socket three times:

`user1 $``s6-ipcclient test-socket ./test-client`
Connecting to server...
Server process created with PID 1992, client is "user1"

`user1 $``s6-ipcclient test-socket ./test-client`
Connecting to server...
Server process created with PID 1994, client is "user1"

`user1 $``s6-ipcclient test-socket ./test-client`
Connecting to server...
Server process created with PID 1996, client is "user1"

`user1 $``ps f -o pid,ppid,args`
PID  PPID COMMAND
...
1977  1974 \\\_ bash
1985  1977     \\\_ s6-ipcserverd -- ./test-server
1992  1985         \\\_ ./test-server
1994  1985         \\\_ ./test-server
1996  1985         \\\_ ./test-server
...

This shows that the super-server spawned three test-server processes to handle each connection, and set the `IPCREMOTEEUID` environment variable to user1's user ID. `s6-ipcserver test-socket ./test-server` is equivalent to `s6-ipcserver-socketbinder test-socket s6-ipcserverd ./test-server`, but shorter. Starting a UCSPI client with effective user *user2*:

`user2 $``s6-ipcclient test-socket ./test-client`
Connecting to server...
Server process created with PID 2009, client is "user2"

This shows that the super-server set the `IPCREMOTEEUID` environment variable to user2's user ID. Starting the super-server with the `-P` option, and a client to connect to test-socket:

`user1 $``s6-ipcserver -P test-socket ./test-server &``user1 $``s6-ipcclient test-socket ./test-client`
Connecting to server...
Server process created with PID 2021, client is "\<unavailable>"

`s6-ipcserver -P test-socket ./test-server` is equivalent to `s6-ipcserver-socketbinder test-socket s6-ipcserverd -P ./test-server`, but shorter. This shows that since credentials lookup was disabled, environment variable `IPCREMOTEEUID` is unset, and test-server displays "\<unavailable>" in place of a username.
