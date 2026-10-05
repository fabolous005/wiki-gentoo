<!-- source: https://wiki.gentoo.org/wiki/SELinux/Information_flow_control | group: Gentoo Wiki (Main) | wiki-title: SELinux/Information flow control -->
---
title: SELinux/Information flow control
url: https://wiki.gentoo.org/wiki/SELinux/Information_flow_control
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-05-10"
fingerprint: "564a4d14148e2acd"
license: CC BY-SA 4.0
---

# SELinux/Information flow control

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In larger environments, information flow control is an important concept to restrict data leakage. The idea is that people are granted a certain clearance level, and resources are given a sensitivity. The clearance and the sensitivity are then checked before a particular access is granted.

## Introduction

As per the [Bell-LaPadula model](https://en.wikipedia.org/wiki/Bell%E2%80%93LaPadula_model), access to a resource is granted by comparing the clearance of the subject with the sensitivity of the object. In the model, two mandatory access control rules are specified:

- A subject at a certain security level may not read an object at a higher level (no read up)
- A subject at a certain security level may not write an object at a lower level (no write down)

### Security level or sensitivity level

The sensitivity level or security level or a subject or object is a hierarchical level assigned to the resource.

The levels are hierarchical to allow the *no read up, no write down* rules to be implemented. Such levels are often described through terms such as *public*, *internal*, *confidential*, *strictly confidential* and so on.

### Categorization

Categorization of resources is used to support an access matrix to resources. Some roles might only be granted access to specific departmental resources. Granting access to confidential data within one department shouldn't mean that user or role has access to confidential data of another department.

By assigning the proper categories to resources, such an access model is possible depending on the clearance of a user.

### Sensitivity

The sensitivity of a resource contains the security level and categories.

### Clearance

The clearance of a subject is the current sensitivity of the subject with the maximum security level and category set that the subject has rights to access.

## SELinux implementation

In SELinux, such a model can be implemented through its Multi-Level Security (MLS) support.

### Sensitivity syntax

The sensitivity in SELinux is provided through an integer for the sensitivity level/security level, and an integer for a category.

For instance, a resource with the lowest sensitivity level and categories 1, 5 and 7 would have `s0:c1,c5,c7` as the sensitivity.

### Clearance syntax

The clearance of a domain uses a similar scheme as the regular sensitivity. However, instead of one sensitivity, it contains two: one is the current sensitivity, the other one is the target (maximum) sensitivity.

For instance, a subject in the first sensitivity level and access to the third sensitivity level and categories 1, 5 and 7 would have `s1-s3:c1,c5,c7` as the security clearance.
