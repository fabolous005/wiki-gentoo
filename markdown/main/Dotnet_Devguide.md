<!-- source: https://wiki.gentoo.org/wiki/Dotnet/Devguide | group: Gentoo Wiki (Main) | wiki-title: Dotnet/Devguide -->
---
title: Dotnet/Devguide
url: https://wiki.gentoo.org/wiki/Dotnet/Devguide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-20"
fingerprint: d7156a698e3fa748
license: CC BY-SA 4.0
---

# Dotnet/Devguide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A .NET guide for Gentoo developers.

### Finding correct NuGet packages

To find a list of NuGet packages one can use the [dev-dotnet/gentoo-dotnet-maintainer-tools](https://packages.gentoo.org/packages/dev-dotnet/gentoo-dotnet-maintainer-tools) package.
First check out via VCS or download sources of a particular project, then cd into its source directory and run:

`user $``gdmt restore --project "$(pwd)"`
#### Debugging NuGet package sources

Sometimes a package will want to download NuGet packages that are not from
the official NuGet Gallery, that is [https://api.nuget.org/v3-flatcontainer](https://api.nuget.org/v3-flatcontainer).
The special configuration to control where the NuGet packages are downloaded
from lives inside NuGet.config file in the project repository.

To see where the packages are downloaded from restore the project with the following command:

`user $``NUGET_PACKAGES="$(pwd)/.cache/nuget_packages" dotnet restore --verbosity Detailed`
Then, inspect the created restore.log file.

#### Using v3 sources

Most of the time when the Gallery API v3 version is used the translation of NuGet sources to package URLs is easy. Trailing index.json should be swapped with flat2.

This example:

**`NuGet.config`**

translates to:

**`pkg-1.ebuild`**

### Maintaining individual packages

#### Maintaining .NET SDK

See also:

#### Maintaining dotnet-runtime-nugets

The [dev-dotnet/dotnet-runtime-nugets](https://packages.gentoo.org/packages/dev-dotnet/dotnet-runtime-nugets) package is Gentoo's way of workaround to SDKs requiring a special set of NuGet packages.
After you have downloaded or compiled the SDK change to the SDK directory containing the dotnet executable.

Then execute:

`user $``gdmt checkcore --sdk-min "6.0" --sdk-ver "9.0" --sdk-exe "$(pwd)/dotnet"`
#### Maintaining pwsh

pwsh is the new name of cross-platform version of Powershell developed by Microsoft. Use the tool gdmt genpwsh from gentoo-dotnet-maintainer-tools. As only argument give it the PowerShell version you want to package.

#### Maintaining fsautocomplete

Prepare the list of NuGet packages with:

`user $````
cd /var/tmp/portage/.../fsautocomplete-.../work/FsAutoComplete-.../
```
`user $````
export MSBUILDDISABLENODEREUSE=1 RollForward=Major UseSharedCompilation=false
```
`user $````
export NUGET_PACKAGES="$(pwd)/.cache/nuget_packages"
```
`user $````
dotnet tool install -g paket
```
`user $````
paket restore
```
`user $````
gdmt restore -p ./src/FsAutoComplete/FsAutoComplete.fsproj -e 9.0 -c ./.cache
```
`user $````
gdmt restore -p ./FsAutoComplete.sln -e 9.0 -c ./.cache
```
## See Also

- [C-Sharp](https://wiki.gentoo.org/wiki/C-Sharp) — an open-source, general-purpose, multi-paradigm, programming language
- [Project:Dotnet](https://wiki.gentoo.org/wiki/Project:Dotnet) — an effort to provide support for the .NET Core development platform within the Gentoo Linux operating system and a comprehensive set of packages for the .NET ecosystem, including the .NET SDK, development tools, and popular applications written using .NET.
- [PowerShell](https://wiki.gentoo.org/wiki/PowerShell) — a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework
