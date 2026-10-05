<!-- source: https://wiki.gentoo.org/wiki/Foldingathome | group: Gentoo Wiki (Main) | wiki-title: Foldingathome -->
---
title: Foldingathome
url: https://wiki.gentoo.org/wiki/Foldingathome
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-19"
fingerprint: e241af4b1dfbb2c6
license: CC BY-SA 4.0
---

# Foldingathome

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Installing folding@home on gentoo linux.

## Install:

To install folding@home client's package, run:

`root #``emerge --ask sci-biology/foldingathome`
The package will install in /opt directory.

## Configure:

`root #````
emerge --config =foldingathome-7.5.1
```
Configuring pkg...
./FAHClient: /opt/foldingathome/libssl.so.10: no version information available (required by ./FAHClient)
./FAHClient: /opt/foldingathome/libcrypto.so.10: no version information available (required by ./FAHClient)
./FAHClient: /opt/foldingathome/libcrypto.so.10: no version information available (required by ./FAHClient)
12:09:37:INFO(1):Read GPUs.txt
User name \[Anonymous\]: myuser     (put the username that you want for the Folding@home-system)
Team number \[0\]: 
Passkey: 
Enable SMP \[true\]: 
Enable GPU \[true\]:

(replace version with the one just emerged.)

## Openrc

To start foldingathome's daemon:

`root #``rc-service foldingathome start`
Once the service has been started, one can control it from the client's webpage: [client.foldingathome.org](https://client.foldingathome.org/).

To stop the daemon:

`root #``rc-service foldingathome stop`
To add the foldingathome's daemon client to the default runlevel, run:

`root #``rc-update add foldingathome default`
To remove the foldingathome's daemon client from the default runlevel, run:

`root #``rc-update del foldingathome default`
## Systemd

### Enable Suspend/Hibernate Resume.

When suspending or hibernating without stopping the foldingathome service OpenCl becomes inoperative, (after a suspend one must also reload the nvidia\_uvm module.

### Service files:

**`/etc/systemd/system/Stop_Foldingathome_before_Hibernate.service`**

**`/etc/systemd/system/Start_Foldingathome_after_Hibernate.service`**

#### enable the 2 service-units:

`root #````
systemctl enable Stop_Foldingathome_before_Hibernate.service
```
`root #````
systemctl enable Start_Foldingathome_after_Hibernate.service
```
!!!foldingathome-service is not started by the Start\_Foldingathome\_after\_Hibernate-service!!!
!!!we need also to reload teh nvidia\_uvm module!!!
!!!folingdathome is started by a suspend-resumescript (see next charpter)!!!

#### System-sleep script:

**`/lib/systemd/system-sleep/suspend-resume.sh`**

#### make the script executable.

`root #````
chmod a+x /lib/systemd/system-sleep/suspend-resume.sh
```
to do: procedure for other videocards.
