<!-- source: https://wiki.gentoo.org/wiki/Vscode | group: Gentoo Wiki (Main) | wiki-title: Vscode -->
---
title: VS Code
url: https://wiki.gentoo.org/wiki/Vscode
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-09"
fingerprint: "9e86bc8f92a1fac1"
license: CC BY-SA 4.0
---

# VS Code

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**VS Code** or **Visual Studio Code** is a lightweight but powerful source code editor which runs on the desktop, and is available for Windows, macOS, and Linux.

## Installation

### USE flags


### Before Installation

In order to install VS Code the user needs to accept the *Microsoft-vscode* license agreement. A copy of the license can be found at /var/db/repos/gentoo/licenses/Microsoft-vscode. Read with:

`user $``less /var/db/repos/gentoo/licenses/Microsoft-vscode`
And to agree:

`root #``echo "app-editors/vscode Microsoft-vscode" >> /etc/portage/package.license`
To prefer the free/libre version of VS Code, install VSCodium with the package [app-editors/vscodium](https://packages.gentoo.org/packages/app-editors/vscodium), which completely removes telemetry. To learn the differences, [read the upstream documentation](https://github.com/VSCodium/vscodium/blob/master/docs/index.md#extensions-marketplace).

### Emerge

Emerge Microsoft's release with:

`root #``emerge --ask app-editors/vscode`
Or emerge the free/libre release with:

`root #``emerge --ask app-editors/vscodium`
## Usage

Depending on the version installed, run VS Code with:

`user $``vscode`
Or:

`user $``vscodium`
### Invocation

`user $``vscode --help````
Visual Studio Code _Version_here_
Usage: code [options][paths...]
To read from stdin, append '-' (e.g. 'ps aux | grep code | code -')
Options
  -d --diff <file> <file>           Compare two files with each other.
  -a --add <folder>                 Add folder(s) to the last active window.
  -g --goto <file:line[:character]> Open a file at the path on the specified
                                    line and character position.
  -n --new-window                   Force to open a new window.
  -r --reuse-window                 Force to open a file or folder in an
                                    already opened window.
  -w --wait                         Wait for the files to be closed before
                                    returning.
  --locale <locale>                 The locale to use (e.g. en-US or zh-TW).
  --user-data-dir <dir>             Specifies the directory that user data is
                                    kept in. Can be used to open multiple
                                    distinct instances of Code.
  -h --help                         Print usage.
Extensions Management
  --extensions-dir <dir>
      Set the root path for extensions.
  --list-extensions
      List the installed extensions.
  --show-versions
      Show versions of installed extensions, when using --list-extensions.
  --category <category>
      Filters installed extensions by provided category, when using --list-extensions.
  --install-extension <extension-id[@version] | path-to-vsix>
      Installs or updates the extension. The identifier of an extension is always `${publisher}.${name}`. Use `--force` argument to update to latest version. To install a
specific version provide `@${version}`. For example: 'vscode.csharp@1.2.3'.
  --pre-release
      Installs the pre-release version of the extension, when using --install-extension
  --uninstall-extension <extension-id>
      Uninstalls an extension.
  --enable-proposed-api <extension-id>
      Enables proposed API features for extensions. Can receive one or more extension IDs to enable individually.
Troubleshooting
  -v --version                       Print version.
  --verbose                          Print verbose output (implies --wait).
  --log <level>                      Log level to use. Default is 'info'.
                                     Allowed values are 'critical', 'error',
                                     'warn', 'info', 'debug', 'trace', 'off'.
  -s --status                        Print process usage and diagnostics
                                     information.
  --prof-startup                     Run CPU profiler during startup.
  --disable-extensions               Disable all installed extensions.
  --disable-extension <extension-id> Disable an extension.
  --sync <on> <off>                  Turn sync on or off.
  --inspect-extensions <port>        Allow debugging and profiling of
                                     extensions. Check the developer tools for
                                     the connection URI.
  --inspect-brk-extensions <port>    Allow debugging and profiling of
                                     extensions with the extension host being
                                     paused after start. Check the developer
                                     tools for the connection URI.
  --disable-gpu                      Disable GPU hardware acceleration.
  --max-memory <memory>              Max memory size for a window (in Mbytes).
  --telemetry                        Shows all telemetry events which VS code
                                     collects.
```
## Tips

### Disable Telemetry

See [information on how to change or completely disable](https://code.visualstudio.com/docs/getstarted/telemetry#_disable-telemetry-reporting) telemetry on VS Code. Read the full page to understand telemetry, data protection, and privacy.

## Troubleshooting

### Login with GitHub doesn't work

Writing login information to the keychain failed with error: *Object does not exist at path "/org/freedesktop/secrets/collection/login"*.
On [systemd](https://wiki.gentoo.org/wiki/Systemd) systems: add this line to the WM init file:[\[1\]](https://code.visualstudio.com/docs/editor/settings-sync#_linux)

`user $``source /etc/X11/xinit/xinitrc.d/50-systemd-user.sh`
On [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) systems, run this program before starting VSCode or add it to WM init file:[\[2\]](https://forums.gentoo.org/viewtopic-t-1149647-start-0-postdays-0-postorder-asc-highlight-.html)

`user $``dbus-update-activation-environment --all`
### Very high CPU usage

VS Code could spawn a process called rg and cause huge stress for the CPU. A workaround can be done by setting the "search.followSymlinks" option to false in the options panel, following issue [https://github.com/microsoft/vscode/issues/98594#issuecomment-643871783](https://github.com/microsoft/vscode/issues/98594#issuecomment-643871783).

## See also

- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
