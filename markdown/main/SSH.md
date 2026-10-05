<!-- source: https://wiki.gentoo.org/wiki/SSH | group: Gentoo Wiki (Main) | wiki-title: SSH -->
---
title: SSH
url: https://wiki.gentoo.org/wiki/SSH
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-22"
fingerprint: b439d55c2c01bfc5
license: CC BY-SA 4.0
---

# SSH

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**SSH** (**S**ecure **SH**ell) is the ubiquitous tool for logging into and working on remote machines securely. All sensitive information is strongly encrypted, and in addition to the remote shell, SSH supports file transfer, and port forwarding for arbitrary protocols, allowing secure access to remote services. It replaces the classic [telnet](https://en.wikipedia.org/wiki/telnet), [rlogin](https://en.wikipedia.org/wiki/Berkeley_r-commands), and similar non-secure tools - but SSH is not just a remote shell, it is a complete environment for working with remote systems.

In addition to the main ssh command, the SSH suite of programs includes tools such as [scp](https://wiki.gentoo.org/wiki/Scp) (**S**ecure **C**opy **P**rogram), [sftp](https://wiki.gentoo.org/wiki/Sftp) (**S**ecure **F**ile **T**ransfer **P**rotocol), or [ssh-agent](https://wiki.gentoo.org/wiki/Keychain) to help with key management. The standard SSH port is port 22.

Several versions of SSH [have existed](https://www.openssh.com/history.html).  Today the most popular implementation of SSH, and de-facto standard, is [OpenBSD](https://www.openbsd.org/)'s **OpenSSH**. This comes **[pre-installed](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) on Gentoo**, and is published under a [BSD ("and freer")](https://cvsweb.openbsd.org/cgi-bin/cvsweb/~checkout~/src/usr.bin/ssh/LICENCE?rev=1.20&content-type=text/plain) license.

SSH is multi-platform, and is very widely used: OpenSSH is installed by default on most Unix-like OSs, on Windows10, on MacOS, and can be installed on Android or "jailbroken" iOS (SSH clients are available). This makes SSH a great tool for working with heterogeneous systems.

## Installation

### Check install

Deployments of Gentoo Linux should already have OpenSSH installed, as the [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) package is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). The presence and proper functioning of OpenSSH can be checked by running the ssh command, which should output a usage statement:

`user $``ssh````
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
           [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
           [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
           [-i identity_file] [-J [user@]host[:port]] [-L address]
           [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
           [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
           [-w local_tun[:remote_tun]] destination [command]
```
If no usage statement is printed, OpenSSH may be corrupt, or not installed. Try re-installation by following the [emerge section](https://wiki.gentoo.org/wiki/SSH#Emerge), just as if rebuilding after a USE flag change. If OpenSSH were uninstalled, this should reinstall it. It should then remain installed, as part of the system set.

If this does not try to install OpenSSH, the package may have been [masked](https://wiki.gentoo.org/wiki//etc/portage/package.mask), or even listed in [package.provided](https://wiki.gentoo.org/wiki//etc/portage/profile/package.provided), though this would be unusual.

### USE flags


| [+pie](https://packages.gentoo.org/useflags/+pie) | Build programs as Position Independent Executables (a security hardening technique) | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Enable additional crypto algorithms via OpenSSL | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldns](https://packages.gentoo.org/useflags/ldns) | Use LDNS for DNSSEC/SSHFP validation. | 
| [legacy-ciphers](https://packages.gentoo.org/useflags/legacy-ciphers) | Enable support for deprecated, soon-to-be-dropped DSA keys. See https://marc.info/?l=openssh-unix-dev>m=170494903207436>w=2. | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [livecd](https://packages.gentoo.org/useflags/livecd) | Enable root password logins for live-cd environment. | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [security-key](https://packages.gentoo.org/useflags/security-key) | Include builtin U2F/FIDO support | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xmss](https://packages.gentoo.org/useflags/xmss) | Enable XMSS post-quantum authentication algorithm | 

### Emerge

After changing USE flags [just for the OpenSSH package](https://wiki.gentoo.org/wiki//etc/portage/package.use), rebuild OpenSSH for the new flags to be applied. As OpenSSH is in the system set, `--oneshot` should be used to avoid adding it to the [world file](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>):

`root #``emerge --ask --changed-use --oneshot net-misc/openssh`
After changing any global USE flags in [make.conf](https://wiki.gentoo.org/wiki/Make.conf) that affect the OpenSSH package, emerge world to update to the new USE flags:

`root #``emerge --ask --verbose --update --deep --newuse @world`
## Usage

### Commands

OpenSSH provides several commands, see each command's [man page](https://wiki.gentoo.org/wiki/Man_page) for usage information:

- [scp(1)](https://man.archlinux.org/man/scp.1.en)- [sftp(1)](https://man.archlinux.org/man/sftp.1.en)- [ssh-add(1)](https://man.archlinux.org/man/ssh-add.1.en)- [ssh-agent(1)](https://man.archlinux.org/man/ssh-agent.1.en)- [ssh-copy-id(1)](https://linux.die.net/man/1/ssh-copy-id)- [ssh-keygen(1)](https://man.archlinux.org/man/ssh-keygen.1.en)- [ssh-keyscan(1)](https://man.archlinux.org/man/ssh-keyscan.1.en)- [sshd(8)](https://man.archlinux.org/man/sshd.8.en)

### Escape sequences

During an active SSH session, pressing the tilde (`~`) key starts an escape sequence. Enter the following for a list of options:

`ssh>``~?`
Note that escapes are only recognized immediately after a newline. They may not always work with some shells, such as [fish](https://wiki.gentoo.org/wiki/Fish).

### Passwordless authentication to a remote SSH server

Handy for [git](https://wiki.gentoo.org/wiki/Git) server management.

Make sure an account for the user exists on the server. The clients' id\_ed25519.pub will be copied to the server's \~/.ssh/authorized\_keys file in the user's home directory.

#### Client

##### ssh-keygen

Clients need public and private keys. A pair may be created with (of course, **not entering** a passphrase):

`user $``ssh-keygen -t ed25519`
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/larry/.ssh/id\_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/larry/.ssh/id\_ed25519
Your public key has been saved in /home/larry/.ssh/id\_ed25519.pub
The key fingerprint is:
SHA256:riDdFuPhN7alEsAvm717gM1IZBP3DYXGo5apQG7OUM1 larry@client
The key's randomart image is:
+--\[ED25519 256\]--+
| .+   ..+o.      |
| o +   Ao+.o     |
|. o A Oo  . .    |
| =   + .o +      |
|  o   . S= + .   |
|     . \*. B + .  |
|      o.=  \* o   |
|     ...+ = .    |
|      .. .+=     |
+----\[SHA256\]-----+

Then authorize the public key with the server:

`user $``ssh-copy-id -i ~/.ssh/id_ed25519.pub <username>@<server>`
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/larry/.ssh/id\_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
larry@\<server>'s password: 
 
Number of key(s) added: 1
 
Now try logging into the machine, with:   "ssh '\<server>'"
and check to make sure that only the key(s) you wanted were added.

Afterwards a passwordless login should be possible doing:

`user $``ssh <server>`
larry@\<server>

##### GnuPG

See [Usage of GPG keys instead of SSH keys](<https://wiki.gentoo.org/wiki/Hetzner_Cloud_(ARM64)#Usage_of_GPG_keys_instead_of_SSH_keys>).



##### Trusted Platform Module (TPM)

See [Using a TPM for your SSH keys](https://wiki.gentoo.org/wiki/Trusted_Platform_Module/SSH).

#### Server

The file /etc/ssh/sshd\_config should be set to `PasswordAuthentication no` after the client adds their public key.

Then [restart the sshd service](https://wiki.gentoo.org/wiki/SSH#Service) to authenticate without passwords.

#### Single machine testing

The above procedure can be tested out locally:

`user $``ssh-keygen -t ed25519`
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/larry/.ssh/id\_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again:
...

`user $``mv ~/.ssh/id_ed25519.pub ~/.ssh/authorized_keys``user $``ssh localhost`
### Remote services over ssh

SSH may be used to access remote services, such as HTTP, HTTPS, fileshares, etc., through an encrypted "tunnel". Remote service access is detailed in the [SSH tunneling](https://wiki.gentoo.org/wiki/SSH_tunneling) and [SSH jump host](https://wiki.gentoo.org/wiki/SSH_jump_host) articles.

### Copying files to a remote host

The [SFTP](https://wiki.gentoo.org/wiki/SFTP) command, a part of SSH, uses the SSH File Transfer Protocol to copy files to a remote host. [rsync](https://wiki.gentoo.org/wiki/Rsync) is also an alternative for this.

### ssh-agent

OpenSSH comes with ssh-agent, a daemon to cache and prevent from frequent ssh password entries. When run, the environment variable `SSH_AUTH_SOCK` is used to point to ssh-agent's communication socket. The normal way to setup ssh-agent is to run it as the top most process of the user's session. Otherwise the environment variables will not be visible inside the session.

Depending on the way the graphical user session is configured to launch, it can be tricky to find a suitable way to launch ssh-agent. As an example for the lightdm display manager, edit and change /etc/lightdm/Xsession from:

`user $````
exec $command
```
into:

`user $````
exec ssh-agent $command
```
To tell ssh-agent the password once per session, either run `ssh-add` manually or make use of the `AddKeysToAgent` option.

Recent [Xfce](https://wiki.gentoo.org/wiki/Xfce) will start ssh-agent (and gpg-agent) automatically. If both are installed both will be started which makes identity management especially with SmartCards more complicated. Either stop XFCE from autostarting at least SSH's agent or disable both and use the shell, X-session or similar.

`user $````
xfconf-query --channel xfce4-session --property /startup/ssh-agent/enabled --create --type bool --set false
```
`user $````
xfconf-query --channel xfce4-session --property /startup/gpg-agent/enabled --create --type bool --set false
```
## Configuration

### Files

- /etc/conf.d/sshd - Gentoo's config file for sshd daemon. See man sshd for options.
- /etc/ssh/ssh\_config - Global (system wide) client configuration file.
- /etc/ssh/sshd\_config - Global (system wide) daemon configuration file.
- /etc/ssh/ssh\_config.d/\*.conf - Conventional sub-directory of .conf files read by ssh
- /etc/ssh/sshd\_config.d/\*.conf - Conventional sub-directory of .conf files read by sshd daemon.
- /etc/ssh/sshd\_config.d/99\_penalities.conf - Conventional filename for additional configuration rules for the daemon.
- \~/.ssh/config - User's configuration file

### Create keys

In order to provide a secure shell, cryptographic keys are used to manage the encryption, decryption, and hashing functionalities offered by SSH.

On first run, the sshd init-script will generate all system keys if they don't already exist. Usually there is no need to recreate them (unless the system has been compromised), but if such a need arises, this can be done manually through the ssh-keygen command (RSA, ECDSA and Ed25519 algorithms):

`root #````
ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key -N ""
```
`root #````
ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ""
```
`root #````
ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
```
To reduce the attack vector, enable only one trusted algorithm, as shown below:

**`/etc/ssh/sshd_config`**

```
# Only Ed25519 host key
HostKey /etc/ssh/ssh_host_ed25519_key
```
And remove other `HostKey` keys, if present.

Check that only the key with the specified algorithm is used:

`user $``ssh-keyscan <SERVER IP>`
### Server configuration

The SSH server deamon can be configured by adding files to the following directory:

- /etc/ssh/sshd\_config.d/

It is also possible to perform further configuration in OpenRC's /etc/conf.d/sshd. For detailed information on how to configure the server read the [sshd\_config(5)](https://man.archlinux.org/man/sshd_config.5.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The server provides means to validate its configuration using test mode:

`root #``/usr/sbin/sshd -t`
### Client configuration

The ssh client and related programs (scp, sftp, etc.) can be configured using the following configuration files or directories:

Per user configuration:

- \~/.ssh/config

Per host configuration:

- /etc/ssh/ssh\_config.d/\*.conf

For more information read the [ssh\_config(5)](https://man.archlinux.org/man/ssh_config.5.en) [man page](https://wiki.gentoo.org/wiki/Man_page).

SSH is a commonly attacked service. OpenSSH version 9.7 released a **built-in** *intrusion prevention mechanism*. To configure and activate the brute-force preventing mechanism use following configuration steps.

Create /etc/ssh/sshd\_config.d/99\_penalities.conf file with following configuration overwriting the default OpenSSH values:

**`/etc/ssh/sshd_config.d/99_penalities.conf`**

```
# IP whitelist, where penalty does not hit. Trusted networks.
# Default is empty
PerSourcePenaltyExemptList 192.168.0.0/16
 
# Block IP subnets instead of individual IP's
# Default value is 32:128 (IPv4/IPv6)
PerSourceNetBlockSize 24:64
 
# Block every occurrence for 3600 seconds
# Default is crash:90 authfail:5 refuseconnection:10 noauth:1 grace-exceeded:10
PerSourcePenalties crash:3600 authfail:3600 refuseconnection:3600 noauth:3600 grace-exceeded:3600
```
Restart the OpenSSH daemon.

`root #``rc-service sshd restart`
Now the OpenSSH daemon blocks every brute-force attack for the configured time of 3600 seconds (1 hour). Adjust the blocking times to your liking.

Additional tools such as [sshguard](https://wiki.gentoo.org/wiki/Sshguard) or [fail2ban](https://wiki.gentoo.org/wiki/Fail2ban) help monitor logs and can black list remote IP's which have repeatedly attempted yet failed to authenticate. Utilize them as needed to secure a frequently attacked system.

### Service

Commands to run the SSH server will depend on active init system.

#### OpenRC

Add the OpenSSH daemon to the default runlevel:

`root #``rc-update add sshd default`
Start the sshd daemon with:

`root #``rc-service sshd start`
The OpenSSH server can be controlled like any other [OpenRC](https://wiki.gentoo.org/wiki/OpenRC)-managed service:

`root #````
rc-service sshd start
```
`root #````
rc-service sshd stop
```
`root #````
rc-service sshd restart
```
#### systemd

To have the OpenSSH daemon start when the system starts:

`root #``systemctl enable sshd.service`
Created symlink from /etc/systemd/system/multi-user.target.wants/sshd.service to /usr/lib64/systemd/system/sshd.service.

To start the OpenSSH daemon now:

`root #``systemctl start sshd.service`
To check if the service has started:

`root #``systemctl status sshd.service`
## Tips

### Terminal multiplexers to preserve sessions

It is possible to use a [terminal multiplexer](https://wiki.gentoo.org/wiki/Recommended_tools#Terminal_multiplexers) to resume a session after a dropped connection. [Tmux](https://wiki.gentoo.org/wiki/Tmux) and [Screen](https://wiki.gentoo.org/wiki/Screen) are two popular multiplexers that can be used to be able to reconnect to a session, even if a command was running when the connection dropped out.

### SSH over intermittent connections

When on unstable Internet connections, or when roaming between networks (such as when moving wifi networks), [mosh](https://wiki.gentoo.org/wiki/Mosh) can help avoid dropping SSH sessions.

### Open new tabs for session with Kitty terminal

By using the [SSH kitten](https://sw.kovidgoyal.net/kitty/kittens/ssh/#opt-kitten-ssh.remote_kitty) for the [Kitty](https://wiki.gentoo.org/wiki/Kitty) terminal emulator, it is possible to open new "tabs", or windows, on the current SSH session without having log in again.

Kitty also provides other practical SSH functionality.

### Benchmark the optimal rounds for an ed25519 key

**`ssh-benchmark.sh`**

**Benchmark SSH Ciphers**

```
#!/bin/sh
rounds="16 32 64 100 150"
num_runs=20
for r in $rounds; do
    printf "Benchmarking 'ssh-keygen -t ed25519 -a %s' on average:\n" "$r"
    total_time=0
    i=1
    while [ $i -le $num_runs ]; do
        start_time=$(date +%s.%N)
        ssh-keygen -t ed25519 -a "$r" -f test -N test >/dev/null 2>&1
        end_time=$(date +%s.%N)
        runtime=$(echo "$end_time - $start_time" | bc)
        total_time=$(echo "$total_time + $runtime" | bc)
        rm test{,.pub} >/dev/null 2>&1
        printf "Run %s: %s seconds\n" "$i" "$runtime"
        i=$((i + 1))
    done
    average_time=$(echo "scale=3; $total_time / $num_runs"| bc)
    printf "Average execution time: %s seconds\n\n" "$average_time"
done
```
Benchmarking is a crucial process to measure the performance and efficiency of a system or a specific component, such as cryptographic algorithms. In the context of SSH (Secure Shell) ciphers, it is important to determine the optimal number of rounds for generating *ed25519* keys.

The provided script, ssh-benchmark.sh, conducts benchmarking on the ssh-keygen command with different round values for *ed25519* keys. The script executes the ssh-keygen command multiple times with varying round values and measures the execution time for each run. It then calculates the average execution time for each round value.

By benchmarking different round values, system administrators and security professionals can identify the optimal round value that strikes a balance between security and performance. Higher round values generally provide stronger security but can result in increased computational overhead. Finding the right balance ensures that *ed25519* keys are generated efficiently without compromising security.

Benchmarking helps identify potential bottlenecks, vulnerabilities, or areas that require improvement in security systems. It assists in selecting the most suitable algorithms and configurations for a particular use case, ensuring that security measures are robust and effective.

## Troubleshooting

There are 3 different levels of debug modes that can help troubleshooting issues. With the `-v` option SSH prints debugging messages about its progress. This is helpful in debugging connection, authentication, and configuration problems. Multiple `-v` options increase the verbosity. Maximum verbosity is three levels deep.

`user $````
ssh example.org -v
```
`user $````
ssh example.org -vv
```
`user $````
ssh example.org -vvv
```
### Permissions are too open

An ssh connection will only work if the file permissions of the \~/.ssh directory and contents are correct.

- The \~/.ssh directory permissions should be 700 (drwx------), i.e. the owner has full access and no one else has any access.
- Under \~/.ssh:
  - public key files' permissions should be 644 (-rw-r--r--), i.e. anyone may read the file, only the owner can write.
  - all other files' permissions should be 600 (-rw-------), i.e. only the owner may read or write the file.

These permissions need to be correct on the client and server.

### Death of long-lived connections

Many internet access devices perform Network Address Translation ([NAT](https://wiki.gentoo.org/wiki/NAT)), a process that enables devices on a private network such as that typically found in a home or business place to access foreign networks, such as the internet, despite only having a single IP address on that network. Unfortunately, not all NAT devices are created equal, and some of them incorrectly close long-lived, occasional-use TCP connections such as those used by SSH.  This is generally observable as a sudden inability to interact with the remote server, even though the ssh client program has not exited.

In order to resolve the issue, OpenSSH clients and servers can be configured to send a 'keep alive', or invisible message aimed at maintaining and confirming the live status of the link:

- To enable keep alive *for all clients connecting to the local server*, set `ClientAliveInterval 30` (or some other value, in seconds) within the /etc/ssh/sshd\_config file.
- To enable keep alive *for all servers connected to by the local client*, set `ServerAliveInterval 30` (or some other value, in seconds) within the /etc/ssh/ssh\_config or \~/.ssh/config file.
- Set `TCPKeepAlive no` to help eliminate disconnections.

For example, to modify the server's configuration, add following file:

**`/etc/ssh/sshd_config.d/01_ClientAlive.conf`**

**Help disconnects occur less often (server)**

To modify the client's configuration, add following file:

**`/etc/ssh/ssh_config.d/01_ServerAlive.conf`**

**Help disconnects occur less often (client)**

### New key does not get used

This scenario covers the case when a key to access a remote system has been created, the public key installed on the remote system, but the remote system is (for some reason) not accessible via ssh. This can happen if the name of the keyfile is not known to ssh.

Confirm which key files ssh is trying by running it with one of the verbose options, as described at the start of the [Troubleshooting section](https://wiki.gentoo.org/wiki/SSH#Troubleshooting). The verbose output will include the names of the keyfiles it is trying, and the one (if any) that actually gets used.

The default key files for the system are listed in the /etc/ssh/ssh\_config, see the commented-out lines containing `IdentityFile` directives.

There are several ways to use a key with a non-default name.

The key name can be specified on the command line every time:

`user $``ssh -i ~/.ssh/my_keyfile user@remotesys`
Alternatively, add following ssh configuration file to add a special case for ssh to the remote system:

**`/etc/ssh/ssh_config/02_remotesys.conf`**

**Define keyfiles to use for host remotesys**

If any are specified, it appears to be necessary to specify *all* the desired keys on a remote host. Read up on the ssh IdentityFile.

### X11 forwarding, not forwarding, or tunneling

**Problem**: After having made the necessary changes to the configuration files for permitting X11 forwarding, it is discovered X applications are executing on the server and are not being forwarded to the client.

**Solution**: What is likely occurring during SSH login into the remote server or host, the `DISPLAY` variable is either being unset or is being set *after* the SSH session sets it.

Test for this scenario perform the following after logging in remotely:

`user $``echo $DISPLAY`
localhost:10.0

The output should be something similar to `localhost:10.0` or `localhost2.local:10.0` using server side `X11UseLocalhost no` setting. If the usual `:0.0` is not displayed, check to make sure the `DISPLAY` variable within \~/.bash\_profile is not being unset or re-initializing. If it is, remove or comment out any custom initialization of the `DISPLAY` variable to prevent the code in \~/.bash\_profile from executing during a SSH login:

`user $``ssh -t larry@localhost2 bash --noprofile`
Be sure to substitute `larry` in the command above with the proper username.

A trick that works to complete this task would be to define an alias within the users' \~/.bashrc file.

### Wayland forwarding over ssh

[Wayland](https://wiki.gentoo.org/wiki/Wayland)-based systems require additional software for forwarding GUI applications.

Install [Waypipe](https://wiki.gentoo.org/wiki/Waypipe) on both client and server:

`root #``emerge --ask gui-apps/waypipe`
To run a remote command with waypipe, prefix the ssh command with waypipe. For example to open Kate on a remote server:

`user $``waypipe ssh <user>@<host> kate`
Further it functions like normal ssh. So to open a secure shell that can launch GUI applications:

`user $``waypipe ssh <user>@<host>`
### The current time is displayed for PrintLastLog

By default, /etc/pam.d/system-login runs:

This updates the last login time, before `PrintLastLog` in sshd. In order for `PrintLastLog` to work, this pam line must be disabled. Alternatively, `PrintLastLog` can be disabled and the *silent* option can be removed:

## See also

- [Autossh](https://wiki.gentoo.org/wiki/Autossh) — a command that detects when [SSH] connections drop and automatically reconnects them.
- [dropbear](https://wiki.gentoo.org/wiki/Dropbear) — a lightweight SSH server. It runs on a variety of POSIX-based platforms.
- [Keychain](https://wiki.gentoo.org/wiki/Keychain) — This document describes how to use [SSH] shared keys along with the keychain program.
- [Mosh](https://wiki.gentoo.org/wiki/Mosh) — a SSH client server that is aware of connectivity problems of the original SSH implementation.
- [SCP](https://wiki.gentoo.org/wiki/SCP) — an interactive file transfer program, similar to the copy command, that copies files over an encrypted SSH transport.
- [SFTP](https://wiki.gentoo.org/wiki/SFTP) — an interactive file transfer program, similar to [FTP](https://wiki.gentoo.org/wiki/FTP), which performs all operations over an encrypted [SSH] transport.
- [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) — a secure shell client used to mount remote filesystems to local machines.
- [SSH tunneling](https://wiki.gentoo.org/wiki/SSH_tunneling) — a method of connecting to machines on the other side of a gateway machine.
- [SSH jump host](https://wiki.gentoo.org/wiki/SSH_jump_host) — employed as an alternative to [SSH tunneling](https://wiki.gentoo.org/wiki/SSH_tunneling) to access internal machines through a gateway.
- [rsync](https://wiki.gentoo.org/wiki/Rsync) — a powerful file sync program capable of efficient file transfers and directory synchronization.
- [Securing the SSH service](https://wiki.gentoo.org/wiki/Security_Handbook/Securing_services#SSH) (Security Handbook)
- [Starting the SSH daemon — Gentoo Handbook — Installation](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media#Optional:_Starting_the_SSH_daemon)

## External resources

- [net-misc/connect](https://packages.gentoo.org/packages/net-misc/connect) — [SSH Proxy Command -- connect.c](https://github.com/gotoh/ssh-connect)
- [https://lonesysadmin.net/2011/11/08/ssh-escape-sequences-aka-kill-dead-ssh-sessions/amp/](https://lonesysadmin.net/2011/11/08/ssh-escape-sequences-aka-kill-dead-ssh-sessions/amp/) - A blog entry on escape sequences.
- [https://hackaday.com/2017/10/18/practical-public-key-cryptography/](https://hackaday.com/2017/10/18/practical-public-key-cryptography/) - Practical public key cryptography (Hackaday).
- [https://www.akadia.com/services/ssh\_putty.html](https://www.akadia.com/services/ssh_putty.html) - Port forwarding explained.
