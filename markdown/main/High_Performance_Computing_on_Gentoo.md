<!-- source: https://wiki.gentoo.org/wiki/High_Performance_Computing_on_Gentoo | group: Gentoo Wiki (Main) | wiki-title: High Performance Computing on Gentoo -->
---
title: High Performance Computing on Gentoo
url: https://wiki.gentoo.org/wiki/High_Performance_Computing_on_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-10"
fingerprint: "3e99b25a52831bed"
license: CC BY-SA 4.0
---

# High Performance Computing on Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document was written by people at the Adelie Linux R&D Center as a step-by-step guide to turn a Gentoo System into a High Performance Computing (HPC) system.

## Introduction

Gentoo Linux, a special flavor of Linux that can be automatically optimized and customized for just about any application or need. Extreme performance, configurability and a top-notch user and developer community are all hallmarks of the Gentoo experience.

Thanks to a technology called Portage, Gentoo Linux can become an ideal secure server, development workstation, professional desktop, gaming system, embedded solution or... a High Performance Computing system. Because of its near-unlimited adaptability, we call Gentoo Linux a metadistribution.

This document explains how to turn a Gentoo system into a High Performance Computing system. Step by step, it explains what packages one may want to install and helps configure them.

Obtain Gentoo Linux from the website [\[1\]](https://www.gentoo.org/), and refer to the [documentation](https://wiki.gentoo.org/wiki/Main_Page) at the same location to install it.

## Configuring Gentoo Linux for Clustering

### Recommended Optimizations

During the installation process, you will have to set your USE variables in /etc/portage/make.conf. We recommended that you deactivate all the defaults (see /etc/portage/make.profile/make.defaults) by negating them in make.conf. However, you may want to keep such use variables as 3dnow, gpm, mmx, nptl, nptlonly, sse, ncurses, pam and tcpd. Refer to the USE documentation for more information.

When you install miscellaneous packages, we recommend installing the following:

`root #``emerge --ask nfs-utils portmap tcpdump ssmtp iptables xinetd`
### Communication Layer (TCP/IP Network)

A cluster requires a communication layer to interconnect the slave nodes to the master node. Typically, a FastEthernet or GigaEthernet LAN can be used since they have a good price/performance ratio. Other possibilities include use of products like [Myrinet](http://www.myricom.com/), [QsNet](http://quadrics.com/) or others.

A cluster is composed of two node types: master and slave. Typically, your cluster will have one master node and several slave nodes.

The master node is the cluster's server. It is responsible for telling the slave nodes what to do. This server will typically run such daemons as dhcpd, nfs, pbs-server, and pbs-sched. Your master node will allow interactive sessions for users, and accept job executions.

The slave nodes listen for instructions (via ssh/rsh perhaps) from the master node. They should be dedicated to crunching results and therefore should not run any unnecessary services.

The rest of this documentation will assume a cluster configuration as per the hosts file below. You should maintain on every node such a hosts file (/etc/hosts) with entries for each node participating node in the cluster.

**`/etc/hosts`**

To setup your cluster dedicated LAN, edit your /etc/conf.d/net file on the master node.

**`/etc/conf.d/net`**

```
# Global config file for net.* rc-scripts
  
# This is basically the ifconfig argument without the ifconfig $iface
#
  
config_eth0="192.168.1.100/24"
# Network Connection to the outside world using dhcp -- configure as required for you network
config_eth1="dhcp"
```
Finally, setup a DHCP daemon on the master node to avoid having to maintain a network configuration on each slave node.

**`/etc/dhcp/dhcpd.conf`**

### NFS/NIS

The Network File System (NFS) was developed to allow machines to mount a disk partition on a remote machine as if it were on a local hard drive. This allows for fast, seamless sharing of files across a network.

There are other systems that provide similar functionality to NFS which could be used in a cluster environment. The [Andrew File System from IBM](http://www.openafs.org), recently open-sourced, provides a file sharing mechanism with some additional security and performance features. The [Coda File System](http://www.coda.cs.cmu.edu/) is still in development, but is designed to work well with disconnected clients. Many of the features of the Andrew and Coda file systems are slated for inclusion in the next version of [NFS (Version 4)](http://www.nfsv4.org). The advantage of NFS today is that it is mature, standard, well understood, and supported robustly across a variety of platforms.

`root #``emerge --ask nfs-utils portmap`
Configure and install a kernel to support NFS v3 on all nodes:

**Required Kernel Configurations for NFS**

On the master node, edit your /etc/hosts.allow file to allow connections from slave nodes. If your cluster LAN is on 192.168.1.0/24, your hosts.allow will look like:

**`/etc/hosts.allow`**

Edit the /etc/exports file of the master node to export a work directory structure (/home is good for this).

**`/etc/exports`**

Add nfs to your master node's default runlevel:

`root #``rc-update add nfs default`
To mount the nfs exported filesystem from the master, you also have to configure your salve nodes' /etc/fstab. Add a line like this one:

**`/etc/fstab`**

You'll also need to set up your nodes so that they mount the nfs filesystem by issuing this command:

`root #``rc-update add nfsmount default`
### RSH/SSH

SSH is a protocol for secure remote login and other secure network services over an insecure network. OpenSSH uses public key cryptography to provide secure authorization. Generating the public key, which is shared with remote systems, and the private key which is kept on the local system, is done first to configure OpenSSH on the cluster.

For transparent cluster usage, private/public keys may be used. This process has two steps:

- Generate public and private keys
- Copy public key to slave nodes

For user based authentication, generate and copy as follows:

`root #``ssh-keygen -t dsa`
Generating public/private dsa key pair.
Enter file in which to save the key (/root/.ssh/id\_dsa): /root/.ssh/id\_dsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id\_dsa.
Your public key has been saved in /root/.ssh/id\_dsa.pub.
The key fingerprint is:
f1:45:15:40:fd:3c:2d:f7:9f:ea:55:df:76:2f:a4:1f root@master
  
WARNING! If you already have an "authorized\_keys" file,
please append to it, do not use the following command.

`root #``scp /root/.ssh/id_dsa.pub node01:/root/.ssh/authorized_keys`
root@master's password:
id\_dsa.pub   100%  234     2.0MB/s   00:00

`root #``scp /root/.ssh/id_dsa.pub node02:/root/.ssh/authorized_keys`
root@master's password:
id\_dsa.pub   100%  234     2.0MB/s   00:00

For host based authentication, you will also need to edit your /etc/ssh/shosts.equiv.

**`/etc/ssh/shosts.equiv`**

And a few modifications to the /etc/ssh/sshd\_config file:

**`/etc/ssh/sshd_config`**

**sshd configurations**

If your application require RSH communications, you will need to emerge [net-misc/netkit-rsh](https://packages.gentoo.org/packages/net-misc/netkit-rsh) and [sys-apps/xinetd](https://packages.gentoo.org/packages/sys-apps/xinetd).

`root #``emerge --ask xinetd netkit-rsh`
Then configure the rsh deamon. Edit your /etc/xinet.d/rsh file.

**`/etc/xinet.d/rsh`**

Edit your /etc/hosts.allow to permit rsh connections:

**`/etc/hosts.allow`**

Or you can simply trust your cluster LAN:

**`/etc/hosts.allow`**

Finally, configure host authentication from /etc/hosts.equiv.

**`/etc/hosts.equiv`**

And, add xinetd to your default runlevel:

`root #``rc-update add xinetd default`
### NTP

The Network Time Protocol (NTP) is used to synchronize the time of a computer client or server to another server or reference time source, such as a radio or satellite receiver or modem. It provides accuracies typically within a millisecond on LANs and up to a few tens of milliseconds on WANs relative to Coordinated Universal Time (UTC) via a Global Positioning Service (GPS) receiver, for example. Typical NTP configurations utilize multiple redundant servers and diverse network paths in order to achieve high accuracy and reliability.

Select a NTP server geographically close to you from [Public NTP Time Servers](http://www.eecis.udel.edu/~mills/ntp/servers.html), and configure your /etc/conf.d/ntp and /etc/ntp.conf files on the master node.

**`/etc/conf.d/ntp`**

Edit your /etc/ntp.conf file on the master to setup an external synchronization source:

**`/etc/ntp.conf`**

And on all your slave nodes, setup your synchronization source as your master node.

**`/etc/conf.d/ntp`**

**On slave node**

```
# /etc/conf.d/ntpd
  
NTPDATE_WARN="n"
NTPDATE_CMD="ntpdate"
NTPDATE_OPTS="-b master"
```
**`/etc/ntp.conf`**

**On slave node**

Then add ntpd to the default runlevel of all your nodes:

`root #``rc-update add ntpd default`
### IPTABLES

To setup a firewall on your cluster, you will need iptables.

`root #``emerge --ask iptables`
Required kernel configuration:

**IPtables kernel configuration**

And the rules required for this firewall:

**`/var/lib/iptables/rule-save`**

Then add iptables to the default runlevel of all your nodes:

`root #``rc-update add iptables default`
## HPC Tools

### OpenPBS

The Portable Batch System (PBS) is a flexible batch queuing and workload management system originally developed for NASA. It operates on networked, multi-platform UNIX environments, including heterogeneous clusters of workstations, supercomputers, and massively parallel systems. Development of PBS is provided by Altair Grid Technologies.

`root #``emerge --ask openpbs`
Before starting using OpenPBS, some configurations are required. The files you will need to personalize for your system are:

- /etc/pbs\_environment
- /var/spool/PBS/server\_name
- /var/spool/PBS/server\_priv/nodes
- /var/spool/PBS/mom\_priv/config
- /var/spool/PBS/sched\_priv/sched\_config

Here is a sample sched\_config:

**`/var/spool/PBS/sched_priv/sched_config`**

To submit a task to OpenPBS, the command `qsub` is used with some optional parameters. In the example below, "-l" allows you to specify the resources required, "-j" provides for redirection of standard out and standard error, and the "-m" will e-mail the user at beginning (b), end (e) and on abort (a) of the job. In the next example, a script is submitted on 2 nodes.

`root #``qsub -l nodes=2 -j oe -m abe myscript`
Normally jobs submitted to OpenPBS are in the form of scripts. Sometimes, you may want to try a task manually. To request an interactive shell from OpenPBS, use the "-I" parameter.

`root #``qsub -I`
To check the status of your jobs, use the qstat command:

`root #``qstat`
Job id  Name  User   Time Use S Queue
------  ----  ----   -------- - -----
2.geist STDIN adelie 0        R upto1nodes

### MPICH

Message passing is a paradigm used widely on certain classes of parallel machines, especially those with distributed memory. MPICH is a freely available, portable implementation of MPI, the Standard for message-passing libraries.

The mpich ebuild provided by Adelie Linux allows for two USE flags: *doc* and *crypt*. *doc* will cause documentation to be installed, while *crypt* will configure MPICH to use `ssh` instead of `rsh`.

`root #``emerge --ask mpich`
You may need to export a mpich work directory to all your slave nodes in /etc/exports:

**`/etc/exports`**

Most massively parallel processors (MPPs) provide a way to start a program on a requested number of processors; `mpirun` makes use of the appropriate command whenever possible. In contrast, workstation clusters require that each process in a parallel job be started individually, though programs to help start these processes exist. Because workstation clusters are not already organized as an MPP, additional information is required to make use of them. Mpich should be installed with a list of participating workstations in the file machines.LINUX in the directory /usr/share/mpich/. This file is used by `mpirun` to choose processors to run on.

Edit this file to reflect your cluster-lan configuration:

**`/usr/share/mpich/machines.LINUX`**

Use the script `tstmachines` in /usr/sbin/ to ensure that you can use all of the machines that you have listed. This script performs an `rsh` and a short directory listing; this tests that you both have access to the node and that a program in the current directory is visible on the remote node. If there are any problems, they will be listed. These problems must be fixed before proceeding.

The only argument to `tstmachines` is the name of the architecture; this is the same name as the extension on the machines file. For example, the following tests that a program in the current directory can be executed by all of the machines in the LINUX machines list.

`root #``/usr/local/mpich/sbin/tstmachines LINUX`
The output from this command might look like:

If `tstmachines` finds a problem, it will suggest possible reasons and solutions. In brief, there are three tests:



- *Can processes be started on remote machines?* tstmachines attempts to run the shell command true on each machine in the machines files by using the remote shell command.
- *Is current working directory available to all machines?* This attempts to ls a file that tstmachines creates by running ls using the remote shell command.
- *Can user programs be run on remote systems?* This checks that shared libraries and other components have been properly installed on all machines.

And the required test for every development tool:

`root #````
cd ~
```
`root #````
cp /usr/share/mpich/examples1/hello++.c ~
```
`root #````
make hello++
```
`root #``mpirun -machinefile /usr/share/mpich/machines.LINUX -np 1 hello++`
For further information on MPICH, consult the documentation at [http://www-unix.mcs.anl.gov/mpi/mpich/docs/mpichman-chp4/mpichman-chp4.htm](http://www-unix.mcs.anl.gov/mpi/mpich/docs/mpichman-chp4/mpichman-chp4.htm) .

## Bibliography

The original document is published at the Adelie Linux R&D Centre web site, and is reproduced here with the permission of the authors and [Cyberlogic](http://www.cyberlogic.ca)'s Adelie Linux R&D Centre.
