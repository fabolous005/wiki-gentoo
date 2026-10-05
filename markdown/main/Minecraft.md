<!-- source: https://wiki.gentoo.org/wiki/Minecraft | group: Gentoo Wiki (Main) | wiki-title: Minecraft -->
---
title: Minecraft
url: https://wiki.gentoo.org/wiki/Minecraft
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-12"
fingerprint: fd88d35bd091bcd5
license: CC BY-SA 4.0
---

# Minecraft

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Article status**

- Add info about modded clients and servers (Forge, Fabric etc.)
- Add info about other popular server cores (Paper, Sponge etc.), alternative implementations (Minestom, Glowstone etc.) etc.
- Add optimizations and tips specifically for Gentoo users

**Minecraft** is a sandbox game developed by Mojang. The game was developed in [Java](https://wiki.gentoo.org/wiki/Java) by creator Markus "Notch" Persson and has been ported to several platforms throughout its development lifecycle. Since the public early alpha release in 2009, the game has sold over 300 million copies making it one of the best selling video games of all time.

## Clients

Minecraft can be installed using any number of available launchers. While some launchers can be compiled from source, it is recommended to install the package using Portage as this ensures the correct Java builds are installed and maintained on the system.

Following list includes applications than can launch official Minecraft client implementation. However, there are also exist alternative client implementations. Find a list of these on [https://wiki.vg/Client\_List](https://wiki.vg/Client_List).

### Official launcher

To install the official Minecraft launcher ([games-action/minecraft-launcher](https://packages.gentoo.org/packages/games-action/minecraft-launcher)), run:

`root #``emerge --ask games-action/minecraft-launcher`
### Prism Launcher

Free/libre software launcher for Minecraft with support for multiple instances, managing modpacks and mods etc.

`root #``emerge --ask games-action/prismlauncher`
### musl-based system

The official launcher is linked to glibc and is therefore not really usable. Prism Launcher can be compiled and run, but it uses the LWJGL binaries provided by Mojang, which are also linked to glibc.

#### GCompat solution

It is possible to workaround this issue with the following steps:

`root #``emerge --ask media-libs/openal``root #``emerge --ask media-libs/glfw`
Add **-Dorg.lwjgl.openal.libname=/usr/lib/libopenal.so.1 -Dorg.lwjgl.glfw.libname=/usr/lib/libglfw.so** to your java arguments (or write the paths in the dedicated interface prism launcher has)

`root #``emerge --ask sys-libs/gcompat::guru`
with the `libucontext obstack` USE flags (make sure to enable the guru overlay, and use the live ebuild if point releases have issues)

Finally, you can either add **-Dorg.lwjgl.system.allocator=system** to your java arguments and enjoy

**OR (RECOMMENDED FOR PERFORMANCE)**

`root #``emerge --ask dev-libs/jemalloc`
and add **-Dorg.lwjgl.system.jemalloc.libname=/usr/lib/libjemalloc.so.2**  instead.

Jemalloc may not work on the musl-llvm profile; if that is the case, try compiling it with gcc.



#### Flatpak solution

Installing and running Prism Launcher using [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) also works:

`user $``flatpak install --user flathub org.prismlauncher.PrismLauncher`
To run Prism Launcher use the following command (it is important to provide the locale, otherwise Minecraft will not start):

`user $``flatpak run --env=LC_ALL=en_US.UTF-8 org.prismlauncher.PrismLauncher`
## Servers

### Official server

To install the official Minecraft server ([games-server/minecraft-server](https://packages.gentoo.org/packages/games-server/minecraft-server)), run:

`root #``emerge --ask games-server/minecraft-server`
### Other servers (PaperMC,Spigot,Bukkit,Sponge,Etc.)

For this example we'll use PaperMC, Although most alternative server software will work in the same way.

#### Check your Java version

The java versions you'll need are detailed in this table.

| Java version table for Minecraft Java Edition |  | 
|---|---|
| Minecraft Version | Java version required | 
|---|---|
| rd-132211 - 1.5.2 | Java 5 (1.5.0) or newer | 
| 1.6.1 (13w16a) - 1.11.2 (1.12: 17w06a) | Java 6 (1.6.0) or newer | 
| 1.12 (17w13a) - 1.16.5 (1.17: 21w18a) | Java 8 (1.8.0) or newer | 
| 1.17 (21w19a) to 1.17.1 (1.18: 1.18 Pre-release 1) | Java 16 or newer | 
| 1.18 (1.18 Pre-release 2) - 1.20.4 (1.20.5: 24w14potato) | Java 17 or newer | 
| 1.20.5 (24w14a) - current | Java 21 or newer | 

#### Download the JAR

This step is pretty self-explanatory.

Simply create a new folder and throw the downloaded JAR there.



`user $``mkdir mc-server && cd mc-server``user $``mv paper-*.jar server.jar`
#### Accept the EULA

Minecraft won't let you run a server without first agreeing with the Minecraft EULA. If you accept the EULA, run the following command to accept it.

`user $``echo "eula=true" > eula.txt`
#### Run the server

Most of the alternative Minecraft server software are distributed as JAR files; These can be ran very easily with Java installed.
Make sure to customize your JVM arguments to both optimize the GC, RAM allocation, Etc.
If you don't wish to manually create a list of JVM arguments, check out this website: [https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/](https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/)

`user $``java <JVM Arguments> -jar server.jar --nogui`
## Troubleshooting

### Minecraft launcher errors

Errors with this package are uncommon, however the following have been noted:

In some instances when executing `minecraft-launcher` the following output is produced:

`user $``./minecraft-launcher`
\[0229/184549.183275:ERROR:sandbox\_linux.cc(346)\] InitializeSandbox() called with multiple threads in process gpu-process.

This can be resolved by issuing the following options in conjunction with minecraft-launcher:

`user $``MESA_GLSL_CACHE_DISABLE=true ./minecraft-launcher`
### Crash in org.lwjgl.opengl.LinuxDisplay.getAvailableDisplayModes

Older Minecraft versions (which use java 1.8, e.g playing specific modpacks) use LWJGL 2 rather than LWJGL 3. It [lacks Wayland support](https://github.com/LWJGL/lwjgl/issues/118).

If getting a crash like:

java.lang.ExceptionInInitializerError
	at azi.ad(SourceFile:284)
	at azi.f(SourceFile:688)
	at net.minecraft.client.main.Main.main(SourceFile:152)
	at org.prismlauncher.launcher.impl.StandardLauncher.launch(StandardLauncher.java:88)
	at org.prismlauncher.EntryPoint.listen(EntryPoint.java:126)
	at org.prismlauncher.EntryPoint.main(EntryPoint.java:71)
Caused by: java.lang.ArrayIndexOutOfBoundsException: 0
	at org.lwjgl.opengl.LinuxDisplay.getAvailableDisplayModes(LinuxDisplay.java:951)
	at org.lwjgl.opengl.LinuxDisplay.init(LinuxDisplay.java:738)
	at org.lwjgl.opengl.Display.\<clinit>(Display.java:138)
	... 6 more

or

RuntimeException: No OpenGL context found in the current thread.

try installing [x11-apps/xrandr](https://packages.gentoo.org/packages/x11-apps/xrandr) and/or trying an X desktop session.

### Failed to download mojang\_x.y.z.jar

If when trying to start up a Minecraft server the error `javax.net.ssl.SSLException: Unexpected error: java.security.InvalidAlgorithmParameterException: the trustAnchors parameter must be non-empty` shows up, the SSL certificates may be missing.

Try updating manually the certificates with the command:

`root #``update-ca-certificates`
And then try running the server again.

### Non-Microsoft Account

If you bought Minecraft before Mojang got bought out by Microsoft, there was a small window of time where they forced you to migrate your copy of the game to a Microsoft account or lose access to the game. There is currently a class-action lawsuit against Microsoft over this. Luckily, it is pretty easy to patch this out in multimc, polymc or prismlauncher:

`root #````
mkdir -p /etc/portage/patches/games-action/prismlauncher 
```
`root #````
 nano /etc/portage/patches/games-action/prismlauncher/remove_drm.patch
```
**`/etc/portage/patches/games-action/prismlauncher/remove_drm.patch`**

**Patching account auth**

```
diff --git a/launcher/minecraft/auth/MinecraftAccount.h b/launcher/minecraft/auth/MinecraftAccount.h
index a82d3f134..0d9aff8a4 100644
--- a/launcher/minecraft/auth/MinecraftAccount.h
+++ b/launcher/minecraft/auth/MinecraftAccount.h
@@ -116,7 +116,7 @@ class MinecraftAccount : public QObject, public Usable {
 
     AccountType accountType() const noexcept { return data.type; }
 
-    bool ownsMinecraft() const { return data.type != AccountType::Offline && data.minecraftEntitlement.ownsMinecraft; }
+    bool ownsMinecraft() const { return true; }
 
     bool hasProfile() const { return data.profileId().size() != 0; }
 
diff --git a/launcher/ui/pages/global/AccountListPage.cpp b/launcher/ui/pages/global/AccountListPage.cpp
index ff250888a..0697dd3b8 100644
--- a/launcher/ui/pages/global/AccountListPage.cpp
+++ b/launcher/ui/pages/global/AccountListPage.cpp
@@ -141,13 +141,6 @@ void AccountListPage::on_actionAddMicrosoft_triggered()
 
 void AccountListPage::on_actionAddOffline_triggered()
 {
-    if (!m_accounts->anyAccountIsValid()) {
-        QMessageBox::warning(this, tr("Error"),
-                             tr("You must add a Microsoft account that owns Minecraft before you can add an offline account."
-                                "<br><br>"
-                                "If you have lost your account you can contact Microsoft for support."));
-        return;
-    }
 
     ChooseOfflineNameDialog dialog(tr("Please enter your desired username to add your offline account."), this);
     if (dialog.exec() != QDialog::Accepted) {
```
If you are hosting a server, you can set `online-mode=false` in the server.properties configuration file to disable Microsoft account authentication of joining players. Keep in mind that in this mode, anyone can join the server with as many clients as they want; It's very easy to DDOS or grief servers with this setup, and some people just scan the whole IPV4 space for open servers like this. Offline mode is really just meant for LAN parties and such, so you should use another authentication mechanism like a plugin for a public-facing server.

## See also

- [Games](https://wiki.gentoo.org/wiki/Games) — a landing page for many of the games (especially open source variants) available in Gentoo's main ebuild repository.
- [Java](https://wiki.gentoo.org/wiki/Java) — a programming language, originally developed by [Sun Microsystems](https://en.wikipedia.org/wiki/Sun_Microsystems), which uses a platform-independent virtual machine to execute Java bytecode in real-time.
- [Luanti](https://wiki.gentoo.org/wiki/Luanti) — a voxel game engine.
