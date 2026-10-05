<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/RISC-V | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/RISC-V -->
---
title: QEMU/Guest/RISC-V
url: https://wiki.gentoo.org/wiki/QEMU/Guest/RISC-V
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: bb55dd11247ada6c
license: CC BY-SA 4.0
---

# QEMU/Guest/RISC-V

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Virtual Machine

This articles describes how to running a ARCH=riscv Gentoo Linux system on a QEMU virtual machine(VM).

#### QEMU

Install **QEMU** virtual machine. We need to enable `QEMU_USER_TARGETS` and `QEMU_SOFTMMU_TARGETS`. To do this, we can simply edit the variables globally in /etc/portage/make.conf, i.e.:

**`/etc/portage/package.use/qemu`**

```
 QEMU_SOFTMMU_TARGETS: riscv32 riscv64
app-emulation/qemu QEMU_USER_TARGETS: riscv32 riscv64
```
`root #``emerge --ask app-emulation/qemu`
#### Firmware

Install U-boot firmware which is used by qemu when bringing up a RISC-V guest virtual machine

`root #``eselect repository enable riscv``root #``emaint sync -r riscv``root #``emerge --ask sys-firmware/u-boot-bin`
#### Image

`root #``emerge --ask sys-block/gparted sys-apps/gptfdisk sys-fs/e2fsprogs`
Prepare disk partition layout and also mount to temporarily directories

**`do_prepare.sh`**

```
#!/bin/bash
UUID=${1:-d07fccbb-c09d-4b9e-af90-9433251ac3c7}
IMG=/var/lib/libvirt/images/riscv64_vm.img
# 1G Size in sectors
GB_SECTOR=$((1024*1024*1024/512))
# Size in GB
DISK_SZ=10
# Num of sectors, 1sector=512bytes
DISK_RESV=4096
DRY=
# Resev 1GB for boot
BOOT_END=$((DISK_RESV + 1 * GB_SECTOR - 1))
ROOT_START=$((BOOT_END + 1))
ROOT_END=$((DISK_SZ * GB_SECTOR - 2 * DISK_RESV - 1))
${DRY} mkdir -p /var/lib/libvirt/images
echo "==> Create image disk.."
${DRY} dd if=/dev/zero of=${IMG} bs=1G count=$((DISK_SZ))
echo "==> Partition disk.."
${DRY} sgdisk -Z ${IMG}
${DRY} sgdisk -n 1:${DISK_RESV}:${BOOT_END} ${IMG}
${DRY} sgdisk -n 2:${ROOT_START}:${ROOT_END} ${IMG}
${DRY} sgdisk -p ${IMG}
echo "==> Setup losetup disk.."
BOOT_DEV=$(losetup -f)
${DRY} losetup --offset=$((DISK_RESV * 512)) --sizelimit=$(((ROOT_START - DISK_RESV) * 512)) ${BOOT_DEV} ${IMG}
ROOT_DEV=$(losetup -f)
${DRY} losetup --offset=$((ROOT_START * 512)) --sizelimit=$(((ROOT_END - BOOT_END) * 512)) ${ROOT_DEV} ${IMG}
echo "==> Format disk.."
sleep 5
${DRY}  mkfs.ext4 ${BOOT_DEV}
sleep 15
${DRY} partprobe ${IMG}
${DRY} mkfs.ext4 -U ${UUID} ${ROOT_DEV}
echo "==> Mount disk.."
${DRY} mkdir -p /tmp/gentoo/{root,boot}
${DRY} mount ${BOOT_DEV} /tmp/gentoo/boot
${DRY} mount ${ROOT_DEV} /tmp/gentoo/root
```
`root #``export UUID="$(uuidgen)"`
Download the script for your convenience

`root #``sh do_prepare.sh ${UUID}` Download riscv stage3 image, and unpack it

`root #``tar Jxf stage3-rv64_lp64d-systemd-20220826T170537Z.tar.xz -C /tmp/gentoo/root`
Download and use prebuilt kernel/ramdisk/modules

`root #``tar Jxf boot_kernel.tar.xz -C /tmp/gentoo/boot``root #``tar Jxf root_modules.tar.xz -C /tmp/gentoo/root`
If you need to additionally customize the kernel boot arguments, please adjust "append" parameters

**`extlinux/extlinux.conf`**

```
 Gentoo
label Gentoo
   linux /gentoo/6.0.0/vmlinuz-6.0.7-riscv64
   initrd /gentoo/6.0.0/initramfs-6.0.7-riscv64.img
   append real_root=UUID=d07fccbb-c09d-4b9e-af90-9433251ac3c7 ipv6.disable=1
```
Fix the rootfs's UUID in extlinux/extlinux.conf

`root #``sed -i -e "s@UUID=.* @UUID=${UUID} @g" /tmp/gentoo/boot/extlinux/extlinux.conf`
Delete root password, this will enable you to login without password

