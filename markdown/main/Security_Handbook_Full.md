<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Full | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Full -->
---
title: Security Handbook/Full
url: https://wiki.gentoo.org/wiki/Security_Handbook/Full
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: b412d918e4233fe5
license: CC BY-SA 4.0
---

# Security Handbook/Full

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



A threat model is a model that describes the threats that a system faces. A threat is a potential or actual undesirable event that may be malicious (such as DoS attack) or incidental (hardware failure)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

It is important to understand the threats that a system faces in order to protect it. For example, a laptop that is used to browse the web is going to face different threats than a server that is used to host a website.

From a user perspective, a jet-setting businessperson who uses their laptop to access sensitive information is going to face different threats than a student who uses their laptop to browse the web.

Here are some examples of threats that a system may face:

- Malicious attacks, such as DoS attacks, malware attacks, and phishing attacks.
- Accidental events, such as hardware failures, software bugs, and human errors.
- Natural disasters, such as floods, fires, and earthquakes.

Understanding the threat model of a system enables risks to be assessed and managed.

Risk is a measure of the extent to which something is threatened by a potential circumstance or event<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Typically risk is assessed by looking at two factors:

- Impact: The potential consequences of the event occurring.
- Likelihood: The probability of the event occurring.

As an example we can consider a laptop that is lost or stolen. A potential impact of this risk is that the data on the laptop may be accessed by unauthorized persons and leaked; the severity of this impact is dependent on the data in question. The likelihood of this event occurring is dependent on the threat model of the user - it is far more likely for a laptop that is regularly taken between multiple locations to be lost or stolen than a laptop that is only used in a single location.

Controls are the means of managing risk. They include policies, procedures, guidelines, practices, or organizational structures, which may be of an administrative, technical, management, or legal nature<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

When considering the hypothetical lost or stolen laptop, there are a number of controls that can be used to manage the risk:

- Full disk encryption can manage the risk of confidential data being accessed by unauthorized persons.
- Regular backups can manage the risk of data loss.
- Physical security measures (such as a lock) can manage the risk of the laptop being stolen.

Example controls may be provided throughout this document. They will be broadly grouped...

