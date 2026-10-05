<!-- source: https://wiki.gentoo.org/wiki/Custom_Initramfs/Examples | group: Gentoo Wiki (Main) | wiki-title: Custom Initramfs/Examples -->
---
title: Custom Initramfs/Examples
url: https://wiki.gentoo.org/wiki/Custom_Initramfs/Examples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-04-05"
fingerprint: "130b541fa56713ec"
license: CC BY-SA 4.0
---

# Custom Initramfs/Examples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article contains fully functional [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) **/init** scripts. If you have made something which you feel is worth showing off, please add it here.

## Multiple RAID, LUKS containers, Encrypted Keyfiles, LVM

**`/usr/src/initramfs/init`**

**Multiple RAID, LUKS containers, Encrypted Keyfiles, LVM**

```
#!/bin/busybox sh
rescue_shell() {
    echo "Something went wrong. Dropping you to a shell."
    busybox --install -s
    exec /bin/sh
}
# Prepare
mount -t devtmpfs none /dev
mount -t proc none /proc
mount -t sysfs none /sys
echo 0 > /proc/sys/kernel/printk
# Assemble RAID:
( sleep 2 # disk not ready?
  mdadm --assemble --scan
  sleep 2
) &
# Unlock Key
cryptsetup luksOpen --header /root/key.luks /root/key KEY
wait # for mdadm
# Unlock SSD
cryptsetup luksOpen --allow-discards --key-file=/dev/mapper/KEY --keyfile-offset=0 --keyfile-size=512 \
    $(findfs UUID="a140ee97-793a-4f69-a591-40cfdf520e25") luksSSD1 &
# Unlock HDD
for i in 1 2 3 4 5 6 7 8
do
    cryptsetup luksOpen --key-file=/dev/mapper/KEY --keyfile-offset=$(($i*512)) --keyfile-size=512 /dev/md"$i" luksHDD"$i" &
done
wait # for cryptsetup
# LVM
lvm lvchange -a y SSD/root
# Mount Root
mount -o ro $(findfs UUID="fa15678f-7e7e-4a47-8ed2-7cea7a5d037d") /mnt/root || rescue_shell 
# Clean up
cryptsetup luksClose KEY
echo 1 > /proc/sys/kernel/printk
umount /dev /proc /sys
# Switcheroo
exec switch_root /mnt/root /sbin/init
```
## Self-Decrypting Server

This is an example for an encrypted server which produces its own key based on hardware data such as CPU, RAM, MAC-Address, a random /root/secret file, etc… That way the machine can reboot (after power loss) without any user interaction whatsoever and still offer some protection against HDD theft. Since such a key is prone to changes, the Initramfs will install the current key if a standard passphrase (foobar) is valid.

**`/usr/src/initramfs/init`**

**Self-Decrypting Server**

```
#!/bin/busybox sh
# Mount the /proc and /sys filesystems.
mount -t devtmpfs none /dev
mount -t proc none /proc
mount -t sysfs none /sys
# Produce key without newlines. (Adapt to your liking.)
( 
    # CPU:
    grep -vE '(MHz|bogomips)' /proc/cpuinfo
    # RAM:
    tail /proc/iomem
    # MAC-Address: (requires network drivers)
    cat /sys/class/net/*/address
    # Block devices and partitions (ignore optional CD drive):
    grep -v sr0 /proc/partitions
    # Random file:
    cat /root/secret
) | sha512sum | xargs echo -n > /root/key
# Reset key. (if applicable)
echo -n foobar | cryptsetup luksChangeKey /dev/sda3 /root/key
# Unlock LUKS container with key.
cryptsetup --key-file=/root/key luksOpen /dev/sda3 lukssda3
# Mount root.
mount -o ro /dev/mapper/lukssda3 /mnt/root
# Clean up.
shred /root/key
umount /proc /sys /dev
# Boot the real thing.
exec switch_root /mnt/root /sbin/init
```
## LUKS, LVM, Resume from Hibernate, Script to Build the Initramfs

The following script will (re)build an initramfs from scratch by copying the required files and all dependencies to the initramfs. An **/init** script is included as a here document.
An unencrypted keyfile is used to decrypt the root partition without user input.

This initramfs is intended for a setup where /boot is encrypted (LVM inside Luks). grub2 can boot from this. If your boot parition is not encrypted, simply remove anything related to crypto\_key.bin.

**`build-initramfs.sh`**

**Script to Build the Initramfs**

