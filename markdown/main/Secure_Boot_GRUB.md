<!-- source: https://wiki.gentoo.org/wiki/Secure_Boot/GRUB | group: Gentoo Wiki (Main) | wiki-title: Secure Boot/GRUB -->
---
title: Secure Boot/GRUB
url: https://wiki.gentoo.org/wiki/Secure_Boot/GRUB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-14"
fingerprint: "5e03195ee2eb1f24"
license: CC BY-SA 4.0
---

# Secure Boot/GRUB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article explains how to set up [GRUB](https://wiki.gentoo.org/wiki/GRUB) to work with [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot).

## Understanding The Boot Chain

Before the installation, it's advised to understand what a **boot chain** is: what happens in between powering on a device and being prompted to login.

| The  db ,  GPG , and  openssl  keys will be explained later. |  | 
|---|---|
| Step 1 | The device powers on, starting the [UEFI](https://wiki.gentoo.org/wiki/UEFI). | 
| Step 2 | The UEFI chooses the GRUB executable as the secondary bootloader. | 
| Step 3 | The UEFI checks the signature of the GRUB executable before running it. The signature is made by the `db` private key and checked with the public key embedded in the UEFI. If the signature is good, then the UEFI runs the GRUB executable to continue the boot chain. | 
| Step 4 | The GRUB executable searches for external GRUB files; these files include the GRUB configuration file, GRUB modules, and other GRUB-related files required to boot; GRUB also searches for [Microcodes](https://wiki.gentoo.org/wiki/Microcode), [Initramfses](https://wiki.gentoo.org/wiki/Initramfs), [Kernels](https://wiki.gentoo.org/wiki/Kernel), and any other files required to boot. | 
| Step 5 | GRUB checks the signatures of all gathered files before they are loaded/ran. The signatures are made by the `GPG` private key and checked with the public key embedded in the GRUB executable. If all the signatures are good, then the files can be loaded/ran and the boot chain can continue. | 
| Step 6 | This is the step where the kernel *would* run and do its thing, but there is something that needs to happen first: the UEFI needs to check the signature of the kernel; this is because the kernel itself is an executable, and the UEFI will refuse to run anything not signed with the `db` key. This means that the kernel is a special file that needs to be signed by **two** keys: the `GPG` key, and the `db` key. If the UEFI determines the kernel's signature to be good, the boot chain can continue. | 
| Step 7 | The kernel searches for any external modules required to boot. | 
| Step 8 | The kernel checks the signatures of the modules before they are loaded. The signatures are made by the kernel's private key and checked with the public key embedded in the kernel. This key can be made automatically via the kernel itself or manually via `openssl`. If the modules' signatures are good, the boot chain can continue. | 
| Step 9 | The kernel starts the [Init system](https://wiki.gentoo.org/wiki/Init_system). | 

`db`,

`GPG`, and

`openssl`keys will be explained later.

## Installation

### Kernel

This guide will cover a manually configured and compiled kernel using the [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) package.

`root #``emerge --ask sys-kernel/gentoo-sources`
### Installkernel

The [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) package will be used in this guide to automatically generate the [initramfs](https://wiki.gentoo.org/wiki/Initramfs) with [Dracut](https://wiki.gentoo.org/wiki/Dracut) and the [GRUB](https://wiki.gentoo.org/wiki/GRUB) configuration. To do this, the USE flags `dracut` and `grub` should be installed before emerging **installkernel**.

**`/etc/portage/package.use/installkernel`**

**Set USE flags**

`root #``emerge --ask sys-kernel/installkernel`

| [dracut](https://packages.gentoo.org/useflags/dracut) | Generate an initramfs or UKI on each kernel installation | 
| [efistub](https://packages.gentoo.org/useflags/efistub) | EXPERIMENTAL: Update UEFI configuration on each kernel installation | 
| [grub](https://packages.gentoo.org/useflags/grub) | Re-generate grub.cfg on each kernel installation, used grub.cfg is overridable with GRUB\_CFG env var | 
| [refind](https://packages.gentoo.org/useflags/refind) | Install a Gentoo icon for rEFInd alongside the (unified) kernel image, used icon is overridable with REFIND\_ICON env var | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Use systemd's kernel-install to install kernels, overridable with SYSTEMD\_KERNEL\_INSTALL env var | 
| [systemd-boot](https://packages.gentoo.org/useflags/systemd-boot) | Use systemd-boot's native layout by default | 
| [ugrd](https://packages.gentoo.org/useflags/ugrd) | Generate an initramfs using UGRD on each kernel installation | 
| [uki](https://packages.gentoo.org/useflags/uki) | Install UKIs to ESP/EFI/Linux for EFI stub booting and/or bootloaders with support for auto-discovering UKIs | 
| [ukify](https://packages.gentoo.org/useflags/ukify) | Build an UKI with systemd's ukify on each kernel installation | 

### Sbsigntools

The [app-crypt/sbsigntools](https://packages.gentoo.org/packages/app-crypt/sbsigntools) package will be used to sign executable files with the `db` private key.

`root #``emerge --ask app-crypt/sbsigntools`
### Efitools

The [app-crypt/efitools](https://packages.gentoo.org/packages/app-crypt/efitools) package will be used to enroll the `PK`, `KEK`, `db`, and `dbx` keys into the UEFI.

`root #``emerge --ask app-crypt/efitools`
## Key Creation

Now, the necessary key pairs that will be used in the boot chain should be made. Different keys are used depending on which step in the boot chain we're at. There are three types of keys: the ones for the UEFI, GRUB, and the Kernel.

### UEFI

The UEFI uses four different types of keys for secure boot: `PK`, `KEK`, `db`, and `dbx`.

- PK - Platform Key - Composed of two parts, PKpub (the public key) and PKpriv (the private key), used to sign the KEK.
- KEK - Key Exchange Key - The key used to sign the Signatures and Forbidden Signatures database, there can be more than one.
- db - Signature Database - Contains lists of public keys, signatures, and hashes which are allowed as part of the boot chain.
- dbx - Forbidden Signature Database - The opposite of the signature database, public keys, signatures, and hashes which should never be allowed to boot.


There is only one `PK` key while there can be multiple `KEK`, `db`, and `dbx` keys.

Imagine the `PK` key as the `root` user of a machine -- an omnipotent user that decides who has the ability to allow/deny executable files.

Imagine a `KEK` key as an unprivileged user that can allow/deny executable files they decide.

An executable file signed by the `db` key will be allowed, while the `dbx` key will deny.

There can be multiple users (`KEK`) each having multiple keys to allow (`db`) and deny (`dbx`). All users must be approved (signed) by root (`PK`).

#### Making the Keys

The keys should be put in an appropriate path such as /etc/ssl/private. Make a directory under this path, where the secure boot keys should be inserted, while setting up the correct permissions.

`root #````
mkdir -p /etc/ssl/private/secure_boot
```
`root #``chmod 700 /etc/ssl/private/secure_boot`

UEFI Secure Boot typically uses **RSA-2048** and **sha256**, but some motherboards might support stronger algorithms.

#### Making the ESLs

Next, we make the EFI signature lists.

`root #````
for key_name in my_pk.pem my_kek.pem my_db.pem my_dbx.pem; do
cert-to-efi-sig-list /etc/ssl/private/secure_boot/{"$key_name","${key_name%.*}".esl}
```

done
#### (Optional) Combining our Keys and the Factory Keys

We can make a backup of the factory keys (except the `PK` key) in the UEFI to later concatenate them with our keys. This is only required for tools like [Shim](https://wiki.gentoo.org/wiki/Shim) to still work.

`root #````
for key_type in KEK db dbx; do
efi-readvar -v "$key_type" -o factory_"${key_type,,}".esl
```

done
If we want to include the factory `esl` files, we simply concatenate them with ours (except the `PK` key) and use the combined files from this point on.

`root #````
for key_type in kek db dbx; do
cat factory_"$key_type".esl my_"$key_type".esl >combined_"$key_type".esl
```

done
#### Signing the Keys

The `PK` and `KEK esl` files are signed by the `PK` key.

`root #````
sign-efi-sig-list -c /etc/ssl/private/secure_boot/my_pk.pem -k /etc/ssl/private/secure_boot/my_pk.pem PK /etc/ssl/private/secure_boot/my_pk.{esl,auth}
```
`root #````
sign-efi-sig-list -c /etc/ssl/private/secure_boot/my_pk.pem -k /etc/ssl/private/secure_boot/my_pk.pem KEK /etc/ssl/private/secure_boot/my_kek.{esl,auth}
```
The database `esl` files are signed by the `KEK` key.

`root #````
sign-efi-sig-list -c /etc/ssl/private/secure_boot/my_kek.pem -k /etc/ssl/private/secure_boot/my_kek.pem db /etc/ssl/private/secure_boot/my_db.{esl,auth}
```
`root #````
sign-efi-sig-list -c /etc/ssl/private/secure_boot/my_kek.pem -k /etc/ssl/private/secure_boot/my_kek.pem dbx /etc/ssl/private/secure_boot/my_dbx.{esl,auth}
```
#### Installing the Keys to the UEFI

Before running the command that installs the keys to the UEFI, the UEFI must be in "Setup Mode"; doing this varies across manufacturers, but the idea is the same. Reboot the machine and boot into the UEFI; there should be a page for "Security" and "Secure Boot" should be in it; there should be an option to clear the current `PK`, `KEK`, `db`, and `dbx` keys. Clear the keys, save the settings, and reboot.

If done correctly, we should get the following output after we reboot:

`root #``efi-readvar`
Variable PK has no entries
Variable KEK has no entries
Variable db has no entries
Variable dbx has no entries
Variable MokList has no entries

Finally, we install the keys to the UEFI.

`root #````
for key_type in PK KEK db dbx; do
efi-updatevar -f /etc/ssl/private/secure_boot/my_"${key_type,,}".auth "$key_type"
```

done
If the keys installed correctly, we should get the following output:

`root #``efi-readvar````
Variable PK, length 923
PK: List 0, type X509
    Signature 0, size 895, owner 00000000-0000-0000-0000-000000000000
        Subject:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
        Issuer:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
Variable KEK, length 923
KEK: List 0, type X509
    Signature 0, size 895, owner 00000000-0000-0000-0000-000000000000
        Subject:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
        Issuer:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
Variable db, length 923
db: List 0, type X509
    Signature 0, size 895, owner 00000000-0000-0000-0000-000000000000
        Subject:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
        Issuer:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
Variable dbx, length 923
dbx: List 0, type X509
    Signature 0, size 895, owner 00000000-0000-0000-0000-000000000000
        Subject:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
        Issuer:
            C=AU, ST=Some-State, O=Internet Widgits Pty Ltd
Variable MokList has no entries
```
### GRUB

GRUB will use its own method of verifying files further in the boot chain via GPG.

#### GPG

GPG has a command line interface tool to make keys, but it also has a "batch" mode so we can put the command in a script. Make the following GPG parameter file:

Now make the key.

`root #``gpg --batch --full-gen-key /root/my_grub_gpg_parameter`
Export the public key so that it can be embedded into the GRUB executable.

`root #``gpg -o /tmp/grub_gpg_public_key --export grub`
### Kernel

We can put the key in an appropriate path such as /etc/ssl/private. Make a directory under this path so that we can place our kernel key in it and set the correct permissions.

`root #````
mkdir -p  /etc/ssl/private/kernel
```
`root #``chmod 700 /etc/ssl/private/kernel`

The kernel only supports a specific set of key types depending on the version.

**Check which key types are supported**

`-*- Cryptographic API --->` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO</code> to find this item.
        Certificates for signature checking --->
            Type of module signing key to be generated (<key type>) --->
Where "\<key type>" is the type of key the kernel will generate if one is not provided.

As of Linux kernel version **6.12.1**, the supported types are:

- RSA
- ECDSA

We will be creating an ECDSA key with the curve **P-521** and hash **SHA3-512** (the strongest curve and hash that the kernel supports). If these are not available, use the strongest possible for the kernel.

## Set Variables

### Portage

#### Secure Boot

On packages with the `secureboot` global USE flag, this flag can be enabled to automatically sign any EFI binaries installed by the package. When this flag is enabled the `SECUREBOOT_SIGN_KEY` and `SECUREBOOT_SIGN_CERT` environment variables must be used to specify the path (or pkcs11 URI) of the `db` key and certificate to use for signing in PEM format.

**`/etc/portage/make.conf`**

```
USE="... secureboot ..."
SECUREBOOT_SIGN_KEY="/etc/ssl/private/secure_boot/my_db.pem"
SECUREBOOT_SIGN_CERT="/etc/ssl/private/secure_boot/my_db.pem"
```
Update all packages with the `secureboot` USE flag now enabled.

`root #``emerge --ask -uND @world`
#### Kernel

In addition to the kernel itself, the kernel modules must also be signed to boot successfully with secure boot enabled. For this purpose, the `modules-sign` global USE flag can be used in addition to the `MODULES_SIGN_KEY` and `MODULES_SIGN_CERT` environment variables.

**`/etc/portage/make.conf`**

```
USE="... modules-sign ..."
MODULES_SIGN_KEY="/etc/ssl/private/kernel/my_kernel_key.pem"
MODULES_SIGN_CERT="/etc/ssl/private/kernel/my_kernel_key.pem"
MODULES_SIGN_HASH="sha3-512"
```
Update all packages with the `modules-sign` USE flag now enabled; this will also sign the modules.

`root #``emerge --ask -uND @world`
## Configuration

### Kernel

There are many kernel options to increase security; for this page, we will only be going over what is needed to get secure boot working. For additional kernel settings for secure boot, see [Handbook:AMD64/Installation/Kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel).

**Enable support for signed kernel modules**

\[\*\] Enable loadable module support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MODULES\</code> to find this item.
    -\*- Module signature verification [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MODULE\_SIG\</code> to find this item.
    \[\*\]     Require modules to be validly signed [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MODULE\_SIG\_FORCE\</code> to find this item.
    \[\*\]     Automatically sign all modules [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MODULE\_SIG\_ALL\</code> to find this item.
        Hash algorithm to sign modules (SHA3-512) ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MODULE\_SIG\_SHA3\_512\</code> to find this item.
-\*- Cryptographic API ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\</code> to find this item.
    Certificates for signature checking --->
        (/etc/ssl/private/kernel/my\_kernel\_key.pem) File name or PKCS#11 URI of module signing key [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MODULE\_SIG\_KEY\</code> to find this item.

## Usage

### Kernel

After we've made all the changes to our kernel, we can compile it.

`root #````
cd /usr/src/linux[version]
```
`root #``make -j$(nproc) && make -j$(nproc) modules_install && make install`
Then sign the kernel with the `db` key for the UEFI to verify.

`root #``sbsign --cert /etc/ssl/private/secure_boot/my_db.pem --key /etc/ssl/private/secure_boot/my_db.pem /boot/vmlinuz-<version>-gentoo --output /boot/vmlinuz-<version>-gentoo`
### GRUB

We need to make and sign the GRUB executable. For the installation, we will need three parameters in addition to the ones that we already use to install GRUB to our machine. For more information on how to install GRUB for a specific machine, see [GRUB](https://wiki.gentoo.org/wiki/GRUB). The three parameters are as follows:

- `--disable-shim-lock` -- Disable the use of [Shim](https://wiki.gentoo.org/wiki/Shim) as we are not using it.
- `--pubkey` -- Embed the GPG public key to verify signatures of files signed by the GPG private key.
- `--modules` -- Embed GRUB modules needed to verify signatures of files signed by the GPG private key.



`root #``grub-install --disable-shim-lock --pubkey=/tmp/grub_gpg_public_key --modules="pgp gcry_sha512 gcry_rsa" [other parameters for machine]`
Now sign the GRUB executable with the `db` key for the UEFI to verify.

`root #``sbsign --cert /etc/ssl/private/secure_boot/my_db.pem --key /etc/ssl/private/secure_boot/my_db.pem /path/to/grub.efi --output /path/to/grub.efi`

Next, we need to sign all the files GRUB needs to verify; we can do this with a script.

**`/etc/bash/bashrc.d/99_my_install_kernel.bash`**

```
# GRUB sign. This function signs files for GRUB needed for boot.
#
# NOTE:
# * Note on 'File_to_sign':
#   Sign all files that GRUB will use in the boot chain; see 'info grub' section 19.2. The list of files to sign include:
#   - GRUB-related files (configs, environment, modules).
#   - kernel.
#   - initramfs.
#   - CPU microcode.
#
# * Note on finding files to sign:
#   The name of the kernel and initramfs files found in /boot can vary depending on what package and USE flags are used to install the kernel; we need
#   to be less restrictive with our regular expression in the 'find' command because of this. Since we don't know how the kernel and initramfs names
#   end, we simply have to include all file names that contain the words "vmlinuz" or "init"; this has the unwanted effect of including files that end
#   with ".sig".
#
#   To fix this, we use 'sed' to filter out the files that end with ".sig". If we didn't do this, we would eventually get files that end in
#   ".sig.sig.sig.sig..." (a never-ending loop of detached signatures of detached signatures).
#
# * Note on deleting old signatures:
#   This is not the same as deleting ".sig" files from the 'File_to_sign' array; this is deleting the detached signatures of files we know are not
#   signatures themselves.
sign_file_for_grub()
{
	# See note on 'File_to_sign'.
	local -a File_to_sign=()
	echo "Signing all files required for boot with the GRUB GPG key..."
	# See note on finding files to sign.
	File_to_sign=($(find /boot -regextype posix-extended -regex '.*(\.cfg|\.lst|\.mod|vmlinuz.*|init.*|grubenv|-uc\.img|\.efi)$' | sed -En '/\.sig$/! p'))
	# Delete the old signatures; see note on deleting old signatures.
	rm -f "${File_to_sign[@]/%/.sig}"
	# Now that all the old signatures are deleted, we can make new detached signatures for the files that GRUB will verify on boot.
	parallel gpg --batch -bu "grub" --digest-algo SHA512 ::: "${File_to_sign[@]}"
	return 0
}
```

Source the file /etc/profile; this file is typically sourced on Bash login shells (like when we first login to a booted machine), but we can also do it manually. For those that are curious, we can open this file in a text editor and follow the logic to see how our file gets sourced.

`root #````
source /etc/profile
```
`root #``sign_file_for_grub`
## Verify

At this point, secure boot is set up; the only thing left to do is reboot into the UEFI and ensure that secure boot is enabled.

`root #``reboot`
After we reboot and make it past GRUB and the login, we can check if secure boot is enabled by running the following command:

`root #``dmesg | grep -i secure`
\[   0.00123\] \[   T0\] Secure boot enabled

## Automation

For future kernel installations, the signing process can be fully automated when we install our kernel. We can append the following to our already existing /etc/bash/bashrc.d/99\_my\_install\_kernel.bash file:

**`/etc/bash/bashrc.d/99_my_install_kernel.bash`**

```
# Kernel install. This function handles various tasks to ensure proper kernel installation.
#
# NOTE:
# * Note on finding the newest kernel:
#   Get the path of the newest modified kernel; to do this, we use 'find'. The following 'find' syntax can be read as:
#   - Find all files in the /boot directory
#   - **AND** that have a modified time newer than our timestamp
#   - **AND** whose file name contains the word 'vmlinuz'
#   - **AND** whose file name does not end with '.old'
#   - **AND** whose file name does not end with '.sig'.
#
#   There should only be one file with all these conditions met.
my_install_kernel()
{
	# The name of the kernel file found in /boot can vary depending on what package and USE flags are used to install the kernel. Since we cannot use the
	# name of the file to reliably determine the newly compiled kernel, we can use the modification date instead.
	local date_before_install=""
	local       newest_kernel=""
	# Check to see if we are in the correct directory.
	if ! pwd -P | grep -E '^/usr/src/linux' 1>/dev/null
	then
		echo "This needs to run in /usr/src/linux[...]!"
		return 1
	fi
	# Compile the kernel and its modules.
	make -j$(nproc) || return 1
	# If external kernel modules (such as NVIDIA or ZFS) are installed on the system, they must be rebuilt against the newly compiled kernel to ensure
	# compatibility.
	emerge -a n @module-rebuild || return 1
	# Install the kernel modules.
	make -j$(nproc) modules_install || return 1
	# Take a timestamp now to know the newest modified kernel in /boot later.
	date_before_install="$(date --iso-8601=seconds)"
	# Install the kernel via sys-kernel/installkernel as usual.
	make install || return 1
	# See note on finding the newest kernel.
	newest_kernel="$(find /boot -newermt "$date_before_install" -name '*vmlinuz*' ! -name '*.old' ! -name '*.sig')"
	# The newly compiled kernel needs to be signed with the UEFI 'db' key.
	echo "Signing $newest_kernel with the UEFI db key..."
	sbsign --cert "/etc/ssl/private/secure_boot/my_db.pem" --key "/etc/ssl/private/secure_boot/my_db.pem" "$newest_kernel" --output "$newest_kernel"
	# Sign/resign files with the GRUB GPG key in case any of them were modified.
	sign_file_for_grub
	return 0
}
```
`root #````
source /etc/profile
```
`root #``my_install_kernel`

If we need to reinstall the GRUB executable, we can append the following:

**`/etc/bash/bashrc.d/99_my_install_kernel.bash`**

```
()
{
	# We need to export the public key so we can embed it in the GRUB executable.
	gpg -o "/tmp/grub_gpg_public_key" --export "grub"
	grub-install --disable-shim-lock --pubkey=/tmp/grub_gpg_public_key --modules="pgp gcry_sha512 gcry_rsa" [other parameters for machine]
	rm "/tmp/grub_gpg_public_key"
	# Sign the GRUB executable with the UEFI 'db' key so the UEFI can verify it for secure boot.
	echo "Signing the GRUB executable with the UEFI db key..."
	sbsign --cert "/etc/ssl/private/secure_boot/my_db.pem" --key "/etc/ssl/private/secure_boot/my_db.pem" "/path/to/grub.efi" --output "/path/to/grub.efi"
	# Sign/resign files with the GRUB GPG key in case the GRUB GPG key changed.
	sign_file_for_grub
	return 0
}
```
`root #````
source /etc/profile
```
`root #``my_grub_install`
## Troubleshooting

### GRUB

#### Error: prohibited by secure boot policy.

Ensure that all needed external GRUB modules in /boot/grub/\<architecture> are signed by the GPG key. This error happens when secure boot is enabled in the UEFI and GRUB loads successfully, but GRUB can't load any of its modules.

#### Error: shim\_lock protocol not found.

Ensure that `--disable-shim-lock` is used. GRUB uses Shim by default, so we need to tell GRUB to disable it because we are using our own keys for secure boot.

`root #``grub-install --disable-shim-lock ...`
#### Error: you need to load the kernel first.

Ensure the kernel is signed by the `db` UEFI key **\*\*THEN\*\*** signed by the GRUB GPG key; the `db` signature will be embedded into the kernel while the GPG signature will be detached.

#### Error: verification requested but nobody cares: (\<drive>,\<partition>)/grub/\<architecture>/\<grub module>.mod.

Ensure that the pgp GRUB module is embedded into the GRUB executable; GRUB needs this module embedded because this is the module that verifies other modules and files.

`root #``grub-install --modules="pgp ..." ...`
#### Error: loading initial key: bad signature

Ensure that the GPG key generated is supported by GRUB and that the public key is exported correctly.

`root #``gpg -o /path/to/grub_gpg_public_key --export grub`
Ensure the GPG public key is embedded into the GRUB executable with `--pubkey`.

`root #``grub-install --pubkey=/path/to/grub_gpg_public_key ...`
## Removal

If secure boot is no longer wanted, it can be disabled in the UEFI; the other signature checking performed by GRUB and the kernel can be kept.

## See also

- [Handbook:AMD64/Installation/Kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel)
- [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) — an enhancement of the security of the pre-boot process of a [UEFI](https://wiki.gentoo.org/wiki/UEFI) system.
- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
- [Full Disk Encryption From Scratch](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch) — a guide which covers the process of configuring a drive to be encrypted using LUKS and btrfs.

## External resources

- [Gentoo forum on GRUB and Secure Boot](https://forums.gentoo.org/viewtopic-t-1172155-highlight-grub+secure+boot.html) -- The Gentoo forum post that resulted in this page being made.
