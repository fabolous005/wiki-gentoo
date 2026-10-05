<!-- source: https://wiki.gentoo.org/wiki/GNOME/GDM | group: Gentoo Wiki (Main) | wiki-title: GNOME/GDM -->
---
title: GNOME/GDM
url: https://wiki.gentoo.org/wiki/GNOME/GDM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-03"
fingerprint: d2dc9f792db3a895
license: CC BY-SA 4.0
---

# GNOME/GDM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As part of the GNOME desktop suite, the **G**NOME **D**isplay **M**anager (GDM) is the daemon responsible for launching graphical display sessions via the [Xorg](https://wiki.gentoo.org/wiki/Xorg) display server or the gnome-shell directly via [Wayland](https://wiki.gentoo.org/wiki/Wayland) display protocol.

## Configuration

### Debugging

To enable debugging, edit the following configuration file:

**`/etc/gdm/custom.conf`**

```
[debug] 
# Uncomment the line below to turn on debugging
Enable=true
```
On systems running systemd, this will configure GDM to write verbose output to the journal. Be sure to disable debugging when finished resolving any issues to save space in the journal.

See [upstream's debug documentation](https://help.gnome.org/admin/gdm/stable/troubleshooting.html.en) for more details on debugging GDM.

## Troubleshooting

### GDM and Optimus

See the [GDM section of the NVIDIA Optimus article](https://wiki.gentoo.org/wiki/NVIDIA/Optimus#Gnome_Display_Manager_.28GDM.29) when using GDM on platforms utilizing NVIDIA Optimus.

### Stop GDM from loading SSH Agent

GDM will call the SSH Agent upon login. This automation can cause an issue in several situations.

To disable this behavior, create or modify the user's \~/.pam\_environment as indicated below.

**`~/.pam_environment`**

**Preventing GDM from loading the SSH Agent**

```
# disable the overwrite of SSH_AUTH_SOCK by gnome-shell
GSM_SKIP_SSH_AGENT_WORKAROUND DEFAULT=1
 
# Load gpg-agent as ssh-agent (optional, use only if you want to use gpg-agent as ssh-agent)
SSH_AGENT_PID DEFAULT=
SSH_AUTH_SOCK DEFAULT="${XDG_RUNTIME_DIR}/gnupg/S.gpg-agent.ssh"
```
### Enable tap to click in GDM

Tap to click is disabled by default in GDM's environment. Enabling this feature requires setting GNOME option org.gnome.desktop.peripherals.touchpad tap-to-click for the gdm user.

Grant permissions for gdm to access the X server:

`user $``xhost +SI:localuser:gdm`
Then set the gsettings configuration value as the gdm user to enable tap to click:

`user $``sudo -u gdm gsettings set org.gnome.desktop.peripherals.touchpad tap-to-click true`
The new setting will take effect after restarting the GDM service.

### GDM crashes when attempting to launch a GNOME Wayland session

**Problem:** GDM is crashing when attempting to launch a GNOME Wayland session. Known to affect at least gdm-3.32.0.

**Cause:** This is most likely occurring because the NVIDIA kernel module is being loaded by the kernel and subsequently detected by udev - even if it not being used by the graphics stack. The udev rule (see /lib/udev/rules.d/61-gdm.rules) is shipped with GDM. Even if the system is not actively using the NVIDIA driver or NVIDIA hardware to render graphics, the rule will still trigger the /usr/libexec/gdm-disable-wayland" executable. This writes a configuration file to /run/gdm/custom.conf which is read by GDM at start time and disables support for Wayland sessions.

**Solution:** There are a few solutions to this problem:

1. Uninstall the [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package and remove the installed NVIDIA kernel modules. This is most likely the easiest solution, since it will remove the NVIDIA kernel module that triggers the udev rule (nvidia.ko) and prevent it from returning.
2. Blacklist the NVIDIA kernel module from loading. This can is performed different ways: kernel command-line parameters via the secondary bootloader ([GRUB](https://wiki.gentoo.org/wiki/GRUB), [systemd-boot](https://wiki.gentoo.org/wiki/Systemd/systemd-boot), etc.) or adding a blacklist configuration file via modprobe.d.
3. System administrators that want to keep the NVIDIA binary blob available for other [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment), but want to launch GNOME on Wayland and follow this last solution. It is simplest to manually editing the offending part of the udev rule so that gdm-disable-wayland cannot create the custom configuration file. To be safe, review Xorg's configuration files in the /etc/X11 directory to be sure NVIDIA is not being set as the primary Xorg driver. It is also a good idea to review the Xorg.0.log to double check. Simply comment out the last line:

**`/lib/udev/rules.d/61-gdm.rules`**

### GNOME Fails to Start Under Wayland with Proprietary NVIDIA Driver

In some cases, when using the proprietary NVIDIA driver, GNOME may fail to start properly under Wayland and return control to GDM, causing the login screen to reappear in a loop. This issue is often related to how the NVIDIA driver handles threading for kernel mode setting (KMS).

#### Symptoms

- GNOME fails to start under Wayland.
- The login screen reappears after attempting to log in.
- Logs may show errors such as:

Failed to make thread 'KMS thread' high priority scheduled: GDBus.Error:org.freedesktop.DBus.Error.NameHasNoOwner: Name "org.freedesktop.RealtimeKit1" does not exist

#### Possible Solutions

##### 1. Set the `MUTTER_DEBUG_KMS_THREAD_TYPE` Environment Variable

You can force Mutter (the GNOME display manager) to use a user-space thread for KMS operations by setting the `MUTTER_DEBUG_KMS_THREAD_TYPE` environment variable.

Create or edit the /etc/environment file and add the following line:

**`/etc/environment`**

```
MUTTER_DEBUG_KMS_THREAD_TYPE=user
```
##### 2. Configure Realtime Permissions for the User

If the above solution does not work, you can configure realtime permissions for your user by adding them to the `realtime` group and ensuring that the system allows sufficient realtime priority.

2.1 Install `rtkit`

If [sys-auth/rtkit](https://packages.gentoo.org/packages/sys-auth/rtkit) is not already installed, install it using the following command:

`root #``emerge --ask sys-auth/rtkit`
2.2 Add Your User to the `realtime` Group

Add your user to the `realtime` group:

`root #``gpasswd -a $USER realtime`
2.3 Create or Update the Realtime Configuration File

Ensure that the system allows sufficient realtime priority by creating or editing the /etc/security/limits.d/99-realtime.conf file. Add the following lines:

**`/etc/security/limits.d/99-realtime.conf`**

This configuration grants users in the `realtime` group the ability to use high-priority scheduling and lock memory for realtime operations.

### GDM produces an all-black screen when more than one video driver is loaded

One of the benefits of Wayland is the ability to run multi-GPU multi-head sessions. It is also possible on some compositors (e.g. Sway) for users to do this with multiple GPU drivers (e.g. running a modern AMDGPU card and an older radeon card in the same machine). GDM is not a fan of this, but it can be coerced into behaving.

If this happens, a workaround is to temporarily blacklist the secondary GPU driver at load-time:

**`/etc/modprobe.d/blacklist.conf`**

Then, after GDM launches, switch to another tty and load the secondary GPU's driver. It will then be possible to switch back to GDM and start the WM, which if it can accept more than one GPU driver will utilize them.

### GDM ignores my keyboard layout

GDM normally uses systemd to get the system keyboard configuration when available. If not, GDM will use the X configuration.

To change the X configuration (e.g. to have a gb layout):

`user $``localectl --no-convert set-x11-keymap gb`
This will produce the following file: /etc/X11/xorg.conf.d/00-keyboard.conf, that should not be edited by hand, but only through the localctl command.

However, the non-systemd GNOME profile on Gentoo defaults to OpenRC and Wayland, so none of this may work. GDM will then use a default us keyboard, which will make it difficult to type a password if this is not a desired layout.

In this case, configure the keyboard layout of the gdm user. This can be done opening Gnome settings (gnome-control-center). In the top right of the *Region & Language* panel, there is a *Login Screen* button which can be toggled to configure gdm language.

Unfortunately, this *Login Screen* button does not appear on every install (why?). If this is the case, change settings through command line.

For example, for a French keyboard, type as root:

`root #``su -s /bin/bash gdm``gdm $````
dbus-run-session gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'fr')]"
```
`gdm $````
gsettings get org.gnome.desktop.input-sources sources
```
`gdm $````
# you should see [('xkb', 'fr')] now
```
`gdm $````
exit
```
If this previous solution does not work, patch *Gnome shell* so that it uses the keyboard defined in the session instead of the system one, which is as simple as creating the following file:

**`/etc/portage/patches/gnome-base/gnome-shell/greeter-keyboard.patch`**

```
diff --git a/js/ui/status/keyboard.js b/js/ui/status/keyboard.js
index 8d98e16..db41b5b 100644
--- a/js/ui/status/keyboard.js
+++ b/js/ui/status/keyboard.js
@@ -340,10 +340,7 @@ export class InputSourceManager extends Signals.EventEmitter {
                 Meta.KeyBindingFlags.IS_REVERSED,
                 Shell.ActionMode.ALL,
                 this._switchInputSource.bind(this));
-        if (Main.sessionMode.isGreeter)
-            this._settings = new InputSourceSystemSettings();
-        else
-            this._settings = new InputSourceSessionSettings();
+        this._settings = new InputSourceSessionSettings();
         this._settings.connect('input-sources-changed', this._inputSourcesChanged.bind(this));
         this._settings.connect('keyboard-options-changed', this._keyboardOptionsChanged.bind(this));
 
End of file (this line is only needed to avoid stripping whitespace above)
```
and re-install *Gnome shell*:

`root #``emerge -1 gnome-shell`
## See also

- [GNOME/Guide](https://wiki.gentoo.org/wiki/GNOME/Guide) — attempts to describe all aspects of GNOME, including installation, configuration, and usage.

## External resources

- [https://help.gnome.org/admin/gdm/stable/](https://help.gnome.org/admin/gdm/stable/) - Upstream's administration guide
