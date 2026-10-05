<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Boot_Path_Security | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Boot Path Security -->
---
title: Security Handbook/Boot Path Security
url: https://wiki.gentoo.org/wiki/Security_Handbook/Boot_Path_Security
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: a58ad9188e378f85
license: CC BY-SA 4.0
---

# Security Handbook/Boot Path Security

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section provides guidance on boot path security.


If an attacker is able to get a system to load arbitrary code they effectively have unrestricted access to the hardware. This may lead to exfiltration of unencrypted data stored on the system; in a typical handbook install /boot is unencrypted and the kernel, initramfs, or bootloader could be tampered with.

The bare minimum that can be done to mitigate against this risk is to restrict permitted boot devices and set a system firmware password to prevent modification of the boot order and firmware configuration; this will prevent an attacker from booting from removable media or a network location.

An additional, but recommended, control is the use of [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) to ensure that the system will only boot from signed EFI files. The only approved keys should be the ones used to sign the bootloader, kernel, initramfs, and modules.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| Physical Security | Boot Path | The system firmware should be configured to only boot from approved locations | 0 | 
| Physical Security | Boot Path | The system firmware configuration should be protected from unauthorised modification | 0 | 
| Physical Security | Boot Path | The system firmware should be configured to only execute a signed payload | 1 | 
| Physical Security | Boot Path | The system firmware configuration should contain only user-provided keys | 3 | 

