<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Pre-deployment_considerations | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Pre-deployment considerations -->
---
title: Security Handbook/Pre-deployment considerations
url: https://wiki.gentoo.org/wiki/Security_Handbook/Pre-deployment_considerations
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-28"
fingerprint: bf125c72df25dfa7
license: CC BY-SA 4.0
---

# Security Handbook/Pre-deployment considerations

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

While ideally all aspects of security would be considered before deploying Gentoo, some controls are harder to implement after a deployment than others; This section provides guidance on items that should be considered before a secure Gentoo deployment is attempted.



### Storage Configuration

The storage configuration of a system can have an impact on its security; the following outlines some best-practice guidelines related to the topic.

Locations that users (or services) have permission to write to (e.g. /home or /tmp) should be on a separate filesystem to system data and should leverage disk quotas or other similar mechanisms to prevent excessive utilisation. This reduces the risk of filling up filesystems that are critical to the operation of the host, accidentally or maliciously, which may result in a Denial of Service.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| System Configuration | Storage | User and System data should be logically separated wherever practical | 0 | 
| System Configuration | Storage | An appropriate technical control should be implemented to prevent users from filling up critical filesystems | 1 | 

### Disk Encryption

Disk encryption is the process of protecting data on a storage device by scrambling it so that it cannot be read without the correct decryption key. This is often used to protect data on laptops and other mobile devices, but can also be used to protect data on servers and workstations. Disk encryption is not a replacement for other security controls, but can be used to mitigate the risk of data being accessed by an unauthorised party if the physical disk is stolen.

| Category | Subcategory | Control | Maturity | 
|---|---|---|---|
| System Configuration | Storage | Disk encryption should be implemented to protect data at rest | 0 | 

LUKS (Linux Unified Key Setup) is a popular disk encryption method that is supported by most Linux distributions. LUKS uses a combination of encryption algorithms and key management tools to provide strong data protection.

Modern systems implement some form of hardaware-backed cryptographic acceleration (such as AES<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>) which can be leveraged to reduce the performance impact of disk encryption.

There are a number of reasons that disk encryption should be considered:

- **Data protection:** Disk encryption can help to protect data from unauthorized access, such as in the case of a lost or stolen device
- **Compliance:** Disk encryption can help organisations to comply with data security regulations, such as the General Data Protection Regulation (GDPR) or Health Insurance Portability and Accountability Act (HIPAA)
- **Ease of use:** Disk encryption tools are becoming increasingly easy to use, making it possible for even non-technical users to protect their data
