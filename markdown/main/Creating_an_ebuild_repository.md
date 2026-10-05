<!-- source: https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository | group: Gentoo Wiki (Main) | wiki-title: Creating an ebuild repository -->
---
title: Creating an ebuild repository
url: https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-30"
fingerprint: "9f89380f94e123e0"
license: CC BY-SA 4.0
---

# Creating an ebuild repository

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Create an [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) to house [ebuilds](https://wiki.gentoo.org/wiki/Ebuild) to be made available to [Portage](https://wiki.gentoo.org/wiki/Portage) for installation.  Once an ebuild repository is created, and ebuilds are added to it, it may be shared with others (via a publicly available git repository, for example). This article will cover all the basics of creating an ebuild repository and maintaining ebuilds in it.

Creating ebuild repositories and ebuilds can be a good way to learn more about how Gentoo fundamentally works. Publishing an ebuild repository can also be a good way to contribute to the community (though helping out with [GURU](https://wiki.gentoo.org/wiki/GURU) or becoming a [proxy maintainer](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Contributor%27s_guide) can be even more helpful).

## Creating an empty ebuild repository

Using the [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) module, it is possible to [make an ebuild](https://wiki.gentoo.org/wiki/Eselect/Repository#Create_a_new_ebuild_repository) [repository skeleton](https://wiki.gentoo.org/wiki/Repository_format) and create its [repos.conf](https://wiki.gentoo.org/wiki/Repos.conf) entry with just one command ([app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) needs to be installed.):

`root #``eselect repository create <repository_name>`
--2022-11-21 15:42:35--  https://qa-reports.gentoo.org/output/repos/repositories.xml
Resolving qa-reports.gentoo.org... 151.101.129.91, 151.101.193.91, 151.101.1.91, ...
Connecting to qa-reports.gentoo.org|151.101.129.91|:443... connected.
HTTP request sent, awaiting response... 304 Not Modified
File '/root/.cache/eselect-repo/repositories.xml' not modified on server. Omitting download.
Adding \<repository\_name> to /etc/portage/repos.conf ...
Repository \<repository\_name> created and added

A new ebuild repository can also be created "by hand", as explained in the [Handbook](https://wiki.gentoo.org/wiki/Handbook) section [defining a custom *ebuild repository*](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Defining_a_custom_ebuild_repository).

If creating a repository [for publication](https://github.com/new) that is mainly destined to contain a central package with its dependencies, it may be helpful to give it a name en rapport with its contents (ebuild repositories often get named for the person maintaining them, which isn't always the most descriptive name). The same applies for a repository that will contain packages with usage centered around a particular theme.

## Track changes (optional)

Using a [Version Control System](https://wiki.gentoo.org/wiki/Category:VCS) (VCS) is good practice when creating or maintaining any ebuild repository. A VCS will track changes to ebuild files and allow "undoing" mistakes, among other useful features. A VCS can also make it easier to share an ebuild repository, if or when desired.

Portage has support for several VCSs to automatically provide [ebuild repository synchronization](https://wiki.gentoo.org/wiki/Ebuild_repository#Repository_synchronization), easily retrieving updates from the ebuild repositories available to Portage. [git](https://wiki.gentoo.org/wiki/Git), [Mercurial](https://en.wikipedia.org/wiki/Mercurial), and [Subversion](https://en.wikipedia.org/wiki/Apache_Subversion) are supported in this fashion. Other VCSs may be used, if an ebuild repository is to provide synchronization by other means, or not at all.

### git

[git](https://wiki.gentoo.org/wiki/Git) allows different version branches, which are useful for testing things out. It provides easy diff facilities, and many other features that can help to create and maintain an ebuild repository.

To start using git to develop an *ebuild repository*, first initialize the *git repository*:

`user $``cd /var/db/repos/local/``root #``git init`
Initialized empty Git repository in /var/db/repos/local/.git/

`root #````
git add .
```
`root #``git commit`
## Adding an ebuild to an ebuild repository

With the basic layout in order thanks to [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository), and a VCS such as git set up, adding [ebuilds](https://wiki.gentoo.org/wiki/Ebuild) to a newly created repository is simple.

For this example, `app-dicts/artha-1.0.2`[\[1\]](https://bugs.gentoo.org/show_bug.cgi?id=312313) will be used; of course, a new ebuild repository will usually be destined to contain [newly written ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds). To follow on with this guide, consider that the artha ebuild is in the homedir of the user [larry](https://wiki.gentoo.org/wiki/Larry_the_cow), in a file named artha-1.0.2.ebuild.

Directories to house the ebuild must first be created. Inside the repository directory, create an appropriate [*category* directory](https://packages.gentoo.org/categories). Inside that, create a directory with the *package name*, to house the ebuild:

`root #``mkdir -p /var/db/repos/local/app-dicts/artha`
Create the ebuild file. Note the ebuild corresponds to a specific version, which is reflected in the file name. Several different versions of a package may be present via several different ebuilds in this package directory. This example uses a preexisting file, so simply copy it:

For a brand new ebuild:

`root #``cp /home/larry/artha-1.0.2.ebuild /var/db/repos/local/app-dicts/artha/artha-1.0.2.ebuild`
for an existing ebuild in ::gentoo:

`root #``cp -rv /var/db/repos/gentoo/app-dicts/artha/* /var/db/repos/local/app-dicts/artha/`
The difference is required as many ebuilds in the ::gentoo repository have patch files which will result in the ebuild failing if the user does not copy those over as.

Give the file the appropriate permissions:

`root #``chown -R portage:portage /var/db/repos/local`
Run [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) manifest to create the package's [Manifest](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest) file:

`root #````
cd /var/db/repos/local/app-dicts/artha
```
`root #``pkgdev manifest`
If pkgdev or pkgcheck errors out, please see [Pkgcheck#error:\_repos.conf:\_default\_repo\_.27gentoo.27\_is\_undefined\_or\_invalid](https://wiki.gentoo.org/wiki/Pkgcheck#error:_repos.conf:_default_repo_.27gentoo.27_is_undefined_or_invalid).

It should now be possible to install the package from the ebuild repository with the emerge command:

`root #``emerge --ask --verbose app-dicts/artha`
## Simple version bump of an ebuild

If a newer version of the package from the previous example were available upstream, and it were possible to bump it by simply updating the version in the filename, just copy the previous ebuild, update the version number, and run pkgdev and [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck):

`root #````
cd /var/db/repos/local/app-dicts/artha/
```
`root #````
cp artha-1.0.2.ebuild artha-<newversion>.ebuild
```
`root #````
pkgdev manifest
```
`root #````
pkgcheck scan
```
### Bump of an ebuild from the Gentoo ebuild repository

If a newer version of a package from the Gentoo ebuild repository is available upstream, and the update is "bumpable", do not try to bump it directly in the Gentoo ebuild repository directory. To bump an ebuild from a synced repository, copy the ebuild to an appropriate ebuild repository that will not sync over the new ebuild.

For example, to perform an elementary "bump" to a newer version of [app-containers/docker](https://packages.gentoo.org/packages/app-containers/docker) from the Gentoo ebuild repository, create the appropriate directories in a repository that will not sync over, make a copy of the ebuild, giving it the correct name, and run pkgcheck scan:

`root #````
mkdir -v /var/db/repos/local/app-containers
```
`root #````
cd /var/db/repos/local/app-containers 
```
`root #````
cp -r ../../gentoo/app-containers/docker .
```
`root #````
cd docker/
```
`root #````
cp docker-1.11.0.ebuild docker-1.12.6.ebuild
```
`root #````
pkgdev manifest
```
`root #````
pkgcheck scan
```
Now test the installation:

`root #``emerge --ask =app-containers/docker-1.12.6`
Finished ebuilds should be added to the version control system, to make the updated ebuild available to others. If using an ebuild repository from [GitHub](https://github.com/gentoo/gentoo/pulls), consider [adding a pull request](https://wiki.gentoo.org/wiki/GitHub_Pull_Requests).

## See also

- [Basic guide to write Gentoo Ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds) — getting started writing **[ebuilds](https://wiki.gentoo.org/wiki/Ebuild)**, to harness the power of [Portage](https://wiki.gentoo.org/wiki/Portage), to install and manage even more software.
- [Defining a custom repository](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Creating_a_custom_ebuild_repository) (**amd64** handbook)
- [Ebuild](https://wiki.gentoo.org/wiki/Ebuild) — a text file, usually stored in a [repository](https://wiki.gentoo.org/wiki/Ebuild_repository), which identifies a specific software package and tells the Gentoo package manager how to handle it.
- [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) — a file-structure  that can provide packages for installation on a Gentoo system.
- [GitHub Pull Requests](https://wiki.gentoo.org/wiki/GitHub_Pull_Requests) — how to contribute to Gentoo by creating [pull requests on GitHub](https://github.com/gentoo/gentoo/pulls).
- [Repository format](https://wiki.gentoo.org/wiki/Repository_format) — A quick reference to Gentoo ebuild repository (overlay) format.
- [Project:Overlays/Overlays\_guide](https://wiki.gentoo.org/wiki/Project:Overlays/Overlays_guide) - Request adding an ebuild repository to [repos.gentoo.org](https://repos.gentoo.org)
