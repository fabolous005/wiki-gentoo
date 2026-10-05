<!-- source: https://wiki.gentoo.org/wiki/Sequoia | group: Gentoo Wiki (Main) | wiki-title: Sequoia -->
---
title: Sequoia
url: https://wiki.gentoo.org/wiki/Sequoia
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-30"
fingerprint: f9b75789bf9cc4e8
license: CC BY-SA 4.0
---

# Sequoia

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Sequoia is a complete implementation of OpenPGP as defined by RFC 9580 as well as the deprecated OpenPGP as defined by RFC 4880, and various related standards.

## Installation

### app-crypt/sequoia-sop

#### USE flags


#### Emerge

`root #``emerge --ask app-crypt/sequoia-sop`
### app-crypt/sequoia-sq

#### USE flags


#### Emerge

`root #``emerge --ask app-crypt/sequoia-sq`
### app-crypt/sequoia-sqv

#### USE flags


#### Emerge

`root #``emerge --ask app-crypt/sequoia-sqv`
## Usage

### Generating a private key

To generate a key with Sequoia, use sq:

`user $``sq key generate --own-key --name="Larry the Cow" --email=larry@gentoo.org````
Please enter the password to protect key (press enter to not use a password):
                                                  Please repeat the password:
 - ┌ CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45
   └ Larry the Cow
   - certification created
 - ┌ CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45
   └ <larry@gentoo.org>
   - certification created
Transferable Secret Key.
      Fingerprint: CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45
  Public-key algo: EdDSA
  Public-key size: 256 bits
       Secret key: Unencrypted
    Creation time: 2025-12-30 03:07:04 UTC
  Expiration time: 2028-12-29 20:33:25 UTC (creation time + 2years 11months 30days 9h 16m 45s)
        Key flags: certification
           Subkey: 9973D6CA7A958FE5480EA80E1E844D52E76956F2
  Public-key algo: EdDSA
  Public-key size: 256 bits
       Secret key: Unencrypted
    Creation time: 2025-12-30 03:07:04 UTC
  Expiration time: 2028-12-29 20:33:25 UTC (creation time + 2years 11months 30days 9h 16m 45s)
        Key flags: authentication
           Subkey: 38FBC49A9FDD30D9C2B8CC76435C985217FB9A04
  Public-key algo: EdDSA
  Public-key size: 256 bits
       Secret key: Unencrypted
    Creation time: 2025-12-30 03:07:04 UTC
  Expiration time: 2028-12-29 20:33:25 UTC (creation time + 2years 11months 30days 9h 16m 45s)
        Key flags: signing
           Subkey: 83A17D6143D68378BA7200F6A9E1CD39D1245FA0
  Public-key algo: ECDH
  Public-key size: 256 bits
       Secret key: Unencrypted
    Creation time: 2025-12-30 03:07:04 UTC
  Expiration time: 2028-12-29 20:33:25 UTC (creation time + 2years 11months 30days 9h 16m 45s)
        Key flags: transport encryption, data-at-rest encryption
           UserID: <larry@gentoo.org>
   Certifications: 1, use --certifications to list
           UserID: Larry the Cow
   Certifications: 1, use --certifications to list
Hint: Because you supplied the `--own-key` flag, the user IDs on this key have been marked as authenticated, and this key has been marked as
      a fully trusted introducer.  If that was a mistake, you can undo that with:
  $ sq pki link retract --cert=CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45 --all
Hint: You can export your certificate as follows:
  $ sq cert export --cert=CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45
Hint: Once you are happy you can upload it to public directories using:
  $ sq network keyserver publish --cert=CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45
```
### Exporting a certificate

To export the certificate of a key, use sq cert export:

`user $``sq cert export --cert-email larry@gentoo.org`
### Publishing a certificate

To publish a certificate, use sq network:

`user $``sq network keyserver publish --cert CD6FD5384B28BFAD8E3DAAE0ACDF8D7BA1B01D45`
## See also

- [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) — a free implementation of the OpenPGP standard (RFC 4880).
