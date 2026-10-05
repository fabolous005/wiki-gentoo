<!-- source: https://wiki.gentoo.org/wiki/Keychain | group: Gentoo Wiki (Main) | wiki-title: Keychain -->
---
title: Keychain
url: https://wiki.gentoo.org/wiki/Keychain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-14"
fingerprint: "2afbfb591c207e9e"
license: CC BY-SA 4.0
---

# Keychain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document describes how to use [SSH](https://wiki.gentoo.org/wiki/SSH) shared keys along with the keychain program. It assumes basic knowledge of public key cryptography.

Keychain is a frontend to ssh-agent and ssh-add, allowing long running sessions and letting the user enter passphases just once. It can also be used to allow scripts access to SSH connections.

## Background

### The problem at hand

Having to type in login passwords on each and every system is inconvenient, especially if many systems are being managed. Some administrators might even have a need for a script or cron-job that needs a convenient way to use an ssh connection. Either way, there is a solution to this problem, and it begins with public key authentication.

### How does public key authentication work?

Assume that a client wants to connect to the ssh daemon on a server. The client first generates a key pair and gives the public key to the server. Afterwards, whenever the client attempts to connect, the server sends a challenge that is encrypted with that public key. Only the holder of the corresponding private key (the client) is able to decrypt it, so the correct response leads to successful authentication.

## How to use public key authentication

### Generating a key pair

The first step is to create a key pair. To do this, use the ssh-keygen command:

`user $``ssh-keygen`
Accept the default values, and make sure to enter a strong passphrase.

After the generation has ended a private key should be located at \~/.ssh/id\_rsa and a public key in \~/.ssh/id\_rsa.pub. The public key is now ready to be copied to the remote host.

Adding or changing a passphrase for the private key can be done as follows:

`user $``ssh-keygen -p -f ~/.ssh/id_rsa`
### Preparing the server

The \~/.ssh/id\_rsa.pub file needs to be copied over to the server running sshd. It has to be added to the \~/.ssh/authorized\_keys file that belongs the connecting user on the remote server. After ssh access to the server has been granted by infrastructure personnel, the following steps can be used to setup automatic login using a public key on the remote server:

`user $``ssh-copy-id -i ~/.ssh/id_rsa.pub server_user@server`
ssh-copy-id is a wrapper script for these steps. If this wrapper script is unavailable, then the following steps can be used:

`user $````
scp ~/.ssh/id_rsa.pub server_user@server:~/myhost.pub
```
`user $````
ssh server_user@server "cat ~/myhost.pub >> ~/.ssh/authorized_keys"
```
`user $``ssh server_user@server "cat ~/.ssh/authorized_keys"`
The output from that last line should show the contents of the \~/.ssh/authorized\_keys file. Make sure the output looks correct.

### Testing the setup

Theoretically, if all went well, and the sshd daemon on the server allows it (as this can be configured), ssh access without entering a password should now be possible on the server. The private key on the client will still need to be decrypted with the passphrase used previously, but this should not be confused with the password of the user account on the server.

`user $``ssh <server_user>@<server>`
It should have asked for a passphrase for id\_rsa, and then grant access via ssh as the user `<server_user>` on the server. If not, login as `<server_user>`, and verify that the contents of \~/.ssh/authorized\_keys has each entry (which is a public key) on a single line. It is also a good idea to check the sshd configuration to make sure that it allows to use public key authorization when available.

At this point, readers might be thinking, "What's the point, I just replaced one password with another?!" Relax, the next section will show exactly how we can use this to only enter the passphrase once and re-use the (decrypted) key for multiple logins.

## Making public key authentication convenient

### Typical key management with ssh-agent

The next step is to decrypt the private key(s) once, and gain the ability to ssh freely, without any passwords. That is exactly what the program ssh-agent is for.

ssh-agent is usually started at the beginning of the X session, or from a shell startup script like \~/.bash\_profile. It works by creating a UNIX socket, and registering the appropriate environment variables so that all subsequent applications can take advantage of its services by connecting to that socket. Clearly, it only makes sense to start it in the parent process of an X session to use the set of decrypted private keys in all subsequent X applications.

`user $``` eval `ssh-agent` ``
When running ssh-agent, it should output the PID of the running ssh-agent, and also set a few environment variables, namely `SSH_AUTH_SOCK` and `SSH_AGENT_PID`. It should also automatically add \~/.ssh/id\_rsa to its collection and ask the user for the corresponding passphrase. If other private keys exist which need to be added to the running ssh-agent, use the ssh-add command:

