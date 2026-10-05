<!-- source: https://wiki.gentoo.org/wiki/Chiffrement_int%C3%A9gral_du_disque_%C3%A0_partir_de_z%C3%A9ro_simplifi%C3%A9 | group: Gentoo Wiki (Main) | wiki-title: Chiffrement intégral du disque à partir de zéro simplifié -->
---
title: Chiffrement intégral du disque à partir de zéro simplifié
url: https://wiki.gentoo.org/wiki/Chiffrement_int%C3%A9gral_du_disque_%C3%A0_partir_de_z%C3%A9ro_simplifi%C3%A9
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-17"
fingerprint: "9d34461b97eeb5cd"
license: CC BY-SA 4.0
---

# Chiffrement intégral du disque à partir de zéro simplifié

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Pour un chiffrement complet du disque, il convient d'utiliser un disque de démarrage séparé : le chiffrement intégral du disque peut être utilisé pour préserver l'intégrité et la confidentialité des données. [dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) peut être utilisé pour configurer les disques afin de les chiffrer avec LUKS ou d'autres formats. Cet article est un guide qui explique comment configurer un disque pour le chiffrer en utilisant LUKS et btrfs.

## Installation

### Emerge

`root #``emerge --ask sys-fs/cryptsetup`
### Logiciels supplémentaires

Si vous utilisez GPG pour sécuriser davantage les fichiers de clé :

`root #``emerge --ask app-crypt/gnupg`
## Préparation du système

