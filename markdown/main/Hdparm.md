<!-- source: https://wiki.gentoo.org/wiki/Hdparm | group: Gentoo Wiki (Main) | wiki-title: Hdparm -->
---
title: hdparm
url: https://wiki.gentoo.org/wiki/Hdparm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-10-09"
fingerprint: a8d635f1e5e7baa9
license: CC BY-SA 4.0
---

# hdparm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


hdparm is a command-line utility to set and view ATA and SATA [hard disk drive](https://wiki.gentoo.org/wiki/HDD) hardware parameters.

## Installation


### Emerge

Install [sys-apps/hdparm](https://packages.gentoo.org/packages/sys-apps/hdparm):

`root #``emerge --ask sys-apps/hdparm`
## Configuration

### Service

#### OpenRC

To set parameters on boot, edit the /etc/conf.d/hdparm configuration file:

- `sdX_args`: Set parameters for the given drive (replace *sdX* with the right device file name).
- `discX_args`: Set parameters for the given disc drive.
- `cdromX_args`: Set parameters for the given optical drive.
- `all_args`: Set parameters for all drives.

For example, to disable power management for all drivers and enable the DMA feature for /dev/sda:

**`/etc/conf.d/hdparm`**

When finished modifying the configuration file be sure to add the hdparm service to the default runlevel so that it can start at system boot:

`root #``rc-update add hdparm default`
## Usage

### General

Show identification and feature info:

`root #``hdparm -I /dev/sdX`
### Benchmarking drives

Be sure that no other programs are stressing the drive at the time of testing:

`root #``hdparm -tT /dev/sdX`
### Get current settings

This is not consistent: for some features the current values can be found in the common info, for other features hdparm must be called with the right parameters, but without any value, like `-d`:

`root #``hdparm -d /dev/sdX`
Set features by put the value directly behind the parameter (without space), e.g. to enable the DMA mode:

`root #``hdparm -d1 /dev/sdX`
The following tables introduces the most common parameters. For a complete list see the *hdparm* [man page](https://wiki.gentoo.org/wiki/Man_page).

| Parameter | Description | 
|---|---|
| `-B` | Set the Advanced  Power  Management feature. The value *1* saves the most energy, the value *255* disables the feature. The values in-between are corresponding steps. Values of *127* and below allow the spin-down of the drive. | 
| `-d` | Set the DMA feature for IDE and PATA drives. The value *1* enables the feature, *0* disables the feature. | 
| `-E` | Set CD / DVD drive speed. Lower speeds can reduces the running noise. | 
| `-M` | Set the Automatic Acoustic Management feature. The values *0* disables the feature, *128* sets the most quiet mode, *254* is the fastest mode. | 
| `-S` | Set the standby (spin-down after idling) timeout. The value *0* disables the feature, the values from *1* to *240* specifies time steps of 5 seconds, *241* to *251* time steps of 30 minutes. | 

## See also

- [HDD](https://wiki.gentoo.org/wiki/HDD) — describes the setup of an internal SATA or PATA (IDE) rotational **hard disk drive**.