```
#!/bin/bash
INITRAMFS_DIR='/usr/src/initramfs'
# default values, may be overwritten on the kernel command line
CRYPT_ROOT=/dev/sda2
ROOT=/dev/vg/root
RESUME=/dev/vg/swap
# needed to make parsing outputs more reliable
export LC_ALL=C
echo_debug() {
	# remove the next line to enable debug output
	return
	echo -ne "\033[01;32m" # green
	echo $@ >&2
	echo -ne "\033[00m"
}
echo_warn() {
	echo -ne "\033[01;33m" # yellow
	echo $@ >&2
	echo -ne "\033[00m"
}
echo_error() {
	echo -ne "\033[01;31m" # red
	echo $@ >&2
	echo -ne "\033[00m"
	exit 1
}
copy() {
	# Usage:
	# copy <source path> [<destination path>]
	# <source path> can be a file, symlink, device node, ...
	# <destination path> can be a directory or file, relative to $INITRAMFS_DIR
	#  if <destination path> is omitted, the base directory from <source path> is used
	echo_debug "copy $@"
	src=$1
	dst=$2
	if [ -z "${dst}" ]; then
		# $dst is not given, use the base directory of $src
		dst="$(dirname -- "$1")/"
	fi
	# check if the file will be copied into the initrd root
	# realpath will remove trailing / that are needed later on...
	add_slash=false
	if [ "${dst%/}" != "${dst}" ]; then
		# $dst has a trailing /
		add_slash=true
	fi
	dst="$(realpath --canonicalize-missing -- ${INITRAMFS_DIR}/${dst})"
	${add_slash} && dst="${dst}/"
	# check if $src exists
	if [ ! -e "${src}" ]; then
		echo_warn "Cannot copy '${src}'. File not found. Skipping."
		return
	fi
	# check if the destination is really inside ${INITRAMFS_DIR}
	if [ "${dst}" = "${dst##${INITRAMFS_DIR}}" ]; then
		echo_warn "Invalid destination $2 for $1. Skipping."
		return
	fi
	# check if the destination is a file or a directory and 
	# if it already exists
	if [ -e "${dst}" ]; then
		# $dst exists, but that's ok if it is a directory
		if [ -d "${dst}" ]; then
			# $dst is an existing directory
			dst_dir="${dst}"
			if [ -e "${dst_dir}/$(basename -- "${src}")" ]; then
				# the file exists in the destination directory, silently skip it
				echo_debug "Target file exists, skiping."
				return
			fi
		else
			# $dst exists, but it's not a directory, silently skip it
			echo_debug "Target file exists, skiping."
			return
		fi
	else
		if [ "${dst%/}" != "${dst}" ]; then
			# $dst ends in a /, so it must be a directory
			dst_dir="$dst"
		else
			# probably a file
			dst_dir="$(dirname -- "${dst}")"
		fi
		# make sure that the destination directory exists
		mkdir -p -- "${dst_dir}"
	fi
	# copy the file
	echo_debug "cp -a ${src} ${dst}"
	cp -a "${src}" "${dst}" || echo_error "Error: Could not copy ${src}"
	if [ -h "${src}" ]; then
		# $src is a symlink, follow it
		link_target="$(readlink -- "${src}")"
		if [ "${link_target#/}" = "${link_target}" ]; then
			# relative link, make it absolute
			link_target="$(dirname -- "${src}")/${link_target}"
		fi
		# get the canonical path, i.e. without any ../ and such stuff
		link_target="$(realpath --no-symlink -- "${link_target}")"
		echo_debug "Following symlink to $link_target"
		copy "${link_target}"
	elif [ -f "${src}" ]; then
		mime_type="$(file --brief --mime-type -- "${src}")"
		if [ "${mime_type}" = "application/x-sharedlib" ] || \
		   [ "${mime_type}" = "application/x-executable" ] || \
		   [ "${mime_type}" = "application/x-pie-executable" ]; then
			# $src may be dynamically linked, copy the dependencies
			# lddtree -l prints $src as the first line, skip it
			lddtree -l "${src}" | tail -n +2 | while read file; do
				echo_debug "Recursing to dependency $file"
				copy "${file}"
			done
		fi
	fi
}
if grep -q /boot /proc/mounts; then
	umount_boot=false
else
	mount /boot || echo_error "Error: Could not mount /boot"
	umount_boot=true
fi
rm -rf "${INITRAMFS_DIR}"
mkdir -p -- "${INITRAMFS_DIR}/"{bin,dev,etc,lib,lib64,mnt/root,proc,root,sbin,sys}
copy /dev/console /dev/
copy /dev/null /dev/
copy /bin/busybox /bin/
# add symlinks
for applet in $(/bin/busybox --list | grep -Fxv busybox); do
	if [ -e "/sbin/${applet}" ]; then
		ln -s /bin/busybox "${INITRAMFS_DIR}/sbin/${applet}"
	else
		ln -s /bin/busybox "${INITRAMFS_DIR}/bin/${applet}"
	fi
done
copy /boot/uk.bkeymap /keymap
# add cryptsetup
copy /sbin/cryptsetup /sbin/
copy /boot/crypto_key.bin /crypto_key.bin
# add lvm
# try the static version first
copy /sbin/lvm.static /sbin/lvm
copy /sbin/lvm /sbin/lvm
# add /init
cat << EOF > "${INITRAMFS_DIR}/init"
#!/bin/sh
rescue_shell() {
	printf '\e[1;31m' # bold red foreground
	printf "\$1 Dropping you to a shell."
	printf "\e[00m\n" # normal colour foreground
	# load the keymap
	[ -f /keymap ] && loadkmap < /keymap
	exec setsid cttyhack /bin/sh
}
# initialise
mount -t proc none /proc || rescue_shell "mount /proc failed."
mount -t sysfs none /sys || rescue_shell "mount /sys failed."
mount -t devtmpfs none /dev || rescue_shell "mount /dev failed."
# set hardcoded default values
crypt_root="${CRYPT_ROOT}"
root="${ROOT}"
resume="${RESUME}"
mount_ro_rw='ro'
# parse kernel command line
for p in \$(cat /proc/cmdline); do
	case "\${p}" in
		crypt_root=*)
			crypt_root="\${p#*=}"
			;;
		root=*)
			root="\${p#*=}"
			;;
		resume=*)
			resume="\${p#*=}"
			;;
		ro|rw)
			mount_ro_rw="\${p}"
			;;
	esac
done
# decrypt
#  convert UUID or LABEL to device node
crypt_root="\$(findfs "\${crypt_root}")"
#  decryption is first tried using the key file /crypto_key.bin
#  if this fails, prompt for a password
cryptsetup open "\${crypt_root}" lvm --type luks --key-file /crypto_key.bin || \
  cryptsetup open "\${crypt_root}" lvm --type luks || \
  rescue_shell "Decryption failed."
# activate lvm
#  create /dev/mapper/control
lvm vgscan --mknodes || rescue_shell "vgscan failed."
#  activate all LVM volumes
lvm vgchange --sysinit -a ly || rescue_shell "vgchange failed."
#  create device nodes for the volumes
lvm vgscan --mknodes || rescue_shell "vgscan failed."
# enable resume from hibernate
if [ -n "\${resume}" ]; then
	# copy the major:minor of the swap block device into /sys/power/resume
	printf '%u:%u\\n' \$(stat -L -c '0x%t 0x%T' "\${resume}") > /sys/power/resume || \
	  rescue_shell "Activating resume failed."
fi
# mount the real root
#  convert UUID or LABEL to device node
root="\$(findfs "\${root}")"
mount -o "\${mount_ro_rw}" "\${root}" /mnt/root || rescue_shell "mount \${root} failed."
# clean up
umount /proc
umount /sys
umount /dev
# boot the real system
exec switch_root /mnt/root /sbin/init
EOF
chmod +x "${INITRAMFS_DIR}/init"
# now build the image file
cd "${INITRAMFS_DIR}"
find . -print0 | cpio --null -ov --format=newc | gzip -9 > ../initramfs.img
# set restrictive permissions, it contains a decryption key
chmod 400 ../initramfs.img
${umount_boot} && umount /boot
```
## Simple initramfs for unlocking LUKS encrypted root remotely over SSH