Each control is assigned a 'Maturity Level', inspired by the [Australian Cyber Security Centre's Essential Eight Maturity Model](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/essential-eight/essential-eight-maturity-model), however rather than assigning a maturity level to an organization or individual based on meeting a certain number of essential controls, this scale assigns each control a maturity level with the intent of guiding users towards an effective security baseline.

The levels are as follows:

If controls assigned this maturity level are not implemented this signifies that there are weaknesses in the user or organization's overall cyber security posture. When exploited, these weaknesses could facilitate the compromise of the confidentiality of their data, or the integrity or availability of their systems and data, as described by the tradecraft and targeting in Maturity Level One below.

The focus of this maturity level is adversaries who are content to simply leverage commodity tradecraft that is widely available in order to gain access to, and likely control of, systems. For example, adversaries opportunistically using a publicly-available exploit for a security vulnerability in an internet-facing service which had not been patched, or authenticating to an internet-facing service using credentials that were stolen, reused, brute forced or guessed.

Generally, adversaries are looking for any victim rather than a specific victim and will opportunistically seek common weaknesses in many targets rather than investing heavily in gaining access to a specific target. Adversaries will employ common social engineering techniques to trick users into weakening the security of a system and launch malicious applications, for example via Microsoft Office macros. If the account that an adversary compromises has special privileges they will seek to exploit it. Depending on their intent, adversaries may also destroy data (including backups).

The focus of this maturity level is adversaries operating with a modest step-up in capability from the previous maturity level. These adversaries are willing to invest more time in a target and, perhaps more importantly, in the effectiveness of their tools. For example, these adversaries will likely employ well-known tradecraft in order to better attempt to bypass security controls implemented by a target and evade detection. This includes actively targeting credentials using phishing and employing technical and social engineering techniques to circumvent weak multi-factor authentication.

Generally, adversaries are likely to be more selective in their targeting but still somewhat conservative in the time, money and effort they may invest in a target. Adversaries will likely invest time to ensure their phishing is effective and employ common social engineering techniques to trick users to weaken the security of a system and launch malicious applications, for example via Microsoft Office macros. If the account that an adversary compromises has special privileges they will seek to exploit it, otherwise they will seek accounts with special privileges. Depending on their intent, adversaries may also destroy all data (including backups) accessible to an account with special privileges.

The focus of this maturity level is adversaries who are more adaptive and much less reliant on public tools and techniques. These adversaries are able to exploit the opportunities provided by weaknesses in their target's cyber security posture, such as the existence of older software or inadequate logging and monitoring. Adversaries do this to not only extend their access once initial access has been gained to a target, but to evade detection and solidify their presence. Adversaries make swift use of exploits when they become publicly available as well as other tradecraft that can improve their chance of success.

Generally, adversaries may be more focused on particular targets and, more importantly, are willing and able to invest some effort into circumventing the idiosyncrasies and particular policy and technical security controls implemented by their targets. For example, this includes social engineering a user to not only open a malicious document but also to unknowingly assist in bypassing security controls. This can also include circumventing stronger multi-factor authentication by stealing authentication token values to impersonate a user. Once a foothold is gained on a system, adversaries will seek to gain privileged credentials or password hashes, pivot to other parts of a network, and cover their tracks. Depending on their intent, adversaries may also destroy all data (including backups).





Physical security is the practice of protecting elements of infrastructure, estates, and personnel against attacks or compromises in the physical (i.e. tangible, real-world) environment[\[1\]](https://wiki.gentoo.org#cite_note-4)


Physical security is an important consideration for all systems. For example, a laptop that is left unattended in a public place is at risk of being stolen; a server that is left in an unlocked room is at risk of being tampered with. Physical security controls should be implemented to manage these risks.

For a practical guide to physical security (including some good controls that may be adapted), see the following resources:

Information Security is the practice of managing risks related to the use, processing, storage, and transmission of information or data. It is also ensuring the systems and processes used for those purposes are in line with organisational policies<sup>[\[2\]](https://wiki.gentoo.org#cite_note-5)</sup>.


Information security is an important consideration for both individuals and organizations though the specific controls that are implemented, and the rationale for implementing them, will vary. A user may primarily be concerned about the security of their personal data, whereas an organization may be primarily concerned about the security of their customer's data or any legislative requirements that they are subject to.

In the case of a user, information security controls may include:

- Regular backups to ensure that data cannot be lost in the event of theft, loss, or hardware failure.
- Use of a password manager to ensure that the compromise of one account does not lead to the compromise of others.
- Use of a [WebAuthn](https://en.wikipedia.org/wiki/WebAuthn) device such as a [Yubikey](https://en.wikipedia.org/wiki/Yubikey) — or some other form of multi-factor authentication — to ensure that accounts cannot be compromised by password alone.
- Implementing full disk encryption to ensure that data cannot be accessed by unauthorized persons.

In the case of an organization, information security controls may include:

- Implementation of a security policy to ensure that all staff are aware of their responsibilities.
- Implementation of a 'need-to-know' principle to ensure that data is only accessible to those who need it.
- Enforcement of a password policy to ensure that passwords are sufficiently strong and are not known to be compromised.
- Implementation of a data retention policy to ensure that data is not kept for longer than necessary and is disposed of securely.
- Implementation of full disk encryption to ensure that data cannot be accessed by unauthorized persons.

Governments deal with sensitive information and are often the target of malicious actors. As such, they have developed a number of resources that may be of interest to users that are interested in developing their own security policies and controls:

This list is not exhaustive and other, more specific, resources may be available.

root is the conventional name for the superuser account used for administration of a UNIX-like system; the user with a user identifier (UID) of zero is the superuser, regardless of the name of that account. root has all rights or permissions (to all files and programs) in all modes (single- or multi-user); it can do many things an ordinary user cannot, such as changing the ownership of files and binding to network ports numbered below 1024.

The root user should not be used as a normal user account:

- If an application run as root is exploited, the attacker will have root access to the system.
- The root user is not subject to the same restrictions as a normal user account. For example, the root user can delete system files that are required for the system to function.

Instead, a normal user account should be used then, when additional privileges are required, elevate permissions with the su (substitute user), sudo (substitute user do), or doas command.

The preferred approach will vary by environment, however the latter methods are preferred as they leave an audit trail of who has used the command and what administrative operations were performed. Gentoo has some default protection against normal users trying to su to root; The default PAM setting requires that a user be a member of the group "wheel" in order to be able to su.

It should be noted that the root user has the ability to modify this audit log if it is stored on the same system.

- **Never** run the display server or **any other user application** as root
- **Never ever** run a web browser as root
- Consider elevating with sudo or doas instead of su
- Try to use absolute paths when logged in as root or always elevate permissions with some variation of su -, which replaces the environment variables of the user with those of root
- *Never* leave an open root terminal unattended on an unlocked workstation.

Regularly updating a Gentoo Linux system is crucial for maintaining its security and stability.

Keeping a system up-to-date helps to protect it from potential vulnerabilities that could be exploited by malicious actors.

- **1. Security patching:** Regular updates ensure that a Gentoo Linux system receives the latest security patches. These patches address known vulnerabilities and weaknesses in the software, preventing potential attackers from exploiting them. By regularly updating a system it is possible to ensure that it is protected from known exploits.

- **2. Bug fixes and stability:** Updates also include bug fixes and improvements that enhance the overall stability of a Gentoo Linux system. These fixes address issues identified by the community and developers, ensuring that the system operates smoothly and reliably. Regularly updating allows the system to benefit from these improvements and helps to maintain a secure and stable environment.

- **3. Testing packages and security:** In Gentoo Linux most architectures offer both a 'stable' and 'testing' (\~arch) keyword. While significant efforts are undertaken by Gentoo Linux developers to ensure that packages marked as stable are thoroughly tested for stability and security, testing packages offer more up-to-date versions of software that may contain new features and security enhancements. Although testing packages may have undergone less rigorous testing, they may be more secure by virtue of including security fixes that have not yet been backported and tested to stable. As such, testing packages can be a useful tool for maintaining a secure Gentoo Linux system.

- **4. Risk assessment and user expertise:** When deciding whether to use stable or testing packages, it is essential to assess based on the user's expertise and requirements. Stable packages are recommended for users who prioritize stability and a higher level of testing. On the other hand, experienced users who are comfortable managing any potential issues that may arise (and who will file bug reports) can opt for testing packages to take advantage of the latest security features.

It is important to consider the potential trade-offs and ensure that a system is configured to meet meets an individual or organization's specific requirements.

Keeping backups of important data is of critical for maintaining the security and integrity of digital information. It helps protect against data loss caused by various factors such as hardware failure, software issues, malware attacks, accidental deletion, or natural disasters. Understanding the importance of backups and the differences between offline and online backups is essential for ensuring a robust data protection strategy.

- **1. Data recovery and continuity:** Backups serve as a safety net, allowing users to restore their data in case of data loss or corruption. By keeping regular backups, individuals and organizations can quickly recover their files, minimize downtime, and maintain business continuity. It is vital to consider the potential impact of data loss and establish a backup regimen accordingly.

- **2. Offline backups:** Offline backups refer to copies of data stored on physical media that are disconnected from the network or computer system. This can include external hard drives, tapes, or removable storage media. Offline backups provide an additional layer of protection against malware attacks, as they are not susceptible to remote infiltration or ransomware encryption. They offer increased security by reducing the attack surface and minimizing the risk of unauthorized access to backup data.

- **3. Online backups:** Online backups involve storing data in remote locations or cloud-based services. This method offers convenience and accessibility, as data can be easily backed up and restored from any location with an internet connection. However, it is important to consider the security measures implemented by the online backup provider to ensure the confidentiality, integrity, and availability of the data. Encryption and strong access controls are vital to safeguard data stored in online backups.

- **4. Snapshots are not backups:** It is important to note that snapshots, while useful for certain purposes, should not be considered as backups on their own. Snapshots provide point-in-time copies of a system or data, allowing for easy rollbacks or recovery within the same system. However, they are **typically stored within the same infrastructure, making them susceptible to the same risks and vulnerabilities**. To ensure comprehensive data protection, it is crucial to have **separate backups** stored in a different location or on offline media.

- **5. Backup frequency and resting:** Regular backup schedules are essential to ensure that the latest changes have been captured and that an potential data loss is minimized. The frequency of backups should align with the criticality of the data and the frequency of updates. Additionally, it is crucial to periodically test the backup and restoration processes to verify the integrity and reliability of the backups. Testing ensures that backups are functional and can be successfully restored when needed.

Remember that backups should be stored securely, and access to them should be restricted to authorized individuals. Implementing encryption and strong access controls helps protect sensitive data from unauthorized disclosure or tampering.

This guide attempts to be as broad as possible however it is important to be aware that any advice offered does not take into account a user or organization's particular set of circumstances; It is essential to take into consideration any specific requirements when identifying threats and mitigating risks.





## Boot Path Security

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
It may be more effective, however, to build a standalone GRUB image with the required modules, key, and minimal grub configuration built-in; this way only the kernel, initramfs, and on-disk grub configuration (if it is changed) need to be signed.[\[2\]](https://wiki.gentoo.org#cite_note-7)

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




Information Security is the practice of protecting information from unauthorized access, use, disclosure, alteration, or destruction; it ensures the safety and privacy of critical data such as personal information, financial data, or intellectual property.

There are a number of ways to protect data, including:

- **Encryption**: Encryption is the process of converting data into a scrambled format that can only be read by someone with the correct decryption key.
- **Access control**: Access control is the process of limiting who has access to data. This can be done by using passwords, security certificates, and other methods.
- **Backups**: Backups are copies of data that are stored in a safe location. This can be done on-site or off-site.
- **Data security policies**: Data security policies are documents that outline the rules for how data should be handled. These policies should be created by businesses and organizations and should be communicated to employees.

A strong password or passphrase is ideally difficult for others (both machine and human) to guess, but easy for the user to remember.

Strong passwords or passphrases help to protect accounts from unauthorized access; if a password is leaked it may be used to access otherwise secure accounts or systems despite there being effective security controls in place.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| Information Security | Authentication | Enforce the use of strong passwords or passphrases | 0 | 
| Information Security | Authentication | Check for known disclosures of passwords | 0 | 
| Information Security | Authentication | Use Multi-Factor Authentication where practical | 1 | 
| Information Security | Authentication | Use a hardware authentication device | 3 | 

Passphrase guidance:

- use a mix of uppercase and lowercase letters, numbers, and symbols
- make your password or passphrase at least 14 characters long
- a passphrase should not be derived from personal information, such as a name, birthday, or address
- use a password manager with an individual passphrase for each account

Never share personal passwords or passphrases with anyone. In the event that a shared account is used, securely record that password or passphrase in a password manager and share the password manager with the other user(s) using its built-in collaboration features.

A **hardware security token**, such as a **FIDO2 Web Authentication (WebAuthn)**  device (e.g. YubiKey), can provide an additional layer of protection beyond that of a traditional usernames and passwords.

These tokens use cryptographic keys stored within the device to authenticate users. This two-factor or multi-factor authentication (2FA/MFA) significantly reduces the risk of unauthorized access and data breaches.

Since the tokens are a physical device, they cannot be easily replicated or intercepted by malicious actors. Even if a user's credentials are unknowingly compromised the hardware token's cryptographic key remains safe, preventing unauthorized access to sensitive accounts.

Hardware security tokens are typically compact and may be easily carried on a keychain or stored in a wallet. This portability allows users to have secure access to their accounts and digital assets from any computer or device with USB or, more recently, NFC capabilities.

The storage configuration of a system can have an impact on its security; the following outlines some best-practice guidelines related to the topic.

Locations that users (or services) have permission to write to (e.g. /home or /tmp) should be on a separate filesystem to system data and should leverage disk quotas or other similar mechanisms to prevent excessive utilization. This reduces the risk of filling up filesystems that are critical to the operation of the host, accidentally or maliciously, which may result in a Denial of Service.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| Information Security | Storage Configuration | User and System data should be logically separated wherever practical | 0 | 
| Information Security | Storage Configuration | An appropriate technical control should be implemented to prevent users from filling up critical filesystems | 1 | 

File permissions are a way of controlling who can access and modify files on a computer system. They are an important mechanism for keeping data safe and secure.

On single-user systems, such as personal laptops, file permissions are usually set by the owner. This typically means that a single user has complete control over who can access and modify all of the files on the system.

On multi-user systems, such as file shares in corporate environments, file permissions are usually set by the system administrator to restrict what each user is able to view and modify.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| Information Security | Storage Configuration | Appropriate file permissions should be set to prevent unauthorized access to or modification of files | 0 | 
| Information Security | Storage Configuration | An audit log should be kept of file access and modifications | 3 | 

General advice:

- carefully consider the permissions for a file or directory
- consider the need-to-know principle when providing access to data
- regularly review file permissions to make sure that they are still correct

In Linux (and other POSIX-like systems) file permissions are controlled by a three-digit number called the mode. The mode is made up of three parts: the owner's permissions, the group's permissions, and the other users' permissions.

Permissions can be read (r), write (w), or execute (x); no permissions is denoted by (-).

Permissions are often defined using octal notation where, for example, mode 755 means that the owner has read, write, and execute permissions, the group has read and execute permissions, and other users have read and execute permissions.

| Three permission triads |  | 
|---|---|
| first triad | what the owner can do | 
| second triad | what the group members can do | 
| third triad | what other users can do | 
| Each triad |  | 
| first character | `r`: readable | 
| second character | `w`: writable | 
| third character | `x`: executable `s` or `t`: [setuid](https://wiki.gentoo.org/index.php?title=Setuid&action=edit&redlink=1)/[setgid](https://wiki.gentoo.org/index.php?title=Setgid&action=edit&redlink=1) or [sticky](https://wiki.gentoo.org/index.php?title=Sticky_bit&action=edit&redlink=1) (also executable) `S` or `T`: setuid/setgid or sticky (not executable) | 

| Symbolic notation | Numeric notation | English | 
|---|---|---|
| `----------` | 0000 | no permissions | 
| `-rwx------` | 0700 | **read, write, & execute only for owner** | 
| `-rwxrwx---` | 0770 | read, write, & execute for owner and group | 
| `-rwxrwxrwx` | 0777 | read, write, & execute for owner, group and others | 
| `---x--x--x` | 0111 | execute | 
| `--w--w--w-` | 0222 | write | 
| `--wx-wx-wx` | 0333 | write & execute | 
| `-r--r--r--` | 0444 | read | 
| `-r-xr-xr-x` | 0555 | read & execute | 
| `-rw-rw-rw-` | 0666 | read & write | 
| `-rwxr-----` | 0740 | owner can read, write, & execute; group can only read; others have no permissions | 

Disk encryption is the process of protecting data on a storage device by scrambling it so that it cannot be read without the correct decryption key. This is often used to protect data on laptops and other mobile devices, but can also be used to protect data on servers and workstations. Disk encryption is not a replacement for other security controls, but can be used to mitigate the risk of data being accessed by an unauthorized party if the physical disk is stolen.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| System Configuration | Storage | Disk encryption should be implemented to protect data at rest | 0 | 

LUKS (Linux Unified Key Setup) is a popular disk encryption method that is supported by most Linux distributions. LUKS uses a combination of encryption algorithms and key management tools to provide strong data protection.

Modern systems implement some form of hardware-backed cryptographic acceleration (such as AES<sup>[\[1\]](https://wiki.gentoo.org#cite_note-8)</sup>) which can be leveraged to reduce the performance impact of disk encryption.

Other systems, such as Opal2-compliant self-encrypting drives, perform encryption in hardware and are controlled by the disk firmware. Some of these devices encrypt *all* data to the disk even before decryption is "enabled" in firmware, only encrypting the internal encryption key if encryption is "enabled", allowing the user to "encrypt" the disk immediately by setting a passphrase.

There are a number of reasons that disk encryption should be considered:

- **Data protection:** Disk encryption can help to protect data from unauthorized access, such as in the case of a lost or stolen device
- **Compliance:** Disk encryption can help organizations to comply with data security regulations, such as the General Data Protection Regulation (GDPR) or Health Insurance Portability and Accountability Act (HIPAA)
- **Ease of use:** Disk encryption tools are becoming increasingly easy to use, making it possible for even non-technical users to protect their data

### References

1. [↑](https://wiki.gentoo.org#cite_ref-8) [https://en.wikipedia.org/wiki/AES\_instruction\_set](https://en.wikipedia.org/wiki/AES_instruction_set)
2. [↑](https://wiki.gentoo.org#cite_ref-9) [https://ieeexplore.ieee.org/abstract/document/8835339](https://ieeexplore.ieee.org/abstract/document/8835339) Self-encrypting deception: weaknesses in the encryption of solid state drives (SSDs), Carlo Meijer, Bernard van Gastel



Logging verbosity can be increased to catch warnings or errors that might indicate an ongoing attack or successful compromise. Attackers often scan or probe before directly attacking a targeted system, and these probes can be detected with proper logging.

It is also vital that log files are readable, well managed, and stored safely. Loggers should be chosen with consideration to security as well as the use case. There is no best logging utility for every job, although some are more versatile than others.

The following log utilities may be unnecessary on systemd systems, since [it includes logging as a core functionality (journald)](https://wiki.gentoo.org/wiki/Systemd#Handling_of_log_files).

[Sysklogd](https://wiki.gentoo.org/wiki/Sysklogd) is very commonly used with Linux and Unix systems in general. It has some log rotation facilities, but using [logrotate](https://wiki.gentoo.org/wiki/Logrotate) in a [cron](https://wiki.gentoo.org/wiki/Cron) job or [systemd](https://wiki.gentoo.org/wiki/Systemd) timer offers more features and control over how log files are rotated. The frequency of log rotation depends on many factors, such as load and capacity.

Below is the standard sysklogd configuration, located at, syslog.conf with some added features. The following *cron* and *tty* lines have been uncommented, and a remote logging server had been added.

**`/etc/syslog.conf`**

**Syslog example**

~~Metalog by Frank Dennis is not able to log to a remote server, but it does have advantages when it comes to performance and logging flexibility~~. It can log by program name, urgency, facility (like sysklogd), and comes with regular expression matching with which you can launch external scripts when specific patterns are found. It is very good at taking action when needed.

The standard configuration is usually enough. To be notified by email whenever a password failure occurs use one of the following scripts.

For postfix:

**`/usr/local/sbin/mail_pwd_failures.sh`**

**Postfix**

```
#!/bin/sh
echo "$3" | mail -s "Warning (program : $2)" root
```
For netqmail:

**`/usr/local/sbin/mail_pwd_failures.sh`**

**Netqmail**

```
#!/bin/sh
echo "To: root
Subject:Failure (Warning: $2)
$3
" | /var/qmail/bin/qmail-inject -f root
```
Remember to make the script executable by issuing chmod +x /usr/local/sbin/mail\_pwd\_failures.sh

Then uncomment the command line under "Password failures" in /etc/metalog/metalog.conf like:

**`/etc/metalog/metalog.conf`**

**Metalog**

```
command  = "/usr/local/sbin/mail_pwd_failures.sh"
```
[Syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng) provides many of the same features as sysklogd and metalog in a single package that does not run as root. It can filter messages based on level and content (like metalog), provide remote logging (like sysklogd), handle logs from *syslog* (or even Solaris).  In addition to standard log handling features, syslog-ng can write to a TTY, execute programs, and act as a logging server.

Below is a copy of the gentoo-hardened configuration from /usr/share/doc/syslog-ng-4.6.0/syslog-ng.conf.gentoo.hardened.bz2, which can be deployed at /etc/syslog-ng/syslog-ng.conf:

**`/etc/syslog-ng/syslog-ng.conf`**

**Syslog-ng**

```
@version: 4.6
# Copyright 1999-2019 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
# https://bugs.gentoo.org/426814
@include "scl.conf"
#
# Syslog-ng configuration file, compatible with default hardened installations.
#
options {
	threaded(yes);
	chain_hostnames(no);
	stats(freq(43200));
};
source src {
    system();
    internal();
};
source kernsrc {
    file("/proc/kmsg");
};
#source net { udp(); };
#log { source(net); destination(net_logs); };
#destination net_logs { file("/var/log/HOSTS/$HOST/$YEAR$MONTH$DAY.log"); };
destination authlog { file("/var/log/auth.log"); };
destination _syslog { file("/var/log/syslog"); };
destination cron { file("/var/log/cron.log"); };
destination daemon { file("/var/log/daemon.log"); };
destination kern { file("/var/log/kern.log"); };
destination lpr { file("/var/log/lpr.log"); };
destination user { file("/var/log/user.log"); };
destination uucp { file("/var/log/uucp.log"); };
#destination ppp { file("/var/log/ppp.log"); };
destination mail { file("/var/log/mail.log"); };
destination avc { file("/var/log/avc.log"); };
destination audit { file("/var/log/audit.log"); };
destination pax { file("/var/log/pax.log"); };
destination grsec { file("/var/log/grsec.log"); };
destination mailinfo { file("/var/log/mail.info"); };
destination mailwarn { file("/var/log/mail.warn"); };
destination mailerr { file("/var/log/mail.err"); };
destination newscrit { file("/var/log/news/news.crit"); };
destination newserr { file("/var/log/news/news.err"); };
destination newsnotice { file("/var/log/news/news.notice"); };
destination debug { file("/var/log/debug"); };
destination messages { file("/var/log/messages"); };
destination console { usertty("root"); };
destination console_all { file("/dev/tty12"); };
#destination loghost { udp("loghost" port(999)); };
destination xconsole { pipe("/dev/xconsole"); };
filter f_auth { facility(auth); };
filter f_authpriv { facility(auth, authpriv); };
filter f_syslog { not facility(authpriv, mail); };
filter f_cron { facility(cron); };
filter f_daemon { facility(daemon); };
filter f_kern { facility(kern); };
filter f_lpr { facility(lpr); };
filter f_mail { facility(mail); };
filter f_user { facility(user); };
filter f_uucp { facility(uucp); };
#filter f_ppp { facility(ppp); };
filter f_news { facility(news); };
filter f_debug { not facility(auth, authpriv, news, mail); };
filter f_messages { level(info..warn)
	and not facility(auth, authpriv, mail, news); };
filter f_emergency { level(emerg); };
filter f_info { level(info); };
filter f_notice { level(notice); };
filter f_warn { level(warn); };
filter f_crit { level(crit); };
filter f_err { level(err); };
filter f_avc { message(".*avc: .*"); };
filter f_audit { message("^(\\[.*\..*\] |)audit.*") and not message(".*avc: .*"); };
filter f_pax { message("^(\\[.*\..*\] |)PAX:.*"); };
filter f_grsec { message("^(\\[.*\..*\] |)grsec:.*"); };
log { source(src); filter(f_authpriv); destination(authlog); };
log { source(src); filter(f_syslog); destination(_syslog); };
log { source(src); filter(f_cron); destination(cron); };
log { source(src); filter(f_daemon); destination(daemon); };
log { source(kernsrc); filter(f_kern); destination(kern); destination(console_all); };
log { source(src); filter(f_lpr); destination(lpr); };
log { source(src); filter(f_mail); destination(mail); };
log { source(src); filter(f_user); destination(user); };
log { source(src); filter(f_uucp); destination(uucp); };
log { source(kernsrc); filter(f_pax); destination(pax); };
log { source(kernsrc); filter(f_grsec); destination(grsec); };
log { source(kernsrc); filter(f_audit); destination(audit); };
log { source(kernsrc); filter(f_avc); destination(avc); };
log { source(src); filter(f_mail); filter(f_info); destination(mailinfo); };
log { source(src); filter(f_mail); filter(f_warn); destination(mailwarn); };
log { source(src); filter(f_mail); filter(f_err); destination(mailerr); };
log { source(src); filter(f_news); filter(f_crit); destination(newscrit); };
log { source(src); filter(f_news); filter(f_err); destination(newserr); };
log { source(src); filter(f_news); filter(f_notice); destination(newsnotice); };
log { source(src); filter(f_debug); destination(debug); };
log { source(src); filter(f_messages); destination(messages); };
log { source(src); filter(f_emergency); destination(console); };
#log { source(src); filter(f_ppp); destination(ppp); };
log { source(src); destination(console_all); };
```
Of course, keeping logs alone is only half the battle. An application such as Logcheck can make regular log analysis much easier. logcheck is a script, accompanied by a binary called logtail, that runs from the cron daemon and checks the system logs against a set of rules for suspicious activity. It then mails the output to root's mailbox.

logcheck and logtail are part of the [app-admin/logcheck](https://packages.gentoo.org/packages/app-admin/logcheck) package.

Logcheck uses four files to filter important log entries from the unimportant:

- logcheck.hacking - Contains known hacking attack messages.
- logcheck.violations - Contains patterns indicating security violations.
- logcheck.violations.ignore - Contains keywords likely to be matched by the violations file, allowing normal entries to be ignored.
- logcheck.ignore - matches those entries to be ignored.





Controlling system resource usage can be very effective when trying to prevent a local Denial of Service (DoS) or restricting the maximum allowed logins for a group or user. However, settings that are too strict will impede the system's behavior, so make sure each setting is sanity checked before implemented.

**`/etc/security/limits.conf`**

```
*    soft core 0
*    hard core 0
*    hard nproc 15
*    hard rss 10000
*    -    maxlogins 2
@dev hard core 100000
@dev soft nproc 20
@dev hard nproc 35
@dev -    maxlogins 10
```
Considering removing a user before setting `nproc` or `maxlogins` to `0`. The example above sets the group dev settings for processes, core file and maxlogins. The rest are set to the default values.

/etc/limits is very similar to the limit file found at /etc/security/limits.conf. The difference between the files is the format and that it only works on users or wild cards (not groups). Let's have a look at a sample configuration:

**`/etc/limits`**

```
   L2 C0 U15 R10000
kn L10 C100000 U35
```
Here we set the default settings and a specific setting for the user kn. Limits are part of the [sys-apps/shadow](https://packages.gentoo.org/packages/sys-apps/shadow) package. It is not necessary to set any limits in this file if the `pam` USE flag has been enabled in /etc/portage/make.conf.

Putting quotas on a file system restricts disk usage on a per-user or per-group basis. Quotas are enabled in the kernel and added to a mount point in /etc/fstab. The kernel option is enabled in the kernel configuration under File systems → Quota support Apply the following settings, rebuild the kernel and reboot using the new kernel.

Start by installing quotas with emerge sys-fs/quota. Then modify /etc/fstab and add `usrquota` and `grpquota` to the partitions to be restricted, like in the example below:

**`/etc/fstab`**

```
 /boot ext2 noauto,noatime 1 1
/dev/sda2 none swap sw 0 0
/dev/sda3 / ext3 notail,noatime 0 0
/dev/sda4 /tmp ext3 noatime,nodev,nosuid,noexec,usrquota,grpquota 0 0
/dev/sda5 /var ext3 noatime,nodev,usrquota,grpquota 0 0
/dev/sda6 /home ext3 noatime,nodev,nosuid,usrquota,grpquota 0 0
/dev/sda7 /usr ext3 notail,noatime,nodev,ro 0 0
/dev/cdroms/cdrom0 /mnt/cdrom iso9660 noauto,ro 0 0
proc /proc proc defaults 0 0
```
On every partition that have quotas enabled, create the quota files (aquota.user and aquota.group) and place them in the root of the partition:

`root #````
touch /tmp/aquota.user
```
`root #````
touch /tmp/aquota.group
```
`root #````
chmod 600 /tmp/aquota.user
```
`root #````
chmod 600 /tmp/aquota.group
```
This step has to be done on every partition where quotas are enabled.

On OpenRC systems, after adding and configuring the quota files, be sure to add the quota script to the boot run level.

Add quota to the boot runlevel:

`root #``rc-update add quota boot`
Quotas can be checked once a week by adding the following line to /etc/crontab:

**`/etc/crontab`**

```
0 3 * * 0 /usr/sbin/quotacheck -avug.
```
After rebooting, it is time to setup the quotas for users and groups. edquota -u kn will start the editor defined in `EDITOR` environment variable (default is nano) and allow editing quotas of the user kn. edquota -g will do the same thing for groups.

`edquota -u kn`
Quotas for user kn: /dev/sda4: blocks in use: 2594, limits (soft = 5000, hard = 6500)

inodes in use: 356, limits (soft = 1000, hard = 1500)
For more detail read man edquota or the [Quota mini howto](https://www.tldp.org/HOWTO/Quota.html).

If an organizational security policy states that users should change their password every other week, change the value of `PASS_MAX_DAYS` variable to `14` and `PASS_WARN_AGE` variable to `7`. It is recommended password aging be implemented since brute force methods can find any password, given enough time. Sysadmins are also encouraged to set `LOG_OK_LOGINS` variable to `yes`.

The access.conf file is also part of the [sys-libs/pam](https://packages.gentoo.org/packages/sys-libs/pam) package, which provides a login access control table. This table is used to control who can and cannot login based on user name, group name or host name. By default, all users on the system are allowed to login, so the file consists only of comments and examples. Whether securing a server or workstation, we recommend this file be secured so no one other the sysadmin has access to the console.

**`/etc/security/access.conf`**

```
-:ALL EXCEPT wheel sync:console
-:wheel:ALL EXCEPT LOCAL .gentoo.org
```
This will setup login access so members of the wheel group can login locally or from the gentoo.org domain. Maybe too paranoid, but better to be safe than sorry.



Non-administrative users should not have access to configuration files or passwords. An attacker can steal passwords from databases or web sites and use them to deface - or even worse, delete-data. This is why it is important that each system have correct file permissions. If a certain file is only used by root, assign the 0600 permissions with chmod and change the owner to root using chown.

Find world-writable files and directories:

`root #````
find / -type f \( -perm -2 -o -perm -20 \) -exec ls -lg {} \; 2>/dev/null >writable.txt
```
`root #````
find / -type d \( -perm -2 -o -perm -20 \) -exec ls -ldg {} \; 2>/dev/null >>writable.txt
```
This will create a huge file with permission of all files having either write permission set to the group or everybody. Check the permissions and eliminate world writable files to everyone, by executing /bin/chmod o-w on the files.

Files with the SUID or SGID bit set execute with privileges of the owning user or group and not the user executing the file. Normally these bits are used on files that must run as root in order to do what they do. These files can lead to local root compromises (if they contain security holes). This is dangerous and files with the SUID or SGID bits set should be avoided at any cost. If you do not use these files, use chmod 0 on them or unmerge the package that they came from (check which package they belong to by using equery; if you do not already have it installed simply type emerge --ask app-portage/gentoolkit). Otherwise just turn the SUID bit off with chmod -s.

Find setuid files:

`root #``find / -type f \( -perm -004000 -o -perm -002000 \) -exec ls -lg {} \; 2>/dev/null >suidfiles.txt`
This will create a file containing a list of all the SUID/SGID files.

List of setuid binaries:

`root #``cat suidfiles.txt`
/bin/su
/bin/ping
/bin/mount
/bin/umount
/var/qmail/bin/qmail-queue
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/crontab
/usr/bin/chage
/usr/bin/expiry
/usr/bin/sperl5.6.1
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/procmail
/usr/bin/suidperl
/usr/lib/misc/pt\_chown
/usr/sbin/unix\_chkpwd
/usr/sbin/traceroute
/usr/sbin/pwdb\_chkpwd

By default Gentoo does not have a lot of SUID files (though this depends on what has been installed on the system). The list may look similar to the one above. Most of the commands should not be used by normal users, only root. Switch off the SUID bit on system utilities such as ping, mount, umount, chfn, chsh, newgrp, suidperl, pt\_chown, and traceroute by executing chmod -s on every file.

Do not remove the bit on su, qmail-queue, or unix\_chkpwd. Removing setuid from these files will prevent users from su'ing and receiving mail. By removing the bit (where it is safe to do so) will help limit the possibility of a normal user (or an attacker) gaining root access through new vulnerabilities found in any of these executables.

The only SUID files that I have on my system are su, passwd, gpasswd, qmail-queue, unix\_chkpwd and pwdb\_chkpwd.

Note, systems running an X server may have more SUID executables since X needs the elevated access afforded by SUID.

A file is only considered deleted when there are no more links pointing to it. This might sound like a strange concept, but consider that a filename like /usr/bin/perl is actually a link to the inode where the data is stored. Any number of links can point to the file, and until all of them are gone, the file still exists.

If users have access to a partition that is not mounted with `nosuid` or `noexec` (for example, if /tmp, /home, or /var/tmp are not separate partitions) take special care to ensure users do not create hard links to SUID or SGID binaries, so that after Portage updates they still have access to the old versions.

To check how many links a file has, you can use the stat command.

`user $``stat /bin/su`
File: \`/bin/su'
  Size: 29350           Blocks: 64         IO Block: 131072 regular file
Device: 900h/2304d      Inode: 2057419     Links: 1
Access: (4711/-rws--x--x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2005-02-07 01:59:35.000000000 +0000
Modify: 2004-11-04 01:46:17.000000000 +0000
Change: 2004-11-04 01:46:17.000000000 +0000

To find the SUID and SGID files with multiple links, use find:

`user $``find / -type f \( -perm -004000 -o -perm -002000 \) -links +1 -ls`




First install [sys-libs/cracklib](https://packages.gentoo.org/packages/sys-libs/cracklib) to allow password policies to be set:

`root #``emerge --ask sys-libs/cracklib`
**`/etc/pam.d/passwd`**

```
     required pam_unix.so shadow nullok
account  required pam_unix.so
password required pam_cracklib.so difok=3 retry=3 minlen=8 dcredit=-2 ocredit=-2
password required pam_unix.so md5 use_authtok
session  required pam_unix.so
```
This will add the cracklib which will ensure that the user passwords are at least 8 characters and contain a minimum of 2 digits, 2 other characters, and are more than 3 characters different from the last password. The PAM [cracklib documentation](https://man7.org/linux/man-pages/man8/pam_cracklib.8.html) can be reviewed for more available options.

**`/etc/pam.d/sshd`**

```
     required pam_unix.so nullok
auth     required pam_shells.so
auth     required pam_nologin.so
auth     required pam_env.so
account  required pam_unix.so
password required pam_cracklib.so difok=3 retry=3 minlen=8 dcredit=-2 ocredit=-2 use_authtok
password required pam_unix.so shadow md5
session  required pam_unix.so
session  required pam_limits.so
```
Every service not configured with a PAM file in /etc/pam.d will use the rules in /etc/pam.d/other. The defaults are set to deny, as they should be.

Also, pam\_warn.so can be added to generate more elaborate logging. And pam\_limits can be used, which is controlled by /etc/security/limits.conf. See the [/etc/security/limits.conf](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations#.2Fetc.2Fsecurity.2Flimits.conf) section for more on these settings.

**`/etc/pam.d/other`**

```
     required pam_deny.so
auth     required pam_warn.so
account  required pam_deny.so
account  required pam_warn.so
password required pam_deny.so
password required pam_warn.so
session  required pam_deny.so
session  required pam_warn.so
```
- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.



Kerneli was a patch developed in the late 1990s/early 2000s[\[1\]](https://wiki.gentoo.org#cite_note-10)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-11)</sup> which added support for cryptographic ciphers, digest algorithms and cryptographic loop filters, as early versions of the kernel did not contain these due to export regulations. Since the introduction of the Crypto API in version 2.5.45[\[3\]](https://wiki.gentoo.org#cite_note-12)<sup>[\[4\]](https://wiki.gentoo.org#cite_note-13)</sup>, this is only of historical interest now.

Removing whatever is unneeded when configuring the kernel will minimize attack surface, create a more optimized kernel, and reduce the chance for bugs in drivers or other features to be a means of compromise.

If loadable module support is unnecessary (`CONFIG_MODULES=n`), disable it. Though it is still possible to add rootkits without this feature, removing it makes it harder for attackers to install them via kernel modules. For further information see [Kernel\_Modules#Going completely "module-less"](https://wiki.gentoo.org/wiki/Kernel_Modules#Going_completely_.22module-less.22). If modules are needed, the kernel should be set to load only digitally signed modules (see [Signed kernel module support](https://wiki.gentoo.org/wiki/Signed_kernel_module_support)).

Information on kernel lockdown modes is available at the [dedicated page](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security/Kernel_Lockdown).

The Kernel Self-Protection Project now has its own [page](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security/Kernel_Self-Protection_Project) that gives an overview of the project and how to enable the recommended hardening options on Gentoo.

[sysctl](https://wiki.gentoo.org/wiki/Sysctl) can be used to manipulate the /etc/sysctl.conf configuration file.

- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [Kernel Modules](https://wiki.gentoo.org/wiki/Kernel_Modules) — object files that contain code to extend the [kernel](https://wiki.gentoo.org/wiki/Kernel) of an operating system.
- [Signed kernel module support](https://wiki.gentoo.org/wiki/Signed_kernel_module_support) — allows further hardening of the system by disallowing unsigned kernel modules, or kernel modules signed with the wrong key, to be loaded.

- [An abridged history of Linux kernel security](https://www.youtube.com/watch?v=LdcnxIviHuk) — Russell Currey (Everything Open 2023)
- [ASLR-NG: ASLR Next Generation](https://web.archive.org/web/20230306172439/http://cybersecurity.upv.es/solutions/aslr-ng/aslr-ng.html)
- [Kernel Address Space Layout Randomization](https://web.archive.org/web/20180619154220/http://selinuxproject.org/~jmorris/lss2013_slides/cook_kaslr.pdf) — Kees Cook, Linux Security Summit 2013<sup>[\[5\]](https://wiki.gentoo.org#cite_note-14)</sup>
- [Kernel address randomization](https://lwn.net/Articles/444503/) — Jonathan Corbet, 2011
- [Kernel Security Is Cool Again](https://www.youtube.com/watch?v=GFGJ3e3oj2c) — Casey Schaufler, linux.conf.au 2019
- [Overview of the Linux Kernel Security Subsystem](https://www.youtube.com/watch?v=L7KHvKRfTzc) — James Morris, Microsoft
- [Rule Set Based Access Control](https://www.rsbac.org/) (RSBAC)
- [The OpenWall Project](https://www.openwall.com/)

## References



## Firewalls

## Introduction

Firewalls can exist at multiple points throughout a network, and multiple firewall types can often be used simultaneously if there is a desire to minimize the possibility of bugs in a single vendor's software being a source of compromise. However, this increases complexity and cost, and misconfiguration of such a setup could also result in having less security than a single firewall done properly.

### Types

Basically there are three types of firewalls:

- Packet filter.
- Circuit relay.
- Application gateway.

#### Packet filtering

All Internet Protocol traffic is sent in the form of packets. Large amounts of traffic is split up into small packets, so it can be handled by L2 devices, and is reassembled at the destination. The packet header contains information where it should be delivered to - as well as the origin. Packet filtering firewalls use IP header information to make filtering decisions on forwarded packets.

Generally, information such as the following is considered:

- Source/destination IP address
- Source/destination port
- Protocol
- Protocol Flags
- Connection state

Weaknesses:

- Address information in a packet can is not authenticated and can be spoofed.
- Data or requests within the allowed packet may contain unwanted data that the attacker can use to exploit known bugs in the services on or behind the firewall.
- Blocking IPs and ports doesn't provide much more security, but setting allowlists is unwieldy.

Advantages:

- In-kernel in the case of Netfilter based firewalls such as nftables, and firewalls which build on the Netfilter Framework.
- Implementation can be straightforward.
- Can give warnings of a possible attack before it happens (e.g. by detecting port scans).
- Good for stopping SYN attacks.

Examples of current free packet filters on Linux include:

iptables has been included in the kernel since version 2.4.[\[1\]](https://wiki.gentoo.org#cite_note-15)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-16)</sup> Prior to that was ipchains (since 2.1.102[\[3\]](https://wiki.gentoo.org#cite_note-17)[\[4\]](https://wiki.gentoo.org#cite_note-18)<sup>[\[5\]](https://wiki.gentoo.org#cite_note-19)</sup>), and prior had been ipfirewall or ipfw since 1.1,<sup>[\[6\]](https://wiki.gentoo.org#cite_note-20)</sup> ported from BSD<sup>[\[7\]](https://wiki.gentoo.org#cite_note-21)</sup> and still part of FreeBSD today<sup>[\[8\]](https://wiki.gentoo.org#cite_note-22)</sup>.

nftables succeeded iptables in 2014 in version 3.13.<sup>[\[9\]](https://wiki.gentoo.org#cite_note-23)</sup> Current documentation is available at the following links:

- [https://netfilter.org/projects/nftables/](https://netfilter.org/projects/nftables/)
- [https://wiki.nftables.org/wiki-nftables/index.php/Main\_Page](https://wiki.nftables.org/wiki-nftables/index.php/Main_Page)
- [Moving from iptables to nftables](https://wiki.nftables.org/wiki-nftables/index.php/Moving_from_iptables_to_nftables)
- [Why nftables?](https://wiki.nftables.org/wiki-nftables/index.php/What_is_nftables%3F#Why_nftables.3F)

#### Circuit relay

A circuit level gateway is a firewall that validates connections before allowing data to be exchanged. This means that it does not simply allow or deny packets based on the packet header but determines whether the connection between both ends is valid according to configurable rules before it opens a session and allows data to be exchanged. Filtering is based on:

- Source/destination IP address
- Source/destination port
- A period of time
- Protocol
- User
- Password

All traffic is validated and monitored, and unwanted traffic can be dropped.

Weakness:

- Operates at the Transport Layer and may require substantial modification of the programs that normally provide transport functions

#### Application gateway

The application level gateway is a proxy for applications, exchanging data with remote systems on behalf of the clients. It is kept away from the public safely behind a DMZ (De-Militarized Zone: the portion of a private network that is visible through the firewall) or a firewall allowing no connections from the outside. Filtering is based on:

- Allow or disallow based on source/destination IP address.
- Based on the packet's content.
- Limiting file access based on file type or extension.

Advantages:

- Can cache files, increasing network performance.
- Detailed logging of all connections.
- Scales well (some proxy servers can "share" the cached data).
- No direct access from the outside.
- Can even alter the packet content on the fly.

Weakness:

- Configuration is complex.
- Application gateways are considered to be the most secure solution since they do not have to run as root and the hosts behind them are not reachable from the Internet.

Example of a free application gateway:

### iptables

In order to use [iptables](https://wiki.gentoo.org/wiki/Iptables), it must be enabled in the kernel. iptables can be loaded as modules (the iptables command will load them as they are needed) or compiled into the kernel if one intends to disable Loadable Kernel Modules as discussed previously). For more information on how to configure the kernel for iptables go to the iptables Tutorial Chapter 5: Preparations. After you have compiled the kernel (or while compiling the kernel), add the iptables command via emerge iptables.

Now test that it works by running iptables -L. If this fails something is wrong and you have to check your configuration once more.

iptables is the new and heavily improved packet filter in the Linux 2.4.x kernel. It is the successor of the previous ipchains packet filter in the Linux 2.2.x kernel. One of the major improvements is that iptables is able to perform stateful packet filtering. With stateful packet filtering it is possible to keep track of each established TCP connection.

A TCP connection consists of a series of packets containing information about source IP address, destination IP address, source port, destination port, and a sequence number so the packets can be reassembled without losing data. TCP is a connection-oriented protocol, in contrast to UDP, which is connectionless.

By examining the TCP packet header, a stateful packet filter can determine if a received TCP packet is part of an already established connection or not and decide either to accept or drop the packet.

With a stateless packet filter it is possible to fool the packet filter into accepting packets that should be dropped by manipulating the TCP packet headers. This could be done by manipulating the SYN flag or other flags in the TCP header to make a malicious packet appear to be a part of an established connection (since the packet filter itself does not do connection tracking). With stateful packet filtering it is possible to drop such packets, as they are not part of an already established connection. This will also stop the possibility of "stealth scans", a type of port scan in which the scanner sends packets with flags that are far less likely to be logged by a firewall than ordinary SYN packets.

iptables provides several other features like NAT (Network Address Translation) and rate limiting. Rate limiting is extremely useful when trying to prevent certain DoS (Denial of Service) attacks like SYN floods.

A TCP connection is established by a so called three-way handshake. When establishing a TCP connection the client-side sends a packet to the server with the SYN flag set. When the server-side receives the SYN packet it responds by sending a SYN+ACK packet back to the client-side. When the SYN+ACK is received the client-side responds with a third ACK packet in effect acknowledging the connection.

A SYN flood attack is performed by sending the SYN packet but failing to respond to the SYN+ACK packet. The client-side can forge a packet with a fake source IP address because it does not need a reply. The server-side system will add an entry to a queue of half-open connections when it receives the SYN packet and then wait for the final ACK packet before deleting the entry from the queue. The queue has a limited number of slots and if all the slots are filled it is unable to open any further connections. If the ACK packet is not received before a specified timeout period the entry will automatically be deleted from the queue. The timeout settings vary but will typically be 30-60 seconds or even more. The client-side initiates the attack by forging a lot of SYN packets with different source IP addresses and sends them to the target IP address as fast as possible and thereby filling up the queue of half-open connections and thus preventing other clients from establishing a legitimate connection with the server.

This is where the rate limit becomes handy. It is possible to limit the rate of accepted SYN packets by using the -m limit --limit 1/s. This will limit the number of SYN packets accepted to one per second and therefore restricting the SYN flood on our resources.

Now some practical stuff!

When iptables is loaded in the kernel it has 5 hooks where you can place your rules. They are called INPUT, OUTPUT, FORWARD, PREROUTING and POSTROUTING. Each of these is called a chain and consists of a list of rules. Each rule says if the packet header looks like this, then here is what to do with the packet. If the rule does not match the packet the next rule in the chain is consulted.

You can place rules directly in the 5 main chains or create new chains and add them to as a rule to an existing chain. iptables supports the following options:

First we will try to block all ICMP packets to our machine, just to get familiar with iptables.

Block all ICMP packets:

`root #``iptables -A INPUT -p icmp -j DROP`
First we specify the chain our rule should be appended to, then the protocol of the packets to match, and finally the target. The target can be the name of a user specified chain or one of the special targets `ACCEPT`, `DROP`, `REJECT`, `LOG`, `QUEUE`, or `MASQUERADE`. In this case we use `DROP`, which will drop the packet without responding to the client.

Now try ping localhost. You will not get any response, since iptables will drop all incoming ICMP messages. You will also not be able to ping other machines, since the ICMP reply packet will be dropped as well. Now flush the chain to get ICMP flowing again:

`root #``iptables -F`
Now lets look at the stateful packet filtering in iptables. If we wanted to enable stateful inspection of packets incoming on eth0 we would issue the command:

`root #``iptables -A INPUT -i eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT`
This will accept any packet from an already established connection or related in the INPUT chain. And you could drop any packet that is not in the state table by issuing iptables -A INPUT -i eth0 -m state --state INVALID -j DROP just before the previous command. This enables the stateful packet filtering in iptables by loading the extension "state". If you wanted to allow others to connect to your machine, you could use the flag --state NEW. iptables contains some modules for different purposes. Some of them are:

| Module/Match | Description | Extended options | 
|---|---|---|
| mac | Matching extension for incoming packets mac address. | --mac-source | 
| state | Enables stateful inspection | --state (states are ESTABLISHED,RELATED, INVALID, NEW) | 
| limit | Rate matching limiting | --limit, --limit-burst | 
| owner | Attempt to match various characteristics of the packet creator | --uid-owner userid --gid-owner groupid --pid-owner processid --sid-owner sessionid | 
| unclean | Various random sanity checks on packets | Example | 

Lets try to create a user-defined chain and apply it to one of the existing chains.

First create a new chain with one rule:

`root #````
iptables -X mychain
```
`root #````
iptables -N mychain
```
`root #````
iptables -A mychain -i eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT
```
The default policy is all outgoing traffic is allowed. Incoming is dropped:

`root #````
iptables -P OUTPUT ACCEPT
```
`root #````
iptables -P INPUT DROP
```
And add it to the INPUT chain:

`root #````
iptables -A INPUT -j mychain
```
By applying the rule to the input chain we get the policy: All outgoing packets are allowed and all incoming packets are dropped.

One can find documentation at Netfilter/iptables documentation.

Lets see a full blown example. In this case my firewall/gateway policy states:

- Connections to the firewall are only allowed through SSH (port 22).
- The local network should have access to HTTP, HTTPS and SSH (DNS should also be allowed).
- ICMP traffic can contain payload and should not be allowed. Of course we have to allow some ICMP traffic.
- Port scans should be detected and logged.
- SYN attacks should be avoided.
- All other traffic should be dropped and logged.

**`/etc/init.d/firewall`**

```
#!/sbin/openrc-run
IPTABLES=/sbin/iptables
IPTABLESSAVE=/sbin/iptables-save
IPTABLESRESTORE=/sbin/iptables-restore
FIREWALL=/etc/firewall.rules
DNS1=212.242.40.3
DNS2=212.242.40.51
#inside
IIP=10.0.0.2
IINTERFACE=eth0
LOCAL_NETWORK=10.0.0.0/24
#outside
OIP=217.157.156.144
OINTERFACE=eth1
opts="${opts} showstatus panic save restore showoptions rules"
depend() {
  need net
}
rules() {
  stop
  ebegin "Setting internal rules"
  einfo "Setting default rule to drop"
  $IPTABLES -P FORWARD DROP
  $IPTABLES -P INPUT   DROP
  $IPTABLES -P OUTPUT  DROP
  #default rule
  einfo "Creating states chain"
  $IPTABLES -N allowed-connection
  $IPTABLES -F allowed-connection
  $IPTABLES -A allowed-connection -m state --state ESTABLISHED,RELATED -j ACCEPT
  $IPTABLES -A allowed-connection -i $IINTERFACE -m limit -j LOG --log-prefix \
      "Bad packet from ${IINTERFACE}:"
  $IPTABLES -A allowed-connection -j DROP
  #ICMP traffic
  einfo "Creating icmp chain"
  $IPTABLES -N icmp_allowed
  $IPTABLES -F icmp_allowed
  $IPTABLES -A icmp_allowed -m state --state NEW -p icmp --icmp-type \
      time-exceeded -j ACCEPT
  $IPTABLES -A icmp_allowed -m state --state NEW -p icmp --icmp-type \
      destination-unreachable -j ACCEPT
  $IPTABLES -A icmp_allowed -p icmp -j LOG --log-prefix "Bad ICMP traffic:"
  $IPTABLES -A icmp_allowed -p icmp -j DROP
  #Incoming traffic
  einfo "Creating incoming ssh traffic chain"
  $IPTABLES -N allow-ssh-traffic-in
  $IPTABLES -F allow-ssh-traffic-in
  #Flood protection
  $IPTABLES -A allow-ssh-traffic-in -m limit --limit 1/second -p tcp --tcp-flags \
      ALL RST --dport ssh -j ACCEPT
  $IPTABLES -A allow-ssh-traffic-in -m limit --limit 1/second -p tcp --tcp-flags \
      ALL FIN --dport ssh -j ACCEPT
  $IPTABLES -A allow-ssh-traffic-in -m limit --limit 1/second -p tcp --tcp-flags \
      ALL SYN --dport ssh -j ACCEPT
  $IPTABLES -A allow-ssh-traffic-in -m state --state RELATED,ESTABLISHED -p tcp --dport ssh -j ACCEPT
  #outgoing traffic
  einfo "Creating outgoing ssh traffic chain"
  $IPTABLES -N allow-ssh-traffic-out
  $IPTABLES -F allow-ssh-traffic-out
  $IPTABLES -A allow-ssh-traffic-out -p tcp --dport ssh -j ACCEPT
  einfo "Creating outgoing dns traffic chain"
  $IPTABLES -N allow-dns-traffic-out
  $IPTABLES -F allow-dns-traffic-out
  $IPTABLES -A allow-dns-traffic-out -p udp -d $DNS1 --dport domain \
      -j ACCEPT
  $IPTABLES -A allow-dns-traffic-out -p udp -d $DNS2 --dport domain \
     -j ACCEPT
  einfo "Creating outgoing http/https traffic chain"
  $IPTABLES -N allow-www-traffic-out
  $IPTABLES -F allow-www-traffic-out
  $IPTABLES -A allow-www-traffic-out -p tcp --dport www -j ACCEPT
  $IPTABLES -A allow-www-traffic-out -p tcp --dport https -j ACCEPT
  #Catch portscanners
  einfo "Creating portscan detection chain"
  $IPTABLES -N check-flags
  $IPTABLES -F check-flags
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL FIN,URG,PSH -m limit \
      --limit 5/minute -j LOG --log-level alert --log-prefix "NMAP-XMAS:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL FIN,URG,PSH -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL ALL -m limit --limit \
      5/minute -j LOG --log-level 1 --log-prefix "XMAS:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL ALL -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL SYN,RST,ACK,FIN,URG \
      -m limit --limit 5/minute -j LOG --log-level 1 --log-prefix "XMAS-PSH:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL SYN,RST,ACK,FIN,URG -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL NONE -m limit \
      --limit 5/minute -j LOG --log-level 1 --log-prefix "NULL_SCAN:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL NONE -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,RST SYN,RST -m limit \
      --limit 5/minute -j LOG --log-level 5 --log-prefix "SYN/RST:"
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,RST SYN,RST -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,FIN SYN,FIN -m limit \
      --limit 5/minute -j LOG --log-level 5 --log-prefix "SYN/FIN:"
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,FIN SYN,FIN -j DROP
  # Apply and add invalid states to the chains
  einfo "Applying chains to INPUT"
  $IPTABLES -A INPUT -m state --state INVALID -j DROP
  $IPTABLES -A INPUT -p icmp -j icmp_allowed
  $IPTABLES -A INPUT -j check-flags
  $IPTABLES -A INPUT -i lo -j ACCEPT
  $IPTABLES -A INPUT -j allow-ssh-traffic-in
  $IPTABLES -A INPUT -j allowed-connection
  einfo "Applying chains to FORWARD"
  $IPTABLES -A FORWARD -m state --state INVALID -j DROP
  $IPTABLES -A FORWARD -p icmp -j icmp_allowed
  $IPTABLES -A FORWARD -j check-flags
  $IPTABLES -A FORWARD -o lo -j ACCEPT
  $IPTABLES -A FORWARD -j allow-ssh-traffic-in
  $IPTABLES -A FORWARD -j allow-www-traffic-out
  $IPTABLES -A FORWARD -j allowed-connection
  einfo "Applying chains to OUTPUT"
  $IPTABLES -A OUTPUT -m state --state INVALID -j DROP
  $IPTABLES -A OUTPUT -p icmp -j icmp_allowed
  $IPTABLES -A OUTPUT -j check-flags
  $IPTABLES -A OUTPUT -o lo -j ACCEPT
  $IPTABLES -A OUTPUT -j allow-ssh-traffic-out
  $IPTABLES -A OUTPUT -j allow-dns-traffic-out
  $IPTABLES -A OUTPUT -j allow-www-traffic-out
  $IPTABLES -A OUTPUT -j allowed-connection
  #Allow client to route through via NAT (Network Address Translation)
  $IPTABLES -t nat -A POSTROUTING -o $OINTERFACE -j MASQUERADE
  eend $?
}
start() {
  ebegin "Starting firewall"
  if [ -e "${FIREWALL}" ]; then
    restore
  else
    einfo "${FIREWALL} does not exists. Using default rules."
    rules
  fi
  eend $?
}
stop() {
  ebegin "Stopping firewall"
  $IPTABLES -F
  $IPTABLES -t nat -F
  $IPTABLES -X
  $IPTABLES -P FORWARD ACCEPT
  $IPTABLES -P INPUT   ACCEPT
  $IPTABLES -P OUTPUT  ACCEPT
  eend $?
}
showstatus() {
  ebegin "Status"
  $IPTABLES -L -n -v --line-numbers
  einfo "NAT status"
  $IPTABLES -L -n -v --line-numbers -t nat
  eend $?
}
panic() {
  ebegin "Setting panic rules"
  $IPTABLES -F
  $IPTABLES -X
  $IPTABLES -t nat -F
  $IPTABLES -P FORWARD DROP
  $IPTABLES -P INPUT   DROP
  $IPTABLES -P OUTPUT  DROP
  $IPTABLES -A INPUT -i lo -j ACCEPT
  $IPTABLES -A OUTPUT -o lo -j ACCEPT
  eend $?
}
save() {
  ebegin "Saving Firewall rules"
  $IPTABLESSAVE > $FIREWALL
  eend $?
}
restore() {
  ebegin "Restoring Firewall rules"
  $IPTABLESRESTORE < $FIREWALL
  eend $?
}
restart() {
  svc_stop; svc_start
}
showoptions() {
  echo "Usage: $0 {start|save|restore|panic|stop|restart|showstatus}"
  echo "start)      will restore setting if exists else force rules"
  echo "stop)       delete all rules and set all to accept"
  echo "rules)      force settings of new rules"
  echo "save)       will store settings in ${FIREWALL}"
  echo "restore)    will restore settings from ${FIREWALL}"
  echo "showstatus) Shows the status"
}
```
Some advice when creating a firewall:

1. Create your firewall policy before implementing it.
2. Keep it simple.
3. Know how each protocol works (read the relevant [RFC](http://www.ietf.org/) (request For comments))
4. Keep in mind that a firewall is just another piece of software running as root.
5. Test your firewall.

If you think that iptables is hard to understand or takes to long to setup a decent firewall you could use [Shorewall](http://www.shorewall.net/). It basically uses iptables to generate firewall rules, but concentrates on rules and not specific protocols.

## Proxies

### Squid

[Squid](https://wiki.gentoo.org/wiki/Squid) is a very powerful proxy server. It can filter traffic based on time, regular expressions on path/URI, source and destination IP addresses, domain, browser, authenticated user name, MIME type, and port number (protocol). I probably forgot some features, but it can be hard to cover the entire list right here.

In the following example I have added a banner filter instead of a filter based on porn sites. The reason for this is that Gentoo.org should not be listed as some porn site. And I do not want to waste my time trying to find some good sites for you.

In this case, my policy states:

- Surfing (HTTP/HTTPS) is allowed during work hours (Mon-Fri 8-17 and Sat 8-13), but if employees are here late they should work, not surf
- Downloading files is not allowed (.exe, .com, .arj, .zip, .asf, .avi, .mpg, .mpeg, etc.)
- We do not like banners, so they are filtered and replaced with a transparent gif (this is where you get creative!).
- All other connections to and from the Internet are denied.

This is implemented in 4 easy steps:

**`/etc/squid/squid.conf`**

```
# Bind to a ip and port
http_port 10.0.2.1:3128
# Standard configuration
hierarchy_stoplist cgi-bin ?
acl QUERY urlpath_regex cgi-bin \?
no_cache deny QUERY
# Add basic access control lists
acl all src 0.0.0.0/0.0.0.0
acl manager proto cache_object
acl localhost src 127.0.0.1/255.255.255.255
# Add who can access this proxy server
acl localnet src 10.0.0.0/255.255.0.0
# And ports
acl SSL_ports port 443
acl Safe_ports port 80
acl Safe_ports port 443
acl purge method PURGE
# Add access control list based on regular
# expressions within urls
acl archives urlpath_regex "/etc/squid/files.acl"
acl url_ads url_regex "/etc/squid/banner-ads.acl"
# Add access control list based on time and day
acl restricted_weekdays time MTWHF 8:00-17:00
acl restricted_weekends time A 8:00-13:00
acl CONNECT method CONNECT
#allow manager access from localhost
http_access allow manager localhost
http_access deny manager
# Only allow purge requests from localhost
http_access allow purge localhost
http_access deny purge
# Deny requests to unknown ports
http_access deny !Safe_ports
# Deny CONNECT to other than SSL ports
http_access deny CONNECT !SSL_ports
# My own rules
# Add a page do be displayed when
# a banner is removed
deny_info NOTE_ADS_FILTERED url_ads
# Then deny them
http_access deny url_ads
# Deny all archives
http_access deny archives
# Restrict access to work hours
http_access allow localnet restricted_weekdays
http_access allow localnet restricted_weekends
# Deny the rest
http_access deny all
```
Next fill in the files you do not want your users to download files. I have added zip, viv, exe, mp3, rar, ace, avi, mov, mpg, mpeg, au, ra, arj, tar, gz, and z files.

**`/etc/squid/files.acl`**

```
\.[Zz][Ii][pP]$
\.[Vv][Ii][Vv].*
\.[Ee][Xx][Ee]$
\.[Mm][Pp]3$
\.[Rr][Aa][Rr]$
\.[Aa][Cc][Ee]$
\.[Aa][Ss][Ff]$
\.[Aa][Vv][Ii]$
\.[Mm][Oo][Vv]$
\.[Mm][Pp][Gg]$
\.[Mm][Pp][Ee][Gg]$
\.[Aa][Uu]$
\.[Rr][Aa]$
\.[Aa][Rr][Jj]$
\.[Tt][Aa][Rr]$
\.[Gg][Zz]$
\.[Zz]$
```


Next we add the regular expressions for identifying banners. You will probably be a lot more creative than I:

**`/etc/squid/banner-ads.acl`**

```
/adv/.*\.gif$
/[Aa]ds/.*\.gif$
/[Aa]d[Pp]ix/
/[Aa]d[Ss]erver
/[Aa][Dd]/.*\.[GgJj][IiPp][FfGg]$
/[Bb]annerads/
/adbanner.*\.[GgJj][IiPp][FfGg]$
/images/ad/
/reklame/
/RealMedia/ads/.*
^http://www\.submit-it.*
^http://www\.eads.*
^http://ads\.
^http://ad\.
^http://ads02\.
^http://adaver.*\.
^http://adforce\.
adbot\.com
/ads/.*\.gif.*
_ad\..*cgi
/Banners/
/SmartBanner/
/Ads/Media/Images/
^http://static\.wired\.com/advertising/
^http://*\.dejanews\.com/ads/
^http://adfu\.blockstackers\.com/
^http://ads2\.zdnet\.com/adverts
^http://www2\.burstnet\.com/gifs/
^http://www.\.valueclick\.com/cgi-bin/cycle
^http://www\.altavista\.com/av/gifs/ie_horiz\.gif
```

And as the last part we want this file to be displayed when a banner is removed. It is basically a half html file with a 4x4 transparent gif image.

**`/etc/squid/errors/NOTE_ADS_FILTERED`**

```
<HTML>
<HEAD>
<META HTTP-EQUIV="REFRESH" CONTENT="0; URL=http://localhost/images/4x4.gif">
<TITLE>ERROR: The requested URL could not be retrieved</TITLE>
</HEAD>
<BODY>
<H1>Add filtered!</H1>
```
As you can see, Squid has a lot of possibilities and it is very effective at both filtering and proxying. It can even use alternative Squid proxies to scale on very large networks. The configuration I have listed here is mostly suited for a small network with 1-20 users.

But combining the packet filter (iptables) and the application gateway (Squid) is probably the best solution, even if Squid is located somewhere safe and nobody can access it from the outside. We still need to be concerned about attacks from the inside.

Now you have to configure your clients browsers to use the proxy server. The gateway will prevent the users from having any contact with the outside unless they use the proxy.

It can also be done transparently by using iptables to forward all outbound traffic to a Squid proxy. This can be done by adding a forwarding/prerouting rule on the gateway:

`root #````
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to proxyhost:3128
```
`root #````
iptables -t nat -A PREROUTING -p tcp --dport 443 -j DNAT --to proxyhost:3128
```
We have learned that:

1. A firewall can be a risk in itself. A badly configured firewall is worse than not having one at all.
2. How to setup a basic gateway and a transparent proxy.
3. The key to a good firewall is to know the protocols you want do allow.
4. That IP traffic does not always contain legitimate data, e.g. ICMP packets, which can contain a malicious payload.
5. How to prevent SYN attack.
6. Filtering HTTP traffic by removing offensive pictures and downloads of viruses.
7. Combining packet filters and application gateways provides better control.

Now, if you really need to, go create a firewall that matches your needs.

## The proc filesystem

Many kernel parameters can be altered through the [/proc file system](https://wiki.gentoo.org/wiki/Procfs) or by using sysctl.

To dynamically change kernel parameters and variables on the fly, you need `CONFIG_SYSCTL` enabled in the kernel. This is on by default in a standard 4.0+ kernel.

Deactivate IP forwarding:

`root #``/bin/echo "0" > /proc/sys/net/ipv4/ip_forward`
Make sure that IP forwarding is turned off. We only want this for a multi-homed host. It's advised to set or unset this flag before all other flags since it enabled/disables other flags as well.

Drop ping packets:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/icmp_echo_ignore_all`
This will cause the kernel to simply ignore all ping messages (also known as ICMP type 0 messages). The reason for this is that an IP packet carrying an ICMP message can contain a payload with information other than you think. Administrators use ping as a diagnostic tool and often complain if it is disabled, but there is no reason for an outsider to be able to ping. However, since it sometimes can be handy for insiders to be able to ping, you can disable ICMP type 0 messages in the firewall (allowing local administrators to continue to use this tool).

Ignore broadcast pings:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts`
This disables response to ICMP broadcasts and will prevent Smurf attacks. The Smurf attack works by sending an ICMP type 0 (ping) message to the broadcast address of a network. Typically the attacker will use a spoofed source address. All the computers on the network will respond to the ping message and thereby flood the host at the spoofed source address.

Disable source routed packets:

`root #``/bin/echo "0" > /proc/sys/net/ipv4/conf/all/accept_source_route`
Do not accept source routed packets. Attackers can use source routing to generate traffic pretending to originate from inside your network, but that is actually routed back along the path from which it came, so attackers can compromise your network. Source routing is rarely used for legitimate purposes, so it is safe to disable it.

Disable redirect acceptance:

`root #``/bin/echo "0" > /proc/sys/net/ipv4/conf/all/accept_redirects`
Do not accept ICMP redirect packets. ICMP redirects can be used to alter your routing tables, possibly to a malicious end.

Enable protection against bad/bogus error message responses:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/icmp_ignore_bogus_error_responses`
Disable [TCP timestamps](https://www.whonix.org/wiki/Disable_TCP_and_ICMP_Timestamps):

`root #``/bin/echo "0" > /proc/sys/net/ipv4/tcp_timestamps`
Enable reverse path filtering:

`root #````
for i in /proc/sys/net/ipv4/conf/*; do
```
/bin/echo "1" > $i/rp_filter

done
Turn on reverse path filtering. This helps make sure that packets use legitimate source addresses by automatically rejecting incoming packets if the routing table entry for their source address does not match the network interface they are arriving on. This has security advantages because it prevents IP spoofing. We need to enable it for each net/ipv4/conf/\* otherwise source validation isn't fully functional.

Log spoofed packets, source routed packets and redirect packets:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/conf/all/log_martians`
All these settings will be reset when the machine is rebooted. I suggest that you add them to /etc/sysctl.conf, which is automatically sourced by the /etc/init.d/bootmisc init script.

The syntax for /etc/sysctl.conf is pretty straightforward. Strip off the /proc/sys/ from the previously mentioned paths and substitute / with .:

**`/etc/sysctl.conf`**

```
net.ipv4.ip_forward = 0
```
The following command will show you *all* actual settings of the /etc/sysctl.conf in a ready to use manner:

`root #``/usr/sbin/sysctl -a`
\<!--snip-->
net.ipv4.conf.all.accept\_local = 0
net.ipv4.conf.all.accept\_redirects = 0
net.ipv4.conf.all.accept\_source\_route = 0
net.ipv4.conf.all.arp\_accept = 0
net.ipv4.conf.all.arp\_announce = 0
net.ipv4.conf.all.arp\_filter = 0
net.ipv4.conf.all.arp\_ignore = 0
net.ipv4.conf.all.arp\_notify = 0
net.ipv4.conf.all.bc\_forwarding = 0
net.ipv4.conf.all.bootp\_relay = 0
net.ipv4.conf.all.disable\_policy = 0
net.ipv4.conf.all.disable\_xfrm = 0
net.ipv4.conf.all.drop\_gratuitous\_arp = 0
net.ipv4.conf.all.drop\_unicast\_in\_l2\_multicast = 0
net.ipv4.conf.all.force\_igmp\_version = 0
net.ipv4.conf.all.forwarding = 0
net.ipv4.conf.all.igmpv2\_unsolicited\_report\_interval = 10000
net.ipv4.conf.all.igmpv3\_unsolicited\_report\_interval = 1000
net.ipv4.conf.all.ignore\_routes\_with\_linkdown = 0
net.ipv4.conf.all.log\_martians = 1
net.ipv4.conf.all.mc\_forwarding = 0
net.ipv4.conf.all.medium\_id = 0
net.ipv4.conf.all.promote\_secondaries = 0
net.ipv4.conf.all.proxy\_arp = 0
net.ipv4.conf.all.proxy\_arp\_pvlan = 0
net.ipv4.conf.all.route\_localnet = 0
net.ipv4.conf.all.rp\_filter = 1
net.ipv4.conf.all.secure\_redirects = 1
net.ipv4.conf.all.send\_redirects = 1
net.ipv4.conf.all.shared\_media = 1
net.ipv4.conf.all.src\_valid\_mark = 0
net.ipv4.conf.all.tag = 0
net.ipv4.conf.default.accept\_local = 0
net.ipv4.conf.default.accept\_redirects = 1
net.ipv4.conf.default.accept\_source\_route = 1
net.ipv4.conf.default.arp\_accept = 0
net.ipv4.conf.default.arp\_announce = 0
net.ipv4.conf.default.arp\_filter = 0
net.ipv4.conf.default.arp\_ignore = 0
net.ipv4.conf.default.arp\_notify = 0
net.ipv4.conf.default.bc\_forwarding = 0
net.ipv4.conf.default.bootp\_relay = 0
net.ipv4.conf.default.disable\_policy = 0
net.ipv4.conf.default.disable\_xfrm = 0
net.ipv4.conf.default.drop\_gratuitous\_arp = 0
net.ipv4.conf.default.drop\_unicast\_in\_l2\_multicast = 0
net.ipv4.conf.default.force\_igmp\_version = 0
net.ipv4.conf.default.forwarding = 0
net.ipv4.conf.default.igmpv2\_unsolicited\_report\_interval = 10000
net.ipv4.conf.default.igmpv3\_unsolicited\_report\_interval = 1000
net.ipv4.conf.default.ignore\_routes\_with\_linkdown = 0
net.ipv4.conf.default.log\_martians = 0
net.ipv4.conf.default.mc\_forwarding = 0
net.ipv4.conf.default.medium\_id = 0
net.ipv4.conf.default.promote\_secondaries = 0
net.ipv4.conf.default.proxy\_arp = 0
net.ipv4.conf.default.proxy\_arp\_pvlan = 0
net.ipv4.conf.default.route\_localnet = 0
net.ipv4.conf.default.rp\_filter = 0
net.ipv4.conf.default.secure\_redirects = 1
net.ipv4.conf.default.send\_redirects = 1
net.ipv4.conf.default.shared\_media = 1
net.ipv4.conf.default.src\_valid\_mark = 0
net.ipv4.conf.default.tag = 0
\<!--snip-->

## See also

## External resources

## References



## Securing services

### Console usage

The /etc/securetty file allows system administrators to specify which TTY (terminal) devices the root user can use to login.

It is suggested to comment out all lines except `vc/1` on system using using devfs and all lines except `tty1` when using udev. This will ensure the root user only can login once and only on one terminal.

**`/etc/securetty`**

### Apache

Apache comes with a pretty decent configuration file. From a security perspective, some things can be improved. Binding Apache to one network interface' IP address and preventing it from volunteering information are two steps that can be taken to harden Apache.

If the `ssl` USE flag was not disabled before emerging Apache the server should be SSL enabled. Inside the /etc/apache2/vhosts.d/ directory example configuration files can be found. These are working examples and it is best to verify or disable them.

It is important to define configuration(s) to listen to a particular IP address (rather than all available IP addresses on the system). For instance the 00\_default\_vhost.conf file:

**`/etc/apache2/vhosts.d/00_default_vhost.conf`**

```
# Make it listen on a single network interface's IP address
Listen 127.0.0.1
```
We also recommend you to disable showing any information about your Apache installation to the world. By default, the configuration will add server version and virtual host name to server-generated pages. To disable this, change the `ServerSignature` variable to `Off`:

**`/etc/apache2/vhosts.d/00_default_vhost.conf`**

```
ServerSignature Off
```
Apache is compiled with `--enable-shared=max` and `--enable-module=all`. This will by default enable all modules, so you should comment out all modules in the LoadModule section (LoadModule and AddModule) that you do not use in the main /etc/apache2/httpd.conf configuration file. When using OpenRC, restart the service by executing /etc/init.d/apache2 restart.

Documentation is available at [https://www.apache.org](https://www.apache.org).

### Bind

One can find [Bind](https://wiki.gentoo.org/wiki/BIND) documentation at the [https://www.isc.org/bind-9-11-arm/](https://www.isc.org/). The [BIND 9 Administrator Reference Manual](https://www.isc.org/downloads/bind/doc/) is also in the doc/arm.

The newer [net-dns/bind](https://packages.gentoo.org/packages/net-dns/bind) BIND ebuilds support chrooting out of the box. After emerging bind follow the simple instructions.

### Djbdns

Djbdns is a DNS implementation on the security of which its author is willing to [bet money](https://cr.yp.to/djbdns/guarantee.html). It is very different from how Bind 9 works but worth a try. More information can be obtained from [https://cr.yp.to/djbdns.html](https://cr.yp.to/djbdns.html)

### FTP

Generally, using FTP (File Transfer Protocol) is a bad idea. It uses unencrypted data (ie. passwords are sent in clear text), listens on 2 ports (normally port 20 and 21), and attackers are frequently looking for anonymous logins for trading warez. Since the FTP protocol contains several security problems you should instead use [sftp](https://wiki.gentoo.org/wiki/Sftp) or HTTP. If this is not possible, secure your services as well as you can and prepare yourself.

#### Proftpd

Proftpd has had several security problems, but most of them seem to have been fixed. Nonetheless, it is a good idea to apply some enhancements:

**`/etc/proftpd/proftpd.conf`**

```
ServerName "My ftp daemon"
# Do not show the identity of the server
ServerIdent on "Go away"
# Makes it easier to create virtual users
RequireValidShell off
# Use alternative password and group file (passwd uses crypt format)
AuthUserFile "/etc/proftpd/passwd"
AuthGroupFile "/etc/proftpd/group"
# Permissions
Umask 077
# Timeouts and limitations
MaxInstances 30
MaxClients 10 "Only 10 connections allowed"
MaxClientsPerHost 1 "You have already logged on once"
MaxClientsPerUser 1 "You have already logged on once"
TimeoutStalled 10
TimeoutNoTransfer 20
TimeoutLogin 20
# Chroot everyone
DefaultRoot ~
# Do not run as root
User  nobody
Group nogroup
# Log every transfer
TransferLog /var/log/transferlog
# Problems with globbing
DenyFilter \*.*/
```
One can find documentation at [http://www.proftpd.org](http://www.proftpd.org).

#### Pure-ftpd

Pure-ftpd is an branch of the original trollftpd, modified for security reasons and functionality by Frank Dennis.

Use virtual users (never system accounts) by enabling the `AUTH` option. Set this to `-lpuredb:/etc/pureftpd.pdb` and create your users by using /usr/bin/pure-pw.

**`/etc/conf.d/pure-ftpd`**

```
AUTH="-lpuredb:/etc/pureftpd.pdb"
## Misc. Others ##
MISC_OTHER="-A -E -X -U 177:077 -d -4 -L100:5 -I 15"
```
Configure the  `MISC_OTHER` setting to deny anonymous logins (`-E`), chroot everyone (`-A`), prevent users from reading or writing to files beginning with a . (dot) (`-X`), max idle time (`-I`), limit recursion (`-L`), and a reasonable umask value.

Warning: Do not use the `-w` or `-W` options! If you want to have a warez site, stop reading this guide!

One can find documentation at [http://www.pureftpd.org](http://www.pureftpd.org).

#### Vsftpd

[Vsftpd](https://wiki.gentoo.org/wiki/Vsftpd) (short for very secure ftp) is a small ftp daemon running a reasonably default configuration. It is simple and does not have as many features as pureftp and proftp.

**`/etc/vsftpd`**

```
anonymous_enable=NO
local_enable=YES
#read only
write_enable=NO
#enable logging of transfers
xferlog_std_format=YES
idle_session_timeout=20
data_connection_timeout=20
nopriv_user=nobody
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chrootlist
ls_recurse_enable=NO
```
As you can see, there is no way for this service to have individual permissions, but when it comes to anonymous settings it is quite good. Sometimes it can be nice to have an anonymous ftp server (for sharing open source), and vsftpd does a really good job at this.

### MySQL

If you only need local applications to access the mysql database, uncomment the following line in /etc/mysql/my.cnf.

Disable network access:

**`/etc/mysql/my.cnf`**

Then we disable the use of the LOAD DATA LOCAL INFILE command. This is to prevent against unauthorized reading from local files. This is relevant when new SQL Injection vulnerabilities in PHP applications are found.

Disable LOAD DATA LOCAL INFILE in the \[mysqld\] section:

**`/etc/mysql/my.cnf`**

```
=local-infile=0
```
Next, we must remove the sample database (test) and all accounts except the local root account.

Removing sample database and all unnecessary users:

`mysql>````
drop database test;
```
`mysql>````
use mysql;
```
`mysql>````
delete from db;
```
`mysql>````
flush privileges;
```
### Netqmail

[Netqmail](https://wiki.gentoo.org/wiki/Netqmail) is often considered to be a very secure mail server. It is written with security (and paranoia) in mind. It does not allow relaying by default and has not had a security hole since 1996. Simply emerge netqmail and go configure!

### Samba

[Samba](https://wiki.gentoo.org/wiki/Samba) is a protocol to share files with Microsoft/Novell networks and it should not be used over the Internet. Nonetheless, it still needs securing.

**`/etc/samba/smb.conf`**

```
[global]
  # Bind to an interface
  interfaces = lo eth0 10.0.0.1/32
  # Only bind to listed interfaces 
  # (don't bind smbd to 0.0.0.0, make nmbd ignore martian broadcast sources) 
  bind interfaces only = yes
  directory security mask = 0700
  # allow traffic from 10.0.0.*
  hosts allow = 10.0.0.
  # Enables user authentication
  # (don't use the share mode)
  security = user
  # Disallow privileged accounts
  invalid users = root @wheel
  # Maximum size smb shows for a share (not a limit)
  max disk size = 102400
  # Uphold the password policy
  min password length = 8
  null passwords = no
  # Use PAM (if added support)
  pam password change = yes
```
Make sure that permissions are set correct on every share and remember to read the [documentation](https://www.samba.org/).

Now restart the server and add the users who should have access to this service. This is done though the command /usr/bin/smbpasswd with the parameter `-a`.

### SSH

The most important securing that OpenSSH needs is turning on a stronger authentication based on public key encryption. Too many sites (like [Sourceforge](https://www.sourceforge.net), [PHP](https://www.php.net) and [Apache](https://www.apache.org)) have suffered unauthorized intrusion due to password leaks or bad passwords.

**`/etc/ssh/sshd_config`**

```
# Do not enable DSA and ECDSA server authentication.
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key
# If you have a recent OpenSSH client disable weak ciphers and Message Authentication Code (MAC) by explicitly enabling stronger ciphers.
# check with ssh -Q cipher resp. ssh -Q mac which ciphers/ MACs are supported
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com
# Disable root login. Users should be using su or sudo to obtain root permissions.
PermitRootLogin no
# Turn on Public key authentication
PubkeyAuthentication yes
AuthorizedKeysFile      .ssh/authorized_keys
# Disable .rhost and normal password authentication.
HostbasedAuthentication no
PasswordAuthentication no
PermitEmptyPasswords no
# Only allow users in the wheel or admin group to login via SSH.
AllowGroups wheel admin
# In the above 'AllowedGroups' directive only allow the following users.
# Note: the @<domainname> is optional but replaces the older AllowHosts directive.
AllowUsers larry@gentoo.org cow@gentoo.org
# Logging
SyslogFacility AUTH
LogLevel INFO
# The ListenAddress directive should be changed to a single IP address
ListenAddress 127.0.0.1
```
Also verify `UsePAM yes` is not in the configuration file; it overrides the public key authentication mechanism. Alternatively `PasswordAuthentication` or `ChallengeResponseAuthentication` directives can be disabled. More information about these options can be found in the sshd\_config manual page (man 5 sshd\_config).

Now all that users have to do is create SSH public/private key pairs and type in a passphrase with the following command:

`root #``ssh-keygen -t ed25519`
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/larry/.ssh/id\_ed25519):\[Press enter\]
Created directory '/home/larry/.ssh'.
Enter passphrase (empty for no passphrase): \[Enter passphrase\]
Enter same passphrase again: \[Enter passphrase again\]
Your identification has been saved in /home/larry/.ssh/id\_ed25519.
Your public key has been saved in /home/larry/.ssh/id\_ed25519.pub.
The key fingerprint is:
SHA256:UZwgOwzktPyblYRMZjKnaD0HizvtnX+qVnk4liaZewI larry@gentoo

This will add two files to the user's \~/.ssh/ directory called id\_ed25519 and id\_ed25519.pub. The file named id\_ed25519 is the private key and should be accessible only to the user who created it. The other file id\_ed25519.pub is to be distributed to every remote server that requires SSH access. Add the key to the users home directory in the \~/.ssh/authorized\_keys file and the user should be able to login. This action can be performed in one-shot by using the ssh-copy-id command:

`user $``ssh-copy-id larry@remote`
Each user should guard their private key well. Put it on encrypted media that is easily accessible or keep it on their workstation (put this in the [password policy](https://wiki.gentoo.org/wiki/Security_Handbook/Pre-installation_concerns#Security_policies)).

For more information on SSH visit the [OpenSSH website](https://www.openssh.com/) and the [SSH](https://wiki.gentoo.org/wiki/SSH) article.

### TCP wrappers

TCP wrappers are a way of controlling access to services normally run by inetd (which Gentoo does not have), but it can also be used by xinetd and other services.

**`/etc/hosts.deny`**

**`/etc/hosts.allow`**

```
 LOCAL @wheel
time: LOCAL, .gentoo.org
```
As you can see the format is very similar to the one in /etc/security/access.conf. The tcpd facility supports a specific service; it does not overlap with /etc/security/access.conf. These settings only apply to services using TCP wrappers.

It is also possible to execute commands when a service is accessed (this can be used when activating relaying for dial-in users) but it is not recommended, since people tend to create more problems than they are trying to solve. An example could be that you configure a script to send an e-mail every time someone hits the deny rule, but then an attacker could launch a DoS attack by keep hitting the deny rule. This will create a lot of I/O and e-mails so don't do it!. Read the man 5 hosts\_access for more information.

### Using xinetd

[xinetd](https://en.wikipedia.org/wiki/xinetd) ([sys-apps/xinetd](https://packages.gentoo.org/packages/sys-apps/xinetd)) is a replacement for inetd (which Gentoo does not have), the Internet services daemon. It supports access control based on the address of the remote host and the time of access. It also provide extensive logging capabilities, including server start time, remote host address, remote user name, server run time, and actions requested.

As with all other services it is important to have a good default configuration. But since xinetd is run as root and supports protocols that you might not know how they work, we recommend not to use it. But if you want to use it anyway, here is how you can add some security to it:

`root #``emerge --ask sys-apps/xinetd sys-apps/tcp-wrappers`
And edit the configuration file:

**`/etc/xinetd.conf`**

```
{
 only_from = localhost
 instances = 10
 log_type = SYSLOG authpriv info
 log_on_success = HOST PID
 log_on_failure = HOST
 cps = 25 30
}
# This will setup pserver (cvs) via xinetd with the following settings:
# max 10 instances (10 connections at a time)
# limit the pserver to tcp only
# use the user cvs to run this service
# bind the interfaces to only 1 ip
# allow access from 10.0.0.*
# limit the time developers can use cvs from 8am to 5pm
# use tpcd wrappers (access control controlled in
# /etc/hosts.allow and /etc/hosts.deny)
# max_load on the machine set to 1.0
# The disable flag is per default set to no but I like having
# it in case of it should be disabled
service cvspserver
{
 socket_type = stream
 protocol = tcp
 instances = 10
 protocol = tcp
 wait = no
 user = cvs
 bind = 10.0.0.2
 only_from = 10.0.0.0
 access_times = 8:00-17:00
 server = /usr/sbin/tcpd
 server_args = /usr/bin/cvs --allow-root=/mnt/cvsdisk/cvsroot pserver
 max_load = 1.0
 log_on_failure += RECORD
 disable = no
}
```
For more information read man 5 xinetd.conf.

### X

By default [Xorg](https://wiki.gentoo.org/wiki/Xorg) is configured to act as an X server. This can be dangerous since X uses unencrypted TCP connections and listens for X clients.

But if you depend on using the workstation as a X server use the /usr/bin/xhost command with caution. This command allows clients from other hosts to connect and use your display. This can become handy if you need an X application from a different machine and the only way is through the network, but it can also be exploited by an attacker. The syntax of this command is /usr/bin/xhost +hostname

A more secure solution is to disable this feature completely by starting X with [startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) -- -nolisten tcp or disable it permanently in the configuration.

**`/usr/bin/startx`**

```
defaultserverargs="-nolisten tcp"
```
To make sure that [startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) does not get overwritten when emerging a new version of Xorg you must protect it. Add the following line to /etc/portage/make.conf:

```
CONFIG_PROTECT="/usr/bin/startx"
```
When using a graphical [login manager](https://wiki.gentoo.org/wiki/Login_manager), a different approach is needed.

#### GDM

For [gdm](https://wiki.gentoo.org/wiki/GNOME/gdm) (Gnome Display Manager):

**`/etc/X11/gdm/gdm.conf`**

```
[server-Standard]
command=/usr/X11R6/bin/X -nolisten tcp
```
#### XDM and KDM

For [xdm](https://wiki.gentoo.org/wiki/XDM) (X Display Manager) and [kdm](https://wiki.gentoo.org/wiki/KDE#Display_manager) (KDE Display Manager):

**`/etc/X11/xdm/Xservers`**

```
 local /usr/bin/X11/X -nolisten tcp
```




### Wayland

#### Input group

Avoid "input" Group: Do not add users to the input group.  In Wayland, the compositor is the *only* process that directly interacts with the raw input devices. It receives all keyboard, mouse, and touch events. Mediated approach is a core security feature of Wayland.

#### Desktop portals

Wayland security standardized API for granting granular permissions to desktop features. See [XDG/xdg-desktop-portal](https://wiki.gentoo.org/wiki/XDG/xdg-desktop-portal) — a frontend to implementations of the xdg-desktop-portal interface..



## Chrooting and virtual servers

### Chrooting

Chrooting a service is a way of limiting a service (or user) environment to only accessing what it should and not gaining access (or information) that could lead to root access. By running the service as a user other than root (e.g. nobody, apache, named) an attacker can only access files with the permissions of this user. This means that an attacker cannot gain root access even if the services has a security flaw.

Some services like pure-ftpd and bind have features for chrooting, and other services do not. If the service supports it, use it, otherwise you have to figure out how to create your own. Lets see how to create a chroot, for a basic understanding of how chroots work, we will test it with bash (easy way of learning).

Create the /chroot directory with mkdir /chroot. And find what dynamic libraries that bash is compiled with (if it is compiled with -static this step is not necessary):

The following command will create a list of libraries used by bash:

`root #``ldd /bin/bash`
libncurses.so.5 => /lib/libncurses.so.5 (0x4001b000)
libdl.so.2 => /lib/libdl.so.2 (0x40060000)
libc.so.6 => /lib/libc.so.6 (0x40063000)
/lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)

Now lets create the environment for bash:

`root #````
mkdir /chroot/bash
```
`root #````
mkdir /chroot/bash/bin
```
`root #````
mkdir /chroot/bash/lib
```
Next copy the files used by bash (/lib) to the chrooted lib and copy the bash command to the chrooted bin directory. This will create the exact same environment, just with less functionality. After copying try it out: chroot /chroot/bash /bin/bash. If you get an prompt saying / it works! Otherwise it will properly tell you what a file is missing. Some shared libraries depend on each other.

You will notice that inside the chroot nothing works except echo. This is because we have no other commands in our chroot environment than bash and echo is a build-in functionality.

This is basically the same way you would create a chrooted service. The only difference is that services sometimes rely on devices and configuration files in /etc. Simply copy them (devices can be copied with cp -a) to the chrooted environment, edit the init script to use chroot before executing. It can be difficult to find what devices and configuration files a service need. This is where the strace command becomes handy. Start the service with /usr/bin/strace bash and look for open, read, stat and maybe connect. This will give you a clue on what files to copy. But in most cases just copy the passwd file (edit the copy and remove users that have nothing to do with the service), /dev/zero, /dev/log and /dev/random.

### User Mode Linux

Another way of creating a more secure environment is by running a virtual machine. A virtual machine, as the name implies, is a process that runs on top of your real operating system providing a hardware and operating system environment that appears to be its own unique machine. The security benefit is that if the server running on the virtual machine is compromised, only the virtual server is affected and not the parent installation.

For more information about how to setup User Mode Linux consult the [User Mode Linux Guide](https://wiki.gentoo.org/wiki/User-mode_Linux/Guide).



## Intrusion detection

### qcheck

The [Q applets](https://wiki.gentoo.org/wiki/Q_applets) program [qcheck](https://wiki.gentoo.org/wiki/Q_applets#Verifying_package_integrity_.28qcheck.29) can be used to check the existence, modification times and MD5 sums of all files of packages installed by portage. It is a fast program that requires no manual configuration in order to check the integrity of your host's installed files. qcheck is provided through the [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) package.

To use qcheck, type in a console:

`user $``qcheck package-name`
Replace `package-name` in the example above with the desired package.

To check the integrity of all packages installed, enter:

`user $``qcheck`
### AIDE

AIDE is a Host-Based Intrusion Detection System (**HIDS**), a free alternative to Tripwire. HIDS are used to detect changes to important system configuration files and binaries, generally by making a unique cryptographic hash for the files to be checked and storing it in a secure place. On a regular basis (such as once a day), the stored "known-good" hash is compared to the one generated from the current copy of each file, to determine if that file has changed. HIDS are a great way to detect disallowed changes to your system, but they take a little work to implement properly and make good use of.

The AIDE ebuild now comes with a working default configuration file, a helper script and a crontab script. The helper script does a number of tasks for you and provides an interface that is a little more script friendly. To see all available options, try aide --help. To get started, all that needs to be done is aide -i and the crontab script should detect the database and send mails as appropriate every day. We recommend that you review the /etc/aide/aide.conf file and ensure that the configuration accurately reflects what is in place on the machine.

Please see [AIDE](https://wiki.gentoo.org/wiki/AIDE) for more details on configuration and usage.

Let's watch a full blown example:

**`/etc/aide/aide.conf`**

```
 TOPDIR
@@define TOPDIR /
@@endif
@@ifndef AIDEDIR
@@define AIDEDIR /etc/aide
@@endif
@@ifhost smbserv
@@define smbactive
@@endif
# The location of the database to be read.
database=file:@@{AIDEDIR}/aide.db
# The location of the database to be written.
database_out=file:aide.db.new
verbose=20
report_url=stdout
# Rule definition
All=R+a+sha1+rmd160
Norm=s+n+b+md5+sha1+rmd160
@@{TOPDIR} Norm
!@@{TOPDIR}etc/aide
!@@{TOPDIR}dev
!@@{TOPDIR}media
!@@{TOPDIR}mnt
!@@{TOPDIR}proc
!@@{TOPDIR}root
!@@{TOPDIR}sys
!@@{TOPDIR}tmp
!@@{TOPDIR}var/log
!@@{TOPDIR}var/run
!@@{TOPDIR}usr/portage
!@@{TOPDIR}var/db/repos/gentoo
@@ifdef smbactive
!@@{TOPDIR}etc/smb/private/secrets.tdb
@@endif
=@@{TOPDIR}home Norm
```
In the above example we specify with some macros where the topdir starts and where the AIDE directory is. AIDE checks the /etc/aide/aide.db file when checking for file integrity. But when updating or creating a new file it stores the information in /etc/aide/aide.db.new. This is done so it won't automatically overwrite the old db file. The option `report_URL` is not yet implemented, but the author's intention was that it should be able to e-mail or maybe even execute scripts.

Now there is some risk inherent with storing the db files locally, since the attacker will (if they know that AIDE is installed) most certainly try to alter the db file, update the db file or modify /usr/bin/aide. So you should create a CD or other media and put on it a copy of the .db file and the AIDE binaries.

You can find information at the [AIDE project page](http://aide.sourceforge.net).

### Snort

Snort is a Network Intrusion Detection System (NIDS). To install and configure it use the following examples.

**`/etc/conf.d/snort`**

```
SNORT_IFACE="eth0"
PIDFILE=/run/snort_eth0.pid
MODE="full"
NETWORK="192.168.0.0/24"
LOGDIR="/var/log/snort"
SNORT_CONF=/etc/snort/snort.conf
SNORT_OPTS="-q -D -u snort -d -l $LOGDIR -h $NETWORK -c $SNORT_CONF"
```
Copy /etc/snort/snort.conf.distrib to /etc/snort/snort.conf.

`root #``cd /etc/snort && cp snort.conf.distrib snort.conf`
You might need to comment out the blacklist and whitelist entries if no lists are created.

More information is at the [Snort website](https://www.snort.org/).

### Detecting malware with chkrootkit

HIDS like AIDE are a great way to detect changes to your system, but it never hurts to have another line of defense. chkrootkit is a utility that scans common system files for the presence of rootkits-software designed to hide an intruder's actions and allow him to retain his access-and scans your system for likely traces of key loggers and other "malware". While chkrootkit (and alternatives like rkhunter) are useful tools, both for system maintenance and for tracking an intruder after an attack has occurred, they cannot guarantee your system is secure.

The best way to use chkrootkit to detect an intrusion is to run it routinely from cron. To start, emerge [app-forensics/chkrootkit](https://packages.gentoo.org/packages/app-forensics/chkrootkit):

`root #``emerge --ask app-forensics/chkrootkit`
chkrootkit can be run from the command line by the command of the same name, or from cron with an entry such as this:





## Keeping up-to-date

The Security team distributes *Gentoo Linux Security Advisories* (GLSAs) through [Portage's glsa-check](https://wiki.gentoo.org/wiki/Portage#glsa-check). They are received, like the news, after a sync and detail some security vulnerabilities related to ::gentoo repository packages.

More work is required after a successful install to ensure a good level of security. Security is an ongoing process much like development; the vast majority of intrusions result from known vulnerabilities in unpatched systems.

Keeping the system up-to-date is the single most valuable step to take for greater security.

First sync the Portage tree:

`root #``emerge --sync`
To check if the system is up to date security-wise:

`root #``glsa-check --list`
\[A\] means this GLSA was marked as applied (injected),
\[U\] means the system is not affected and
\[N\] indicates that the system might be affected.
200406-03 \[N\] sitecopy: Multiple vulnerabilities in included libneon ( net-misc/sitecopy )
200406-04 \[U\] Mailman: Member password disclosure vulnerability ( net-mail/mailman )
.......

All lines with a `[A]` and `[U]` can be almost safely ignored as the system is not affected by this GLSA.

To check all GLSAs:

`root #``glsa-check --test all`
This system is not affected by any of the listed GLSAs

`root #``glsa-check -t all`
This system is affected by the following GLSA:
200504-06
200510-08
200506-14
200501-35
200508-12
200507-16

To see what packages would be emerged:

`root #``glsa-check -p $(glsa-check -t all)````
Checking GLSA 200504-06
The following updates will be performed for this GLSA:
     app-arch/sharutils-4.2.1-r11 (4.2.1-r10)
     **********************************************************************
     Checking GLSA 200510-08
     The following updates will be performed for this GLSA:
          media-libs/xine-lib-1.1.0-r5 (1.1.0-r4)
```
To apply required fixes:

`root #``glsa-check -f $(glsa-check -t all)`
Then restart any upgraded running services.

Keeping the [kernel up-to-date](https://wiki.gentoo.org/wiki/Kernel/Upgrade) is also recommended.

To receive an email each time a GLSA is released, subscribe to the [gentoo-announce mailing list](https://archives.gentoo.org/gentoo-announce/). Instructions for joining it and many other great mailing lists can be found in the [Gentoo mailing lists](https://www.gentoo.org/get-involved/mailing-lists/all-lists.html).

Another great security resource is the [Bugtraq mailing list](https://www.securityfocus.com/archive/1).