Si cette opération est effectuée dans le cadre d'une nouvelle installation de Gentoo, la procédure d'installation peut être suivie jusqu'à l'étape suivante : [Handbook:AMD64/Full/Installation/fr#Designing\_a\_partition\_scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation/fr#Designing_a_partition_scheme)

Si vous convertissez un système existant à une configuration chiffrée, il faut soit ajouter du stockage, soit utiliser cette procédure pour créer de nouvelles partitions en utilisant l'espace libre, où les données peuvent ensuite être copiées après la création.

Selon le type de disque, il peut être difficile, voire impossible, d'écraser véritablement les parties du disque où se trouvent des données non chiffrées, mais déréférencées. Il est préférable de procéder à un effacement sécurisé à l'aide du micrologiciel du disque avant de le réutiliser.

## Préparation du disque

Cet exemple utilise GPT comme table de partitions et GRUB comme chargeur de démarrage. fdisk est utilisé comme outil de partitionnement, mais n'importe quel utilitaire de partitionnement fonctionnera.

Pour un chiffrement complet du disque, il convient d'utiliser un disque de démarrage séparé :

### Formatage du système racine

Pour créer une disposition de partitions avec fdisk, commencez par créer une nouvelle table de partitions sur le disque racine, /dev/nvme0n1 :

`root #``fdisk /dev/nvme0n1`
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x81391dbc.
Command (m for help): g
Created a new GPT disklabel (GUID: 8D91A3C1-8661-2940-9076-65B815B36906).

Une fois la table de partition créée, une nouvelle partition du disque peut être créée en utilisant **n** en acceptant les valeurs par défaut :

`Command (m for help):``n````
Partition number (1-128, default 1): 
First sector (2048-1953525134, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-1953525134, default 1953523711): 
Created a new partition 1 of type 'Linux filesystem' and of size 931.5 GiB.
```
Enfin, les modifications peuvent être écrites avec **w** :

`Command (m for help):``w`
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

Enfin, les propriétés de l'**ESP** peuvent être définies à l'aide de **t** :

`Command (m for help):``t`
Selected partition 1
Partition type or alias (type L to list all): 1
Changed type of partition 'Linux filesystem' to 'EFI System'.

### Phrase de passe sécurisée de l'en-tête LUKS

La façon la plus simple de configurer un volume chiffré est d'utiliser :

`root #``cryptsetup luksFormat --key-size 512 /dev/nvme0n1p1`
WARNING!
========
This will overwrite data on /dev/nvme0n1p1 irrevocably.
Are you sure? (Type 'yes' in capital letters): 
YES
Enter passphrase for /dev/sda2:

#### Ajout d'une phrase de passe à un volume à l'aide de fichiers de clés sécurisées GPG

La clé peut être déchiffrée vers ce tuyau :

`root #``mkfifo key_pipe`
Une phrase de passe peut ensuite être ajoutée avec :

`root #``gpg --decrypt key_file > key_pipe &`
Le pipe nommé peut être effacé avec :

`root #``cryptsetup luksAddKey --key-file key_pipe /dev/nvme0n1p1`
Le pipe nommé peut être effacé avec :

`root #``rm key_pipe`
### En-tête sécurisé du fichier de clé

#### Création d'un fichier de clé de base

Un fichier de clé basique peut être créé avec *dd* et /dev/urandom :

`/tmp/ #``dd bs=8388608 count=1 if=/dev/urandom of=crypt_key.luks`
1+0 records in
1+0 records out
8388608 bytes (8.4 MB, 8.0 MiB) copied, 0.014407 s, 582 MB/s

#### Fichier de clés symétriquement chiffrées GPG

Pour une plus grande sécurité, le fichier de clé peut être immédiatement chiffré avec [GnuPG](https://wiki.gentoo.org/wiki/GnuPG).

`/media/sda1/ #``dd bs=8388608 count=1 if=/dev/urandom | gpg --symmetric --cipher-algo AES256 --output crypt_key.luks.gpg`
1+0 records in
1+0 records out
8388608 bytes (8.4 MB, 8.0 MiB) copied, 7.50139 s, 1.1 MB/s

#### Fichier de clé GPG à chiffrement asymétrique

Un fichier de clé peut être protégé à l'aide de la cryptographie à clé publique en utilisant une carte à puce telle qu'une [YubiKey](https://wiki.gentoo.org/wiki/YubiKey). [Ce guide du GPG YubiKey](https://wiki.gentoo.org/wiki/YubiKey/GPG) peut être utilisé pour générer des clés [GPG](https://wiki.gentoo.org/wiki/GnuPG) sur une YubiKey. Une fois les clés publiques chargées, les clés peuvent être chiffrées avec pour destinataire le détenteur de la clé :

`/media/sda1/ #``dd bs=8388608 count=1 if=/dev/urandom | gpg --recipient larry@gentoo.org --output crypt_key.luks.gpg --encrypt`
1+0 records in
1+0 records out
8388608 bytes (8.4 MB, 8.0 MiB) copied, 7.50139 s, 1.1 MB/s

Au démarrage, assurez-vous que la carte à puce est insérée. Dracut demandera le code PIN et, selon la configuration, il se peut qu'il faille taper sur l'appareil pour terminer la détection de présence.

#### luksFormat en utilisant un fichier de clé

Pour sécuriser la partition avec un fichier de clé simple :

`/media/sda1/ #``cryptsetup --key-size 512 luksFormat /dev/nvme0n1p1 crypt_key.luks`
WARNING!
========
This will overwrite data on /dev/nvme0n1p1 irrevocably.
Are you sure? (Type 'yes' in capital letters): YES

Pour ajouter ce fichier de clé à une partition déjà chiffrée :

`/media/sda1/ #``cryptsetup luksAddKey /dev/nvme0n1p1 crypt_key.luks`
Enter any existing passphrase:

#### luksFormat en utilisant un fichier de clé protégé par GPG

Pour sécuriser la partition à l'aide d'un fichier de clé protégé par GPG :

`/media/sda1/ #``gpg --decrypt crypt_key.luks.gpg | cryptsetup luksFormat --key-size 512 /dev/nvme0n1p1 -`
gpg: AES256.CFB encrypted data
WARNING!
========
This will overwrite data on /dev/nvme0n1p1 irrevocably.
Are you sure? (Type 'yes' in capital letters): YES

Pour ajouter le fichier de clé chiffré par GPG à une partition déjà chiffrée, il faut utiliser des tubes nommés afin d'éviter de déchiffrer la clé sur le disque, car gpg et cryptsetup attendent tous deux des entrées provenant de stdin :

`/media/sda1/ #``mkfifo crypt_key``/media/sda1/ #``mkfifo cryptsetup_pass`
Une fois les fichiers créés, la clé doit être déchiffrée dans crypt\_key, et la phrase de passe de récupération doit être transmise à cryptsetup\_pass :

`/media/sda1/ #``gpg --decrypt crypt_key.luks.gpg > crypt_key &``/media/sda1/ #``read -s -r -p 'LUKS passphrase: ' CRYPT_PASS; echo "$CRYPT_PASS" > cryptsetup_pass &`
Enfin, **cat** peut être utilisé pour transmettre ces informations à **cryptsetup** :

`/media/sda1/ #``cat cryptsetup_pass crypt_key | cryptsetup luksAddKey /dev/nvme0n1p1 -`
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase
\[1\]-  Done                    read -s -r -p 'LUKS passphrase: ' CRYPT\_PASS; echo "$CRYPT\_PASS" > cryptsetup\_pass
\[2\]+  Done                    gpg -d crypt\_key.luks.gpg > crypt\_key

### Sauvegarde de l'en-tête LUKS

Les en-têtes peuvent être sauvegardés avec :

`root #``cryptsetup luksHeaderBackup /dev/nvme0n1p1 --header-backup-file crypt_headers.img`
## Préparation du système de fichiers

Une fois le volume LUKS créé, il doit être mappé pour que les systèmes de fichiers sous-jacents puissent être créés.

### Ouvrir le volume LUKS

Le dispositif chiffré doit être ouvert et mappé avant de pouvoir être utilisé, ce qui peut être fait avec :

`root #``cryptsetup luksOpen /dev/nvme0n1p1 crypt`
En cas d'utilisation d'un fichier clé :

`/media/sda1/ #``cryptsetup --key-file=crypt_key.luks open /dev/nvme0n1p1 crypt`
Si vous utilisez un fichier de clés chiffrées GPG :

`/media/sda1/ #``gpg --decrypt crypt_key.luks.gpg | cryptsetup --key-file=- open /dev/nvme0n1p1 crypt`
### Formatage des systèmes de fichiers

Créez un système de fichiers pour /dev/sda1, la partition de démarrage qui contiendra les fichiers GRUB et le noyau. Cette partition est lue par l'UEFI. La plupart des cartes mères ne peuvent lire qu'un système de fichiers [FAT32](https://wiki.gentoo.org/wiki/FAT) :

`root #``mkfs.vfat -F32 /dev/sda1`
Pour créer le système de fichiers racine [btrfs](https://wiki.gentoo.org/wiki/Btrfs) sur la partition LUKS :

`root #``mkfs.btrfs -L rootfs /dev/mapper/crypt`
#### Création de sous-volumes btrfs optionnels

Pour créer des sous-volumes pour /etc, /home et /var, le système de fichiers doit d'abord être monté :

`root #``mount LABEL=rootfs /mnt/gentoo`
Chaque sous-volume peut ensuite être créé :

`root #````
btrfs subvolume create /mnt/gentoo/etc
```
`root #````
btrfs subvolume create /mnt/gentoo/home
```
`root #``btrfs subvolume create /mnt/gentoo/var`
## Configuration de l'initramfs

Un initramfs doit être utilisé pour déchiffrer et monter la partition racine. Cela peut être réalisé en utilisant un initramfs personnalisé minimal, en utilisant quelques commandes, ou en utilisant un outil comme dracut pour déchiffrer en utilisant des paramètres passés dans la ligne de commande du noyau. Dracut

Les modules suivants doivent être ajoutés à la directive add\_dracutmodules dans le fichier /etc/dracut.conf :

**`/etc/dracut.conf`**

**Composants minimaux requis pour déchiffrer les volumes LUKS à l'aide de dracut**

Si des clés GPG sont utilisées, le module suivant doit également être ajouté : **crypt-gpg**

**`/etc/dracut.conf`**

**Composants minimum nécessaires pour déchiffrer les volumes LUKS à l'aide de dracut**

Dracut peut être configuré pour être construit avec la configuration pour LUKS codée en dur, les informations sur le premier disque doivent être obtenues :

`root #``lsblk -o name,uuid`
NAME        UUID
sda
└─sda1      BDF2-0139
nvme0n1
└─nvme0n1p1 4bb45bd6-9ed9-44b3-b547-b411079f043b
  └─crypt   cb070f9e-da0e-4bc5-825c-b01bb2707704

**`/etc/dracut.conf`**

**Paramètres cmdline intégrés pour le déchiffrage de rootfs**

Si vous utilisez systemd comme init, vous devez également ajouter l'use flag **cryptsetup** :

**`/etc/portage/package.use/systemd`**

Et faire un rebuild :

`root #``emerge --ask --newuse sys-apps/systemd`
Une fois Dracut configuré, un nouveau fichier initramfs peut être généré en exécutant :

`root #``dracut`
If the initramfs is being generated for a kernel other than the currently active one, **--kver** must be used:

`root #````
dracut --kver 6.1.28-gentoo
```
Cela peut se produire dans une situation où la version du noyau dans le CD Gentoo Live diffère de la version émergée de sys-kernel/gentoo-sources dans le processus de compilation du noyau.

Dracut a maintenant généré un initramfs, mais la configuration n'est pas terminée. Les paramètres de la ligne de commande doivent être définis, soit en les ajoutant manuellement au noyau, soit en les configurant dans le bootloader.

#### Extraction de l'initramfs

Il est possible d'utiliser **dracut** pour générer une image *initramfs*, puis de l'extraire pour l'intégrer au noyau.

`/usr/src/initramfs #``/usr/lib/dracut/skipcpio  /boot/initramfs-6.1.28-gentoo-initramfs.img | zcat | cpio -ivd`
#### Intégration de l'initramfs

Une fois l'*initramfs* extrait dans /usr/src/initramfs, le noyau peut être configuré pour l'intégrer :

**Intégration l'initramfs dans le noyau**

Équivalent en .config :

Avec cette configuration, le noyau intégrera automatiquement ce qui existe sous /usr/src/initramfs dans le noyau lorsqu'il est construit, et tentera de l'utiliser au démarrage. Ceci est particulièrement utile pour un démarrage [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot).

## Installation de Gentoo

Si cette procédure est suivie lors d'une installation Gentoo (à la place de [Handbook:AMD64/Full/Installation/fr#Designing\_a\_partition\_scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation/fr#Designing_a_partition_scheme) via [Handbook:AMD64/Full/Installation/fr#Monter\_la\_partition\_racine](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation/fr#Monter_la_partition_racine)), les étapes suivantes peuvent être utilisées pour monter la partition créée, afin de poursuivre l'installation.

### Monter la partition racine

Le volume logique du système de fichiers racine peut être monté à cet emplacement créé avec :

`root #``mount LABEL=rootfs /mnt/gentoo`
### Configuration de fstab

fstab  .

Pour un montage cohérent des volumes, les étiquettes et les UUID doivent être utilisés. Les périphériques de bloc et les ID de partition qui leur sont associés peuvent être visualisés à l'aide de :

`root #``lsblk -o name,uuid`
NAME        UUID
sda
└─sda1      BDF2-0139
nvme0n1
└─nvme0n1p1 4bb45bd6-9ed9-44b3-b547-b411079f043b
  └─crypt   cb070f9e-da0e-4bc5-825c-b01bb2707704

Une fois les UUID et les labels des partitions identifiés, le fichier [/etc/fstab](https://wiki.gentoo.org/wiki/Fstab) peut être modifié pour ajouter les montages appropriés :

**`/mnt/gentoo/etc/fstab`**

### Finalisation de l'installation de Gentoo

A ce stade, l'installation de Gentoo peut être poursuivie normalement : [Installation d'une archive tar](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation/fr#Installation_d.27une_archive_tar)

## Informations complémentaires

### Astuces pour les SSD

Le [trim](<https://en.wikipedia.org/wiki/Trim_(computing)>) SSD permet à un système d'exploitation d'indiquer à un lecteur à semi-conducteurs (SSD) quels blocs de données ne sont plus considérés comme utilisés et peuvent être effacés en interne. Le fonctionnement de bas niveau des disques SSD étant très différent de celui des disques durs, la façon dont les systèmes d'exploitation traitent habituellement les opérations telles que les suppressions et les formatages a entraîné une dégradation progressive imprévue des performances des opérations d'écriture sur les disques SSD. Le trim permet au disque SSD de gérer plus efficacement la collecte des données résiduelles, ce qui ralentirait les futures opérations d'écriture sur les blocs concernés. Pour activer le découpage du SSD du système de fichiers racine crypté sur LVM, modifiez le fichier /etc/default/grub si vous utilisez genkernel :

**`/etc/default/grub`**

Si vous utilisez dracut pour générer les intiramfs, utilisez :

**`/etc/default/grub`**

Si vous utilisez initramfs basé sur un système utilisant systemd, utilisez :

**`/etc/default/grub`**

Cela indiquera au noyau d'activer trim sur la racine. Modifiez le fichier de configuration /etc/lvm/lvm.conf :

**`/etc/lvm/lvm.conf`**

Cela notifiera à la couche LVM d'activer trim sur les disques SSD.
Lors de l'utilisation de disques SSD et de l'UEFI-boot, la séquence de démarrage peut être trop rapide. Lorsque vous entrez la phrase d'authentification correcte, le noyau se plaindra de modules manquants ou de l'absence de périphérique racine. Essayez d'ajouter `rootdelay=3` à `GRUB_CMDLINE_LINUX_DEFAULT` dans /etc/default/grub, ou ajoutez-le directement en mode édition du menu GRUB lors du démarrage.
