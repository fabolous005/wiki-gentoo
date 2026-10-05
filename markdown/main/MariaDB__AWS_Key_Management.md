<!-- source: https://wiki.gentoo.org/wiki/MariaDB:_AWS_Key_Management | group: Gentoo Wiki (Main) | wiki-title: MariaDB: AWS Key Management -->
---
title: "MariaDB: AWS Key Management"
url: https://wiki.gentoo.org/wiki/MariaDB:_AWS_Key_Management
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-14"
fingerprint: b6947c5047e1119c
license: CC BY-SA 4.0
---

# MariaDB: AWS Key Management

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide describes managing [AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) keys in [MariaDB](https://wiki.gentoo.org/wiki/MariaDB).

# Important

Much of what is described here should be considered a hack. It is not neat. It is not pretty. And improving it (though it would be appreciated) is a rabbit hole you likely do not want to descend into.

# The Challenges

## awscli v2 source build

As of right now, it seems packaging awscli:2 is challenging. Instead the recommendation is to use the v2 binary package.

## AWS default SDK paths

By default the AWS SDK loads configuration and SSO information from \~/.aws/. Unfortunately, the home directory of the mysql user is /dev/null. For unknown reasons, this makes the AWS SDK use /.aws/. If mariadb was started from a root-shell, it can also end up using /root/.aws/ due to inheritance of the environment.

# Installation

Install [dev-db/mariadb](https://packages.gentoo.org/packages/dev-db/mariadb) with flag `aws-km` (available from version 11.4.8-r2), and [app-admin/awscli-bin](https://packages.gentoo.org/packages/app-admin/awscli-bin) which likely needs to be unmasked with **\~amd64**.

`root #``emerge --ask mariadb awscli-bin`
# Configuration

## AWS SDK

SSO credentials have to be created in AWS. For this, follow the [instructions from the MariaDB docs](https://mariadb.com/docs/server/security/securing-mariadb/encryption/data-at-rest-encryption/key-management-and-encryption-plugins/aws-key-management-encryption-plugin#preparation).

You should obtain 4 pieces of information:

1. Access Key ID
2. Secret Access Key
3. Region
4. Key ID or Key Alias (Alias is recommended).


Now follow [AWS's Quick Start guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html) section *long-term credentials* which states to enter the 4 values into:

`user $` `awscli configure`
## MariaDB

To make the AWS credentials accessible to MariaDB, and since MariaDB has no home directory, a directory needs to be selected. /etc/mysql seems as good as any.

After creation with `awscli`, the files and credentials are stored in \~/.aws of the user that ran the command.

`root #````
cp -a /home/<user>/.aws /etc/mysql/
  
```
`root #````
chown -R mysql: /etc/mysql/.aws
```
### OpenRC

On [OpenRC](https://wiki.gentoo.org/wiki/OpenRC), we overwrite the `HOME` environment variable manually in the mariadb service by editing /etc/conf.d/mysql and adding:

**`/etc/conf.d/mysql`**

**set home in openrc service**

```
export HOME="$(dirname "${MY_CNF}")"
```
The dir /etc/mysql that /etc/mysql/my.cnf (the value of `MY_CNF`) resides in can also be hardcoded:

**`/etc/conf.d/mysql`**

**set home to confdir**

```
export HOME=/etc/mysql
```
### systemd

On [systemd](https://wiki.gentoo.org/wiki/Systemd), run

`root #``systemctl edit mariadb.service`
and input

**`/etc/systemd/system/mariadb.service.d/override.conf`**

**set home in systemd service**

```
[Service]
Environment="HOME=/etc/mysql"
```
### Enabling AWS support in MariaDB

From here we follow [the instructions from the MariaDB docs](https://mariadb.com/docs/server/security/securing-mariadb/encryption/data-at-rest-encryption/key-management-and-encryption-plugins/aws-key-management-encryption-plugin#configuring-the-aws-key-management-plugin). A minimal configuration whilst still getting sensible logs to troubleshoot is:

**`/etc/mysql/mariadb.d/90-aws-keys.cnf`**

```
[mariadb]
plugin_load_add = aws_key_management
aws_key_management_master_key_id = alias/<key alias>
aws_key_management_log_level = info
aws_key_management_region = af-south-1
```
Once MariaDB starts successfully, run `show variables like '%aws%';` to confirm the plugin is operational. I that state the output will appear incomplete:

`MariaDB [(none)]>``show variables like '%aws%';`
+------------------------------------+--------------------------+
| Variable\_name                      | Value                    |
+------------------------------------+--------------------------+
| aws\_key\_management\_endpoint\_url    |                          |
| aws\_key\_management\_key\_spec        | AES\_128                  |
| aws\_key\_management\_keyfile\_dir     |                          |
| aws\_key\_management\_log\_level       | Info                     |
| aws\_key\_management\_master\_key\_id   | alias/\<go for it>        |
| aws\_key\_management\_region          | af-south-1               |
| aws\_key\_management\_request\_timeout | 0                        |
| aws\_key\_management\_rotate\_key      | 0                        |
+------------------------------------+--------------------------+
8 rows in set (0.001 sec)

### Encrypting MariaDB

Database binlogs (writing out recent database actions in case of unexpected shutdown) can now be encrypted:

**`/etc/mysql/mariadb.d/99-binlog-encryption.cnf`**

```
[mariadb]
encrypt_binlog = 1
```
Relevant tables have to be encrypted individually by setting `encrypted=yes`, depending on whether it's a new or existing table one of the following should do the trick:

`MariaDB [(none)]>``CREATE TABLE foo(bar int) ENCRYPTED=YES;``MariaDB [(none)]>``ALTER TABLE foo ENCRYPTED=YES;`
# Security considerations

## MariaDB encryption requirements

A simple test using grep found that MariaDB data can be written to up to three places in /var/lib/mysql:

1. ib\_logfile0
2. mysqld-bin.?????? (eg. mysqld-bin.000001, mysqld-bin.000002, ...)
3. \<db>/\<table>.ibd

Basically the binlog, then the innodb logfile to which pages is written first, and finally to the table `ibd` itself (or `ibdata` if you're still not using file per table).

It's recommended that you perform a test similar to this to ensure that your encryption is working properly:

`MariaDB [test]>````
|create table crypt_test (val varchar(1024)) ENCRYPTED=YES;
|insert into crypt_test values('CRYPT TEST PLAINTEXT 01');
Query OK, 1 row affected (0.031 sec)
|flush tables crypt_test for export;
```
Query OK, 0 rows affected (0.192 sec)

Query OK, 0 rows affected (0.055 sec)
Which should not show up on your filesystem as:

`root #````
 grep -r 'CRYPT TEST PLAINTEXT 01' /var/lib/mysql
```
grep: ib\_logfile0: binary file matches
grep: mysqld-bin.001752: binary file matches
grep: test/crypt\_test.ibd: binary file matches

For repeat tests, change the string. Upon correct configuration, grep should not return any matches.

## Encryption at rest

Encryption at rest intends to protect against physical theft, or against your VM provider being a bad actor/compromised themselves. Nothing more. Nothing less.

I would recommend rather looking at full disk encryption using LUKS with a TPM2.0 based unlock if possible. If you don't have access to a functional TPM2.0 setup ... you're probably at the mercy of a password during boot, or only encrypting part of your disk which can be done via ssh post boot. You will somehow (especially in the case of a VM) verify the legitimacy of the boot image and non-encrypted parts of the operating system. Both of those have surety issues that your boot path has not been compromised (and I've personally seen cases where even with changes to the BIOS the TPM will still consider the boot path as valid, and thus unlock and provide keys).

## Protecting .../.aws/

The credentials in .../.aws/ are long-term. This means that should an attacker lay hands on them - it's game over. This can be done if this itself isn't encrypted, and if it is, those keys has to be well protected - which brings us back to TPM.

Either way: It's important to revoke access to the AWS user that's used by mariadb the moment you become aware of theft or compromise of these.

# IAM User Permissions

The IAM user should also have various rights, which I'm unclear on, but you'll get errors in the mariadb log files if this isn't set up, eg:

```
WARN] 2025-11-13 12:21:28.880 AWSErrorMarshaller [139631421925376] Encountered AWSError 'AccessDeniedException': User: arn:aws:iam::123456:user/mariadb-kms-user is not authorized to perform: kms:GenerateDataKeyWithoutPlaintext on resource: arn:aws:kms:af-south-1:329992286798:key/b145837b-bcbe-47f0-9d64-d6ed83641bfb because no identity-based policy allows the kms:GenerateDataKeyWithoutPlaintext action
[ERROR] 2025-11-13 12:21:28.880 AWSJsonClient [139631421925376] HTTP response code: 400
Resolved remote host IP address: 13.246.244.67
Request ID: {UUID}
Exception name: AccessDeniedException
Error message: User: arn:aws:iam::123456:user/mariadb-kms-user is not authorized to perform: kms:GenerateDataKeyWithoutPlaintext on resource: arn:aws:kms:af-south-1:123456:key/{UUID} because no identity-based policy allows the kms:GenerateDataKeyWithoutPlaintext action
```
You will unfortunately have to dig through those, what I know about AWS is extremely dangerous, I just pass these errors on to the people that manage the account and have them sort it out.
