<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/General_Guidance | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/General Guidance -->
---
title: Security Handbook/General Guidance
url: https://wiki.gentoo.org/wiki/Security_Handbook/General_Guidance
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: dd1298946021d705
license: CC BY-SA 4.0
---

# Security Handbook/General Guidance

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section provides high-level guidance on security considerations.



Physical security is the practice of protecting elements of infrastructure, estates, and personnel against attacks or compromises in the physical (i.e. tangible, real-world) environment[\[1\]](https://wiki.gentoo.org#cite_note-1)


Physical security is an important consideration for all systems. For example, a laptop that is left unattended in a public place is at risk of being stolen; a server that is left in an unlocked room is at risk of being tampered with. Physical security controls should be implemented to manage these risks.

For a practical guide to physical security (including some good controls that may be adapted), see the following resources:

Information Security is the practice of managing risks related to the use, processing, storage, and transmission of information or data. It is also ensuring the systems and processes used for those purposes are in line with organisational policies<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.


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