Many bootloaders offer the ability to edit the kernel commandline, which can be used to pass parameters to the kernel. This can be used to bypass security controls such as [SELinux](https://wiki.gentoo.org/wiki/SELinux) or to boot into single-user mode.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| Physical Security | Boot Path | The system bootloader should not allow the kernel commandline to be edited without authorisation | 0 | 
| Physical Security | Boot Path | The system bootloader should be configured to execute only signed payloads | 2 | 

General configuration guidance for enforcing these controls is provided below.

The system firmware is executed early in the boot process and is typically the first code that a user is able to interact with.

It is responsible for initializing the hardware and loading the bootloader.

The firmware configuration should be protected with a password to prevent modification of the boot order and firmware configuration. The method for setting a password varies between manufacturers and models, but is typically found in the security section of the firmware configuration.

For x86 and amd64 architectures consult the manufacturer documentation for guidance on accomplishing this task.

For architectures like aarch64 and riscv that use U-Boot there are further actions that can be taken to secure the boot process. See the [U-Boot](https://wiki.gentoo.org/wiki/U-Boot) section for more information.

For UEFI implementations, [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) and [Measured Boot](https://wiki.gentoo.org/wiki/User:Ajak/Measured_Boot) may be used to ensure that the system boot path has not been tampered with.

With the greater control offered by U-Boot it is possible to harden the bootloader and secondary program loader at compile time. It is important to recognize that embedded systems are subject to their own unique set of security concerns and have historically been a target for attackers.

Once the system firmware is configured to load only an appropriate bootloader the next step is to harden the bootloader itself to prevent unauthorized modification of the boot process.

To harden [GRUB](https://wiki.gentoo.org/wiki/GRUB), first, generate a password hash using grub-mkpasswd-pbkdf2:

`root #``grub-mkpasswd-pbkdf2`
Enter password:
Reenter password:
PBKDF2 hash of your password is grub.pbkdf2.sha512.10000.abcdef...

Repeat this process for each user (or permission level) required.

Next, define any GRUB users in /etc/grub.d/40\_custom.

In this example two users are defined, root, the superuser, and larry who will only have permission to boot specific entries.

**`/etc/grub.d/40_custom`**

```
set superusers="root"
password_pbkdf2 root grub.pbkdf2.sha512.10000.aaa
password_pbkdf2 larry grub.pbkdf2.sha512.10000.ccc
```
It is often desirable for default boot entries to continue without requiring an additional password. To define an entry as unrestricted, add `--unrestricted` to each menuentry line in the /etc/grub.d/10\_linux configuration file.

This will look something like the following:

**`/etc/grub.d/10_linux`**

**Unrestricted boot entry**

```
echo "menuentry '$(echo "$title" | grub_quote)' --unrestricted ${CLASS} \$menuentry_id_option 'gnulinux-$version-$type-$boot_device_id' {" | sed "s/^/$submenu_indentation/"
```
To restrict entries to specific users (and require their password) add the `--users` option to the menuentry lines:

**`/etc/grub.d/10_linux`**

**Specific user boot entry**

```
echo "menuentry '$(echo "$title" | grub_quote)' --users larry ${CLASS} \$menuentry_id_option 'gnulinux-$version-$type-$boot_device_id' {" | sed "s/^/$submenu_indentation/"
```
Finally, regenerate grub.cfg file using the grub-mkconfig command:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
To ensure that the files used to boot the system are not tampered with it is possible for GRUB to load the kernel and initramfs from an encrypted partition:

**`/etc/default/grub`**

```
GRUB_ENABLE_CRYPTODISK=y
GRUB_PRELOAD_MODULES="cryptodisk lvm luks"
GRUB_TERMINAL_INPUT=usb_keyboard
```
GRUB's core.img can optionally provide enforcement that all files subsequently read from disk are covered by a valid digital signature.

If the GRUB environment variable `check_signatures` is set to enforce, every attempt by the GRUB core.img to load another file `foo` implicitly invokes `verify_detached foo foo.sig`. `foo.sig` must contain a valid digital signature for the contents of `foo`, which can be verified with a public key currently trusted by GRUB. If validation fails the file will not be loaded which may halt or otherwise impact the boot process.

An initial trusted public key can be embedded within the GRUB core.img using the `--pubkey` option when invoking `grub-install`:

`root #``grub-install --pubkey /path/to/key.pub /dev/sda`
GRUB uses GPG-style detached signatures (meaning that the file `foo.sig` will be produced when file `foo` is signed) and supports the DSA and RSA signing algorithms.

A signing key can be generated using the following command:

`user $``gpg --gen-key`
An individual file may be signed as follows:

`user $``gpg --detach-sign /path/to/file`
From here, each component that GRUB needs to load may be individually signed:

`root #````
for i in `find /boot -name "*.cfg" -or -name "*.lst" -or \
```
-name "*.mod" -or -name "vmlinuz*" -or -name "initrd*" -or \
 -name "grubenv"`;

do

gpg --batch --detach-sign --passphrase-fd 0 $i < \
   /dev/shm/passphrase.txt

done`root #``shred /dev/shm/passphrase.txt`
It may be more effective, however, to build a standalone GRUB image with the required modules, key, and minimal grub configuration built-in; this way only the kernel, initramfs, and on-disk grub configuration (if it is changed) need to be signed.[\[2\]](https://wiki.gentoo.org#cite_note-2)

`root #````
grub-mkstandalone --pubkey "/mnt/grub/grub.pub" --directory "/usr/lib/grub/x86_64-efi" \
```
--format "x86_64-efi" \
 --modules "pgp part_gpt fat ext2 configfile gcry_sha256 gcry_rsa password_pbkdf2 normal linux all_video search search_fs_uuid reboot sleep loadenv minicmd test echo font" \
 --disable-shim-lock --output "/boot/EFI/gentoo/grubx64.efi" "/boot/grub/grub.cfg=/etc/default/grub-signed.cfg" \

"/boot/grub/grub.cfg.sig=/etc/default/grub-signed.cfg.sig"
With the following grub configuration (used to load the on-disk grub config):

**`/etc/default/grub-signed.cfg`**

```
set check_signatures=enforce
export check_signatures
set superusers="root"
export superusers
password_pbkdf2 root grub.pbkdf2.sha512.10000.aaa
set root=(memdisk)
set prefix=$(root)/grub
search --no-floppy --fs-uuid --set=root 7DF7-8065
configfile /grub/grub.cfg
echo The on-disk grub.cfg did not boot the system and instead returned to grub-signed.cfg.
echo Exiting in 10 seconds.
sleep 10
exit
```
This may be automated (and combined with Secure Boot signing) using a script similar to the following:

**`/usr/bin/sign-installed-kernels`**

```
#!/bin/bash
for image in /boot/vmlinuz-*-x86_64 /boot/initramfs*.zstd
do
   modified=`date -r $image`
   read -p "Do you want to sign $image, last modified on $modified? (y/n)" yn
   case $yn in
      [yY] ) gpg --verbose --homedir=/mnt/grub --pinentry-mode=ask -b $image || exit;;
      *    ) echo "Skipping $image"
esac
done
echo "Generating GRUB image..."
grub-mkstandalone --pubkey "/mnt/grub/grub.pub" --directory "/usr/lib/grub/x86_64-efi" --format "x86_64-efi" --modules "pgp part_gpt fat ext2 configfile gcry_sha256 gcry_rsa password_pbkdf2 normal linux all_video search search_fs_uuid reboot sleep loadenv minicmd test echo font" --disable-shim-lock --output "/boot/EFI/gentoo/grubx64.efi" "/boot/grub/grub.cfg=/etc/default/grub-signed.cfg" "/boot/grub/grub.cfg.sig=/etc/default/grub-signed.cfg.sig" || exit
read -p "Do you want to sign /boot/EFI/gentoo/grubx64.efi? (y/n)" yn
case $yn in
   [yY] ) sbsign --key /mnt/efikeys/db.key --cert /etc/efikeys/db.crt -o /boot/EFI/gentoo/grubx64.efi /boot/EFI/gentoo/grubx64.efi;;
       * ) echo "NOT signing GRUB image, Secure boot will NOT work!"
esac
```
