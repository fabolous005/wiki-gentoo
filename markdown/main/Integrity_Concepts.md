<!-- source: https://wiki.gentoo.org/wiki/Integrity/Concepts | group: Gentoo Wiki (Main) | wiki-title: Integrity/Concepts -->
---
title: Integrity/Concepts
url: https://wiki.gentoo.org/wiki/Integrity/Concepts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-23"
fingerprint: "959b983c306f2e67"
license: CC BY-SA 4.0
---

# Integrity/Concepts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Integrity** is about trusting components within the environment and, when working on workstations, servers and machines. It should be certain that credentials when logging on to workstation infrastructure is not compromised in any way. This "trust" within the environment is a combination of various factors: physical security, system security patching process, secure configuration, access controls and more.

Integrity plays a role in this security field and tries to ensure systems have not been tampered with by malicious people or organizations. This extends to a wide range of components and require validation. Binary programs and loaded libraries, built from source code, or provided by a third party, must, for certain, be trusted. The running Linux kernel, and loaded modules, must be free from tampering.

People can trust themselves and consider things they built themselves to have integrity. Systems in place must not be the final yes or no when supporting this claim. Reviewing trusted information, services, technologies, processes, and algorithms supports the claim of integrity and answers two questions. Were the binary programs really validated? Was the system compromised?

The Gentoo Hardened Integrity subprojects' vision and roadmap involves a few of these components.

## Hash results

### Algorithmically validating a file's content

Hashes are a primary method for validating if a file (or other resource) has not been changed since it was first inspected. A hash is the result of a mathematical calculation on the content of a file (most often a number or ordered set of numbers), and exhibits the following properties:

- The resulting number is represented in a *small (often fixed-size) length*. This is necessary to allow fast verification if two hash values are the same or not, but also to allow storing the value in a secure location (which is, more than often, much more restricted in space).
- The hash function always *returns the same hash* (output) when the file it inspects has not been changed (input). Otherwise it'll be impossible to ensure that the file content hasn't changed.
- The hash function is fast to run (the calculation of a hash result does not take up too much time or even resources). Without this property, it would take too long to generate and even validate hash results, leading to users being malcontent (and more likely to disable the validation altogether).
- The hash result *cannot be used to reconstruct* the file. Although this is often seen as a result of the first property (small length), it is important because hash results are often also seen as a "public validation" of data that is otherwise private in nature. In other words, many processes rely on the inability of users (or hackers) to reverse-engineer information based on its hash result. A good example are passwords and password databases, which *should* store hashes of the passwords, not the passwords themselves.
- Given a hash result, it is near impossible to find another file with the same hash result (or create such a file). Since the hash result is limited in space, there are many inputs that will map onto the same hash result. The power of a good hash function is that it is not feasible to find them (or calculate them) except by brute force. When such a match is found, it is called a *collision* .

Compared with checksums, hashes try to be more cryptographically secure (and as such more effort is made in the last property to make sure collisions are very hard to obtain). Some even try to generate hash results in a way that the duration to calculate hashes cannot be used to obtain information from the data (such as if it contains more 0s than 1s, etc.)

### Hashes in integrity validation

