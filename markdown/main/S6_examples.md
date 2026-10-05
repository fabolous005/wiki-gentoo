<!-- source: https://wiki.gentoo.org/wiki/S6/examples | group: Gentoo Wiki (Main) | wiki-title: S6/examples -->
---
title: S6/examples
url: https://wiki.gentoo.org/wiki/S6/examples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
fingerprint: b919ea7523e29d2b
license: CC BY-SA 4.0
---

# S6/examples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page provides various examples of using [s6](https://wiki.gentoo.org/wiki/S6).

## Examples


### s6 supervision

Example s6 scandir with down, finish, and timeout-kill files, as well as a symbolic link to a supervise directory elsewhere, and execline scripts:

`user $``ls -l * .s6-svscan`
.s6-svscan:
total 4
-rwxr-xr-x 1 user user 20 Mar 24 10:00 finish
test-service1:
total 8
-rwxr-xr-x 1 user user 52 Mar 24 10:00 finish
-rwxr-xr-x 1 user user 32 Mar 24 10:00 run
lrwxrwxrwx 1 user user 24 Mar 24 10:00 supervise -> ../../external-supervise
test-service2:
total 12
-rw-r--r-- 1 user user  0 Mar 24 10:00 down
-rwxr-xr-x 1 user user 99 Mar 24 10:00 finish
-rwxr-xr-x 1 user user 76 Mar 24 10:00 run
-rw-r--r-- 1 user user  6 Mar 24 10:00 timeout-finish
test-service3:
total 12
-rwxr-xr-x 1 user user 75 Mar 24 10:00 finish
-rwxr-xr-x 1 user user 39 Mar 24 10:00 run
-rw-r--r-- 1 user user  6 Mar 24 10:00 timeout-kill

**`.s6-svscan/finish`**

```
#!/bin/execlineb -P
```
This file is used for s6-svscan's [finish procedure](https://wiki.gentoo.org#s6svscanfinish).

**`test-service1/run`**

```
#!/bin/execlineb -P
test-daemon
```
This file allows executing a hipothetical test-daemon program as a supervised process.

**`test-service1/finish`**

```
#!/bin/execlineb -P
s6-permafailon 10 2 SIGINT exit
```
This makes test-service1 fail permanently if test-daemon is killed by a `SIGINT` signal 2 or more times in 10 seconds or less.

**`test-service2/run`**

```
#!/bin/execlineb -P
foreground { echo Starting test-service2/run }
sleep 10
```
**`test-service2/finish`**

```
#!/bin/execlineb -S0
foreground { echo Executing test-service2/finish with arguments $@ }
sleep 10
```
Since the test-service2/finish script runs for more than 5 seconds, a timeout-finish file is needed to prevent the process from being killed by s6-supervise before it completes its execution.

**`test-service2/timeout-finish`**

```
20000
```
**`test-service3/run`**

```
#!/bin/execlineb -P
test-daemon-sighup
```
This file allows executing a hipothetical test-daemon-sighup program as a supervised process, that is assumed to use signal `SIGHUP` as its 'stop' command, instead of `SIGTERM`.

**`test-service3/finish`**

```
#!/bin/execlineb -S0
echo Executing test-service3/finish with arguments $@
```
**`test-service3/timeout-kill`**

```
10000
```
This makes s6-supervise send test-daemon-sighup a `SIGKILL` signal if it is still alive after 10 seconds have elapsed since an s6-svc -d command has been used to try stop the daemon.

Resulting supervision tree when s6-svscan is run on this scandir as a background process in an interactive shell, assuming it is the working directory (i.e. launched with `s6-svscan &`):

`user $``ps xf -o pid,ppid,pgrp,euser,args`
PID  PPID  PGRP EUSER    COMMAND
...
1476  1461  1476 user     -bash
1753  1476  1753 user      \\\_ s6-svscan
1754  1753  1753 user          \\\_ s6-supervise test-service3
1757  1754  1757 user          |   \\\_ test-daemon-sighup
1755  1753  1753 user          \\\_ s6-supervise test-service1
1758  1755  1758 user          |   \\\_ test-daemon
1756  1753  1753 user          \\\_ s6-supervise test-service2
...

Status of all services reported by s6-svstat in human-readable format:

`user $```for i in *; do printf "$i: `s6-svstat $i`\n"; done``
test-service1: up (pid 1758) 47 seconds
test-service2: down (exitcode 0) 47 seconds, ready 47 seconds
test-service3: up (pid 1757) 47 seconds

Output when only the service state, PID, exit code and killing signal information is requested:

`user $```for i in *; do printf "$i: `s6-svstat -upes $i`\n"; done``
test-service1: true 1758 -1 NA
test-service2: false -1 0 NA
test-service3: true 1757 -1 NA

This s6-svstat invocation is equivalent to `s6-svstat -o up,pid,exitcode,signal $i`. The PID is displayed as "-1" for test-service2 because it is in down state.

supervise subdirectory contents:

`user $``ls -l */supervise ../external-supervise`
lrwxrwxrwx 1 user user   24 Mar 24 10:00 test-service1/supervise -> ../../external-supervise
../external-supervise:
total 4
prw------- 1 user user  0 Mar 24 10:05 control
-rw-r--r-- 1 user user  0 Mar 24 10:05 death\_tally
-rw-r--r-- 1 user user  0 Mar 24 10:05 lock
-rw-r--r-- 1 user user 35 Mar 24 10:05 status
test-service2/supervise:
total 4
prw------- 1 user user  0 Mar 24 10:05 control
-rw-r--r-- 1 user user  0 Mar 24 10:05 death\_tally
-rw-r--r-- 1 user user  0 Mar 24 10:05 lock
-rw-r--r-- 1 user user 35 Mar 24 10:05 status
test-service3/supervise:
total 4
prw------- 1 user user  0 Mar 24 10:05 control
-rw-r--r-- 1 user user  0 Mar 24 10:05 death\_tally
-rw-r--r-- 1 user user  0 Mar 24 10:05 lock
-rw-r--r-- 1 user user 35 Mar 24 10:05 status

