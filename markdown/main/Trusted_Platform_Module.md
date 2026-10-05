<!-- source: https://wiki.gentoo.org/wiki/Trusted_Platform_Module | group: Gentoo Wiki (Main) | wiki-title: Trusted Platform Module -->
---
title: Trusted Platform Module
url: https://wiki.gentoo.org/wiki/Trusted_Platform_Module
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-08"
fingerprint: "864d597f6daa9c50"
license: CC BY-SA 4.0
---

# Trusted Platform Module

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Trusted Platform Module**, or **TPM** for short, is a secure, isolated, cryptographic processor that is typically built into most modern computers. The TPM can be used to hash files and store the hash in PCRs, using a blockchain for measurement. Additionally, a TPM can be used to securely store cryptographic keys, preventing unauthorized access, or access once the boot environment has changed.

The two most common versions of the TPM are 1.2 and 2.0. Both versions are supported on Linux, but this article covers TPM 2.0.
There are a few ways to use TPM under Linux: storing [SSH](https://wiki.gentoo.org/wiki/SSH), [GPG](https://wiki.gentoo.org/wiki/GPG), and [LUKS](https://wiki.gentoo.org/wiki/Dm-crypt) keys in the TPM, measuring the integrity of files and boot components, and generating random data.

## Introduction

### TPM terms

- **SRTM** - Static Root of Trust for Measurements
- **DRTM** - Dynamic Root of Trust for Measurements
- **CRTM** - Core Root of Trust for Measurements
- **SRK** - Storage Root Key
- **EK** - Endorsement Key
- **PK** - Platform Key
- **UEFI** - Unified Extensible Firmware Interface
- **PCR** - Platform Configuration Registers

### Hierarchies

TPMs typically have 3 builtin seeds, each associated with a hierarchy. These seeds are used to generate private keys, and each is typically used for different purposes.

- **Platform** - This hierarchy is typically associated with early boot code and firmware, typically used for boot integrity verification.
- **Storage** (*Owner*) - This hierarchy is meant to be used by the device owner, and is often used to wrap and seal data.
- **Endorsement** - This hierarchy is associated with the TPM itself, and can be used to generate keys that are associated with the particular device.

### Keys

Each seed type has an associated key, which is generated and used in a different manner.

- **Platform Key** - Deterministically generated using the *Platform Seed*, used for attestation and verifying boot integrity.
- **Storage Root Key** - Randomly generated when the TPM is initialized, primarily used to wrap other keys.
- **Endorsement Key** - Derived from the *endorsement Seed*. Used to verify the authenticity of the TPM itself, keys derived from this correlate to that specific TPM.

### Wrapping

In order to protect data, the TPM *wraps* or *binds* the data. Data, often private keys, is *wrapped* using the TPM's **SRK**. *Wrapping* involves encrypting the data using the internal key, so it can later only be decrypted using this key. Because the TPM has full control of this key, conditions can be set deciding when data can be decrypted.

### Sealing

Once data has been encrypted or *wrapped* using the TPM, conditions can be defined controlling when the TPM will allow decryption of this data, *sealing* the data. If a decryption attempt has been made, and the appropriate **PCR** values are measured, the TPM will decrypt or *unseal* the data.

### Platform Configuration Registers

PCRs typically contain hashes which can only be written by extension. The extension operation uses the value of the currently stored value along with the new data, forming a cryptographic blockchain.

The Trusted Computing Group PC Client Specific Platform Profile Specification defines commonly used TPM2 registers:[\[1\]](https://wiki.gentoo.org#cite_note-1)[\[2\]](https://wiki.gentoo.org#cite_note-2)

| PCR | TCG Description | Notes | 
|---|---|---|
| 0 | SRTM, BIOS, Host Platform Extensions, Embedded Option, ROMs and PI Drivers | Stores firmware, may change if the UEFI/BIOS is updated. Acts as the root for the chain of trust. Some implementations record CPU microcode measurements here. | 
| 1 | Host Platform Configuration | The UEFI configuration itself, such as settings. This typically contains the entire contents of the CMOS/NVRAM, minus any dynamic or security-sensitive data. | 
| 2 | UEFI driver and application Code | Option ROMs | 
| 3 | UEFI driver and application Configuration and Data | Option ROM configuration | 
| 4 | UEFI Boot Manager Code (usually the MBR) and Boot Attempts | Measures manager code itself, and that a use was attempted | 
| 5 | Boot Manager Code Configuration and Data (for use by the Boot Manager Code) and GPT/Partition Table | Measures the configuration of the boot device, including the GPT partition table of the device | 
| 6 | Host Platform Manufacturer Specific | May be used for S4 and S5 Power State Events | 
| 7 | Secure Boot Policy | Measures contents of Secure Boot keys and certificates used to verify boot applications | 
| 8 - 15 | Defined for use by the Static OS |  | 
| 16 | Debug | Optional, and sometimes unused PCR | 
| 23 | Application Support | Can be set and reset by the OS | 

## Installation

### Kernel

**Enable support for TPM**

```
 Device Drivers --->
   Character devices --->
     [*] TPM Hardware Support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_TCG_TPM</code> to find this item. --->
       <*> TPM HW Random Number Generator support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HW_RANDOM_TPM</code> to find this item.
       <*> TPM Interface Specification 1.2 Interface / TPM 2.0 FIFO Interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_TCG_TIS</code> to find this item.
       <*> TPM 2.0 CRB Interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_TCG_CRB</code> to find this item.
### USE flags


| [+fapi](https://packages.gentoo.org/useflags/+fapi) | Enable feature API (requires openssl as crypto backend) | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl as crypto engine | 
| [+policy](https://packages.gentoo.org/useflags/+policy) | Enable policy library (requires openssl as crypto backend) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [mbedtls](https://packages.gentoo.org/useflags/mbedtls) | Use net-libs/mbedtls as crypto engine | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask app-crypt/tpm2-tools``root #``emerge --ask app-crypt/tpm2-tss`
## Usage

### Checking the TPM version

/sys/class/tpm/ can be used to view TPM details such as the version:

`user $``cat /sys/class/tpm/tpm0/tpm_version_major`
### Checking the TPM status

To see if the TPM is locked:

`root #``tpm2_getcap properties-variable`
TPM2\_PT\_PERMANENT:
  ownerAuthSet:              0
  endorsementAuthSet:        0
  lockoutAuthSet:            0
  reserved1:                 0
  disableClear:              0
  inLockout:                 0
  tpmGeneratedEPS:           0
  reserved2:                 0
TPM2\_PT\_STARTUP\_CLEAR:
  phEnable:                  1
  shEnable:                  1
  ehEnable:                  1
  phEnableNV:                1
  reserved1:                 0
  orderly:                   1
...

This output indicates the TPM is unlocked and ready to be used.

### Password protecting the TPM

For each authorization type (*owner*, *endorsement*, and *lockout*), tpm\_changeauth -c can be used to update the password:

`root #``tpm2_changeauth -c owner new_owner_auth_password`
### Reading public keys

To view the public keys stored on the TPM, first a list of *persistent handles* must be obtained with:

`root #``tpm2_getcap handles-persistent`
\- 0x81000001
- 0x81000002
- 0x81000009
- 0x81010001

With the *handle*, the public key contents can be viewed with:

`root #``tpm2_readpublic -c 0x81000001`
### Viewing TPM register values

tpm2\_pcrread lists ALL PCR values of the TPM. If the system's configuration changes (for instance, if [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) is disabled, the CMOS is reset, or if any UEFI settings are changed), then these values will change.

`root #``tpm2_pcrread````
  sha1:
  sha256:
    0 : 0xB3BDD700D0E3BE5248B0B37395F8C07CCD3D12C9BEBFEFD2717E63A74C16171F
    1 : 0xF540343BED5BFD8D8902C874F2ADB907A6FA8213FA7942389D3AE4DD1D4B7570
    2 : 0xEF674CD26AD78DD8240A4723B1A527DE84218773ADEAAABF68A9A71B64E1DBA8
    3 : 0x3D458CFE55CC03EA1F443F1562BEEC8DF51C75E14A9FCF9A7234A13F198E7969
    4 : 0xB1F4D1EB09E1786C2FE17A2D7A0235D25981D3D42967551E75C4E6E02D774864
    5 : 0x1F94EED8C1BB1BB3D3782D0D2F75216F6EC4D67F435D872F139DDE8126D99BC8
    6 : 0x3D458CFE55CC03EA1F443F1562BEEC8DF51C75E14A9FCF9A7234A13F198E7969
    7 : 0x6438D21F9AB1B83FD3E7341F7C4C379DBE7003A2D4CC9BFB0C1856694BDA5492
    8 : 0x0000000000000000000000000000000000000000000000000000000000000000
    9 : 0x0000000000000000000000000000000000000000000000000000000000000000
    10: 0x4F27447C738A6F4CADD90E05B40888A72F448C57607C60B3A2B8F81FB7D8934B
    11: 0x0000000000000000000000000000000000000000000000000000000000000000
    12: 0x0000000000000000000000000000000000000000000000000000000000000000
    13: 0x0000000000000000000000000000000000000000000000000000000000000000
    14: 0x0000000000000000000000000000000000000000000000000000000000000000
    15: 0x0000000000000000000000000000000000000000000000000000000000000000
    16: 0x0000000000000000000000000000000000000000000000000000000000000000
    17: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    18: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    19: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    20: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    21: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    22: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    23: 0x0000000000000000000000000000000000000000000000000000000000000000
  sha384:
    0 : 0x417D61CA58074DE5CFD625F0D85B870F9A5CB47B5B91B8B4A61B16C368A6B5B1C26F7EB1F3DD2F1423BE80CA6E70A96D
    1 : 0xB5D743284345EAAFCD479CECA26E4A2534F1AA96C247D48B5A20556F040F07CE90B3B6ACAA8FBB5D16B842300D7C1886
    2 : 0xAB9EC365C4A0929685BF9FFE968B5A4815267AE3B01B81A03829E6CF36D03AA3CB0C8CB82B8272BE23C7C7E453E097E7
    3 : 0x518923B0F955D08DA077C96AABA522B9DECEDE61C599CEA6C41889CFBEA4AE4D50529D96FE4D1AFDAFB65E7F95BF23C4
    4 : 0x21031F9A56551177B82CCEB6A498188C674906E6F67588EC2B5DDF3EE50A01B60C8FAA323BE2F70F249A548315F6929C
    5 : 0xC0E688F4FBFF3F7F28D57A64C2439D8536855373BE4EE329C5A98F77FE239E89ADC6A4FFAA9BCB31D8ACC91FC798AA60
    6 : 0x518923B0F955D08DA077C96AABA522B9DECEDE61C599CEA6C41889CFBEA4AE4D50529D96FE4D1AFDAFB65E7F95BF23C4
    7 : 0x0B3BE1E49223C893F4C2006235E501DFA5EDC7B6CA743EAC123F2E73F19BE36CD5303E130BFFDA2EF2EB7273800C6063
    8 : 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    9 : 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    10: 0x9701564F204AB8CBF8E3F08B1690212168A23B3C7D62CB4B4A75F6CCFCC70966AED8455DE56B211CE5C2DB51493B53D5
    11: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    12: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    13: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    14: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    15: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    16: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    17: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    18: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    19: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    20: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    21: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    22: 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
    23: 0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```
To read a single value, such as the *sha384* hash of the *Secure Boot Policy*:

`root #``tpm2_pcrread sha384:7````
  sha384:
    7 : 0x0B3BE1E49223C893F4C2006235E501DFA5EDC7B6CA743EAC123F2E73F19BE36CD5303E130BFFDA2EF2EB7273800C6063
```
### Reading the TPM event log

`root #``tpm2_eventlog <(cat /sys/kernel/security/tpm0/binary_bios_measurements)`
The output of this command will contain detailed information about all TPM events.

Each event should contain the following attributes:

- **EventNum** - The event number, starts at 0.
- **PCRIndex** - The PCR index this event recorded to.
- **EventType** The type of event.
- **Digest** - The resulting digest of the event.
- **DigestCount** - How many digests are associated with this event.
- **Event** - Details about the event, such as the variable name and UUID if reading an EFI variable.

### Creating a new Primary Key

To create a new primary key using the *Storage* key:

`root #``tpm2_createprimary --key-context primary.ctx`
#### Saving the Primary Key context into the TPM

To load the primary key context into the TPM, for later use:

`root #``tpm2_evictcontrol -c primary.ctx`
### Save PCR data for key sealing

Once a trusted state has been established, *PCR* data can be logged to a file for key sealing:

`root #``tpm2_pcrread --output safestate.pcrread sha256:0,7` ### Create a TPM PCR policy for data sealing

A PCR policy can be created with:

`root #``tpm2_createpolicy --policy-pcr --pcr-list sha256:0,7 --pcr safestate.pcrread --policy trusted_policy.pol`
### Seal data

To store data in NVRAM wrapped using keys in the TPM:

`root #``printf "key data" | tpm2_create --parent-context primary.ctx --policy trusted_policy.pol --key-context data.ctx --public seal.pub --private seal.priv --sealing-input -`
#### CreateLoaded not supported

If there is an error saying *CreateLoaded* is not supported or *NOTE: The TPM does not support CreateLoaded command!*, the data must first be sealed, then loaded:

`root #````
printf "key data" | tpm2_create --parent-context primary.ctx --policy trusted_policy.pol --public seal.pub --private seal.priv --sealing-input -
```
`root #``tpm2_load --public seal.pub --private seal.priv --parent-context primary.ctx --key-context data.ctx`
### Unseal data

If the private key context is not already loaded, it must be loaded with tpm2\_load, using the handle returned by tpm2\_evictcontrol:

`root #``tpm2_load --public seal.pub --private seal.priv --parent-context 0x81000000 --key-context data.ctx`
If PCR register data matches the policy, and the keys are loaded, data can be unsealed with:

`root #``tpm2_unseal --object-context data.ctx --auth pcr:sha256:0,7`
### Disk Encryption

### SSH

### Random Number Generation

Using a hardware random number generator gives more entropy to the system and can therefore give faster random numbers. When the TPM drivers compiled in the kernel, there will be a new device named /dev/hwrng. This is the TPM random number generator.

To use it, first, emerge the package [sys-apps/rng-tools](https://packages.gentoo.org/packages/sys-apps/rng-tools) which will be used to redirect /dev/hwrng into /dev/random.

`root #``emerge --ask sys-apps/rng-tools`
Then, simply start the service. By default, rng-tools looks for /dev/hwrng so it does not need any configuration to work.

`root #``rc-service rngd start`
rngd                 | \* Caching service dependencies ...    \[ ok \]
rngd                 | \* Starting rngd ...                   \[ ok \]

`root #``rc-update add rngd default`
## NVRAM Usage

Some TPMs have NVRAM which can be used to store data persistently. Most commands default to using the *Storage* or *Owner* hierarchy. This can be changed by using **--hierarchy p** to use the *Platform* hierarchy.

### Viewing the NVRAM

To list populated public sections of the NVRAM, the following command can be used:

`root #``tpm2_nvreadpublic`
The output of this command contains several attributes such as the **handle** (*index*), in hexadecimal format, **name**, and:

- **hash algorithm** - Has the friendly name, such as *sha256* and the raw value, such as *0xbB*.
- **attributes** - The friendly list of attributes, and the corresponding bitfield mask value.
- **size** - The size of this memory region.

### Defining a Public NVRAM Region

**tpm2\_nvdefine** can be used to define memory regions, the size is in bytes, the command returns the handle of the defined region:

`root #``tpm2_nvdefine --size 32`
### Writing to a Public NVRAM Region

Using a handle, the NVRAM can be written to with **tpm2\_nvwrite**:

`root #``echo "test content" | tpm2_nvwrite 0x1000000 --input -`
### Reading a public NVRAM Region

To read a defined region, the **tpm2\_nvread** command can be used:

`root #``tpm2_nvread 0x1000000`
WARN: Reading full size of the NV index
test content

### Removing a NVRAM Region Definition

Memory regions can be undefined with:

`root #``tpm2_nvundefine 0x1000000`
## Troubleshooting

### The TPM event log cannot be read

For reasons currently unknown, sometimes the TPM event log will not be readable in Linux, a method to resolve this is to clear the TPM from Windows. This can be done by running **Clear-TPM** in an admin powershell session or selecting "Cear TPM" under the Device Security -> Security Processor Settings menu.

## See also

- [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) — an enhancement of the security of the pre-boot process of a [UEFI](https://wiki.gentoo.org/wiki/UEFI) system.
- [Nokia TPM Course](https://github.com/nokia/TPMCourse/tree/master/docs)
- [Arch Wiki: TPM](https://wiki.archlinux.org/title/Trusted_Platform_Module)

## External resources

- [Home Made TPM 2.0 Module](https://diy.viktak.com/2022/04/home-made-tpm2-0-module.html) — how to make a TPM module yourself.
- [Arch Linux Wiki - Trusted Platform Module](https://wiki.archlinux.org/title/Trusted_Platform_Module)
- [Trusted Computing Group - TPM 2.0 Documentation](https://trustedcomputinggroup.org/wp-content/uploads/PC-ClientSpecific_Platform_Profile_for_TPM_2p0_Systems_v51.pdf)
