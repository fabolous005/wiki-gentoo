<!-- source: https://wiki.gentoo.org/wiki/Pip | group: Gentoo Wiki (Main) | wiki-title: Pip -->
---
title: pip
url: https://wiki.gentoo.org/wiki/Pip
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-06"
fingerprint: ca1dfe3c0663f510
license: CC BY-SA 4.0
---

# pip

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**pip** is [Python](https://wiki.gentoo.org/wiki/Python)'s package management system. It references packages available in the official **Py**thon **P**ackage **I**ndex (PyPI).

## Install

### Emerge

Install [dev-python/pip](https://packages.gentoo.org/packages/dev-python/pip):

`root #``emerge --ask dev-python/pip`
## Usage

In order for pip to operate correctly when installing new packages an active internet connection is a requirement.

### Invocation

`user $``pip --help````
Usage:   
  pip <command> [options]
Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  list                        List installed packages.
  show                        Show information about installed packages.
  check                       Verify installed packages have compatible dependencies.
  config                      Manage local and global configuration.
  search                      Search PyPI for packages.
  cache                       Inspect and manage pip's wheel cache.
  wheel                       Build wheels from your requirements.
  hash                        Compute hashes of package archives.
  completion                  A helper command used for command completion.
  debug                       Show information useful for debugging.
  help                        Show help for commands.
General Options:
  -h, --help                  Show help.
  --isolated                  Run pip in an isolated mode, ignoring environment variables and user configuration.
  -v, --verbose               Give more output. Option is additive, and can be used up to 3 times.
  -V, --version               Show version and exit.
  -q, --quiet                 Give less output. Option is additive, and can be used up to 3 times (corresponding to WARNING, ERROR, and CRITICAL logging levels).
  --log <path>                Path to a verbose appending log.
  --proxy <proxy>             Specify a proxy in the form [user:passwd@]proxy.server:port.
  --retries <retries>         Maximum number of retries each connection should attempt (default 5 times).
  --timeout <sec>             Set the socket timeout (default 15 seconds).
  --exists-action <action>    Default action when a path already exists: (s)witch, (i)gnore, (w)ipe, (b)ackup, (a)bort.
  --trusted-host <hostname>   Mark this host or host:port pair as trusted, even though it does not have valid or any HTTPS.
  --cert <path>               Path to alternate CA bundle.
  --client-cert <path>        Path to SSL client certificate, a single file containing the private key and the certificate in PEM format.
  --cache-dir <dir>           Store the cache data in <dir>.
  --no-cache-dir              Disable the cache.
  --disable-pip-version-check
                              Don't periodically check PyPI to determine whether a new version of pip is available for download. Implied with --no-index.
  --no-color                  Suppress colored output
  --no-python-version-warning
                              Silence deprecation warnings for upcoming unsupported Pythons.
```
### Package installation

To install `package` using the pip command, first create and activate a virtual environment:

`user $````
python -m venv /path/to/venv
```
`user $``source /path/to/venv/bin/activate`
Then install into the virtual environment:

`user $``pip install <package>`
To exit the virtual environment, run:

`user $``deactivate`
The virtual environment is not deleted, and can be re-entered by re-sourcing the activate file (with the command beginning with the period).

To make a library installed in a virtual environment available to other software in that environment, add the relevant path to the `PYTHONPATH`:

`user $``export PYTHONPATH="${PYTHONPATH}:/path/to/venv/lib/python-<version>/site-packages/"`
To create a virtual environment with access to the system packages (i.e. the packages installed via Portage), use the `--system-site-packages` option when creating the environment:

`user $``python -m venv --system-site-packages /path/to/venv`
Once the virtual environment has been activated, confirm that the system packages are available by running `pip list`.

## Removal

Again, this should probably only ever be used in a virtual environment.

To remove a package using pip:

`user $``pip uninstall <package>`
## See also

- [Application level package management](https://wiki.gentoo.org/wiki/Application_level_package_management) — provides best practice recommendations on managing the coexistence of operating system and **application level package managers** on Gentoo.
- [Gem](https://wiki.gentoo.org/wiki/Gem) — programs and libraries for the [Ruby](https://wiki.gentoo.org/wiki/Ruby) programming language.
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
- [Project:Python/Dependencies](https://wiki.gentoo.org/wiki/Project:Python/Dependencies) - How to turn PyPI dependencies to those of ebuild.

## External resources

- [Externally managed environments](https://packaging.python.org/en/latest/specifications/externally-managed-environments/)
- [PEP 668](https://peps.python.org/pep-0668/)
- [Python/Virtual environment](https://wiki.archlinux.org/index.php/Python/Virtual_environment) (ArchWiki article) - Isolated Python's environment, enabling sandboxed installation of packages.
- [pipx - Install and Run Python Applications in Isolated Environments](https://github.com/pypa/pipx)
