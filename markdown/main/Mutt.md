<!-- source: https://wiki.gentoo.org/wiki/Mutt | group: Gentoo Wiki (Main) | wiki-title: Mutt -->
---
title: Mutt
url: https://wiki.gentoo.org/wiki/Mutt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: e615695c49a7b8f4
license: CC BY-SA 4.0
---

# Mutt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**mutt** is a text-based, command-line mail user agent (MUA). mutt is one of the current console-based mail clients that's still under active development and has a vast crowd of active supporters (and users). It is powerful, highly customizable, small, and efficient.

[neomutt](https://wiki.gentoo.org/wiki/Neomutt) is a fork of mutt which is very similar, to which most of this article also applies.

## Introduction

While Mutt was originally designed to read mail from the local mbox mail spool (e.g. /var/spool/mail/), nowadays it comes with full support for Maildir stored folders, remote fetching from POP3 servers and complete management of IMAP accounts. For a full description of what Mutt can do, please read [the Mutt manual](http://www.mutt.org/doc/manual).

## Installation

Starting your Mutt adventure simply requires you to emerge it. Unfortunately, Mutt has a lots of options, which enable or disable certain functionalities of Mutt. We now briefly discuss the most important USE flags that you may want to enable based on your intended usage of Mutt. Please note that enabling most of them won't harm your Mutt, but may make it do more than an experienced Mutt user would like.

### Emerge

`root #``emerge --ask --verbose mail-client/mutt`
### USE flags


| [+hcache](https://packages.gentoo.org/useflags/+hcache) | Enable header cache, one database backend needs to be enabled | 
| [+imap](https://packages.gentoo.org/useflags/+imap) | Add support for IMAP (Internet Mail Application Protocol) | 
| [+lmdb](https://packages.gentoo.org/useflags/+lmdb) | Enable dev-db/lmdb database backend for header caching | 
| [+sasl](https://packages.gentoo.org/useflags/+sasl) | Add support for the Simple Authentication and Security Layer | 
| [+smtp](https://packages.gentoo.org/useflags/+smtp) | Enable support for direct SMTP delivery | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [autocrypt](https://packages.gentoo.org/useflags/autocrypt) | Enable autocrypt.org support | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Enable sys-libs/db database backend for header caching | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Enable sys-libs/gdbm database backend for header caching | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [gpgme](https://packages.gentoo.org/useflags/gpgme) | Build gpgme backend to support S/MIME, PGP/MIME and traditional/inline PGP | 
| [gsasl](https://packages.gentoo.org/useflags/gsasl) | Use GNU SASL via net-misc/gsasl instead of Cyrus SASL (requires USE=sasl) | 
| [idn](https://packages.gentoo.org/useflags/idn) | Enable support for Internationalized Domain Names | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [mbox](https://packages.gentoo.org/useflags/mbox) | Add support for mbox (/var/spool/mail) style mail spools | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pop](https://packages.gentoo.org/useflags/pop) | Enable support for POP3 mailboxes | 
| [prefix](https://packages.gentoo.org/useflags/prefix) | Defines if a Gentoo Prefix offset installation is used | 
| [qdbm](https://packages.gentoo.org/useflags/qdbm) | Add support for the qdbm (Quick Database Manager) library | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [slang](https://packages.gentoo.org/useflags/slang) | Add support for the slang text display library (it's like ncurses, but different) | 
| [tokyocabinet](https://packages.gentoo.org/useflags/tokyocabinet) | Enable dev-db/tokyocabinet database backend for header caching | 
| [vanilla](https://packages.gentoo.org/useflags/vanilla) | Do not add extra patches which change default behaviour; DO NOT USE THIS ON A GLOBAL SCALE as the severity of the meaning changes drastically | 

#### IMAP

The `imap` USE flag is probably the most important feature to enable for Mutt. Enabling it is only beneficial. Most email providers, even free ones such as Gmail, use IMAP, as it is the most convenient way to store email that is accessed from multiple clients at the same time and/or different locations. Because IMAP keeps all mail at the server, Mutt just downloads the messages that you want to view.

#### Header cache backends

Next to reading messages, it often happens that you will list a mailbox, to see what is in there.  For this information, Mutt has to download the message headers.  When you switch folders frequently, or your folders contain a large amount of emails, downloading the message headers over and over again will take some time. Since this simply is a waste, Mutt uses a so-called header cache (USE flag `hcache`) to keep the most important bits of messages that it needs to greatly speedup opening folders. This header cache is backed by a db-library, of which five flavours exist: `berkdb`, `gdbm`, `lmdb`, `qdbm`, and `tokyocabinet`. If you don't have any preference yourself, pick `lmdb` as it is the fastest when used with Mutt. You can only enable at most one db-library USE flag for `hcache` backend.  If you re-emerge Mutt with a different db-library later, Mutt will rebuild its caches automatically when it opens a folder.

#### Sending mail

While IMAP is important for reading mail, sending mail requires a mail server. Mutt can deliver mail using a local (send)mail submission, but often that's not a good solution for users that travel around (e.g. laptop users). Mutt comes with SMTP support which is enabled by default with the `smtp` USE flag. Mutt's SMTP support allows you to send mail over a mail server of your choice with the option to authenticate an account. This is usually an SMTP server given to you by the email provider.

#### Secure protocols

Both IMAP and SMTP protocols send data over encrypted channels. With the `imap` and/or `smtp` USE flags enabled, it is  wise to enable the `ssl` or `gnutls` USE flags. Both add the secure variants (imaps and smtps) to Mutt's list of supported protocols using either OpenSSL's or GNUTLS' implementation respectively. Most readers will not have a strong preference between OpenSSL and GNUTLS, so just go for `ssl`, since it is most likely this is already in the system's global list of USE flags. When intending to authenticate when sending e-mail, be sure to also include the `sasl` USE flag. The `sasl` USE flag is required for authentication when sending email.

#### Encryption

Even with secure IMAPS and SMTPS protocols, it is best practice to sign and/or encrypt email messages. Mutt supports traditional OpenPGP and S/MIME. Both of these are supported using the gpgme wrapper. The easiest way to setup support for signed and encrypted messages is using the `gpgme` USE flag. Documentation and experiences in this area can be confusing to say the least. When enabling `gpgme` USE flag, ensure the backend has been properly setup in the configuration (see below).

## Configuration

After you emerge Mutt with some choice USE flags, the only necessary next step is to create a .muttrc file. muttrc's are to be found in many places on the web and in Mutt's documentation. In /usr/share/doc/mutt-\<version>/samples some muttrc samples from the official distribution can be found. A very minimal .muttrc for an IMAP based account with SMTP mail delivery is shown below. It also enables signing emails via gpg using the gpgme backend.

### Files

#### .muttrc

**`~/.muttrc`**

**A .muttrc example file**

The example .muttrc above sets up an IMAP account, uses an SMTP server to send mail, stores its cache in /var/tmp/.mutt, reads the known address aliases (think of it as an address book) from \~/.aliases and appends the signature from \~/.signature when composing new mail. For some IMAP servers it may be necessary to change the spool, record and postponed directories, as the folders Sent and Drafts may be under a folder called INBOX. Simply trying this out with Mutt is the simplest way to figure this out.

Once the .muttrc is setup, you are ready to launch Mutt by just running mutt. If you entered a valid IMAP server URL, Mutt will prompt for a password and afterwards load all messages. Note that the first time entering your mailbox may take a while if you have quite some messages, since Mutt's header cache is still empty. If this succeeds you're in your IMAP mailbox ready to go.

Navigation is intuitive, as is reading messages by just pressing the `Enter` key or `Space` bar. Mutt is quite Vim alike in that it uses key strokes to perform most of its actions. You best read Mutt's manual to become familiar with all existing functions (or press `?` in Mutt) and what key they are bound to, or better, what key you like it to be bound to. Some essential keys are `m` (for message) to start composing a new message, `q` for quit, `r` for reply, `s` for save and `p` for print.

#### .mailcap

One of the features that Mutt has that is still not in today's most savvy email clients is the ability to display attachments inline through some viewer. The `auto_view` directive in the .muttrc file tells Mutt which attachments (based on their MIME-type) it should view inline. To figure out how to do that, Mutt uses mailcap files to lookup how to display a certain MIME-type. Usually the system wide mailcap file isn't sufficient here, so you better start a \~/.mailcap file to put items in there for `copiousoutput` that Mutt can display inline.

In the example .muttrc above `auto_view` is enabled for `application/msword` and `application/pdf` files. These two show the extreme usefulness of this capability, because it means meeting notes sent as doc file now are perfectly fine readable without having to save the attachment and open it in LibreOffice. Instead the text just shows up in the message reader, that is, if you have a matching entry in your \~/.mailcap file.

**`~/.mailcap`**

**Example .mailcap file**

The above .mailcap example tells Mutt what to do to "view" `msword` and `pdf` files. For the former it should run a program called [antiword](https://wiki.gentoo.org/wiki/Antiword), for the latter the program pdftotext (emerge app-text/poppler). You can go wild with these to for example display rendered HTML (give [app-text/vilistextum](https://packages.gentoo.org/packages/app-text/vilistextum) a try), render vcards, or show ASCII representation of attached images. All you need to do is define how to call the program in your .mailcap, and tell Mutt to try to view it inline using the `auto_view` directive.

### Configuration Scripts

#### Mutt Wizard

Initial setup of Neomutt can be intimidating to those who have not done it before. To make the process easier, [mail-client/mutt-wizard](https://packages.gentoo.org/packages/mail-client/mutt-wizard) was created. As the name suggests, it walks the user step-by-step through the process of setting up a proper muttrc file.

## Conclusions

Mutt is a very versatile console email client. If you like the concept, Mutt can be altered to behave in nearly any way through its configuration. Search the web to find others explaining how they did "it", or find one of the many patches that exist to make Mutt do even more. Gentoo applies a couple of very popular patches to Mutt, so make sure to check mutt -v if you want something more to make sure it is not yet already at your disposal. While learning Mutt is not necessarily easy, once it is in your fingers, it can make your mail experience much faster and efficient than with other clients. Searching for example is quite powerful if you know how to hit the right flags and know which regular expression narrows your search down. Enjoy Mutting!

## See also

- [Aerc](https://wiki.gentoo.org/wiki/Aerc) — a lightweight, command-line [mail user agent](https://en.wikipedia.org/wiki/mail_user_agent) (MUA) written in the [Go](https://wiki.gentoo.org/wiki/Go) programming language.
- [Neomutt](https://wiki.gentoo.org/wiki/Neomutt) — command-line mail client forked from [mutt].
- [Thunderbird](https://wiki.gentoo.org/wiki/Thunderbird) — Mozilla's solution to the e-mail client.