Messages sent by test-service2/run to s6-svscan's standard output when manually started:

`user $``s6-svc -u test-service2`
Starting test-service2/run
Executing test-service2/finish with arguments 0 0
Starting test-service2/run
Executing test-service2/finish with arguments 0 0
Starting test-service2/run
Executing test-service2/finish with arguments 0 0
...

Current death tally for test-service2:

`user $``s6-svdt test-service2`
@400000005c9785c6127e81fc exitcode 0
@400000005c9785da1358c8b4 exitcode 0
@400000005c9785ee13c9e85b exitcode 0

The timestamps are in external TAI64N format; they can be displaed in human-readable format and local time using s6-tai64nlocal:

`user $``s6-svdt test-service2 | s6-tai64nlocal`
2019-03-24 10:26:57.310280700 exitcode 0
2019-03-24 10:27:17.324585652 exitcode 0
2019-03-24 10:27:37.331999323 exitcode 0

`user $``s6-svstat test-service2`
up (pid 2237) 5 seconds, normally down

After enough seconds have elapsed:

`user $``s6-svstat test-service2`
down (exitcode 0) 6 seconds, want up

The output of s6-svdt, s6-svstat and test-service2/finish shows that test-service2/run exits each time with an exit code of 0. Reliably sending a `SIGSTOP` signal, and later a `SIGTERM` signal, to test-service2/run:

`user $````
s6-svc -p test-service2
```
`user $``s6-svc -t test-service2``user $``s6-svstat test-service2`
up (pid 2312) 18 seconds, normally down, paused

The output of s6-svstat shows that test-service2/run is stopped indeed ("paused"), so `SIGTERM` doesn't have any efect yet. To resume the process a `SIGCONT` signal is needed:

`user $``s6-svc -c test-service2`
Executing test-service2/finish with arguments 256 15
Starting test-service2/run
Executing test-service2/finish with arguments 0 0
Starting test-service2/run
...

The output of test-service2/finish shows that after resuming execution, test-service2/run was killed by the `SIGTERM` signal that was awaiting delivery (signal 15), and since the process is supervised, s6-supervise restarts test-service2/run after test-service2/finish exits.

Messages sent by test-service2/run to s6-svscan's standard output when manually stopped:

`user $``s6-svc -d test-service2`
Executing test-service2/finish with arguments 256 15

As shown by test-service2/finish, s6-supervise stopped test-service2/run by killing it with a `SIGTERM` signal (signal 15).

Sending two consecutive and sufficiently close `SIGINT` signals to test-daemon:

`user $````
s6-svc -i test-service1
```
`user $``s6-svstat test-service1`
up (pid 1799) 7 seconds

`user $``s6-svc -i test-service1`
s6-permafailon: info: PERMANENT FAILURE triggered after 2 events involving signal 2 in the last 10 seconds

This shows that s6-permafailon's condition was triggered, so it exited with code 125. Because it was executed from test-service1/finish, this signals permanent failure to s6-supervise, so test-daemon is not restarted:

`user $``s6-svstat test-service1`
down (signal SIGINT) 16 seconds, normally up, ready 16 seconds

`user $``s6-svdt test-service1 | s6-tai64nlocal`
2019-03-24 10:39:42.918138981 signal SIGINT
2019-03-24 10:39:50.705347226 signal SIGINT

This shows test-daemon's two recorded termination events ("involving signal 2", i.e. `SIGINT`, as reported by s6-permafailon's message).

Manually stopping test-daemon-sighup:

`user $````
s6-svc -d test-service3
```
`user $``s6-svstat test-service3`
up (pid 1757) 137 seconds, want down
Executing test-service3/finish with arguments 256 9

The output of s6-svstat shows that test-daemon-sighup could not be stopped ("up" but also "want down") because it ignores `SIGTERM`. The service directory contains a timeout-kill file, so after waiting the specified 10 seconds, s6-supervise killed test-daemon-sighup with a `SIGKILL` signal (signal 9), as shown by test-service3/finish's message.

`user $``s6-svstat test-service3`
down (signal SIGKILL) 14 seconds, normally up, ready 14 seconds

The output of s6-svstat confirms that test-daemon-sighup was killed by a `SIGKILL` signal. Output of s6-svstat when only the service state, PID, exit code and killing signal information is requested:

`user $```for i in *; do printf "$i: `s6-svstat -upes $i`\n"; done``
test-service1: false -1 -1 SIGINT
test-service2: false -1 -1 SIGTERM
test-service3: false -1 -1 SIGKILL

This shows that no services are currently in up state ("false"), so their PIDs are displayed as "-1", and that the processes have been killed by signals, so their exit codes are displayed as "-1".

Creating a down-signal file in service directory test-service3, restarting test-daemon-sighup and then using an s6-svc -r command:

`user $````
echo SIGHUP >test-service3/down-signal
```
`user $````
s6-svc -u test-service3
```
`user $``s6-svc -r test-service3`
test-daemon-sighup: Got SIGHUP, exiting...
Executing test-service3/finish with arguments 0 0

