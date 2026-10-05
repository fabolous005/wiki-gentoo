<!-- source: https://wiki.gentoo.org/wiki/OATH-Toolkit | group: Gentoo Wiki (Main) | wiki-title: OATH-Toolkit -->
---
title: OATH-Toolkit
url: https://wiki.gentoo.org/wiki/OATH-Toolkit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-01"
fingerprint: be11e53806d7b9d5
license: CC BY-SA 4.0
---

# OATH-Toolkit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**OATH-Toolkit** is a free software toolkit for (OTP) One-Time Password authentication using HOTP/TOTP algorithms. The software ships a small set of command line utilities covering most OTP operation related tasks.

This configuration procedure explains OATH-Toolkit PAM authentication using the OpenSSH deamon. Further configuration examples are found in the official documentation.

## Installation

Following packages need to be build using PAM authentication support, when configuring authentication on the server:

Create following portage configuration files on the server:

**`/etc/portage/package.use/oath-toolkit`**

**Enabling PAM USE flag for oath-toolkit**

**`/etc/portage/package.use/openssh`**

**Enabling PAM USE flag for openssh**

Rebuild the software listed using `pam` support on the SSH server.

### USE flags


| [pam](https://packages.gentoo.org/useflags/pam) | Build PAM module for pluggable login authentication for OATH | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask sys-auth/oath-toolkit`
### Additional software

Install PAM supporting software, the OpenSSH server:

`root #``emerge --ask net-misc/openssh`
## Configuration

The setup procedure described is taken from the [official website](https://www.nongnu.org/oath-toolkit/pam_oath.html) and applied to gentoo specifics.

### Client

Prior to configuration of the OATH-toolkit, each system user needs a secret. The secrets mentioned

- **Hex secret** \~/.oath\_toolkit on the target SSH server, as a plain text file, not encrypted.
- **Base32 secret** in a [password management tools](https://wiki.gentoo.org/wiki/Password_management_tools) on personal computer, mobile, encrypted storage.

#### Generate secret

Generating a secret using oathtool and the random output of openssl library:

`user $``oathtool --verbose $(openssl rand -hex 16)`
Hex secret: c869145915b76907f0bd7e33feacf3bd
Base32 secret: ZBURIWIVW5UQP4F5PYZ75LHTXU======
Digits: 6
Window size: 0
Start counter: 0x0 (0)
260781

#### Hex secret

Generated Hex secret is stored to \~/.oath\_toolkit file, using following syntax:

**`~/.oath_toolkit`**

```
#token         user   PIN  secret
HOTP/T30       larry  -    c869145915b76907f0bd7e33feacf3bd
```
The specific entries and the syntax are explained on [the official documentation](https://code.google.com/archive/p/mod-authn-otp/wikis/UsersFile.wiki) website.

Finally restrict the file permissions to be **read** and **write** for to current user only:

`user $``chmod go-rw ~/.oath_toolkit`
#### Base32 secret

Use one of the [password management tools](https://wiki.gentoo.org/wiki/Password_management_tools) listed to store the **Base32 secret** encrypted. Password management tools generate the OTP one-time password needed for authentication.

### Server

#### Environment variables

- `usersfile` - Specify filename where credentials are stored, f.e. **/etc/users.oath**. The placeholder values **${USER}** and **${HOME}** may be used to specify the filename on a per-user basis. The values **${USER}** and **${HOME}** expand to the user's username and home directory, respectively.
- `window` - Specify search depth, an integer typically from **5** to **50** but other values can be useful too.
- `digits` - Specify number of digits in the one-time password, required when using passwords in usersfile. Supported values are **6** , **7**, and **8**.

#### Files

- /etc/pam.d/sshd - OpenSSH's PAM module authentication configuration file.
- /etc/ssh/sshd\_config.d/10\_OTP-OATH-toolkit.conf - Mandatory OpenSSH's configuration setting for OATH toolkit.
- ${HOME}/.oath\_toolkit - Conventional path and filename setting for user credentials.

##### OATH-Toolkit

**path** and **filename** variables used for `usersfile:`, can be set to *everything* to match the specific environement needs.

The filename for user credentials is set in this document to \~/.oath\_toolkit. This is to match the *default naming scheme* of  [Google Authenticator](https://wiki.gentoo.org/wiki/Google_Authenticator) which creates \~/.google\_authenticator. It makes both setups easier to compare. Both packages are just different implementations of the same technology.

3 different types of tokens can be configured for the authentication process:

- **event-based**: `HOTP` - HOTP event-based token with six digit OTP
- **time-based**: `HOTP/T60/5` - HOTP time-based token with 60 second interval and five digit OTP
- **mobile**: `MOTP` - Mobile-OTP time-based token 10 second interval and six digit OTP

Further file format and parameter examples are listed in [the official documentation](https://code.google.com/archive/p/mod-authn-otp/wikis/UsersFile.wiki) website.

##### OpenSSH

Following steps are mandatory for the PAM authentication to work:

**PAM**

Read the PAM module manual for the configuration parameters set in /etc/pam.d/sshd:

`user $``man pam.conf`
Further specific module configuration parameters used (*usersfile=, window=,digits=*) for **pam\_oath.so** are explained in the [manual for pam\_oath on the official website](https://www.nongnu.org/oath-toolkit/pam_oath.html).

Add the first **requisite** configuration line to the /etc/pam.d/sshd file, and keep the added rule at the highest position in this file:

**`/etc/pam.d/sshd`**

**Set*${HOME}/.oath\_toolkit* as default credentials file**

```
auth       requisite    pam_oath.so usersfile=${HOME}/.oath_toolkit window=20 digits=6
auth       include      system-remote-login
account    include      system-remote-login
password   include      system-remote-login
session    include      system-remote-login
```
**Authentication**

Create OATH specific autentication configuration:

**`/etc/ssh/sshd_config.d/10_OTP-OATH-toolkit.conf`**

```
UsePAM yes
ChallengeResponseAuthentication yes
```
After applied configuration settings, restart the openssh daemon:

`root #``service sshd restart`
## Usage

Demostration of a working 2FA authentication sequence for a example SSH session for [larry](https://wiki.gentoo.org/wiki/Larry):

`user $``ssh gentoo`
(larry@gentoo) One-time password (OATH) for \`larry': 
(larry@gentoo) Password:
larry@gentoo \~ $

The authenticating SSH user **larry** will get displayed:

1. display ``One-time password (OATH) for `larry':`` at the prompt. User OTP authentication.
2. IF the put in token is found VALID, THEN display `Password:` at the prompt. User keyboard-interactive authentication.

Once a user authentication process has been successfully finished, \~/.oath\_toolkit will show a added timestamp. Timestamp for the **last** successful authentication, and the used (OTP) one-time password `665004`:

**`/home/larry/.oath_toolkit`**

```
#token         user   PIN  secret
HOTP/T30        larry   -       c869145915b76907f0bd7e33feacf3bd  0       665004  2024-01-11T13:37:42L
```
### Generate OTP

Example how to generate a OTP one-time password token using the oathtool and the given secrets.

Generate a one-time password using the Hex secret:`c869145915b76907f0bd7e33feacf3bd`:

`user $``oathtool --totp c869145915b76907f0bd7e33feacf3bd`
665004

Generate a one-time password using the Base32 secret:`ZBURIWIVW5UQP4F5PYZ75LHTXU`:

`user $``oathtool --totp -b ZBURIWIVW5UQP4F5PYZ75LHTXU`
665004

### Invocation

`user $``oathtool --help````
Usage: oathtool [OPTION]... [KEY [OTP]]...
Generate and validate OATH one-time passwords.  KEY and OTP is the string '-'
to read from standard input, '@FILE' to read from indicated filename, or a hex
encoded value (not recommended on multi-user systems).
  -h, --help                    Print help and exit
  -V, --version                 Print version and exit
      --hotp                    use event-based HOTP mode  (default=on)
      --totp[=MODE]             use time-variant TOTP mode (values "SHA1",
                                  "SHA256", or "SHA512")  (default=`SHA1')
  -b, --base32                  use base32 encoding of KEY instead of hex
                                  (default=off)
  -c, --counter=COUNTER         HOTP counter value
  -s, --time-step-size=DURATION TOTP time-step duration  (default=`30s')
  -S, --start-time=TIME         when to start counting time steps for TOTP
                                  (default=`1970-01-01 00:00:00 UTC')
  -N, --now=TIME                use this time as current time for TOTP
                                  (default=`now')
  -d, --digits=DIGITS           number of digits in one-time password
  -w, --window=WIDTH            number of additional OTPs to generate or
                                  validate against
  -v, --verbose                 explain what is being done  (default=off)
Report bugs to: oath-toolkit-help@nongnu.org
oathtool home page: <https://www.nongnu.org/oath-toolkit/>
General help using GNU software: <https://www.gnu.org/gethelp/>
```
## Troubleshooting

### Debugging authenticaton

To get more verbose debug while troubleshooting authentication configuration, add the **debug** configuraton option to the system PAM module used. In example shown enabling debug on su binary.

**`/etc/pam.d/su`**

```
auth [user_unknown=ignore success=ok] pam_oath.so debug usersfile=/etc/users.oath window=20
```
Run su. At the prompt type anything for the password and the OTP token to get interactive debug output:

`user $``su`
\[pam\_oath.c:parse\_cfg(122)\] called.
\[pam\_oath.c:parse\_cfg(123)\] flags 0 argc 4
\[pam\_oath.c:parse\_cfg(125)\] argv\[0\]=alwaysok
\[pam\_oath.c:parse\_cfg(125)\] argv\[1\]=debug
\[pam\_oath.c:parse\_cfg(125)\] argv\[2\]=usersfile=/etc/users.oath
\[pam\_oath.c:parse\_cfg(125)\] argv\[3\]=window=20
\[pam\_oath.c:parse\_cfg(126)\] debug=1
\[pam\_oath.c:parse\_cfg(127)\] alwaysok=1
\[pam\_oath.c:parse\_cfg(128)\] try\_first\_pass=0
\[pam\_oath.c:parse\_cfg(129)\] use\_first\_pass=0
\[pam\_oath.c:parse\_cfg(130)\] usersfile=/etc/users.oath
\[pam\_oath.c:parse\_cfg(131)\] digits=0
\[pam\_oath.c:parse\_cfg(132)\] window=20
\[pam\_oath.c:pam\_sm\_authenticate(162)\] get user returned: root
One-time password (OATH) for \`root':
\[pam\_oath.c:pam\_sm\_authenticate(237)\] conv returned: 328482
\[pam\_oath.c:pam\_sm\_authenticate(297)\] OTP: 328482
\[pam\_oath.c:pam\_sm\_authenticate(308)\] authenticate rc 0 last otp Wed Jan  3 19:22:50 2024
\[pam\_oath.c:pam\_sm\_authenticate(329)\] done. \[Success\]
\[pam\_oath.c:pam\_sm\_setcred(341)\] called.
\[pam\_oath.c:pam\_sm\_setcred(347)\] retval: 0
\[pam\_oath.c:pam\_sm\_setcred(367)\] done. \[Success\]

Applications need to be rebuild using the `debug` portage USE flag to generate more verbose authentication logs.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-auth/oath-toolkit`
## See also

- [Google Authenticator](https://wiki.gentoo.org/wiki/Google_Authenticator) — describes an easy way to setup two-factor authentication on Gentoo.
- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
- [PAM securetty](https://wiki.gentoo.org/wiki/PAM_securetty) — restricting root authentication with [PAM](https://wiki.gentoo.org/wiki/PAM).
- [Password management tools](https://wiki.gentoo.org/wiki/Password_management_tools) — This meta article is dedicated to secure password generation, auditing of generated passwords for security, and management of existing passwords.
- [YubiKey](https://wiki.gentoo.org/wiki/YubiKey) — a hardware security device that can be used to safely store cryptographic keys, OTP tokens, and challenge response seeds
