<!-- source: https://wiki.gentoo.org/wiki/Emacs_NotMuch_Email_Client | group: Gentoo Wiki (Main) | wiki-title: Emacs NotMuch Email Client -->
---
title: Emacs NotMuch Email Client
url: https://wiki.gentoo.org/wiki/Emacs_NotMuch_Email_Client
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-03-07"
fingerprint: "62949a530a8fb0d8"
license: CC BY-SA 4.0
---

# Emacs NotMuch Email Client

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describe installation and usage of NotMuch implementation of email client for Emacs.

Full stack consist ofː isync, notmuch, emacs, smtpmail-multi.

Features of this implementationː

\- simple sandboxing of IMAP client

\- granular control without ELisp

\- good control over several email accounts

\- html interpretation mode

Known troubles:

\- impossible to proxy IMAP

## Steps

```
1) emerge net-mail/isync
2) USE="emacs doc" emerge net-mail/notmuch
3) useradd -m email
4) /etc/sudoers.d/email 
5) chown email:current_user /home/email
6) chmod g+rwxs /home/email 
7) configure isync .mbsyncrc.
8) # mkdir /home/email/.mail/
9) # chown -R email:user /home/email/.mail
10) # find /home/email/.mail/ -type d -exec chmod -R g+rxs {} \;
11) # find /home/email/.mail/ -type f -exec chmod g+rw {} \;
12) mbsync gmail or mbsync -aV
13) notmuch setup # create /home/email/.notmuch-config
14) notmuch new # create a database that indexes all of your mail
15) cp /home/email/.notmuch-config /home/user/
16) M-x package-install smtpmail-multi
17) configure smtpmail-multi in .emacs, create "~/.authinfo" or "~/.authinfo.gpg" or "~/.netrc"
18) $ emacs ; M-x notmuch
19) email retriving: # mbsync -aV && notmuch new && notmuch tag --input=my.notmuch && find .mail/ -type f -exec chmod g+rw {} \;
```
## Tagging for incoming emails (in sandbox)

FILE **`my.notmuch`**

```
+saved -- folder:SAVED
+sent -- folder:Sent
+spam -- folder:Spam
+book -- folder:book
+pol -- folder:pol
# remove spam, draft, deleted from inbox
-inbox -- tag:spam or tag:draft or tag:deleted
# blacklistː
-inbox -unread +deleted -- tag:inbox and "from:/.*@.*[.]pinterest[.]com/"
```
## NotMuch configuration for Emacs (not in sandbox)

FILE **`.emacs`**

```
;; ------ notmuch
;; C-c m opens up Notmuch from any buffer
(global-set-key (kbd "C-c m") 'notmuch)
;; sort order for notmuch-search
(setq notmuch-search-oldest-first nil)
;; Drafts folder
(setq notmuch-draft-folder "Drafts")  ;; default: drafts
(setq notmuch-draft-tags '("+draft" "-inbox" )) ;; when saved, default within inbox
;; default Emacs message composer for C-x m compose-mail (optional)
(require 'notmuch)
(setq mail-user-agent 'notmuch-user-agent) ;; report-emacs-bug, 
;; fix Fcc - save sent message folder from sent->Sent
(setq notmuch-fcc-dirs
  '(
      (".*" . "Sent")))
;; mark sent message +sent tag - Send messages are saved to Save folder and must be retagged to be shown in notmuch-emacs in sent.
(defun my/notmuch-sent-hook ()
  (shell-command "sudo -u email notmuch tag -unread -inbox +sent -- tag:inbox and from:fox@gmx.com"))
(add-hook 'notmuch-search-hook 'my/notmuch-sent-hook)
;; ------ editor org-mode integration
(add-hook 'message-mode-hook #'turn-on-orgtbl)
;; (add-hook 'message-mode-hook #'turn-on-orgstuct) ;; not working
;; (add-hook 'mail-mode-hook 'turn-on-orgstruct)  ;; not working
(global-set-key (kbd "C-c f") 'org-footnote-action)
;; ------ smtpmail-multi
 (setq smtpmail-multi-accounts
   '(
      (gmx . ("fox@gmx.com" "mail.gmx.com" 587 "fox@gmx.com" nil nil nil nil))
      ;; (gmail-main . ("firmin.martin@gmail.com" "smtp.gmail.com" 587 "firmin.martin@gmail.com" nil nil nil nil))
      ))
(setq smtpmail-multi-associations
  '(
     ("fox@gmx.com" gmx)
     ;; ("firmin.martin@gmail.com" gmail-main)
     ))
(setq smtpmail-multi-default-account 'gmx)
(setq message-send-mail-function 'smtpmail-multi-send-it)
(setq smtpmail-debug-info t)
(setq smtpmail-debug-verbose t)
```
## Authentication information for smtpmail-multi (not in sandbox)

FILE **`.authinfo`**

```
machine mail.gmx.com login fox@gmx.com port 587 password 123passw0rd
machine smtp.gmail.com login firmin.martin port 587 password abc123
```
## Keys

\- q or x	quit
- C-m		activate
- tab/S-tab	move to next/previous button
- g or = or G	update
- s		search
- C-M-s	regex search
- z		tree search
- +,-		Add or remove arbitrary tags from the current message.
- k		tagging menu
- a		next message
- r

in message:
- V		raw message
- r		reply to the sender
- R		reply to the sender and all recipients of the current message.
- f		forward
- Z		show tree of messages
- C-x C-s	save as draft
- C-c C-s	send

## Signature

\~/.signature, insert always by default, or disable and use C-c C-w.

## Additional

Email retriving may be called from notmuch hooks.
