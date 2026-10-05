<!-- source: https://wiki.gentoo.org/wiki/SSHFS | group: Gentoo Wiki (Main) | wiki-title: SSHFS -->
---
title: SSHFS
url: https://wiki.gentoo.org/wiki/SSHFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-04"
fingerprint: "323ff49e9baa536c"
license: CC BY-SA 4.0
---

# SSHFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- Add instructions for using SSHFS with more desktop environments.
- Extra details would help.

**SSHFS** (**SSH F**ile **S**ystem) is a secure shell client used to mount remote filesystems to local machines. SSHFS uses [Filesystem in Userspace (FUSE)](https://wiki.gentoo.org/wiki/Filesystem_in_Userspace) to mount filesystems in a location users can easily access, and accesses files over the [SFTP](https://wiki.gentoo.org/wiki/SFTP) protocol.

## Installation

### Kernel

Since SSHFS uses FUSE it will need to be enabled in the kernel.

**Enabling FUSE in the kernel**

### USE flags

There are currently no available USE flags for SSHFS.

### Emerge

Use the emerge command to ask Portage to install [net-fs/sshfs](https://packages.gentoo.org/packages/net-fs/sshfs):

`root #``emerge --ask net-fs/sshfs`
## Configuration

Permissions may need to be set, or mounting may produce a *"fuse: failed to open /dev/fuse: Permission denied"* error. It's better to come back to this section only if necessary.

If the above error is encountered, first create a group for those who will be allowed to use SSHFS, then add the accounts that will use it, to the group:

`root #``groupadd crypto``root #``usermod -aG crypto $USERNAME`
Log user out then in, if needed, to apply new group permissions.

Now [create](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file) the /etc/udev/rules.d/60-fuse.rules file and add the following contents:

**`/etc/udev/rules.d/60-fuse.rules`**

Reload `fuse` module, for changes to apply:

`root #``modprobe -r fuse``root #``modprobe fuse``root #``ls -l /dev/fuse`
crw-rw-rw- 1 root crypto 10, 229 Nov  8 22:29 /dev/fuse

## Usage

### Mounting

In order to use SSHFS, an [SSH daemon](https://wiki.gentoo.org/wiki/SSH) needs to be running on the remote machine, the *sftp* subsystem may need to be activated, and at least one user must be configured to be able to access the machine via SSH.

To mount a remote file system locally, the right credentials will be needed. If no user name is given on the command line, the current user name will be used by default. For example, if the user *larry* is currently the active user on the system, and this command is run:

`larry@example $``sshfs remotehost:/home/larry ~/remote_mount`
The username *larry* will be sent to the remote system, so the credentials for access to a remote account named *larry* will be needed. After entering the password for *larry*, this would mount the remote home directory of the user *larry* to a directory \~/remote\_mount on the local machine. The previous command is the equivalent of running this command:

`larry@example $``sshfs larry@remotehost:/home/larry ~/remote_mount`
To change the user, put the name of the user before the IP address to domain name. For example, to login to the remote system using the remote system's *root* user name and password, use:

`user $``sshfs root@remotehost:remotehost:/ ~/remote_mount`
Root access must be enabled on the server for this.

#### Sudo

If a user can sudo on the remote host to another user, it may be possible to access files as that user on the remote host (if the access to sudo does not need a password). For example, if a user can su to root on the remote host without entering a password, mount with:

`user $``sshfs -o sftp_server="/usr/bin/sudo /usr/lib64/misc/sftp-server" larry@remotehost:/path/to/remote/system /path/to/local/sshfs/mount`
These paths are valid for a default Gentoo installation on the remote host, they may need to be changed if the server is running other distributions.

### Unmounting

To unmount a directory with SSHFS use the fusermount command with the `-u` option:

`larry@example $``fusermount -u /path/to/local/sshfs/mount`
### Permissions based options

In order to have read/write access to a mounted remote directory the allow\_other and/or allow\_root options may be needed, depending on if the user is root or a regular user. Simply enable as shown below (replace allow\_other with allow\_root, if root).

`larry@example $``sshfs -o allow_other larry@remotehost:/path/to/remote/directory /path/to/local/sshfs/mountpoint`
Alternatively, the uid, gid, and umask options can be used to further fine tune permissions. When setting multiple options at the same time use a space separated list after a single `-o`.

### Automating the connection

For remote file systems that need to be mounted frequently, it is the sign in process may be automated. Automation can be achieved by using a public/private SSH key pair combined with a mechanism to mount the remote filesystem on a specific event (user login, or system boot for example).

The first step is to setup the SSH key pair on the local and remote machines. Visit the [Passwordless Authentication section of the SSH article](https://wiki.gentoo.org/wiki/SSH#Passwordless_Authentication) for further instructions on how set up an SSH key pair. When finished return to this article.

After the key pair has been created and properly set up, determine what event will be used to start the connection automatically. It is common for a system to attempt to remotely mount a file system upon user login or system boot. Controlling the sshfs mount depends on what software the user will be implementing in their local environment. There are several ways to handle the task.

#### fstab

sshfs can be used inside a system's [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) file. This enables over-the-network filesystems to be assigned to act as local filesystem mounts. Filesystems using sshfs require slightly different mount options, so be sure to look at the man page to be sure the options are correct in each use case. A example of using sshfs in fstab:

**`/etc/fstab`**

**Adding sshfs to fstab**

```
# Automatically mount ~/Music on connection
sshfs#SERVER_USER@remotehost:/SOURCEDIR /home/USER/Music fuse user,_netdev,idmap=user,transform_symlinks,identityfile=/home/USER/.ssh/id_rsa,allow_other,default_permissions,uid=1000,gid=1000 0 0
```
#### Login shells

Most shells include support for commands to be executed during user login or logout. This section will provide examples on how to automate the connection using built in shell script.

Before proceeding, it is necessary to know which shell is being used. Execute the following command as the user of interest to determine which shell is being used:

`user $``echo $SHELL`
Possible output:

- /bin/bash for bash
- /bin/sh for sh
- /bin/tcsh for tcsh (csh)
- /bin/zsh for zsh

##### Bash

When using a bash shell, create a \~/.bash\_login and \~/.bash\_logout files in the user's home directory and add the sshfs command to the file.

`user $``touch ~/.bash_login ~/.bash_logout`
Mount on shell login:

**`~/.bash_login`**

**Adding sshfs mount to bash login shell**

```
# Added to mount a remote directory at user login
sshfs larry@remotehost:/path/to/remote/directory /path/to/local/sshfs/mountpoint
```
Unmounting on shell logout:

**`~/.bash_logout`**

**Adding sshfs unmount to bash logout shell**

```
# Added to unmount a remote directory at user logout
fusermount -u /path/to/local/sshfs/mountpoint
```
##### Sh

`user $``touch ~/.profile`
**`~/.profile`**

**Adding sshfs mount to sh login shell**

```
# Added to mount a remote directory at user login
sshfs larry@remotehost:/path/to/remote/directory /path/to/local/sshfs/mountpoint
```
##### Zsh

`user $``touch ~/.zlogin`
**`~/.zlogin`**

**Adding sshfs mount to zsh login shell**

```
# Added to mount a remote directory at user login
sshfs larry@remotehost:/path/to/remote/directory /path/to/local/sshfs/mountpoint
```
##### Tcsh (Csh)

`user $``touch ~/.login`
**`~/.login`**

**Adding sshfs mount to tcsh or csh login shell**

```
# Added to mount a remote directory at user login
sshfs larry@remotehost:/path/to/remote/directory /path/to/local/sshfs/mountpoint
```
### Desktop environments & Window Manager

Most desktop environments include methods for automatically starting programs. Some Window Manager also have their own way to automate this as well.

#### Openbox

[Openbox](https://wiki.gentoo.org/wiki/Openbox) uses the autostart file located in each user's home directory.

**`~/.config/openbox/autostart`**

**Adding sshfs mount Openbox autostart file**

```
# Added to mount a remote directory at user login
sshfs larry@remotehost:/path/to/remote/directory /path/to/local/sshfs/mountpoint &
```
## See also

- [CurlFtpFS](https://wiki.gentoo.org/wiki/CurlFtpFS) — allows for [mounting](https://wiki.gentoo.org/wiki/Mount) an FTP folder as a regular directory to the local directory tree.
- [SCP](https://wiki.gentoo.org/wiki/SCP) — an interactive file transfer program, similar to the copy command, that copies files over an encrypted SSH transport.
- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
- [SFTP](https://wiki.gentoo.org/wiki/SFTP) — an interactive file transfer program, similar to [FTP](https://wiki.gentoo.org/wiki/FTP), which performs all operations over an encrypted [SSH](https://wiki.gentoo.org/wiki/SSH) transport.
