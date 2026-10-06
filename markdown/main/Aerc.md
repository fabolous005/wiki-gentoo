<!-- source: https://wiki.gentoo.org/wiki/Aerc | group: Gentoo Wiki (Main) | wiki-title: Aerc -->
---
title: aerc
url: https://wiki.gentoo.org/wiki/Aerc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-11"
fingerprint: f7bbf9c9dfc77885
license: CC BY-SA 4.0
---

# aerc

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

aerc is a lightweight, command-line [mail user agent](https://en.wikipedia.org/wiki/mail_user_agent) (MUA) written in the [Go](https://wiki.gentoo.org/wiki/Go) programming language. Setup includes a simple startup wizard to assist users in connecting their first mail server.

## Installation

### USE flags


### USE flags for
            [mail-client/aerc](https://packages.gentoo.org/packages/mail-client/aerc)
            
            Email client for your terminal

| [notmuch](https://packages.gentoo.org/useflags/notmuch) | Enable support for net-mail/notmuch | 

### Emerge

`root #``emerge --ask mail-client/aerc`
### Additional software

#### text/html MIME type support

To view email with embedded text/html MIME type content, the [www-client/w3m](https://packages.gentoo.org/packages/www-client/w3m) and [net-proxy/dante](https://packages.gentoo.org/packages/net-proxy/dante) packages should be installed (according to man aerc-tutorial), and the text/html filter uncommented in the \~/.config/aerc/aerc.conf configuration file.

## Configuration

### Setup wizard

In general, to setup, run aerc and enter the values as appropriate, substituting the following information as necessary. It should take all of about 2 minutes to get connected to a mail server presuming the connection information is readily available.

For example, for a Gentoo developer to connect their email account to the mail server provided by Gentoo's [Infrastructure project](https://wiki.gentoo.org/wiki/Project:Infrastructure):

**Basic account information:**

| First page of startup wizard |  | 
|---|---|
| Field | Value | 
|---|---|
| Name | Gentoo | 
| Full name | Larry the Cow | 
| Email address | larry@gentoo.org | 

**Incoming mail (IMAP):**

| Second page of startup wizard (IMAP) |  | 
|---|---|
| Field | Value | 
|---|---|
| username | larry | 
| password | \<password> | 
| Server address | dev.gentoo.org:143 | 
| Connection mode | IMAP with STARTTLS | 

**Outgoing mail (SMTP):**

| Third page of startup wizard (SMTP) |  | 
|---|---|
| Field | Value | 
|---|---|
| username | larry | 
| password | \<password> | 
| Server address | smtp.gentoo.org:587 | 
| Connection mode | SMTP with STARTTLS | 

Once the program is connected to a mail account, it is helpful to perform some configuration changes to make it more friendly for reading Gentoo related mail.

### Files

If conversation threads/threading is desired, then (for at least v0.7.1 and prior) a filter will need to be applied to enable threading:

**`~/.config/aerc/aerc.conf`**

**Helpful aerc customization for Gentoo**

```
#
# aerc main configuration
# Added to thread discussions in Gentoo
[ui:account=Gentoo]
threading-enabled=true
```
## See also

- [Mutt](https://wiki.gentoo.org/wiki/Mutt) — a text-based, command-line mail user agent (MUA).
- [Neomutt](https://wiki.gentoo.org/wiki/Neomutt) — command-line mail client forked from [mutt](https://wiki.gentoo.org/wiki/Mutt).
- [Thunderbird](https://wiki.gentoo.org/wiki/Thunderbird) — Mozilla's solution to the e-mail client.
