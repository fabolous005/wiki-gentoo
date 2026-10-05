<!-- source: https://wiki.gentoo.org/wiki/QEMU/VirGL | group: Gentoo Wiki (Main) | wiki-title: QEMU/VirGL -->
---
title: QEMU/VirGL
url: https://wiki.gentoo.org/wiki/QEMU/VirGL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-22"
fingerprint: fd97993691a0f31
license: CC BY-SA 4.0
---

# QEMU/VirGL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

VirGL is a virtual 3D GPU for use inside [QEMU](https://wiki.gentoo.org/wiki/QEMU) virtual machines,
that allows the guest operating system to use the capabilities of the host GPU to accelerate 3D rendering.

## Host System

On host system [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) should be built with [opengl](https://packages.gentoo.org/useflags/opengl) [and](https://wiki.gentoo.org/wiki/USE_flag) [virgl](https://packages.gentoo.org/useflags/virgl) [USE flags.](https://wiki.gentoo.org/wiki/USE_flag)

When using [app-emulation/virt-manager](https://packages.gentoo.org/packages/app-emulation/virt-manager) Video Device model should be set to Virtio with 3D acceleration box checked.

Spice should be used for Display with Listen type set to None and with OpenGL box checked.

If using `virt-viewer` with multiple displays the above may not work, instead you can try editing the machine XML either via the Virtmanager gui or by `virsh edit` and edit the graphics and video sections to include:

```
 <graphics type='egl-headless'>                                                                                                         
  <gl rendernode='/dev/dri/renderD128'/>                                                        
 </graphics>                                                                                     
 <video>                                                                                         
    <model type='virtio' heads='2' primary='yes'>                                                 
      <acceleration accel3d='yes'/>                                                               
    </model>                                                                                                       
 </video>                       
```
Replacing `/dev/dri/renderD128` with the path to your video card. Also note that multiple heads only work when the guest is running Wayland.

## Guest System

Mesa VirGL driver should be installed on the Guest system.
On Gentoo this can be done by setting [VIDEO\_CARDS](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#VIDEO_CARDS) to `"virgl"` and ensuring that [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa)
was recompiled with this flag enabled.

To test that everything works properly, the `glxinfo` command from the [x11-apps/mesa-progs](https://packages.gentoo.org/packages/x11-apps/mesa-progs) package can be used.

An example of working output of `glxinfo  | grep 'OpenGL render'` is:

```
OpenGL renderer string: virgl (Mesa Intel(R) Iris(R) Xe Graphics (RPL-P))
```

## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
