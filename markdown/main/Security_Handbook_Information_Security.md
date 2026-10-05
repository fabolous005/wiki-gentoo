<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Information_Security | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Information Security -->
---
title: Security Handbook/Information Security
url: https://wiki.gentoo.org/wiki/Security_Handbook/Information_Security
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: ce125c1a0a349785
license: CC BY-SA 4.0
---

# Security Handbook/Information Security

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section provides guidance on information security.


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

Modern systems implement some form of hardware-backed cryptographic acceleration (such as AES<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>) which can be leveraged to reduce the performance impact of disk encryption.

Other systems, such as Opal2-compliant self-encrypting drives, perform encryption in hardware and are controlled by the disk firmware. Some of these devices encrypt *all* data to the disk even before decryption is "enabled" in firmware, only encrypting the internal encryption key if encryption is "enabled", allowing the user to "encrypt" the disk immediately by setting a passphrase.

There are a number of reasons that disk encryption should be considered:

- **Data protection:** Disk encryption can help to protect data from unauthorized access, such as in the case of a lost or stolen device
- **Compliance:** Disk encryption can help organizations to comply with data security regulations, such as the General Data Protection Regulation (GDPR) or Health Insurance Portability and Accountability Act (HIPAA)
- **Ease of use:** Disk encryption tools are becoming increasingly easy to use, making it possible for even non-technical users to protect their data

### References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [https://en.wikipedia.org/wiki/AES\_instruction\_set](https://en.wikipedia.org/wiki/AES_instruction_set)
2. [↑](https://wiki.gentoo.org#cite_ref-2) [https://ieeexplore.ieee.org/abstract/document/8835339](https://ieeexplore.ieee.org/abstract/document/8835339) Self-encrypting deception: weaknesses in the encryption of solid state drives (SSDs), Carlo Meijer, Bernard van Gastel
