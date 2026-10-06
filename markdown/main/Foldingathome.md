<!-- source: https://wiki.gentoo.org/wiki/Foldingathome | group: Gentoo Wiki (Main) | wiki-title: Foldingathome -->
---
title: Foldingathome
url: https://wiki.gentoo.org/wiki/Foldingathome
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-19"
fingerprint: e245af4b1dbbe3c5
license: CC BY-SA 4.0
---

# Foldingathome

From Gentoo Wiki

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

FILE **`/etc/systemd/system/Stop_Foldingathome_before_Hibernate.service`**

```
[Unit]
Description=Stop foldingathome before hibernate
Before=suspend.target
Before=hibernate.target
Before=hybrid-sleep.target
[Service]
ExecStart=/bin/bash -c "/bin/systemctl stop foldingathome"
ExecStartPost=/bin/sleep 10
[Install]
WantedBy=suspend.target
WantedBy=hibernate.target
WantedBy=hybrid-sleep.target
```
FILE **`/etc/systemd/system/Start_Foldingathome_after_Hibernate.service`**

```
[Unit]
Description=Restart foldingathome after hibernate
#After=suspend.target
After=hibernate.target
#After=hybrid-sleep.target
[Service]
Type=simple
ExecStart=/bin/systemctl start foldingathome
[Install]
#WantedBy=suspend.target
WantedBy=hibernate.target
#WantedBy=hybrid-sleep.target
```
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

FILE **`/lib/systemd/system-sleep/suspend-resume.sh`**

```
#!/bin/bash
echo $1/$2 
case $1/$2 in
   pre/hibernate)
	echo "Going to $1 $2..."
	# flush caches before hibernating to save time on sleep/wakeup
	sync
	echo 1 > /proc/sys/vm/drop_caches
	sync
	echo 2 > /proc/sys/vm/drop_caches
	sync
	echo 3 > /proc/sys/vm/drop_caches
	sync
   ;;
   post/hibernate)
	echo "Waking up from $1 $2..."
   ;;
   pre/suspend)
	echo "Going to $2..."
   ;;
   post/suspend)
	echo "Waking up from $2..."
	# reload the nvidia_uvm module to enable cuda again after resume from suspend
 
	rmmod nvidia_uvm
	modprobe nvidia_uvm
	systemctl start foldingathome
   ;;
esac
```
#### make the script executable.

`root #````
chmod a+x /lib/systemd/system-sleep/suspend-resume.sh
```
to do: procedure for other videocards.
