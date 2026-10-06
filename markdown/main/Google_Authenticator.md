<!-- source: https://wiki.gentoo.org/wiki/Google_Authenticator | group: Gentoo Wiki (Main) | wiki-title: Google Authenticator -->
---
title: Google Authenticator
url: https://wiki.gentoo.org/wiki/Google_Authenticator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-17"
fingerprint: aaa8c83a0e997dcd
license: CC BY-SA 4.0
---

# Google Authenticator

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes an easy way to setup two-factor authentication on Gentoo. The google-authenticator project provides mobile applications and a PAM module that make this possible. Setting up two-factor authentication using Google Authenticator requires a supported mobile device. Currently Android, iOS and Blackberry are supported. Note that this authentication method does not send information to Google's servers, it just uses Google's two-factor authentication protocol and (open source) code.

Google published a document how the two-factor authentication system works [\[1\]](https://support.google.com/accounts/bin/answer.py?hl=en&topic=1056283&answer=180744&rd=1) for their web-based systems. Using the code provided by the google-authenticator project it's possible to setup the same authentication method for your own systems.

## Server setup

The google-authenticator project comes with a PAM module that allows any PAM-aware program to support two-factor authentication. The host-side setup consists of installing and configuring the PAM module. This allows any PAM-aware program use two-factor authentication.

### Installation

Install google-authenticator:

`root #``emerge --ask sys-auth/google-authenticator`
#### Additional software

As per the ebuild's messages [media-gfx/qrencode](https://packages.gentoo.org/packages/media-gfx/qrencode) is used to generate QR codes, which makes transferring shared secrets easier. The installation of qrencode is optional, not mandatory for the setup.

`root #``emerge --ask media-gfx/qrencode`
### Configuration

To enable two-factor authentication for the whole system, globally, add following line to the PAM configuration file.

**`/etc/pam.d/system-auth`**

```
auth 	   required	pam_google_authenticator.so
```
Rather than requiring multi-factor authentication every time, it is possible to make it optional for public places, unsafe networks, CCTV-covered areas, but fall back to using the regular password at home or when it's felt safe to do so.

**`/etc/pam.d/system-auth`**

```
auth 	   sufficient	pam_google_authenticator.so
```
It's also possible to enable two-factor authentication for specific programs by configuring them one at a time.

#### OpenSSH

Use following configuration to setup PAM and two-factor authentication for OpenSSH only:

**`/etc/ssh/sshd_config.d/10_OTP-google-auth.conf`**

```
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
# Change to no to disable s/key passwords
ChallengeResponseAuthentication yes
# Set this to 'yes' to enable PAM authentication, account processing, 
# and session processing. If this is enabled, PAM authentication will 
# be allowed through the ChallengeResponseAuthentication and
# PasswordAuthentication.  Depending on your PAM configuration,
# PAM authentication via ChallengeResponseAuthentication may bypass
# the setting of "PermitRootLogin without-password".
# If you just want the PAM account and session checks to run without
# PAM authentication, then enable this but set PasswordAuthentication
# and ChallengeResponseAuthentication to 'no'.
UsePAM yes
```
To allow 2FA for the root user to login with a password, following options in the configuration need to be set. If not, an "Invalid verification code" will be displayed in the log:

**`/etc/ssh/sshd_config.d/20_OTP-google-auth-root.conf`**

```
#change the default if you want to re-allow that
#PermitRootLogin prohibit-password
PermitRootLogin yes
```
Finally enable two-factor authentication for OpenSSH only:

**`/etc/pam.d/sshd`**

```
auth 	   required	pam_google_authenticator.so
auth       include	system-remote-login
account    include	system-remote-login
password   include	system-remote-login
session	   include	system-remote-login
```
### Secret generation

The easiest way to distribute secrets to client machines is by scanning QR codes. If you're planning on using a QR code you'll have to have a terminal window that is at least 85 characters wide otherwise the QR code will be unreadable and you'll need to generate another code (or deal with editing the ASCII art). Once you've resized your terminal window properly you can generate a key by logging in to the host and running the key generation wizard.

`user $``google-authenticator`
Do you want authentication tokens to be time-based (y/n) y
Warning: pasting the following URL into your browser exposes the OTP secret to Google:
  https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://totp/larry@gentoo%3Fsecret%3DZBURIWIVW5UQP4F5PYZ75LHTXU%26issuer%3Dgentoo
Failed to use libqrencode to show QR code visually for scanning.
Consider typing the OTP secret into your app manually.
Your new secret key is: ZBURIWIVW5UQP4F5PYZ75LHTXU
Enter code from app (-1 to skip): -1
Code confirmation skipped
Your emergency scratch codes are:
  26221962
  22963189
  57587651
  88889973
  75012523
Do you want me to update your "/home/larry/.google\_authenticator" file? (y/n) y
Do you want to disallow multiple uses of the same authentication
token? This restricts you to one login about every 30s, but it increases
your chances to notice or even prevent man-in-the-middle attacks (y/n) y
By default, a new token is generated every 30 seconds by the mobile app.
In order to compensate for possible time-skew between the client and the server,
we allow an extra token before and after the current time. This allows for a
time skew of up to 30 seconds between authentication server and client. If you
experience problems with poor time synchronization, you can increase the window
from its default size of 3 permitted codes (one previous code, the current
code, the next code) to 17 permitted codes (the 8 previous codes, the current
code, and the 8 next codes). This will permit for a time skew of up to 4 minutes
between client and server.
Do you want to do so? (y/n) n
If the computer that you are logging into isn't hardened against brute-force
login attempts, you can enable rate-limiting for the authentication module.
By default, this limits attackers to no more than 3 login attempts every 30s.
Do you want to enable rate-limiting? (y/n) y

The wizzard will ask questions and generate an ASCII art QR code. Keep this QR code visible and continue with setting up your client. This QR code contains the shared secret that will be transferred to your client device, so it's important to keep this safe. The QR code will only be used once, so I recommend just keeping it in your terminal's buffer.

You can generate a new secret at any time, so it's not necessary to backup this secret (in fact, that somewhat defeats the point). By default the wizard will create a file called .google\_authenticator in the users home directory that contains the shared secret, this file must be kept secure.

**`/home/larry/.google_authenticator`**

```
ZBURIWIVW5UQP4F5PYZ75LHTXU
" RATE_LIMIT 3 30
" DISALLOW_REUSE
" TOTP_AUTH
26221962
22963189
57587651
88889973
75012523
```
The generated .google\_authenticator file has following, working default file permissions:

`user $``ls -lah`
-r-------- 1 larry larry 110 Jan  7 09:45 .google\_authenticator

### Exclude IP range

Depending on the given situation it might be useful to disable 2FA for certain, trusted IP ranges on the server side. In example 3 IP networks are excluded from activating the 2FA authentication process on the server:

- 192.168.10.0/24 - example local eth0 interface addressing
- 203.0.113.0/24 - example IP public range
- 127.0.0.0/8 - localhost lo interface

To exclude specific networks from google authenticator create the file /etc/security/access-trusted.conf defining the excluded IP network ranges:

**`/etc/security/access-trusted.conf`**

```
# Skip 2FA for following trusted IP networks:
+ : ALL : 192.168.10.0/24
+ : ALL : 203.0.113.0/24
+ : ALL : LOCAL
- : ALL : ALL
```
The LOCAL entry disables authentication if the request is comming from the the server(localhost) itself with the source ip address 127.0.0.1 which is lo. No need to enable 2FA here if you are already authenticated. Exclude LOCAL (lo) networks. The last configuration entry enables 2FA if source IP address of the request does not match.

Activate the IP ranges by changing the /etc/pam.d/sshd file to look like shown in example:

**`/etc/pam.d/sshd`**

```
auth [success=1 default=ignore] pam_access.so accessfile=/etc/security/access-trusted.conf
auth      required  pam_google_authenticator.so
auth      include   system-remote-login
account   include   system-remote-login
password  include   system-remote-login
session   include   system-remote-login
```
It is important the exception handling rule is configured at the first entry.

Restart the OpenSSH server to activate the configuration:

`root #``service sshd restart`
### Enforcing 2FA: Public Key and Token Authentication

In order to enforce Public key and Token authentication, a few extra steps must be taken.

Comment out auth ( as shown below ) and modify the access-file directive as required.

If the access-trusted.conf is not needed you may comment it out.

**`/etc/pam.d/sshd`**

```
auth [success=1 default=ignore] pam_access.so accessfile=/etc/security/access-trusted.conf
auth      required  pam_google_authenticator.so
# auth      include   system-remote-login
account   include   system-remote-login
password  include   system-remote-login
session   include   system-remote-login
```
Review 20\_OTP-google-auth-root.conf as you may not want root authentication. If this is not required within your environment, set PermitRootLogin to no.

**`/etc/ssh/sshd_config.d/20_OTP-google-auth-root.conf`**

```
#change the default if you want to re-allow that
#PermitRootLogin prohibit-password
PermitRootLogin no
```
Update the main sshd\_config to reflect our new authentication scheme. The configuration file should include the directives shown below.

Depending on your specific implementation, you may or may not want the allowUsers configuration directive. This will however isolate the ssh daemon to only specific users who may log in remotely with valid two factor authentication.

sshd\_config — OpenSSH daemon configuration file:
[https://man.openbsd.org/sshd\_config](https://man.openbsd.org/sshd_config)

**`/etc/ssh/sshd_config`**

```
PermitRootLogin no
# AllowGroups root
AllowUsers username
PermitEmptyPasswords no 
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive:pam
```
#### \*Warning\*

It is advised to keep an open privileged session in a secondary terminal prior to reloading the sshd daemon.

Reload the ssh daemon

`root #``service sshd restart``root #``systemctl restart sshd.service`
Test the connection. You should be presented with public key authentication ( unless ssh-agent is already exported ) and a token will be requested to finish the authenticated session.

## Client setup

The client-side setup is specific to the device that will be used to store the shared secret. The google-authenticator project provides client programs for Android, iOS, and Blackberry.

### Linux

Use one of the password manager tools to generate OTP tokens

### Android

Use on of the available fdroid TOTP tools to create one time passwords:

## See also

- [OATH-Toolkit](https://wiki.gentoo.org/wiki/OATH-Toolkit) — toolkit for (OTP) One-Time Password authentication using HOTP/TOTP algorithms.
- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
- [PAM securetty](https://wiki.gentoo.org/wiki/PAM_securetty) — restricting root authentication with [PAM](https://wiki.gentoo.org/wiki/PAM).
- [YubiKey](https://wiki.gentoo.org/wiki/YubiKey) — a hardware security device that can be used to safely store cryptographic keys, OTP tokens, and challenge response seeds
- [Password management tools](https://wiki.gentoo.org/wiki/Password_management_tools) — This meta article is dedicated to secure password generation, auditing of generated passwords for security, and management of existing passwords.
