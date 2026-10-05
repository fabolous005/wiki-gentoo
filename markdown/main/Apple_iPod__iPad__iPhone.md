<!-- source: https://wiki.gentoo.org/wiki/Apple_iPod,_iPad,_iPhone | group: Gentoo Wiki (Main) | wiki-title: Apple iPod, iPad, iPhone -->
---
title: Apple iPod, iPad, iPhone
url: https://wiki.gentoo.org/wiki/Apple_iPod,_iPad,_iPhone
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: "3e49180787af13e8"
license: CC BY-SA 4.0
---

# Apple iPod, iPad, iPhone

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a placeholder for a more comprehensive article on how to manage an Apple device on a Gentoo system. 
The goal is to mount the iPod/iPhone/iPad somewhere so that it can be interfaced with by other tools, such as [app-pda/gtkpod](https://packages.gentoo.org/packages/app-pda/gtkpod) or others. It also shows how to copy files to the iDevice using a file manager, without the need of syncing via iTunes.

## The idevice Software Stack

This is the idevice software stack. The dependencies will be pulled in automatically, but this is useful as a reference when trying to figure out which layers are or are not working.

![Iphonelinux-stack-2011.png](https://wiki.gentoo.org/images/f/f0/Iphonelinux-stack-2011.png)


## USB Device Access

[app-pda/usbmuxd](https://packages.gentoo.org/packages/app-pda/usbmuxd) must be explicity emerged since no other stack package will pull it in.  Run the daemon to make the device available to higher layers:

`root #``usbmuxd -f -v`
The flags are optional but make managing the daemon easy and transparent.

## Pairing

Pairing functionality is provided by [app-pda/libimobiledevice](https://packages.gentoo.org/packages/app-pda/libimobiledevice). Emerge it, then test it by running:

`user $``idevice_id -l`
This should print the device serial number which appeared in your dmesg log.

If this has been successful, run:

`user $``ideviceinfo`
to verify that communications with the device are functioning correctly.

Now run:

`user $``idevicepair pair`
to pair with the device. Check this with:

`user $``idevicepair validate`
## Mounting

[app-pda/libimobiledevice](https://packages.gentoo.org/packages/app-pda/libimobiledevice) is supported by [gnome-base/gvfs](https://packages.gentoo.org/packages/gnome-base/gvfs), so if you use GNOME, it is easy for you. If not, you will need [app-pda/ifuse](https://packages.gentoo.org/packages/app-pda/ifuse). Both of these will mount /var/root/Media from the idevice to a target directory. If your device is 'jailbroken', you may use the **--root** flag to gain access to the root of the device.

Note that media files must be in /var/mobile/Applications//.data/Movies/ to be accessible by applications running on the device. More on this later.

Simply emerge gvfs with USE="ios", and it should auto-mount it for you. You can also mount the device manually using a frontend to GIO/GVfs such as [x11-misc/gigolo](https://packages.gentoo.org/packages/x11-misc/gigolo), [xfce-base/thunar](https://packages.gentoo.org/packages/xfce-base/thunar) (>=1.6.x) or [gnome-base/nautilus](https://packages.gentoo.org/packages/gnome-base/nautilus).

`root #``emerge -av x11-misc/gigolo`
First, emerge ifuse, then try running it:

`root #``emerge -av app-pda/ifuse`
Users wishing to access the idevice must be members of the **plugdev** group, and must have write-access to the mount point.

`root #``gpasswd -a USER plugdev`
Remember, you need to start a new user session after doing this for it to take effect. Most users quit their window manager and re-enter it to do this.

Also you need to change **fusermount** permissions:

`root #``chmod 4755 /usr/bin/fusermount`
Create a mount-point for the **Media** folder on the device.

`root #``mkdir /media/ipad-Media`
Create some App-specific mount points (I use the AppIDs as directory names, more on finding them later)

`root #``mkdir /media/ipad-com.apple.iBooks /media/ipad-com.olimsoft.oplayer.hd.lite`
Set appropriate permissions.

`root #``chown root:plugdev /media/ipad*``root #``chmod 775 /media/ipad*`
Now mount the device as a user. First, the Media "partition":

`user $``ifuse /media/ipad-Media`
Now an App's Documents folder:

`user $``ifuse --appid com.olimsoft.oplayer.hd.lite /media/ipad-com.olimsoft.oplayer.hd.lite`
If you get error "bad mount point", try this command instead previous:

`user $``ifuse --documents com.olimsoft.oplayer.hd.lite /media/ipad-com.olimsoft.oplayer.hd.lite`
Finally, the device root (jailbroken devices only - Requires 'Apple File Conduit "2"' Tweak):

`user $``ifuse --root /media/ipad-root`
If the preceding steps worked, this should be enough to mount the device. This can be automated using [udev](https://wiki.gentoo.org/wiki/Udev) rules.

You should note that the author's experience is that throughput is slow, and is seriously affected by multiple copies in parallel - **iotop** reports transfer rates of approximately 1200 KiB/s for a single transfer, but a total of 600 KiB/s for two in parallel.

## Transferring Media

For transferring audio files to the idevice, mount the Media partition, make sure that your user can read and write files there and point gtkpod at it.

For transfering files through file manager on iPhone - using Sambda share, another option:

### To Apps on the iDevice

#### Frontends to [gnome-base/gvfs](https://packages.gentoo.org/packages/gnome-base/gvfs)

Once you've "connected" to the 'Documents on My iPad' device using Gigolo, you can access the device via a file manager such as Thunar (>=1.6.x) or Nautilus and access the 'Documents' folder of the apps that support the [iTunes Document Sharing](http://www.hadess.net/2010/12/house-arrest-or-just-document-sharing.html) feature (also called the "House Arrest" protocol). Copying files to this 'Documents' folder will make some apps pick up the files and allow you to view/edit/delete them on the iPad.

Following is a non-exhaustive list of free apps that support file transfers as described above:

- Videos: 'OPlayerHD Lite', 'HD Player'
- Audio: 'OPlayerHD Lite'
- Photos: 'Another Photo Viewer'
- PDFs or E-books: 'Documents 2', 'ciando Reader'

Sometimes it may be necessary to restart the app to make it aware of the transferred file, or possibly even reboot the iPad.

**ifuse** can also mount areas of the ipad **by AppID**, which allows the user to transfer files, which may then be used by the particular application. The AppID is [a kind of namespace](http://stackoverflow.com/questions/555424/what-is-the-app-id-that-the-apple-developer-connection-site-is-asking-me-for), particular to each application.

To find these AppIDs, you must install [app-pda/ideviceinstaller](https://packages.gentoo.org/packages/app-pda/ideviceinstaller).

`root #``emerge app-pda/ideviceinstaller`
List the AppIDs of the Apps currently installed on your device.

`user $``ideviceinstaller -l`
Mount by AppID:

`user $``ifuse --appid com.apple.iBooks /media/ipad-com.apple.iBooks`
If you get error "bad mount point", try this command instead previous:

`user $``ifuse --documents com.apple.iBooks /media/ipad-com.apple.iBooks`
Now you are free to copy files to the mounted folder, which will be accessible to that App (iBooks in this example).

## Troubleshooting

### imobiledevice Troubleshooting

#### QueryType failed, error code -256

This error either appears on its own or followed by failed XML parsing, such as:

`root #``ideviceinfo`
Entity: line 1: parser error : Start tag expected, '\<' not found ��w���w�8�H 8�H .0" encoding="UTF-8"?>

^
In 2011-11, this was due to an Apple update, and git versions of [app-pda/libimobiledevice](https://packages.gentoo.org/packages/app-pda/libimobiledevice) and [app-pda/ifuse](https://packages.gentoo.org/packages/app-pda/ifuse) were needed

### error opening socket

If your device was connected before installing imobiledevice, you might get

`user $````
idevice_id -l
```
usbmuxd\_get\_device\_list: error opening socket!
No device found, is it connected?

Unplug and replug your device, and try again.

### ifuse Troubleshooting

#### GnuTLS Error: A TLS packet with unexpected length was received

`root #``ifuse /media/iphone`
In 2010, this could be solved by re-merging [net-libs/gnutls](https://packages.gentoo.org/packages/net-libs/gnutls) and [dev-libs/libgcrypt](https://packages.gentoo.org/packages/dev-libs/libgcrypt). See [this forum thread](https://forums.gentoo.org/viewtopic-t-783245.html) for more informations.

#### Permissions

Make sure that you are mounting as a normal user.

### GVfs Troubleshooting

#### Unhandled Apple File Control error

If you're getting a: "Failed to open directory "ggImageViewer". Unhandled Apple File Control error (7).", then it is possible that the relevant app doesn't properly support the Apple file sharing protocol. Sometimes though it helps if you simply disconnect the 'Documents on My iPad' device, unplug the iPad, then plug it back and reconnect the device. Then you might be able to normally access an app's sandboxed 'Documents' folder.
