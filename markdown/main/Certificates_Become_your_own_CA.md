<!-- source: https://wiki.gentoo.org/wiki/Certificates/Become_your_own_CA | group: Gentoo Wiki (Main) | wiki-title: Certificates/Become your own CA -->
---
title: Certificates/Become your own CA
url: https://wiki.gentoo.org/wiki/Certificates/Become_your_own_CA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-21"
fingerprint: b450e8721d3ecef1
license: CC BY-SA 4.0
---

# Certificates/Become your own CA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Applies to:

- small local networks running a critical number of TLS-secured services (HTTP server, message broker, LDAP)
- mutual TLS for authentication in a local network, esp. hosts that don't have any user input options

## What has to be done

1. Init a safe storage for key material
2. Write a configuration file (for OpenSSL) to limit the interaction for repeated tasks
3. create key material for a root CA with certificate on the safe storage
4. create key material and certificate for an intermediate CA on a safe machine and sign it with the root CA's certificate
5. issue a first CSR for a server certificate to be used with Apache/ Inspircd, OpenLDAP or others
6. sign the first CSR as intermediate CA
7. issue a second CSR for a client certificate to be used for example with Apache mutual TLS
8. sign the second CSR as intermediate CA

See [Local certificates](https://wiki.gentoo.org/wiki/Local_certificates) for an explanation how to publish the root CA's certificate to various hosts and truststores.

Necessary Material:

1. A safe storage for root CA key material (2 thumb drives plus either a hard disk or better a tape drive)
2. A safe host to start the root CA and run the intermediate CA
3. An offline password manager or strategy to store strong passwords safely and for a long time

## Root CA

### Safe storage

Safe storage means implies the **3-2-1 backup rule** has been implemented: data doesn't exist unless it exists in three copies, on two different media types. Two copies of the data should exist on independent storage technologies, with third copy off being located offsite. For example, one copy of the data will be on a USB flash storage drive, one copy on a DVD disk, and another copy on a USB drive that is stored off site.

The root CA will be valid more than 20 years and with the certificate spreading out onto hosts and services it is not easy to update. Ensure all media containing the CA root is encrypted, preferably with file-level *and* block level encryption. Watch the storage media's lifetime, wear, and health metrics. USB flash drives and/or hard drives need to be attached from time to time to a running system so that the drive's firmware can move blocks around from worn or bad sectors. It is also good to ensure the drive is still operating as expected.

Tape drives allow long term storage, whereas optical media such as CD, DVD, or Bluray disks are water proof and resistant to magnetic interference.

Prepare a first medium of the safe storage for example a thumb drive on the physical layer by encrypting it entirely or creating partitions or even volume groups. Later mirror that first medium to two other copies with different security properties like passwords. See for example [Dm-crypt full disk encryption](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption).

Now that the safe storage is unlocked and mounted to a safe system it is necessary to initiate it once with some directories and flat file databases. Also permissions need to be granted with safety in mind. Change to the root directory of the safe storage. All commands will be run relative to this root directory:

`user $````
cd root_dir
```
`user $````
mkdir certs crl newcerts private
```
`user $````
chmod 700 private
```
`user $````
touch index.txt
```
`user $````
echo 1000 > serial
```
### OpenSSL configuration file

The following configuration file example is used throughout the process. This limits interaction with commands and lowers the chance of errors when signing CSRs or issuing more certificates. Some sections reference each other. Consult the man page of openssl for details beyond the comments. Treat all properties with caution lookup more recent recommendations because safe algorithms when this was written might be weak when applying these steps.

**`openssl.cnf`**

```
[ca]
default_ca = CA_default
[CA_default]
# Directory and file locations. CRL is certificate revocation list
dir               = .
certs             = $dir/certs
crl_dir           = $dir/crl
new_certs_dir     = $dir/newcerts
database          = $dir/index.txt
serial            = $dir/serial
RANDFILE          = $dir/private/.rand
# The root key and root certificate.
private_key       = $dir/private/root-ca.key.pem
certificate       = $dir/certs/root-ca.cert.pem
# For certificate revocation lists.
crlnumber         = $dir/crlnumber
crl               = $dir/crl/ca.crl.pem
crl_extensions    = crl_ext
default_crl_days  = 30
# SHA-1 is deprecated, so use SHA-2 instead.
default_md        = sha256
name_opt          = ca_default
cert_opt          = ca_default
default_days      = 375
preserve          = no
policy            = policy_strict
[ policy_strict ]
# The root CA should only sign intermediate certificates that match.
# See the POLICY FORMAT section of `man ca`.
countryName             = match
stateOrProvinceName     = match
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional
[v3_ca]
# Extensions for a typical CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
```
### Key material and certificate

Use a strong password. If you prefer elliptic curve keys see openssl man pages for correct syntax. First a key pair is created:

`user $``openssl genrsa -aes256 -out private/root-ca.key.pem 4096`
enter passphrase: \*\*\*
verify passphares: \*\*\*

And secured against overwriting:

`user $``chmod 400 private/root-ca.key.pem`
Based on the key material a certificate has to be created. Mind that req either requires -config or falls back to a default configuration in /etc:

`user $````
openssl req -config openssl.cnf \
```
```
     -key private/root-ca.key.pem \
     -new -x509 -days 7300 -sha256 -extensions v3_ca \
     -out certs/ca.cert.pem
```
## Intermediate CA

## CSR for server certificate

## CSR for client certificate

## See also

Now forward links, later backward/ symmetric

- [Mosquitto](https://wiki.gentoo.org/wiki/Mosquitto)
- [Centralized authentication using OpenLDAP](https://wiki.gentoo.org/wiki/Centralized_authentication_using_OpenLDAP)
- [https://jamielinux.com/docs/openssl-certificate-authority/index.html](https://jamielinux.com/docs/openssl-certificate-authority/index.html) – Source of configuration, commands and hints
- [https://www.passwordcard.org/en](https://www.passwordcard.org/en) – printable password helper
- [https://github.com/62726164/dpg](https://github.com/62726164/dpg) – sample key exploder (derive a strong password from a simple phrase)
