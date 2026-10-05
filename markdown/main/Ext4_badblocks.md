<!-- source: https://wiki.gentoo.org/wiki/Ext4/badblocks | group: Gentoo Wiki (Main) | wiki-title: Ext4/badblocks -->
---
title: Ext4/badblocks
url: https://wiki.gentoo.org/wiki/Ext4/badblocks
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-05"
fingerprint: a28b280541a7bb83
license: CC BY-SA 4.0
---

# Ext4/badblocks

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

badblocks is a small program for stress testing [block devices](https://en.wikipedia.org/wiki/Block_device#Block_devices). Similar to memtest86+, badblocks reads and writes small patterns of bytes to block devices.

## Installation

### Emerge

badblocks comes as part of the [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) package and should be available as part of the default system profile.

## Usage

### Invocation

`user $``badblocks````
Usage: /sbin/badblocks [-b block_size] [-i input_file] [-o output_file] [-svwnf]
       [-c blocks_at_once] [-d delay_factor_between_reads] [-e max_bad_blocks]
       [-p num_passes] [-t test_pattern [-t test_pattern [...]]]
       device [last_block [first_block]]
```
### Test a drive

To test a drive with visual progress use the `-s` and `-w` options followed by the path to the block device. -b flag is added as a precaution and to support large drives.

`root #```badblocks -s -w -b `blockdev --getbsz /dev/<device>` /dev/<device>``
Replace `<device>` in the command above with the block device that is to be tested. badblocks should run through a series of four tests and return output similar to the following:

Testing with pattern 0xaa: done
Reading and comparing: done
Testing with pattern 0x55: done
Reading and comparing: done
Testing with pattern 0xff: done
Reading and comparing: done
Testing with pattern 0x00: done
Reading and comparing: done

badblocks also supports a non-destructive read-write mode when using the `-n` option instead of `-w`. Users are advised to create backups nonetheless.

## See also

- [Memtest86+](https://wiki.gentoo.org/wiki/Memtest86%2B) — memory test software based on the commercially available (from Passmark) [memtest86](https://www.memtest86.com/) program.
