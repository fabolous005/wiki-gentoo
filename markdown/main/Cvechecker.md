<!-- source: https://wiki.gentoo.org/wiki/Cvechecker | group: Gentoo Wiki (Main) | wiki-title: Cvechecker -->
---
title: cvechecker
url: https://wiki.gentoo.org/wiki/Cvechecker
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-26"
fingerprint: b18955cf84679e57
license: CC BY-SA 4.0
---

# cvechecker

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**cvechecker** is a small utility that provides reports on potentially vulnerable software.

cvechecker is for Gentoo systems and written and is maintained by former Gentoo developer  [Sven Vermeulen (SwifT)](https://wiki.gentoo.org/wiki/User:SwifT) .

## Installation

### Manual installation

Installation is possible via swift's overlay. Use [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) (or equivalent) to add the [unregistered repository](https://wiki.gentoo.org/wiki/Eselect/Repository#Add_unregistered_repositories):

`root #``eselect repository add sjvermeu git https://github.com/sjvermeu/gentoo.overlay`
Perform the initial sync:

`root #``emaint sync -r sjvermeu`
Emerge the package:

`root #``emerge --ask app-admin/cvechecker`
## Configuration

### User account

Upstream instructions suggest creating a service account to run cvechecker.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Usage

Quick start instructions can be [found upstream](https://github.com/sjvermeu/cvechecker).

### Database initialization

The initial pull of the database using the below command will take some time and is synced into the /var/lib/cvechecker/cache/ directory:

`root #``pullcves pull`
### Invocation

`user $``cvechecker --help````
Usage: cvechecker [OPTION...]
cvechecker -- Verify the state of the system against a CVE database
  -b, --binlist=binlist      List of binary files on the system
  -c, --cvedata=cvefile      CSV file with CVE information (cfr. nvd2simple)
  -C, --csvoutput            Use (parseable) CSV output
  -d, --deltaonly            Given binaries or lists should be added only (not
                             a full replacement)
  -D, --deletedeltaonly      Given binaries or lists should be removed (not a
                             full replacement)
  -f, --fileinfo=binfile     File to obtain detected CPE of
  -H, --reporthigher         Report also when CVEs have been detected for
                             higher versions
  -i, --initdbs              Initialize all databases
  -l, --loaddata=datafile    Load version gathering data file
  -r, --runcheck             Execute the checks (match installed software with
                             CVEs)
  -s, --showinstalled        Output detected software/versions
  -S, --showinstalledfiles   Output detected software/versions with file
                             information
  -w, --watchlist=watchlist  List of CPEs to watch for (assume these are
                             installed)
  -?, --help                 Give this help list
      --usage                Give a short usage message
  -V, --version              Print program version
Mandatory or optional arguments to long options are also mandatory or optional
for any corresponding short options.
Report bugs to <sven.vermeulen@siphos.be>.
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-admin/cvechecker`
## See also

- [Greenbone Vulnerability Management](https://wiki.gentoo.org/wiki/Greenbone_Vulnerability_Management) — a network security scanner with associated tools like a graphical user front-end.

## External resources

- Author's blog: [https://blog.siphos.be/](https://blog.siphos.be/)