`user $``ssh-add somekeyfile`
Now for the magic. With the decrypted private key ready, ssh into a (public key configured) server without entering any passwords:

`user $``ssh server`
In order to shut down ssh-agent (and as such require entry of the passphrase again later):

`user $``ssh-agent -k`
To get even more convenience from ssh-agent, proceed to the next section on using keychain. Be sure to kill the running ssh-agent as keychain will handle the ssh-agent sessions itself.

### Squeezing the last drop of convenience out of ssh-agent

Keychain will allow to reuse an ssh-agent between logins, and optionally prompt for passphrases each time the user logs in. Let's emerge it first:

`root #``emerge --ask net-misc/keychain`
Assuming that was successful, keychain can now be used.

Add the following to the shell initialization file (\~/.bash\_profile, \~/.zshrc, or similar) to enable it:

**`~/.bash_profile`**

**Enabling Keychain in Bash**

```
 ~/.ssh/id_rsa
. ~/.keychain/${HOSTNAME}-sh
. ~/.keychain/${HOSTNAME}-sh-gpg
```
**`~/.zshrc`**

**Enabling Keychain in zsh**

```
 ~/.ssh/id_rsa
. ~/.keychain/${HOST}-sh
. ~/.keychain/${HOST}-sh-gpg
```
Now test it. First make sure the ssh-agent processes from the previous section are killed, then start up a new shell, usually by just logging in, or spawning a new terminal. It should prompt for the password for each key specified on the command line.

All shells opened after that point should reuse the ssh-agent, allowing to use passwordless SSH connections over and over.

### Using keychain with Plasma 5

Plasma 5 users, instead of using \~/.bash\_profile, can let Plasma manage ssh-agent for them. In order to do so, edit /etc/xdg/plasma-workspace/env/10-agent-startup.sh, which is read during Plasma's startup, and /etc/xdg/plasma-workspace/shutdown/10-agent-shutdown.sh, which is executed during its shutdown.

Here is how one could edit those files:

**`/etc/xdg/plasma-workspace/env/10-agent-startup.sh`**

**Editing for Plasma 5**

```
SSH_AGENT=true
```
**`/etc/xdg/plasma-workspace/shutdown/10-agent-shutdown.sh`**

**Editing for Plasma 5**

```
if [ -n "${SSH_AGENT_PID}" ]; then
  eval "$(ssh-agent -k)"
fi
```
Now, all that has to be done is launch a terminal of choice, like [kde-apps/konsole](https://packages.gentoo.org/packages/kde-apps/konsole), and load the right set of keys to use. For example:

`user $``keychain ~/.ssh/id_rsa`
The keys will be remembered until the end of the Plasma session (or until the ssh-agent process is killed manually).

#### Alternatively use KWallet with kde-plasma/ksshaskpass under Plasma 5

You can also have Plasma automatically ask you for your passphrase upon desktop login. Emerge kde-plasma/ksshaskpass, which will set up an environment variable to use the ksshaskpass application whenever ssh-add is run outside of a terminal. Then create a script as follows, and install it via the Plasma -> System Settings -> Startup and Shutdown -> Autostart.

**`~/ssh.sh`**

**Create ssh.sh script**

```
#!/bin/sh
ssh-add < /dev/null
```
## Concluding remarks

### Security considerations

Of course, the use of ssh-agent may add a bit of insecurity to the system. If another user would gain access to a running shell, he could login to all of the servers without passwords. As a result, it is a risk to the servers, and users should be sure to consult the local security policy (if any). Be sure to take the appropriate measures to ensure the security of all sessions.

### Troubleshooting

Most of this should work pretty well, but if problems do come up, then the following items might be of assistance.

- If connecting without ssh-agent does not seem to work, consider using ssh with the `-vvv` options to find out what's happening. Sometimes the server is not configured to use public key authentication, sometimes it is configured to ask for local passwords anyway! If that is the case, try using the `-o` option with ssh, or change the server's sshd\_config.
- If connecting with ssh-agent or keychain does not seem to work, then it may be that the current shell does not understand the commands used. Consult the man pages for ssh-agent and keychain for details on working with other shells.

## See also

- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.

## External resources

- [The official Keychain project page](http://www.funtoo.org/Keychain) at Funtoo.org.
- [IBM developerWorks article series](http://www.funtoo.org/OpenSSH_Key_Management,_Part_1) introducing the concepts behind Keychain.
