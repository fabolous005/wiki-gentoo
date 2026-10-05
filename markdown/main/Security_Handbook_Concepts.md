<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Concepts | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Concepts -->
---
title: Security Handbook/Concepts
url: https://wiki.gentoo.org/wiki/Security_Handbook/Concepts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: b512898080b7dddd
license: CC BY-SA 4.0
---

# Security Handbook/Concepts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section provides an overview of general security concepts. It is important to understand these concepts before moving on to more advanced topics.

It should be noted that security is a very broad topic. This section only covers the basics. For more information, reach out on IRC and read up on sources directly.



A threat model is a model that describes the threats that a system faces. A threat is a potential or actual undesirable event that may be malicious (such as DoS attack) or incidental (hardware failure)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

It is important to understand the threats that a system faces in order to protect it. For example, a laptop that is used to browse the web is going to face different threats than a server that is used to host a website.

From a user perspective, a jet-setting businessperson who uses their laptop to access sensitive information is going to face different threats than a student who uses their laptop to browse the web.

Here are some examples of threats that a system may face:

- Malicious attacks, such as DoS attacks, malware attacks, and phishing attacks.
- Accidental events, such as hardware failures, software bugs, and human errors.
- Natural disasters, such as floods, fires, and earthquakes.

Understanding the threat model of a system enables risks to be assessed and managed.

Risk is a measure of the extent to which something is threatened by a potential circumstance or event<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Typically risk is assessed by looking at two factors:

- Impact: The potential consequences of the event occurring.
- Likelihood: The probability of the event occurring.

As an example we can consider a laptop that is lost or stolen. A potential impact of this risk is that the data on the laptop may be accessed by unauthorized persons and leaked; the severity of this impact is dependent on the data in question. The likelihood of this event occurring is dependent on the threat model of the user - it is far more likely for a laptop that is regularly taken between multiple locations to be lost or stolen than a laptop that is only used in a single location.

Controls are the means of managing risk. They include policies, procedures, guidelines, practices, or organizational structures, which may be of an administrative, technical, management, or legal nature<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

When considering the hypothetical lost or stolen laptop, there are a number of controls that can be used to manage the risk:

- Full disk encryption can manage the risk of confidential data being accessed by unauthorized persons.
- Regular backups can manage the risk of data loss.
- Physical security measures (such as a lock) can manage the risk of the laptop being stolen.

Example controls may be provided throughout this document. They will be broadly grouped...

Each control is assigned a 'Maturity Level', inspired by the [Australian Cyber Security Centre's Essential Eight Maturity Model](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/essential-eight/essential-eight-maturity-model), however rather than assigning a maturity level to an organization or individual based on meeting a certain number of essential controls, this scale assigns each control a maturity level with the intent of guiding users towards an effective security baseline.

The levels are as follows:

If controls assigned this maturity level are not implemented this signifies that there are weaknesses in the user or organization's overall cyber security posture. When exploited, these weaknesses could facilitate the compromise of the confidentiality of their data, or the integrity or availability of their systems and data, as described by the tradecraft and targeting in Maturity Level One below.

The focus of this maturity level is adversaries who are content to simply leverage commodity tradecraft that is widely available in order to gain access to, and likely control of, systems. For example, adversaries opportunistically using a publicly-available exploit for a security vulnerability in an internet-facing service which had not been patched, or authenticating to an internet-facing service using credentials that were stolen, reused, brute forced or guessed.

Generally, adversaries are looking for any victim rather than a specific victim and will opportunistically seek common weaknesses in many targets rather than investing heavily in gaining access to a specific target. Adversaries will employ common social engineering techniques to trick users into weakening the security of a system and launch malicious applications, for example via Microsoft Office macros. If the account that an adversary compromises has special privileges they will seek to exploit it. Depending on their intent, adversaries may also destroy data (including backups).

The focus of this maturity level is adversaries operating with a modest step-up in capability from the previous maturity level. These adversaries are willing to invest more time in a target and, perhaps more importantly, in the effectiveness of their tools. For example, these adversaries will likely employ well-known tradecraft in order to better attempt to bypass security controls implemented by a target and evade detection. This includes actively targeting credentials using phishing and employing technical and social engineering techniques to circumvent weak multi-factor authentication.

Generally, adversaries are likely to be more selective in their targeting but still somewhat conservative in the time, money and effort they may invest in a target. Adversaries will likely invest time to ensure their phishing is effective and employ common social engineering techniques to trick users to weaken the security of a system and launch malicious applications, for example via Microsoft Office macros. If the account that an adversary compromises has special privileges they will seek to exploit it, otherwise they will seek accounts with special privileges. Depending on their intent, adversaries may also destroy all data (including backups) accessible to an account with special privileges.

The focus of this maturity level is adversaries who are more adaptive and much less reliant on public tools and techniques. These adversaries are able to exploit the opportunities provided by weaknesses in their target's cyber security posture, such as the existence of older software or inadequate logging and monitoring. Adversaries do this to not only extend their access once initial access has been gained to a target, but to evade detection and solidify their presence. Adversaries make swift use of exploits when they become publicly available as well as other tradecraft that can improve their chance of success.

Generally, adversaries may be more focused on particular targets and, more importantly, are willing and able to invest some effort into circumventing the idiosyncrasies and particular policy and technical security controls implemented by their targets. For example, this includes social engineering a user to not only open a malicious document but also to unknowingly assist in bypassing security controls. This can also include circumventing stronger multi-factor authentication by stealing authentication token values to impersonate a user. Once a foothold is gained on a system, adversaries will seek to gain privileged credentials or password hashes, pivot to other parts of a network, and cover their tracks. Depending on their intent, adversaries may also destroy all data (including backups).
