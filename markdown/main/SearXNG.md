<!-- source: https://wiki.gentoo.org/wiki/SearXNG | group: Gentoo Wiki (Main) | wiki-title: SearXNG -->
---
title: SearXNG
url: https://wiki.gentoo.org/wiki/SearXNG
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: f097f47ce4054fe2
license: CC BY-SA 4.0
---

# SearXNG

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**SearXNG** (or "searching") is a free internet metasearch engine which aggregates results from various search services and databases. SearXNG allows users to specify which search engines they want to include in their search results, group engines in categories, specify engine timeouts, and more. SearXNG can be used via someone else's instance<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> or a self-hosted instance. This wiki page teaches users how to host an instance.

## Installation

### Emerge

We will use [Git](https://wiki.gentoo.org/wiki/Git) ([dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git)) to download the SearXNG repository. If Git isn't installed, install it now.

`root #``emerge --ask dev-vcs/git`
### Make the SearXNG user

The SearXNG server will be running as user searxng; technically, we can name this user anything. We also specify several other options:

- `--shell /bin/bash` -- Specify the shell of this user.
- `--system` -- Make a system account; this user is intended to be ran by the machine and not by a human (this user will be given numeric identifiers that represent system identifiers).
- `-m` -- Make a home directory for this user.
- `--home-dir /usr/local/searxng` -- Specify the home directory.
- `--comment 'Privacy-respecting metasearch engine'` -- Add a comment to describe this user.

`root #``useradd --shell /bin/bash --system -m --home-dir /usr/local/searxng --comment 'Privacy-respecting metasearch engine' searxng`
Most of the following commands will need to be ran by the searxng user; switch to this user now.

`root #``sudo -u searxng -i`
### Make the virtual environment

SearXNG uses several [Python](https://wiki.gentoo.org/wiki/Python) packages installed with [pip](https://wiki.gentoo.org/wiki/Pip). We need to make a virtual environment for pip to install packages into so that they don't conflict with system packages.

`searxng $``python -m venv /usr/local/searxng/searx-pyenv`
At this point, the virtual environment is installed, but it must be activated to use it; this is done by sourcing the file /usr/local/searxng/searx-pyenv/bin/activate. It can get tiresome to source this file every time we need to manage SearXNG; to fix this, we can append a command to searxng's .bashrc file so that it gets sourced every time we switch to this user.

`searxng $``echo ". /usr/local/searxng/searx-pyenv/bin/activate" >>/usr/local/searxng/.bashrc`
We can go ahead and source the file that activates the virtual environment. The result of this command should prefix "(searx-pyenv)" to PS1 (the prompt); this is how we can tell the virtual environment is activated.

`searxng $``. /usr/local/searxng/searx-pyenv/bin/activate`
#### Update the boilerplate

With the user and virtual environment set up, we can now use pip to install packages.

`(searx-pyenv) searxng $``pip install -U pip setuptools wheel pyyaml msgspec typing-extensions`
#### Install SearXNG into the virtual environment

We can combine the cloning of the SearXNG repository and it's installation into the Python virtual environment in a single command; this will clone the repository into /usr/local/searxng/searx-pyenv/src/searxng.

Run `exit` to return to the root user.

`(searx-pyenv) searxng $``exit`
### (Optional) Install uWSGI

SearXNG can be started manually by logging in as searxng and running a Python script. To have SearXNG start at boot, we can set up a Python web server; in this case, we will be using uWSGI.

To run SearXNG, uWSGI will need to be installed with the `python` USE flag enabled because SearXNG is made in Python.

**`/etc/portage/package.use/uwsgi`**

Install uWSGI ([www-servers/uwsgi](https://packages.gentoo.org/packages/www-servers/uwsgi)).

`root #``emerge --ask www-servers/uwsgi`
## Configuration

### Files

#### SearXNG

Make the directory that will contain the configuration files for SearXNG.

`root #``mkdir -p /etc/searxng`
SearXNG can be customized via two methods:

- Visiting the SearXNG webpage and selecting the "Preferences" button to make changes and saving those changes in our browser's cookies.
- Making changes to the /etc/searxng/settings.yml file directly.

Customizing SearXNG via a browser will only affect the user of that browser; customizing the configuration file will affect all users that visit the SearXNG webpage. The following command gets the latest default configuration file for SearXNG from the official repository and puts it in the correct location:

Substitute the text "`ultrasecretkey`" with a random hexadecimal 16 bytes long.

`root #``sed -i -e "s/ultrasecretkey/$(openssl rand -hex 16)/g" /etc/searxng/settings.yml`
#### uWSGI

The default configuration file for a uWSGI service is located at /etc/conf.d/uwsgi. We can make a copy of this file to /etc/conf.d/uwsgi.searxng and customize it to our needs. We can use the following configuration file for uWSGI to have it run our SearXNG instance:

**`/etc/conf.d/uwsgi.searxng`**

```
# SearXNG has its uWSGI config in the 'ini' format.
UWSGI_EXTRA_OPTIONS="--ini /etc/searxng/searxng.ini"
```
Next, we need to tell uWSGI how to run SearXNG, answering questions:

- What user/group does SearXNG run as?
- How many threads do we use?
- What plugins do we use?
- Etc...

We can copy a template provided when we installed SearXNG and put it in the correct location.

`root #``cp /usr/local/searxng/searx-pyenv/src/searxng/utils/templates/etc/uwsgi/apps-available/searxng.ini /etc/searxng/searxng.ini`
Most of the content in /etc/searxng/searxng.ini is already correct, but some modifications will need to be made.

**`/etc/searxng/searxng.ini`**

**Changes to this file**

```
uid = searxng
gid = searxng
chdir = /usr/local/searxng/searx-pyenv/src/searxng/searx
env = SEARXNG_SETTINGS_PATH=/etc/searxng/settings.yml
virtualenv = /usr/local/searxng/searx-pyenv
pythonpath = /usr/local/searxng/searx-pyenv/src/searxng
http = 127.0.0.1:8888
static-map = /static=/usr/local/searxng/searx-pyenv/src/searxng/searx/static
```
The variable `plugin` in /etc/searxng/searxng.ini will also need to be changed, but the value can vary depending on the version of Python installed on the system. The list of available uWSGI modules is in /usr/lib64/uwsgi; remember that the number of modules available depends on whether or not the `embedded` USE flag is enabled.

`user $``ls -1 /usr/lib64/uwsgi`
asyncio312\_plugin.so
cache\_plugin.so
carbon\_plugin.so
cheaper\_busyness\_plugin.so
corerouter\_plugin.so
fastrouter\_plugin.so
http\_plugin.so
logfile\_plugin.so
logsocket\_plugin.so
mongodblog\_plugin.so
nagios\_plugin.so
pam\_plugin.so
ping\_plugin.so
python312\_plugin.so
rawrouter\_plugin.so
redislog\_plugin.so
router\_basicauth\_plugin.so
router\_cache\_plugin.so
router\_expires\_plugin.so
router\_hash\_plugin.so
router\_http\_plugin.so
router\_memcached\_plugin.so
router\_metrics\_plugin.so
router\_redirect\_plugin.so
router\_redis\_plugin.so
router\_rewrite\_plugin.so
router\_static\_plugin.so
router\_uwsgi\_plugin.so
rpc\_plugin.so
rrdtool\_plugin.so
rsyslog\_plugin.so
signal\_plugin.so
spooler\_plugin.so
sslrouter\_plugin.so
symcall\_plugin.so
syslog\_plugin.so
transformation\_chunked\_plugin.so
transformation\_gzip\_plugin.so
transformation\_offload\_plugin.so
transformation\_tofile\_plugin.so
ugreen\_plugin.so
zergpool\_plugin.so

SearXNG needs the python and http plugins at a bare minimum because that is how we will be using it. We can add other plugins for more features:

- asyncio for increased performance.
- pam to use [PAM](https://wiki.gentoo.org/wiki/PAM).
- Etc...

Some plugins have numbers after them specifying a specific version, these numbers must be included in the name of the plugin.

**`/etc/searxng/searxng.ini`**

**Changes to this file**

```
# This will **NOT** work!
plugin = python,asyncio,http
# This will work.
plugin = python312,asyncio312,http
```
### Service

#### OpenRC

If we try to run `rc-service uwsgi start`, [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) will tell us that we should make a symlink to the uWSGI service we want; so, we do exactly that.

`root #``ln -s /etc/init.d/uwsgi{,.searxng}` Don't forget to add the service to the default run level.

`root #``rc-update add uwsgi.searxng default`
## Usage

### Manual

SearXNG can be started manually by running the Python executable as the searxng user.

`user $``sudo -u searxng -i``(searx-pyenv) searxng $``python searx-pyenv/src/searxng/searx/webapp.py`
Now, open any web browser from Links to [Firefox](https://wiki.gentoo.org/wiki/Firefox) and visit [http://127.0.0.1:8888](http://127.0.0.1:8888)

### uWSGI

If uWSGI isn't already running, run the following command:

`root #``rc-service uwsgi.searxng start`
If everything is working, use any web browser from Links to Firefox and visit [http://127.0.0.1:8888](http://127.0.0.1:8888)

## Upgrade

Upgrading SearXNG involves upgrading several components: the Python packages installed in the virtual environment (this includes SearXNG itself), and the virtual environment itself. First, switch to the searxng user.

`user $``sudo -u searxng -i`
Upgrade the virtual environment.

`(searx-pyenv) searxng $``python -m venv --upgrade /usr/local/searxng/searx-pyenv`
Upgrade all Python packages (this includes SearXNG) installed in the virtual environment.

`(searx-pyenv) searxng $``pip install -U --use-pep517 --no-build-isolation $(pip freeze)`
This can be done as any user with a single command; remember that we need to activate the virtual environment before we use `pip`!

`user $``sudo -u searxng bash -c '. /usr/local/searxng/searx-pyenv/bin/activate && python -m venv --upgrade /usr/local/searxng/searx-pyenv && pip install -U --use-pep517 --no-build-isolation $(pip freeze)'`
## Removal

### SearXNG

Delete the searxng user; the `-r` option also deletes this user's home directory (/usr/local/searxng) and everything in it. We don't need to uninstall the Python packages because they're all contained in the virtual environment -- which is inside this user's home directory.

`root #``userdel -r searxng`
Delete the SearXNG configuration directory and everything in it.

`root #``rm -rd /etc/searxng`
### uWSGI

Stop the uWSGI service.

`root #``rc-service uwsgi.searxng stop`
Delete the uWSGI service from all runlevels.

`root #``rc-update del -a uwsgi.searxng`
Delete all files associated with uWSGI.

`root #````
rm /etc/init.d/uwsgi.searxng
```
`root #````
rm /etc/conf.d/uwsgi.searxng
```
`root #````
rm /etc/searxng/searxng.ini
```
Uninstall uWSGI.

`root #``emerge --ask --depclean --verbose www-servers/uwsgi`
## See also

- [nginx](https://wiki.gentoo.org/wiki/Nginx) — a robust, small, high performance [web server](https://wiki.gentoo.org/wiki/Category:Web_servers) and reverse proxy server.
