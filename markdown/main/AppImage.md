<!-- source: https://wiki.gentoo.org/wiki/AppImage | group: Gentoo Wiki (Main) | wiki-title: AppImage -->
---
title: AppImage
url: https://wiki.gentoo.org/wiki/AppImage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-27"
fingerprint: bc00bf5a82ab299f
license: CC BY-SA 4.0
---

# AppImage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**AppImage** is portable software that do not require root permission to be run.

## Additional software

Most AppImages require [sys-fs/fuse](https://packages.gentoo.org/packages/sys-fs/fuse) on slot 0:

`user $``./ExamplePackage.AppImage`
dlopen(): error loading libfuse.so.2

AppImages require [FUSE](https://wiki.gentoo.org/wiki/Filesystem_in_Userspace) to run. You might still be able to extract the contents of this AppImage if you run it with the `--appimage-extract` option.

This error can be solved by installing [sys-fs/fuse](https://packages.gentoo.org/packages/sys-fs/fuse) in slot 0. Slot 0 has version 2 of FUSE required by most AppImages. Whereas [sys-fs/fuse](https://packages.gentoo.org/packages/sys-fs/fuse) in slot 3 which has version 3 of fuse. The highest version available is installed by default, currently in slot 3, if no slot is specified and doesn't solve the dependency problem.

`root #``emerge --ask sys-fs/fuse:0`
## Usage

### Invocation

AppImages can be run without root permission:

`user $````
chmod +x ExamplePackage.AppImage
```
`user $````
./ExamplePackage.AppImage
```
### Creation of AppImage

This guide provides instructions for creating an AppImage of ripgrep, a line-oriented search tool that recursively searches directories for a specified pattern. The AppImage allows you to run ripgrep on compatible Linux systems without the need for installation.

**Step 1: Set up your build environment**

Make sure you have the necessary tools and dependencies installed on your system, including gcc, cmake, pkg-config, and appimagetool.

**Step 2: Clone the ripgrep repository**

`root #``git clone` [https://github.com/BurntSushi/ripgrep.git](https://github.com/BurntSushi/ripgrep.git)
**Step 3: Navigate to the ripgrep directory**

`root #``cd ripgrep`
**Step 4: Build ripgrep**

`root #``cargo build --release`
**Step 5: Install the AppStream CLI (optional)**

If you want to include AppStream metadata in your AppImage, you can install the appstreamcli tool. This step is optional but recommended.

**Step 6: Create the AppDir**

Create the AppDir using appimagetool with the following command:

`root #``appimagetool --appimage-extract`
**Step 7: Copy ripgrep files**

Copy the ripgrep binary and any necessary files to the appropriate locations within the AppDir. Use the following commands:

`root #``cp target/release/rg squashfs-root/usr/bin/rg`
**Step 8: Create an AppRun file**

Create a file named AppRun within the AppDir and make it executable. Add the following content to the file:

**`AppRun`**

**AppRun**

```
#!/bin/sh
exec "$(dirname "$0")"/squashfs-root/usr/bin/rg "$@"
```
**Step 9: Set permissions**

Ensure that the AppRun file is executable by running the following command:

`root #``chmod +x squashfs-root/AppRun`
**Step 10: Generate the AppImage**

Finally, generate the AppImage using appimagetool with the following command:

`root #``appimagetool squashfs-root`
After following these steps, you will have an AppImage file named something like ripgrep-x.x.x-x86\_64.AppImage, where x.x.x represents the version number of ripgrep.

This AppImage can be distributed and run on compatible Linux systems without the need for installation.
