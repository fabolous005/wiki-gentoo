<!-- source: https://wiki.gentoo.org/wiki/Hardened/Grsecurity_Trusted_Path_Execution | group: Gentoo Wiki (Main) | wiki-title: Hardened/Grsecurity Trusted Path Execution -->
---
title: Hardened/Grsecurity Trusted Path Execution
url: https://wiki.gentoo.org/wiki/Hardened/Grsecurity_Trusted_Path_Execution
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-01-10"
fingerprint: ce890952f7e01104
license: CC BY-SA 4.0
---

# Hardened/Grsecurity Trusted Path Execution

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

TPE tends to be one of the harder to understand parts of GRSecurity as options like invert GID can be confusing at times. In this documents we explain how each possible TPE setup behaves and summarize it with the results of a simple test suite.

## Introduction

Trusted Path Execution (TPE) is a protection which restricts the execution of files under certain circumstances determined by their path. Using it will make privilege escalation harder when an account restricted by TPE is compromised as the attacker won't be able to execute custom binaries which are not in the trusted path.

You can also enable a weaker restriction which will prevent race conditions on code executed by non root users. This weaker condition makes non-root users able to run only executables on directories owned by them or root and writeable only by the owner.

To explain how TPE works we will first explain what each kernel option does, and then show the results with an example.

## The different setups

### No TPE

Here TPE is disabled. So it won't won't affect the executable permissions.

**Setup 1 (No TPE)**

### Basic TPE

Here we use the minimal setup of TPE. With it all the users in the group with the indicated GID (100 by default) will be able to execute only files in root owned directories writable only by root (and nothing more).

**Setup 2 (Basic)**

### TPE with with partial restrictions

Now we also enable the partial restriction, this means that now aside from the previous restriction, we now add another for the non-root users not affected by it which will allow execution only in root owned directories writable only by root and directories owned by the executing user which aren't group writable nor world writable (and nothing more).

**Setup 3 (with partial retriction)**

### TPE with with inverted gid match

Now, we enabled the invert GID option, so now all the users not in the group with the indicated GID (100 by default) will be able to execute only files in root owned directories writable only by root (and nothing more).

**Setup 4 (with inverted GID match)**

### TPE with with partial restrictions and inverted gid match

Again we also enable the partial restriction, this means that now aside from the previous restriction, we now add another for the non-root users not affected by it which will allow execution only in root owned directories writable only by root and directories owned by the executing user which aren't group writable nor world writable (and nothing more).

**Setup 5 (with inverted GID match and partial restriction)**

## Testing the different restrictions

To make things even clearer we have executed a small test suite on each of the possible setups.

### The test suite

The test suite consist of a series of directories with different names each with different permissions and ownership. These directories have exactly the same contents: a set of files again with different permissions and ownership each. The files are just a simple bash script printing OK.

### Example Results

Below are the results for each execution attempt on each of the presented setups. user1 is in the group set by the GID, while user2 isn't. A YES means the file indicated was executable by the indicated user in the indicated setup. A NO means the permission to execute the file was denied.

| Directory | File | Setup 1 |  | Setup 2 |  | Setup 3 |  | Setup 4 |  | Setup 5 |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|
|  |  | user1 | user2 | user1 | user2 | user1 | user2 | user1 | user2 | user1 | user2 | 
| 01 | 01 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 02 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 03 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 04 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 05 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 06 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 07 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 08 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 09 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 10 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 11 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
|  | 12 | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
| 02 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 03 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 04 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 05 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
| 06 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 07 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 08 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 09 | 01 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
| 10 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 11 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 12 | 01 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 02 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 03 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 04 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 05 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 06 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 07 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 08 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 09 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 10 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 11 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
|  | 12 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 

## Conclusion

As we can see the results are not dependent on the files ownership or permissions but on the directories ones. Below is a summed up more readable table. Remember that user1 is in the group set by the GID, while user2 isn't and that a YES means the files in the dir were executable by the indicated user in the indicated setup. A NO means the permission to execute the files was denied.

| Directory | Permissions | Owner | Group | Setup 1 |  | Setup 2 |  | Setup 3 |  | Setup 4 |  | Setup 5 |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|  |  |  |  | user1 | user2 | user1 | user2 | user1 | user2 | user1 | user2 | user1 | user2 | 
| 01 | drwxr-xr-x | root | root | YES | YES | YES | YES | YES | YES | YES | YES | YES | YES | 
| 02 | drwxr-xrwx | root | root | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 03 | drwxrwxr-x | root | root | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 04 | drwxrwxrwx | root | root | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 05 | drwxr-xr-x | user1 | user1 | YES | YES | NO | YES | NO | NO | YES | NO | YES | NO | 
| 06 | drwxr-xrwx | user1 | user1 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 07 | drwxrwxr-x | user1 | user1 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 08 | drwxrwxrwx | user1 | user1 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 09 | drwxr-xr-x | user2 | user2 | YES | YES | NO | YES | NO | YES | YES | NO | NO | NO | 
| 10 | drwxr-xrwx | user2 | user2 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 11 | drwxrwxr-x | user2 | user2 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 
| 12 | drwxrwxrwx | user2 | user2 | YES | YES | NO | YES | NO | NO | YES | NO | NO | NO | 

We have shown how TPE makes file execution more restrictive. We also have shown that the partial setting will apply to all the user not matched by the GID condition. And we finally showed that TPE restrictions only depend on the permissions and ownership of the directory containing the executable and not on the ones of the executable itself, so an executable owned by other user can still be modified by that user.