`user $``s6-svstat test-service3`
up (pid 1760) 24 seconds

The output of s6-svstat and test-service3/finish shows that test-daemon-sighup exited normally with code 0, because s6-supervise sent it the required 'stop' signal (`SIGHUP`, as shown by test-daemon-sighup's message), and was then restarted it as usual. Stopping test-daemon-sighup with an s6-svc -d command:

`user $``s6-svc -d test-service3`
test-daemon-sighup: Got SIGHUP, exiting...
Executing test-service3/finish with arguments 0 0

`user $``s6-svstat test-service3`
down (exitcode 0) 84 seconds, normally up, ready 84 seconds

Again, as shown by the output of s6-svstat and test-service3/finish, test-daemon-sighup exited normally with code 0. Displaying its death tally:

`user $``s6-svdt test-service3 | s6-tai64nlocal`
2019-03-24 10:52:12.326931580 signal SIGKILL
2019-03-24 10:53:56.611814401 exitcode 0
2019-03-24 10:54:32.035755511 exitcode 0


#### s6-svscan's finish procedure

When s6-svscan is asked to exit using s6-svscanctl, it tries to execute a file named finish, expected to be in the .s6-svscan control subdirectory of the scan directory. The program does this using the POSIX `execve()` call, so no new process will be created, and .s6-svscan/finish will have the same process ID as s6-svscan.

.s6-svscan/finish is invoked with a single argument that depends on how s6-svscanctl is invoked:

- If s6-svscanctl is invoked with the `-s` option, .s6-svscan/finish will be invoked with a **halt** argument.
- If s6-svscanctl is invoked with the `-p` option, .s6-svscan/finish will be invoked with a **poweroff** argument.
- If s6-svscanctl is invoked with the `-r` option, .s6-svscan/finish will be invoked with a **reboot** argument.

This behaviour supports [running s6-svscan as process 1](https://wiki.gentoo.org#s6process1). Just as run or finish files in a service directory, .s6-svscan/finish can have any file format that the kernel knows how to execute, but is usually an execline script. If s6-svscan is not running as process 1, the argument supplied to .s6-svscan/finish is usually meaningless and can be ignored. The file can be used just for cleanup in that case, and if no special cleanup is needed, it can be this minimal do-nothing execline script:

**`.s6-svscan/finish`**

**Minimal execline finish script**

```
#!/bin/execlineb -P
```
If no `-s`, `-p` or `-r` option is passed to s6-svscanctl, or if s6-svscan receives a `SIGABRT`, or if s6-svscan receives a `SIGTERM`, `SIGTHUP` or `SIGQUIT` signal and [signal diversion](https://wiki.gentoo.org#s6signaldivert) is turned off, .s6-svscan/finish will be invoked with a 'reboot' argument.

If s6-svscan encounters a error situation it cannot handle, or if it is asked to exit and there is no .s6-svscan/finish file, it will try to execute a file named crash, also expected to be in the .s6-svscan control subdirectory. This is also done using `execve()`, so no new process will be created, and .s6-svscan/crash will have the same process ID as s6-svscan. If there is no .s6-svscan/crash file, s6-svscan will give up and exit with an exit code of **111**.

s6-svscanctl can also be invoked in this abbreviated forms:

- s6-svscanctl -0 (*halt*) is equivalent to s6-svscanctl -st.
- s6-svscanctl -6 (*reboot*) is equivalent to s6-svscanctl -rt.
- s6-svscanctl -7 (*poweroff*) is equivalent to s6-svscanctl -pt.
- s6-svscanctl -8 (*other*) is equivalent to s6-svscanctl -0, but .s6-svscan/finish will be invoked with an 'other' argument instead of a 'halt' argument.
- s6-svscanctl -i (*interrupt*) is equivalent to s6-svscanctl -6, and equivalent to sending s6-svscan a `SIGINT` signal, unless signal diversion is turned on.

Contents of the .s6-svscan subdirectory with example finish and crash files, once s6-svscan is running:

`user $``ls -l .s6-svscan`
total 8
prw------- 1 user user  0 Jul 19 12:00 control
-rwxr-xr-x 1 user user 53 Jul 19 12:00 crash
-rwxr-xr-x 1 user user 72 Jul 19 12:00 finish
-rw-r--r-- 1 user user  0 Jul 19 12:00 lock

**`.s6-svscan/finish`**

```
#!/bin/execlineb -S0
echo Executing .s6-svscan/finish with arguments $@
```
**`.s6-svscan/crash`**

```
#!/bin/execlineb -S0
echo Executing .s6-svscan/crash
```
Messages sent by .s6-svscan/finish to s6-svscan's standard output as a result of different s6-svscanctl invocations:

`user $``s6-svscanctl -t .`
Executing .s6-svscan/finish with arguments reboot

`user $``s6-svscanctl -st .`
Executing .s6-svscan/finish with arguments halt

`user $``s6-svscanctl -7 .`
Executing .s6-svscan/finish with arguments poweroff

`user $``s6-svscanctl -8 .`
Executing .s6-svscan/finish with arguments other

Messages printed by s6-svscan on its standard error, and sent by .s6-svscan/crash to s6-svscan's standard output, as a result of invoking s6-svscanctl after deleting .s6-svscan/finish:

`user $``rm .s6-svscan/finish``user $``s6-svscanctl -t .`
s6-svscan: warning: unable to exec finish script .s6-svscan/finish: No such file or directory
s6-svscan: warning: executing into .s6-svscan/crash
Executing .s6-svscan/crash


#### s6-svscan's signal diversion feature

When s6-svscan is invoked with an `-S` option, or with neither an `-s` nor an `-S` option, and it receives a `SIGINT`, `SIGHUP`, `SIGTERM` or `SIGQUIT` signal, it behaves as if s6-svscanctl had been invoked with its scan directory pathname and an option that depends on the signal.

When s6-svscan is invoked with an `-s` option, *signal diversion* is turned on: if it receives any of the aforementioned signals, a `SIGUSR1` signal, or a `SIGUSR2` signal, s6-svscan tries to execute a file with the same name as the received signal, expected to be in the .s6-svscan control subdirectory of the scan directory (e.g. .s6-svscan/SIGTERM, .s6-svscan/SIGHUP, etc.). These files will be called *diverted signal handlers*, and are executed as a child process of s6-svscan. Just as run or finish files in a service directory, they can have any file format that the kernel knows how to execute, but are usually execline scripts. If the diverted signal handler corresponding to a received signal does not exist, the signal will have no effect. When signal diversion is turned on, s6-svscan can still be controlled using s6-svscanctl.

The best known use of this feature is to support the [s6-rc](https://wiki.gentoo.org/wiki/S6-rc) service manager as an init system component when s6-svscan is running as process 1; see [s6 and s6-rc-based init system](https://wiki.gentoo.org/wiki/S6_and_s6-rc-based_init_system).

Example .s6-svscan subdirectory with diverted signal handlers for `SIGHUP`, `SIGTERM` and `SIGUSR1`:

`user $``ls -l .s6-svscan`
total 16
-rwxr-xr-x 1 user user 53 Jul 19 12:00 crash
-rwxr-xr-x 1 user user 72 Jul 19 12:00 finish
-rwxr-xr-x 1 user user 51 Jul 19 12:00 SIGHUP
-rwxr-xr-x 1 user user 52 Jul 19 12:00 SIGTERM
-rwxr-xr-x 1 user user 52 Jul 19 12:00 SIGUSR1

**`.s6-svscan/SIGHUP`**

```
#!/bin/execlineb -P
echo s6-svscan received SIGHUP
```
**`.s6-svscan/SIGTERM`**

```
#!/bin/execlineb -P
echo s6-svscan received SIGTERM
```
**`.s6-svscan/SIGUSR1`**

```
#!/bin/execlineb -P
echo s6-svscan received SIGUSR1
```
Output of ps showing s6-svscan's process ID and arguments:

`user $``ps -o pid,args`
PID COMMAND
...
2047 s6-svscan -s
...

Messages printed to s6-svscan's standard output as a result of sending signals with the kill utility:

`user $``kill 2047`
s6-svscan received SIGTERM

`user $``kill -HUP 2047`
s6-svscan received SIGHUP

`user $``kill -USR1 2047`
s6-svscan received SIGUSR1


### s6 notification

Creating a publicly accessible fifodir named fifodir1 and a fifodir restricted to members of group *user* (assumed to have group ID 1000) named fifodir2:

`user $````
s6-mkfifodir fifodir1
```
`user $````
s6-mkfifodir -g 1000 fifodir2
```
`user $``ls -ld fifodir*`
drwx-wx-wt 2 user user 4096 Aug  2 12:00 fifodir1
drwx-ws--T 2 user user 4096 Aug  2 12:00 fifodir2

Creating listeners that subscribe to fifodir1 and wait for event sequences 'message1' and 'message2', respectively, as background processes:

`user $````
s6-ftrig-wait fifodir1 message1 &
```
`user $````
s6-ftrig-wait -t 20000 fifodir1 message2 &
```
`user $``ls -l fifodir1`
total 0
prw--w--w- 1 user user 0 Aug  2 21:44 ftrig1:@40000000598272220ea9fa39:-KnFNSkhmW1pQPY0
prw--w--w- 1 user user 0 Aug  2 21:46 ftrig1:@400000005982728b3a8d09c2:\_UjWhNPn3Z0Q\_VFQ

This shows that a FIFO has been created in the fifodir for each s6-ftrig-wait process, with names starting with 'ftrig1:'.

`user $``ps f -o pid,ppid,args`
PID  PPID COMMAND
...
2026  2023 \\\_ bash
2043  2026     \\\_ s6-ftrig-wait fifodir1 message1
2044  2043     |   \\\_ s6-ftrigrd
2051  2026     \\\_ s6-ftrig-wait -t 20000 fifodir1 message2
2052  2051         \\\_ s6-ftrigrd
...
s6-ftrig-wait: fatal: unable to match regexp on message2: Connection timed out

The output of ps shows that each s6-ftrig-wait process has spawned a child s6-ftrigrd helper, and because the one waiting for event sequence 'message2' has a timeout of 20 seconds ("-t 20000"), after that time has elapsed whithout getting the expected notifications it unsubscribes, and exits with an error status that is printed on the shell's terminal ("Connection timed out").

`user $``ls -l fifodir1`
total 0
prw--w--w- 1 user user 0 Aug  2 21:44 ftrig1:@40000000598272220ea9fa39:-KnFNSkhmW1pQPY0

This shows that the s6-ftrig-wait process without a timeout is still running, and its FIFO is still there. Notifying all fifodir1 listeners about event sequence 'message1':

`user $``s6-ftrig-notify fifodir1 message1`
1

The '1' printed on the shell's terminal after the s6-ftrig-notify invocation is the last event the s6-ftrig-wait process was notified about (i.e. the last character in string 'message1'), which then exits because the notifications have matched its regular expression.

`user $``ls -l fifodir1`
total 0

This shows that since all listeners have unsubscribed, the fifodir is empty.

**`test-script`**

**Example execline script for testing s6-ftrig-listen**

```
#!/bin/execlineb -P
foreground {
   s6-ftrig-listen -o { fifodir1 message fifodir2 message }
   foreground { ls -l fifodir1 fifodir2 }
   foreground { ps f -o pid,ppid,args }
   s6-ftrig-notify fifodir1 message
}
echo s6-ftrig-listen exited
```
Executing the example script:

`user $``./test-script`
fifodir1:
total 0
prw--w--w- 1 user user 0 Aug  2 22:28 ftrig1:@4000000059827c60124f916d:51Xhg7STswW-yFst
fifodir2:
total 0
prw--w--w- 1 user user 0 Aug  2 22:28 ftrig1:@4000000059827c601250c752:oXikN3Vko3JipuvU
 PID  PPID COMMAND
...
2176  2026 \\\_ foreground  s6-ftrig-listen ...
2177  2176     \\\_ s6-ftrig-listen -o  fifodir1 ...
2178  2177         \\\_ s6-ftrigrd
2179  2177         \\\_ foreground  ps ...
2181  2179             \\\_ ps f -o pid,ppid,args
...
s6-ftrig-listen exited

The output of ls shows that two listeners were created, one subscribed to fifodir1 and the other to fifodir2, and the output of ps shows that both are implemented by a single s6-ftrigrd process that is a child of s6-ftrig-listen. It also shows that s6-ftrig-listen has another child process, executing (at that time) the execline foreground program, which in turn has spawned the ps process. After that, foreground replaces itself with s6-ftrig-notify, which notifies all fifodir1 listeners about event sequence 'message'. Because s6-ftrig-listen was invoked with an `-o` option, and the fifodir1 listener got notifications that match its regular expression, s6-ftrig-listen exits at that point ("s6-ftrig-listen exited").

`user $``ls fifodir*`
fifodir1:
total 0
fifodir2:
total 0

This shows that the listener subscribed to fifodir2 has unsubscribed and exited, even if it didn't get the expected notifications.

Modifying the test script to invoke s6-ftrig-listen with the `-a` option instead (i.e. as `s6-ftrig-listen -a { fifodir1 message fifodir2 message }`) and reexecuting it in the background:

`user $``./test-script &`
fifodir1:
total 0
prw--w--w- 1 user user 0 Aug  2 22:56 ftrig1:@40000000598282e4210384d5:wikPBCD-Aw5Erijp
fifodir2:
total 0
prw--w--w- 1 user user 0 Aug  2 22:56 ftrig1:@40000000598282e42104bc57:Yop6JbMNBJo1r-uI
 PID  PPID COMMAND
...

The output of the script does not have a "s6-ftrig-listen exited" message, so it is still running:

`user $``ls -l fifodir*`
fifodir1:
total 0
fifodir2:
total 0
prw--w--w- 1 user user 0 Aug  2 22:56 ftrig1:@40000000598282e42104bc57:Yop6JbMNBJo1r-uI

This confirms that the listener subscribed to fifodir2 is still running, waiting for events. Notifying all fifodir2 listeners about event sequence 'message':

`user $``s6-ftrig-notify fifodir2 message`
s6-ftrig-listen exited

This shows that once the remaining listener has gotten notifications that match its regular expression, s6-ftrig-listen exits.


### s6 readiness notification

Example s6 scan directory containing services that support readiness notification:

`user $````
s6-mkfifodir test-service1/event
```
`user $``ls -l *`
test-service1:
total 12
-rw-r--r-- 1 user user    0 Jul 30 12:00 down
drwx-wx-wt 2 user user 4096 Jul 30 12:00 event
-rwxr-xr-x 1 user user   29 Jul 30 12:00 finish
-rwxr-xr-x 1 user user   32 Jul 30 12:00 run
test-service2:
total 8
-rw-r--r-- 1 user user  0 Jul 30 12:00 down
-rw-r--r-- 1 user user  2 Jul 30 12:00 notification-fd
-rwxr-xr-x 1 user user 39 Jul 30 12:00 run
test-service3:
total 16
-rw-r--r-- 1 user user  0 Jul 30 12:00 down
-rwxr-xr-x 1 user user 29 Jul 30 12:00 finish
-rw-r--r-- 1 user user  2 Jul 30 12:00 notification-fd
-rwxr-xr-x 1 user user 39 Jul 30 12:00 run
-rw-r--r-- 1 user user  6 Jul 30 12:00 timeout-finish

**`test-service1/run`**

```
#!/bin/execlineb -P
test-daemon
```
**`test-service1/finish`**

```
#!/bin/execlineb -P
exit 125
```
**`test-service2/run`**

```
#!/bin/execlineb -P
test-daemon --s6=5
```
**`test-service2/notification-fd`**

**`test-service3/run`**

```
#!/bin/execlineb -P
test-daemon --s6=5
```
**`test-service3/notification-fd`**

**`test-service3/finish`**

```
#!/bin/execlineb -P
sleep 10
```
**`test-service3/timeout-finish`**

It is assumed that test-daemon is a program that supports an `--s6` option to turn readiness notification on, specifying the notification channel's file descriptor (5), which is also stored in a notification-fd file. test-service1/finish exits with an exit code of 125, so that if the corresponding test-daemon process stops, it won't be restarted. The s6-mkfifodir invocation creates test-service1/event as a publically accesible fifodir. Using s6-ftrig-listen1 on it to start the supervision tree and verify that s6-supervise notifies listeners about the start event:

`user $``s6-ftrig-listen1 test-service1/event s s6-svscan`
s

`user $``ls -ld */event`
drwx-wx-wt 2 user user 4096 Jul 30 12:22 test-service1/event
drwx-ws--T 2 user user 4096 Jul 30 12:22 test-service2/event
drwx-ws--T 2 user user 4096 Jul 30 12:22 test-service3/event

This shows that s6-supervise has created all missing event directories as restricted fifodirs, but uses the publicly accessible one created by s6-mkfifodir.

**`test-script`**

**Example execline script for testing s6-svwait**

```
#!/bin/execlineb -P
foreground { s6-svwait -u test-service1 }
echo s6-svwait exited
```
Executing the example script:

`user $````
../test-script &
```
`user $``ps xf -o pid,ppid,args`
PID  PPID COMMAND
...
2166  2039 \\\_bash
2387  2166    \\\_ foreground  s6-svwait ...
2388  2387        \\\_ s6-svwait -u test-service1
2389  2388            \\\_ s6-ftrigrd
...

`user $``ls -l test-service1/event`
total 0
prw--w--w- 1 user user 0 Jul 30 12:22 ftrig1:@40000000597df9d12c8328da:v84Zc\_E\_LyaqxlDh

This shows that the s6-svwait process has spawned a child s6-ftrigrd helper, and created a FIFO in test-service1/event so that it can be notified about the up event. Manually starting test-service1/run:

`user $``s6-svc -u test-service1`
s6-svwait exited

The message printed by the test script to its standard output shows that the s6-svwait process got the expected notification, so it exited.

**`test-script`**

**Example execline script for testing up and ready event notifications**

```
#!/bin/execlineb -P
define -s services "test-service2 test-service3"
foreground {
   s6-svlisten -U { $services }
   foreground {
      forx svc { $services }
         importas svc svc
         foreground { s6-svc -wu -u $svc }
         pipeline { echo s6-svc -wu -u $svc exited } s6-tai64n
   }
   ps xf -o pid,ppid,args
}
pipeline { echo s6-svlisten -U exited } s6-tai64n
```
The script calls s6-svlisten to subscribe to fifodirs test-service2/event and test-service3/event and wait for up and ready events. Then it uses a `s6-svc -wu -u` command to manually start test-service2/run and test-service3/run, and wait for up events. Both run scripts invoke test-daemon with readiness notification on. A message timestamped using s6-tai64n is printed to the standard output when the listeners get their expected notifications. Executing the example script:

`user $``../test-script | s6-tai64nlocal`
2017-07-30 19:45:38.458536857 s6-svc -wu -u test-service2 exited
2017-07-30 19:45:38.467353962 s6-svc -wu -u test-service3 exited
 PID  PPID COMMAND
2379  2378 \\\_ foreground  s6-svlisten  -U ...
2381  2379     \\\_ s6-svlisten -U ...
2382  2381         \\\_ s6-ftrigrd
2383  2381         \\\_ ps xf -o pid,ppid,args
2017-07-30 19:45:48.472237201 s6-svlisten -U exited

This shows that the s6-svc processes waiting for up events are notified first, so they exit, and that the s6-svlisten process waiting for up and ready events is notified 10 seconds later. The output of ps shows that when the s6-svc processes exited, the s6-svlisten process and its s6-ftrigrd child were still running.

`user $```for i in *; do printf "$i: `s6-svstat $i`\n"; done``
test-service1: up (pid 2124) 42 seconds, normally down
test-service2: up (pid 2332) 29 seconds, normally down, ready 19 seconds
test-service3: up (pid 2338) 29 seconds, normally down, ready 19 seconds

This confirms that both test-daemon processes have notified readiness to their s6-supervise parent ("ready 19 seconds") 10 seconds after being started. Using s6-ftrig-listen1 on fifodir test-service1/event to verify that s6-supervise notifies listeners about a once event when test-daemon is killed with a `SIGTERM`, because of test-service1/finish's exit code:

`user $``s6-ftrig-listen1 test-service1/event O s6-svc -t test-service1`
O

**`test-script`**

**Example execline script for testing really down event notifications**

```
#!/bin/execlineb -P
define -s services "test-service2 test-service3"
foreground {
   s6-svlisten -d { $services }
   forx svc { $services }
      importas svc svc
      foreground { s6-svc -wD -d $svc }
      pipeline { echo s6-svc -wD -d $svc exited } s6-tai64n
}
foreground {
   pipeline { echo s6-listen -d exited } s6-tai64n
}
ps xf -o pid,ppid,args
```
The script calls s6-svlisten to subscribe to fifodirs test-service2/event and test-service3/event and wait for down events. Then it uses a `s6-svc -wD -d` command to manually stop the test-daemon processes corresponding to test-service2 and test-service3, and wait for really down events. test-service3 has a finish script that sleeps for 10 seconds, so test-service2/event listeners should be notified earlier than test-service3/event listeners. A message timestamped using s6-tai64n is printed to the standard output when the listeners get their expected notifications. Executing the example script:

`user $``../test-script | s6-tai64nlocal`
2017-07-30 22:23:17.063815232 s6-svc -wD -d test-service2 exited
2017-07-30 22:23:17.071855769 s6-listen -d exited
 PID  PPID COMMAND
2326     1 forx svc  test-service2  test-service3 ...
2333  2326  \\\_ foreground  s6-svc  -wD  ...
2334  2333      \\\_ s6-svlisten1 -D -- test-service3 s6-svc -d -- test-service3
2335  2334          \\\_ s6-ftrigrd
2017-07-30 22:23:27.078874158 s6-svc -wD -d test-service3 exited

This shows that the s6-svlisten process waiting for down events and the s6-svc process subscribed to test-service2/event and waiting for a really down event are notified first with almost no delay between them, so they exit, and that the s6-svc process subscribed to test-service3/event and waiting for a really down event is notified 10 seconds later. The output of ps shows that when the s6-svlisten process exited, an s6-svc process that had replaced itself with s6-svlisten1 (because of the `-w` option) and its s6-ftrigrd child were still running.

`user $```for i in *; do printf "$i: `s6-svstat $i`\n"; done``
test-service1: down (signal SIGTERM) 83 seconds, ready 83 seconds
test-service2: down (exitcode 0) 31 seconds, ready 31 seconds
test-service3: down (exitcode 0) 31 seconds, ready 21 seconds

This confirms that the test-daemon process corresponding to test-service1 hasn't been restarted after test-service1/finish exited (83 seconds in down state and no 'wanted up'), and that the down and ready events for the test-daemon processes corresponding to test-service2 and test-service3 have a 10 seconds delay between them ("ready 21 seconds" compared to "ready 31 seconds"). Using s6-ftrig-listen1 on fifodir test-service2/event to stop the supervision tree and verify that s6-supervise notifies listeners about the exit event:

`user $``s6-ftrig-listen1 test-service2/event x s6-svscanctl -t .`
x


### s6 sudo

**`test-script`**

**Example execline script to be executed by s6-sudod**

```
#!/bin/execlineb -S0
pipeline { id -u } withstdinas -n localuser
importas localuser localuser
importas -D unavailable IPCREMOTEEUID IPCREMOTEEUID
importas -D unset VAR1 VAR1
importas -D unset VAR2 VAR2
importas -D unset VAR3 VAR3
foreground { echo Script run with effective user ID $localuser and arguments $@ }
echo IPCREMOTEEUID=$IPCREMOTEEUID VAR1=$VAR1 VAR2=$VAR2 VAR3=$VAR3
```
Testing the script by executing it directly:

`user1 $``VAR1="s6-sudoc value" VAR2="ignored variable" ./test-script arg1 arg2`
Script run with effective user ID 1000 and arguments arg1 arg2
IPCREMOTEEUID=unavailable VAR1=s6-sudoc value VAR2=ignored variable VAR3=unset

The script is executed with effective user *user1* (UID 1000), `IPCREMOTEEUID` and `VAR3` are unset, and `VAR1` and `VAR2` are set to the specified values.

**`s6-sudod-wrapper`**

**Example execline script to launch an s6-sudod process with access control**

```
 run-test-script
s6-ipcserver-access -v 2 -i rules
s6-sudod ./test-script arg1 arg2
```
s6-ipcserver-access's `-v 2` argument increments its verbosity level. Contents of rules directory rules:

`user1 $``ls -l rules/*/*`
rules/uid/1002:
total 4
-rw-r--r-- 1 user1 user1    0 Aug  4 12:00 allow
drwxr-xr-x 2 user1 user1 4096 Aug  4 12:00 env
rules/uid/default:
total 0
-rw-r--r-- 1 user1 user1 0 Aug  4 12:00 deny

`user1 $``ls -1 rules/uid/1002/env`
VAR1
VAR3

**`rules/uid/1002/env/VAR3`**

File rules/uid/1002/env/VAR1 contains an empty line, so the corresponding environment variable will be set, but empty. Launching the s6-sudod process:

`user1 $``execlineb -P s6-sudod-wrapper &``user1 $``ls -l run-test-script`
srwxrwxrwx 1 user1 user1 0 Aug  4 12:10 run-test-script

This shows that a UNIX domain socket named run-test-script was created in the working directory. Running s6-sudo with effective user *user2* (UID 1001):

`user2 $``VAR1="s6-sudoc value" VAR2="ignored variable" s6-sudo run-test-script arg3 arg4`
s6-ipcserver-access: info: deny pid 2125 uid 1001 gid 1001: Permission denied
s6-sudoc: fatal: connect to the s6-sudod server - check that you have appropriate permissions

`s6-sudo run-test-script arg3 arg4` is equivalent to `s6-ipcclient run-test-script s6-sudoc arg3 arg4`, but shorter. This shows that the rules directory setup denied execution of test-script to user2 (UID 1001); it only allows it to the user with UID 1002. Modifying rules:

`user1 $``mv rules/uid/100{2,1}``user1 $``ls -1 rules/*/*`
rules/uid/1001:
allow
env
rules/uid/default:
deny

Retrying s6-sudo:

`user2 $``VAR1="s6-sudoc value" VAR2="ignored variable" s6-sudo run-test-script arg3 arg4`
s6-ipcserver-access: info: allow pid 2148 uid 1001 gid 1001
Script run with effective user ID 1000 and arguments arg1 arg2 arg3 arg4
IPCREMOTEEUID=1001 VAR1=s6-sudoc value VAR2=unset VAR3=s6-sudod value

Comparing to the output of the script when run directly by user1, this shows that test-script's arguments are the concatenation of the ones supplied to s6-sudod in script s6-sudod-wrapper, *arg1* and *arg2*, and the ones specified in the s6-sudo invocation, *arg3* and *arg4*. Also, test-script's environment has s6-sudod's variables: `IPCREMOTEEUID`, inherited from s6-ipcserverd, and `VAR3`, inherited from s6-ipcserver-access, which in turn sets it based on environment directory rules/uid/1002/env. Because variable `VAR1` is set by s6-ipcserver-access but empty, s6-sudod sets it to the value it has in s6-sudoc's environment. And because variable `VAR2` is set in s6-sudoc's environment but not in s6-sudod's, it is also unset in test-script's environment.


### OpenRC s6 integration

Example setup for a hypothetical supervised *test-daemon* process with a dedicated logger:

**`/etc/init.d/test-service`**

**OpenRC service script**

```
#!/sbin/openrc-run
description="A supervised test service with a logger"
supervisor=s6
s6_service_path=/home/user/test/svc-repo/test-service
depend() {
   need s6-svscan
}
```
**`/etc/conf.d/test-service`**

**OpenRC service-specific configuration file**

```
s6_svwait_options_start=-U
```
`user $``/sbin/rc-service test-service describe`
\* A supervised test service with a logger
\* cgroup\_cleanup: Kill all processes in the cgroup

The service directory:

`user $``ls -l /home/user/test/svc-repo/test-service /home/user/test/svc-repo/test-service/log`
/home/user/test/svc-repo/test-service:
total 12
drwxr-xr-x 2 user user 4096 Aug  8 12:00 log
-rw-r--r-- 1 user user    2 Aug  8 12:00 notification-fd
-rwxr-xr-x 1 user user   86 Aug  8 12:00 run
/home/user/test/svc-repo/test-service/log:
total 4
-rwxr-xr-x 1 user user 65 Aug  8 12:00 run

**`/home/user/test/svc-repo/test-service/run`**

```
#!/bin/execlineb -P
s6-softlimit -o 5
s6-setuidgid daemon
fdmove -c 2 1
/home/user/test/test-daemon --s6=5
```
**`/home/user/test/svc-repo/test-service/notification-fd`**

This launches test-daemon with effective user *daemon* and the maximum number of open file descriptors set to 5. This is the same as if test-daemon performed a `setrlimit(RLIMIT_NOFILE, &rl)` call itself with `rl.rlim_cur` set to 5, provided that value does not exceed the corresponding hard limit. The program supports an `--s6` option to [turn readiness notification on](https://wiki.gentoo.org#s6readiness), specifying the notification file descriptor (5), and also periodically prints to its standard error a message of the form 'Logged message #n', with an incrementing number **n** between 0 and 9. The redirection of test-daemon's standard error to standard output, using execline's fdmove program with the `-c` (copy) option, allows logging its messages using s6-log:

**`/home/user/test/svc-repo/test-service/log/run`**

```
#!/bin/execlineb -P
s6-setuidgid user
s6-log t /home/user/test/logdir
```
An automatically rotated logging directory named *logdir* will be used, and messages will have a timestamp in [external TAI64N format](https://cr.yp.to/libtai/tai64.html) prepended to them.

Manually starting test-service:

`root #``time rc-service test-service start`
\* Creating s6 scan directory
\* /run/openrc/s6-scan: creating directory
\* Starting s6-svscan ...                    \[ ok \]
\* Starting test-service ...                 \[ ok \]
real	0m11.681s
user	0m0.039s
sys	0m0.034s

`root #``rc-service test-service status`
up (pid 2279) 33 seconds, ready 23 seconds

This shows that test-daemon took about 10 seconds to notify readiness to s6-supervise, and that the rc-service start command waited until the up and ready event, because of the s6-svwait `-U` option passed via `s6_svwait_options_start` in /etc/conf.d/test-service.

`user $``rc-status`
Runlevel: default
...
Dynamic Runlevel: needed/wanted
...
s6-svscan                                   \[  started  \]
...
Dynamic Runlevel: manual
test-service                                \[  started  \]

The scan directory:

`user $``ls -la /run/openrc/s6-scan`
total 0
drwxr-xr-x  3 root root  80 Aug  8 22:38 .
drwxrwxr-x 15 root root 360 Aug  8 22:38 ..
drwx------  2 root root  80 Aug  8 22:38 .s6-svscan
lrwxrwxrwx  1 root root  46 Aug  8 22:38 test-service -> /home/user/test/svc-repo/test-service

The supervision tree:

`user $``ps axf -o pid,ppid,pgrp,euser,args`
PID  PPID  PGRP EUSER    COMMAND
...
2517     1  2517 root     /bin/s6-svscan /run/openrc/s6-scan
2519  2517  2517 root      \\\_ s6-supervise test-service/log
2523  2519  2523 user      |   \\\_ s6-log t /home/user/test/logdir
2520  2517  2517 root      \\\_ s6-supervise test-service
2522  2520  2522 daemon        \\\_ /home/user/test/test-daemon --s6=5
...

Messages from the test-daemon process go to the logging directory:

`user $``ls -l /home/user/test/logdir`
total 12
-rwxr--r-- 1 user user 352 Aug  8 22:39 @40000000598a67ec2d5d7180.s
-rwxr--r-- 1 user user 397 Aug  8 22:40 @40000000598a681919d6e581.s
-rwxr--r-- 1 user user 397 Aug  8 22:40 current
-rw-r--r-- 1 user user   0 Aug  8 22:38 lock
-rw-r--r-- 1 user user   0 Aug  8 22:38 state

`user $``cat /home/user/test/logdir/current | s6-tai64nlocal`
2017-08-08 22:40:20.562745759 Logged message #1
2017-08-08 22:40:25.565816199 Logged message #2
2017-08-08 22:40:30.570600144 Logged message #3
2017-08-08 22:40:35.578765601 Logged message #4
2017-08-08 22:40:40.585146120 Logged message #5
2017-08-08 22:40:45.591282433 Logged message #6