This is a very basic script that creates and installs an initramfs that unlocks LUKS encrypted root remotely over SSH. It's an idempotent script that could be run every time the initramfs needs to be re-created, or when the kernel upgrades as it takes the current kernel (one linked under **/usr/src/linux/**) version for the initramfs filename.

The script is simplistic and takes a passphrase, but it could be modified to use keyfiles for better security.

**`/bin/mkinitramfs`**

**Simple initramfs for unlocking LUKS encrypted root remotely over SSH**

```
#!/bin/bash
set -e
# These defaults should rarely change between machines so they're coded here instead of
# taking values from the kernel command line. You might wish to use a nonstandard SSH port, tho'.
# We use "root" for the root mapper to be consistent with genkernel's implementation that
# unlocks the root into /dev/mapper/root, but really it's arbitrary and could be anything
NET_NIC="eth0"
SSH_PORT="22"
MAPPER="root"
# Add dynamic lib'ed binaries we require for the initramfs. Here we include btrfs as an example
# though this script's generated init doesn't run btrfs device scan
BINS="/sbin/cryptsetup /sbin/btrfs"
# The initramfs root directory
RD=$(mktemp -d)
# Prepare basic structure and copy basic files and bins
# This assumes dropbear and busybox are built static
mkdir -p ${RD}/{bin,dev,etc/dropbear,lib64,mnt/root,proc,root/.ssh,sys,usr/sbin,var/log,var/run}
cp -a /bin/busybox ${RD}/bin/busybox
cp -a /usr/sbin/dropbear ${RD}/usr/sbin/dropbear
cp -a /etc/localtime ${RD}/etc/
# Copy the authorized keys for your regular user you administrate with (here root for example)
cp /root/.ssh/authorized_keys ${RD}/root/.ssh/
# Copy OpenSSH's host keys to keep both initramfs' and regular ssh signed the same
# otherwise openssh clients will see different host keys and chicken out. Here we only copy the
# ecdsa host key, because ecdsa is default with OpenSSH. For RSA and others, copy adequate keyfile.
dropbearconvert openssh dropbear /etc/ssh/ssh_host_ecdsa_key ${RD}/etc/dropbear/dropbear_ecdsa_host_key
# These two libs are needed for dropbear, even if it's built statically, because we don't use PAM
# and dropbear uses libnss to find user to authenticate against
cp -L /lib64/libnss_compat.so.2 ${RD}/lib64/
cp -L /lib64/libnss_files.so.2 ${RD}/lib64/
# Basic system defaults
echo "root:x:0:0:root:/root:/bin/sh" > ${RD}/etc/passwd
echo "root:*:::::::" > ${RD}/etc/shadow
echo "root:x:0:root" > ${RD}/etc/group
echo "/bin/sh" > ${RD}/etc/shells
chmod 640 ${RD}/etc/shadow
cat << EOF > ${RD}/etc/nsswitch.conf
passwd:	files
shadow:	files
group:	files
EOF
# For each of the non-static binary listed above, copy the required libs
# Note: lddtree belongs to app-misc/pax-utils the newer versions of which
# can automatically copy the whole lib tree with --copy-to-tree option
for B in $BINS; do
	for L in $(lddtree -l $B); do
		DIR=$(dirname $L)
		mkdir -p ${RD}${DIR}
		cp -L $L ${RD}${L}
	done
done
# The main init script
cat << EOF > ${RD}/init
#!/bin/busybox sh
/bin/busybox mkdir -p /usr/sbin /usr/bin /sbin /bin
/bin/busybox --install -s
touch /var/log/lastlog
mount -t devtmpfs none /dev
mount -t proc proc /proc
mount -t sysfs none /sys
# Root partition and networking could be different between machines so take those configs from the 
# kernel command line
for x in \$(cat /proc/cmdline); do
	case "\${x}" in
		crypt_root=*)
			CRYPT_ROOT=\${x#*=}
		;;
		net_ipv4=*)
			NET_IPv4=\${x#*=}
		;;
		net_gw=*)
			NET_GW=\${x#*=}
		;;
	esac
done
# Bootstrap the network
ifconfig ${NET_NIC} \${NET_IPv4}
route add default gw \${NET_GW}
# Start dropbear sshd
/usr/sbin/dropbear -s -g -p $SSH_PORT -B
sleep 1
clear
echo "Waiting for root unlock..."
# Wait for the unlocked root mapper to appear
while [ ! -e /dev/mapper/${MAPPER} ]; do
	sleep 1
done
mount -o ro /dev/mapper/${MAPPER} /mnt/root
# We kill dropbear or else it will remain in memory and OpenSSH won't be able to bind to the same port!
killall dropbear
umount /sys
umount /proc
umount /dev
# Off we go!
exec switch_root /mnt/root /sbin/init
EOF
# A very simplistic unlocking helper script that takes a passphrase
# This could be adjusted to take keys for better security
cat << EOF > ${RD}/root/unlock
#!/bin/sh
if [ -z "\$1" ]; then
	echo "Invalid passphrase"
	exit 1
fi
for x in \$(cat /proc/cmdline); do
	case "\${x}" in 
		crypt_root=*)
			CRYPT_ROOT=\${x#*=}
		;;
	esac
done
echo -n "\$1" | /sbin/cryptsetup luksOpen \${CRYPT_ROOT} ${MAPPER}
EOF
chmod +x ${RD}/init
chmod +x ${RD}/root/unlock
# Get kernel version from current src Makefile (stolen from genkernel and modified)
KERN_VER=`grep ^VERSION\ \= /usr/src/linux/Makefile | awk '{ print $3 };'`
KERN_PAT=`grep ^PATCHLEVEL\ \= /usr/src/linux/Makefile | awk '{ print $3 };'`
KERN_SUB=`grep ^SUBLEVEL\ \= /usr/src/linux/Makefile | awk '{ print $3 };'`
KERN_EXV=`grep ^EXTRAVERSION\ \= /usr/src/linux/Makefile | sed -e "s/EXTRAVERSION =//" -e "s/ //g"`
RD_FILE="initramfs-${KERN_VER}.${KERN_PAT}.${KERN_SUB}${KERN_EXV}.img"
# Finally build the initramfs cpio from our RD root, and remove the dir
pushd ${RD} > /dev/null
find . -print0 | cpio --null -ov --format=newc | gzip -9 > /boot/${RD_FILE}
popd > /dev/null
rm -rf ${RD}
```