Integrity validation services are often based on hash generation and validation. Tools such as [tripwire](http://www.tripwire.org/) or [AIDE](http://aide.sourceforge.net/) generate hashes of files and directories on systems and then ask to be stored safely. Lists are provided to programs when checking integrity, likely as read-only to prevent modification, which recalculates the hashes of the files and compares them with the given list. Any changes in files are detected and can be reported.

A popular hash functions is SHA-1 (generated and validated using the **sha1sum** command) which gained momentum after MD5 (using **md5sum**) was found to be less secure (nowadays collisions in MD5 are easy to generate). SHA-1 is now insecure as collisions can be generated in under a year. At least SHA-2 is recommended (but is less popular than SHA-1) and can be played with using the commands **sha224sum**, **sha256sum**, **sha384sum** and **sha512sum**.

`user $``sha1sum ~/Downloads/pastie-4301043.rb`
6b9b4e0946044ec752992c2afffa7be103c2e748  /home/swift/Downloads/pastie-4301043.rb

### Hashes are a means, not a solution

Hashes, in the field of integrity validation, are a means to compare data and integrity in a relatively fast way. However, only just hashes, like **sha1sum**, cannot be used to provide integrity assurance towards the administrator.

The **sha1sum** application is not guaranteed to behave correctly or be free from tampering. Malicious modifications of **sha1sum** can easily return the expected SHA-1 sum against the real sum. A way to thwart this is to provide the binary together with the hash values on read-only media.

It is still uncertain that it is that application that is executed. Modified systems appear to execute that application, but instead is using a different application. A higher-positioned and trusted service must ensure correct application execution. Running with a trusted kernel helps here, but might not provide 100% closure on it, but most likely needs assistance from the hardware. The Trusted Platform Module will be discussed.

Hash result files are not guaranteed to verify integrity. Another file (with modified content) may be bind-mounted on top of it. To support integrity validation with a trusted information source, some solutions use HMAC digests instead of plain hashes.

Finally, checksums should not only be taken on file level, but also its attributes (which are often used to provide access controls or even toggle particular security measures on/off on a file, such as is the case with PaX markings), directories (holding information about directory updates such as file adds or removals) and privileges. These are things that a program like **sha1sum** doesn't offer (but tools like AIDE do).

## Hash-based Message Authentication Codes

### Trusting the hash result

In order to trust a hash result, some solutions use HMAC digests instead. An HMAC digest combines a regular hash function (and its properties) with a a secret cryptographic key. As such, the function generates the hash of the content of a file together with the secret cryptographic key. This not only provides integrity validation of the file, but also a signature telling the verification tool that the hash was made by a trusted application (one that knows the cryptographic key) in the past and has not been tampered with.

By using HMAC digests, malicious users will find it more difficult to modify code and then present a "fake" hash results file since the user cannot reproduce the secret cryptographic key that needs to be added to generate this new hash result. Terms like *HMAC-SHA1* mean that a SHA-1 hash result is used together with a cryptographic key.

### Managing the keys

Keys "protecting" hash results introduces another level of complexity. What is the proper and secure storage of keys and how will they be accessed? Keys can not just be embedded in the hash list since tampered systems may intercept the keys and generate its own results file for further verification. Keys must also not be embedded in applications because a tampered system may intercept applications to find keys. Rebuilding the application completely with a new key may be required once compromised.

It is tempting to just provide the key as a command-line argument. Again, it is uncertain that malicious users are idling on the system waiting to capture valuable information from **ps** output or other programs. The need to trust a higher-level component again arises. Trusting the kernel allows the use of the kernel key ring.

## Using private/public key cryptography

### Validating integrity using public keys

One way to work around the vulnerability of having the malicious user getting hold of the secret key is to not rely on the key for the authentication of the hash result in the first place when verifying the integrity of the system. This can be accomplised by encrypting the HMAC digest with a private key. HMAC is decrypted with the public key, and not the private key, allowing regeneration of the HMAC digests. In this approach, an attacker cannot forge a fake HMAC since forgery requires access to the private key, and the private key is never used on the system to validate signatures. As long as no collisions occur, the encrypted HMAC values can not be reused preventing replay attacks.

### Ensuring the key integrity

Of course, this still requires that the public key is not modifiable by a tampered system: a fake list of hash results can be made using a different private key, and the moment the tool wants to decrypt the encrypted values, the tampered system replaces the public key with its own public key, and the system is again vulnerable.

## Trust chain

### Handing over trust

Something must always be trusted. By trusting nothing, nothing can be validated because nothing returns trusted responses. Trust means to have confidence in a system and its resources.

For many users, the hardware level is something they trust. After all, as long as no burglar has come in the house and tampered with the physical hardware, it is reasonable to expect that the hardware is still the same. In effect, the users trust that the physical protection of a house is sufficient.

For companies, the physical protection of the working environment is not sufficient for ultimate trust. They want to make sure that the hardware is not tampered with (or different hardware is suddenly used), specifically when that company uses laptops instead of (less portable) workstations.

The more things are untrusted, the more accommodations are required to be confident that the systems have not been tampered. In the Gentoo Hardened Integrity subproject we will use the following "order" of resources:

- *System root-owned files and root-running processes*. In most cases and most households, properly configured and protected systems will trust root-owned files and processes. Any request for integrity validation of the system is usually applied against user-provided files (no-one tampered with the user account or specific user files) and not against the system files.
- *Operating system kernel*. The Linux kernel is used by Gentoo. Although some precautions need to be taken, a properly configured and protected kernel can provide a higher trust level. Integrity validation on kernel level can offer a higher trust in the systems' integrity, though most kernels still reside on the system.
- *Live environments* . A bootable (preferably) read-only medium can be used to boot up a validation environment that scans and verifies the integrity of the system-under-investigation. Even tampered kernel boot images can be detected. Ensuring network access is disabled from boot up until the final compliance check are proper precautions taken to allow confidence in the entire system state.
- *Hypervisor level* . Hypervisors are by many organizations seen as trusted resources (the isolation of a virtual environment is hard to break out of). Integrity validation on the hypervisor level can therefore provide confidence, especially when "chaining trusts": the hypervisor first validates the kernel to boot, and then boots this (now trusted) kernel which loads up the rest of the system.
- *Hardware level* . Whereas hypervisors are still "just software", trust can be lifted up to the hardware level and use the hardware-offered integrity features providing confidence that the system to be booted has not been tampered with.

The Gentoo Hardened Integrity subproject aims to eventually support all these levels, and perhaps more, to provide users the tools and methods needed to validate the system integrity to required trust levels. The less is trusted, the more complex a trust chain might become to validate and manage. Research and support is not limited to a single technology or chain of technologies.

Chaining trust is an important aspect to keep things from becoming too complex and unmanageable. It also allows users to just "drop in" at the level of trust they feel is sufficient, rather than requiring technologies for higher levels.

For instance:

- A trusted hardware component (like a *Trusted Platform Module* or a specific BIOS-supported functionality) verifies the integrity of the boot regions on the disk. When ok, it passes control over to the bootloader.
- The bootloader now validates the integrity of its configuration and of the files (kernel and initramfs) it is told to boot up. If it checks out, it boots the kernel and hands over control to this kernel.
- The kernel, together with the initial ram file system, verifies the integrity of the system components (and for instance SELinux policy) before the initial ram system changes to the real system and boots up the (verified) init system.
- The (root-running) init system validates the integrity of the services it wants to start before handing over control of the system to the user.

An even longer chain can be seen with hypervisors:

- Hardware validates boot loader
- Boot loader validates hypervisor kernel and system
- Hypervisor validates kernel(s) of the images (or the entire images)
- Hypervisor-managed virtual environment starts the image
- ...

### Integrity on serviced platforms

Sometimes, verifying services are not tampered with from higher positioned untrusted components is desired. An example would be when systems are hosted in a remote, non-accessible data center or when a managed image resides on a virtualized hosting provider like the cloud. It must be assured that the image has is not tampered with and is free from trojans or other backdoors. A manageable level of distrust may be used instead of trusting the higher components. The Gentoo Hardened Integrity subproject aims to provide some confidence at this level too.

### From measurement to protection

When dealing with integrity (and trust chains), the idea behind the top-down trust chain is that higher level components first measure the integrity of the next component, validate (and take appropriate action) and then hand over control to this component. This is what we call *protection* or *integrity enforcement* of resources.

If the system cannot validate the integrity, or the system is too volatile to enforce this integrity from a higher level, it is necessary to provide a trusted method for other services to validate the integrity. In this case, the system *attests* the state of the underlying component(s) towards a third party service, which *appraises* this state against a known "good" value.

In HMAC-based checks, there is no enforcement of file integrity, but the tool attests the state of the resources by generating new HMAC digests and validating (appraising) it against the list of HMAC digests it took before.

## An implementation: the Trusted Computing Group functionality

### Trusted Platform Module

Years ago, a non-profit organization called the [Trusted Computing Group](http://www.trustedcomputinggroup.org) was formed to work on and promote open standards for hardware-enabled trusted computing and security technologies, including hardware blocks and software interfaces across multiple platforms. One of its deliverables is the *Trusted Platform Module*, abbreviated to TPM, to help achieve these goals. What are these goals, especially in light of the integrity project, exactly?

- Support hardware-assisted record (measuring) of what software is (or was) running on the system since it booted in a way that modifications to this record (or the presentation of a different, fake record) can be easily detected
- Support the secure reporting to a third party of this state (measurement) so that the third party can attest that the system is indeed in a sane state

The idea of providing a hardware-assisted method is to prevent software-based attacks or malpractices that would circumvent security measures. By running some basic (but important) functions in a protected, tamper-resistant hardware module (the TPM) even rooted devices cannot work around some of the measures taken to "trust" a system.

The TPM chip alone does not influence the execution of a system. It is, in fact, a simple request/reply service and needs to be called by software functions. However, it provides a few services that make it a good candidate to set up a trusted platform (next to its hardware-based protection measures to prevent tampering of the TPM hardware):

- Asymmetric crypto engine, supporting the generation of asymmetric keys (RSA with a keylength of 2048 bits) and standard operations with those keys
- A random noise generator
- A SHA-1 hashing engine
- Protected (and encrypted) memory for user data and key storage
- Specific registers (called PCRs) to which a system can "add" data to

### Platform Configuration Registers, Reporting and Storage

PCR registers are made available to support securely recording the state of (specific parts of) the system. Unlike processor registers that software can reset as needed, PCR registers can only be "extended": the previous value in the register is taken together with the new provided value, hashed and stored again. This has the advantage that a value stores both the knowledge of the data presented to it as well as its order (providing values AAA and BBB gives a different end result than providing values BBB and AAA), and that the PCR can be extended an unlimited number of times.

A system that wants to securely "record" each command executed can take the hash of each command (before it executes it), send that to the PCR, record the event and then execute the command. The system (kernel or program) is responsible for recording the values sent to the PCR, but at the end, the value inside the PCR has to be the same as the one calculated from the record. If it differs, then the list is incorrect and the "secure" state of the system cannot be proven.

To support secure reporting of this value to a "third party" (be it a local software agent or a remote service) the TPM supports secure reporting of the PCR values: an RSA signature is made on the PCR value as well as on a random number (often called the "nonce") given by the third party (proving there is no man-in-the-middle or replay attack). Because the private key of this signature is securely stored on the TPM this signature cannot be forged.

The TPM chip has (at least) 24 PCR registers available. These registers will contain the extended values for

- BIOS, ROM and memory block data (PCR 0-4)
- OS loaders (PCR 5-7)
- Operating System-provided data (PCR 8-15)
- Debugging data (PCR 16)
- Localities and Trusted Operating System data (PCR 17-22)
- Application-specific data (PCR 23)

The idea of using PCRs is to first *measure* the data a component is about to execute (or transfer control to), then *extend* the appropriate PCR, then *log* this event in a measurement log and finally *transfer control* to the measured component. This provides a trust "chain".

### Trusting the TPM

In order to trust the TPM, the TCG basis its model on asymmetric keys. Each TPM chip has a 2048-bit private RSA key securely stored in the chip. This key, called the *Endorsement Key*, is typically generated by the TPM manufacturer during the creation of the TPM chip, and is backed by an Endorsement Key certificate issued by the TPM manufacturer. This EK certificate guarantees that the EK is in fact an Endorsement Key for a given TPM (similar to how an SSL certificate is "signed" by a Root CA). The private key cannot leave the TPM chip.

A second key, called the *Storage Root Key*, is generated by the TPM chip when someone takes "ownership" of the TPM. Although the key cannot leave the TPM chip, it can be removed (when someone else takes ownership). This key is used to encrypt data and other keys (user *Storage Keys* and *Signature Keys*).

The other keys (storage and signature keys) can leave the TPM chip, but always in an encrypted state that only the TPM can decrypt. That way, the system can generate specific user storage keys securely and extract them, storing them on non-protected storage and reload them when needed in a secure manner).