`root #``sed -i -e "s@root:x:@root::@g" /tmp/gentoo/root/etc/passwd`
Unmount all directories

`root #``umount /tmp/gentoo/{root,boot}` Release all loop devices

`root #``losetup -D`
#### Libvirt

Install the libvirt tool

`root #``emerge --ask app-emulation/libvirt`
Enable and start libvirtd

`root #``systemctl enable --now libvirtd`

Example of Libvirt VM configuration

**`riscv64_vm.xml`**

```
<domain type='qemu' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
  <name>riscv64_vm</name>
  <memory unit='KiB'>4194304</memory>
  <currentMemory unit='KiB'>4194304</currentMemory>
  <vcpu placement='static'>8</vcpu>
  <resource>
    <partition>/machine</partition>
  </resource>
  <os>
    <type arch='riscv64' machine='virt'>hvm</type>
    <kernel>/usr/share/u-boot-bin/u-boot-bin-riscv64-generic.bin</kernel>
    <boot dev='hd'/>
  </os>
  <clock offset='utc'/>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <devices>
    <emulator>/usr/bin/qemu-system-riscv64</emulator>
    <disk type='file' device='disk'>
      <driver name='qemu' type='raw' queues='4'/>
      <source file='/var/lib/libvirt/images/riscv64_vm.img'/>
      <backingStore/>
      <target dev='sda' bus='virtio'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x07' function='0x0'/>
    </disk>
    <controller type='usb' index='0' model='qemu-xhci' ports='15'>
      <address type='pci' domain='0x0000' bus='0x02' slot='0x00' function='0x0'/>
    </controller>
    <controller type='pci' index='0' model='pcie-root'/>
    <controller type='pci' index='1' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='1' port='0x8'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x0' multifunction='on'/>
    </controller>
    <controller type='pci' index='2' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='2' port='0x9'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x1'/>
    </controller>
    <controller type='pci' index='3' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='3' port='0xa'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x2'/>
    </controller>
    <controller type='pci' index='4' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='4' port='0xb'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x3'/>
    </controller>
    <controller type='pci' index='5' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='5' port='0xc'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x4'/>
    </controller>
    <controller type='pci' index='6' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='6' port='0xd'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x5'/>
    </controller>
    <controller type='pci' index='7' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='7' port='0xe'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x6'/>
    </controller>
    <controller type='pci' index='8' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='8' port='0xf'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x7'/>
    </controller>
    <controller type='pci' index='9' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='9' port='0x18'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
    </controller>
    <controller type='virtio-serial' index='0'>
      <address type='pci' domain='0x0000' bus='0x03' slot='0x00' function='0x0'/>
    </controller>
    <interface type='bridge'>
      <mac address='52:54:00:12:34:56'/>
      <source bridge='virbr0'/>
      <model type='virtio'/>
      <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    </interface>
    <serial type='pty'>
      <target type='system-serial' port='0'>
        <model name='16550a'/>
      </target>
    </serial>
    <console type='pty'>
      <target type='serial' port='0'/>
    </console>
    <channel type='unix'>
      <target type='virtio' name='org.qemu.guest_agent.0'/>
      <address type='virtio-serial' controller='0' bus='0' port='1'/>
    </channel>
    <input type='mouse' bus='usb'>
      <address type='usb' bus='0' port='1'/>
    </input>
    <input type='keyboard' bus='usb'>
      <address type='usb' bus='0' port='2'/>
    </input>
    <graphics type='vnc' port='-1' autoport='yes' listen='0.0.0.0'>
      <listen type='address' address='0.0.0.0'/>
    </graphics>
    <sound model='ac97'>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x09' function='0x0'/>
    </sound>
    <audio id='1' type='none'/>
    <video>
      <model type='vga' vram='131072' heads='1' primary='yes'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x0'/>
    </video>
    <memballoon model='virtio'>
      <address type='pci' domain='0x0000' bus='0x05' slot='0x00' function='0x0'/>
    </memballoon>
    <rng model='virtio'>
      <backend model='random'>/dev/urandom</backend>
      <address type='pci' domain='0x0000' bus='0x06' slot='0x00' function='0x0'/>
    </rng>
  </devices>
  <seclabel type='dynamic' model='dac' relabel='yes'/>
</domain>
```
Enable additional RISC-V ISA, example here will enable the vector extension:

**`riscv64_vm.xml`**

```
<qemu:commandline>
    <qemu:arg value='-cpu'/>
    <qemu:arg value='rv64,v=true,vlen=256,elen=64'/>
  </qemu:commandline>
```
Create the riscv guest VM:

`root #``virsh define riscv64_vm.xml`
Start the riscv guest VM:

`root #``virsh start --console riscv64_vm`
Once this is done, you should see a riscv64 VM running, kernel booting, and finally got your familiar Gentoo messages..
