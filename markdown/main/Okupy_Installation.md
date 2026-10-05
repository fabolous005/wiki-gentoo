<!-- source: https://wiki.gentoo.org/wiki/Okupy/Installation | group: Gentoo Wiki (Main) | wiki-title: Okupy/Installation -->
---
title: Okupy/Installation
url: https://wiki.gentoo.org/wiki/Okupy/Installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-16"
fingerprint: eb75d8147781a1f4
license: CC BY-SA 4.0
---

# Okupy/Installation

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Development environment

### Repositories

- Clone somewhere the [gentoo-identity-bootstrap](https://github.com/dastergon/gentoo-identity-bootstrap) repository:

- Clone (in a different directory) the [identity.gentoo.org](https://github.com/gentoo/identity.gentoo.org) repository:

### Dependencies

Get the dependencies (choose one of the followings):

#### With *pip*

- *Optional*: setup [virtualenv](https://wiki.gentoo.org#virtualenv)
- Install the dependencies:

`user $``pip install -r requirements/base.txt --use-mirrors`
#### With *setup.py*

- *Optional*: setup [virtualenv](https://wiki.gentoo.org#virtualenv)
- Install the dependencies:

`user $``./setup.py install`
#### With *emerge* (Gentoo-specific)

- Add the [okupy](https://github.com/tampakrap/okupy-overlay) overlay:

`root #````
eselect repository add okupy git https://github.com/tampakrap/okupy-overlay.git
```
`root #``emerge --sync okupy`
- Install the dependencies:

`root #``ACCEPT_KEYWORDS="**" emerge --onlydeps okupy`
### Configuration

- Copy the sample settings files:

`user $````
cd identity.gentoo.org
```
`user $````
cp okupy/settings/development.py.sample okupy/settings/development.py
```
`user $````
cp okupy/settings/local_settings.py.sample okupy/settings/local_settings.py
```
- Edit development.py:
  - In STATICFILES\_DIRS, replace /path/to/gentoo-identity-bootstrap with the absolute path that you cloned the gentoo-identity-bootstrap repository earlier
- Edit local\_settings.py
  - Add sqlite3 db (sufficient for testing)
  - Add [LDAP configuration](https://wiki.gentoo.org#OpenLDAP) (if applicable)
- Configure [Memcached](https://wiki.gentoo.org#memcached)
- Sync the database:

`user $``python manage.py syncdb`
## Production environment

- Create the dedicated user that will run okupy

`root #``useradd -m okupy`
- Perform the same setup as for Development environment (using the okupy user)

### uWSGI setup

- Install [www-servers/uwsgi](https://packages.gentoo.org/packages/www-servers/uwsgi) with *USE=python*
- Copy /etc/conf.d/uwsgi to /etc/conf.d/uwsgi.okupy
- Put the following options in /etc/conf.d/uwsgi.okupy

FILE **`/etc/conf.d/uwsgi.okupy`**

- Symlink to /etc/init.d/uwsgi from /etc/init.d/uwsgi.okupy, and start it:

`root #````
ln -s /etc/init.d/uwsgi /etc/init.d/uwsgi.okupy
```
`root #``/etc/init.d/uwsgi.okupy start`
### NGINX setup

- Install [www-servers/nginx](https://packages.gentoo.org/packages/www-servers/nginx)

`root #``emerge --ask --verbose www-servers/nginx`
- Copy the server certificates and private keys to /etc/ssl/nginx/
- Concatenate all the allowed CA certificates for client auth:

`root #``cat /etc/ssl/* > /etc/ssl/nginx/all_certs.pem`
- Add the following options in /etc/nginx/nginx.conf

FILE **`/etc/nginx/nginx.conf`**

## Additional

### virtualenv

- Install virtualenv (replace the following command with an equivalent in case you are working in a non-Gentoo distro)

`root #````
emerge -av dev-python/virtualenv
```
`root #````
virtualenv .virtualenv
```
`root #``source .virtualenv/bin/activate`
- The .virtualenv directory is already in .gitignore, so please prefer this name
- The *deactivate* command will exit the virtual environment

### memcached

- Copy /etc/conf.d/memcached to /etc/conf.d/memcached.okupy

`root #``cp /etc/conf.d/memcached /etc/conf.d/memcached.okupy`
- Symlink /etc/init.d/memcached.okupy to /etc/init.d/memcached

`root #``ln -s /etc/init.d/memcached /etc/init.d/memcached.okupy`
- Put the following data in /etc/conf.d/memcached.okupy:

FILE **`/etc/conf.d/memcached.okupy`**

```
# The user that will be running okupy
MEMCACHED_RUNAS="okupy"
# disable TCP/IP
LISTENON=""
PORT=""
# enable UNIX socket (put correct path here as well)
MISC_OPTS="-s /home/okupy/memcached.sock"
```
- edit okupy/settings/local.py and put the same path in CACHES:

FILE **`okupy/settings/local.py`**

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
        'LOCATION': 'unix://home/okupy/memcached.sock',
    }
}
```
- Start memcached

`root #``/etc/init.d/memcached.okupy start`
### OpenLDAP

#### OpenLDAP Server

- (TODO)

#### OpenLDAP client only

- We have a testing instance on *ldap://evidence.tampakrap.gr*
- Contact [tampakrap](https://wiki.gentoo.org/wiki/User:Tampakrap) to get the certificates and the rootDN credentials
- Install OpenLDAP package:
  - In Gentoo:

`root #````
echo net-nds/openldap minimal >> /etc/portage/package.use/okupy
```
`root #``emerge --ask --verbose openldap`
- Put the certificates in /etc/openldap/ssl
- Put the following content in /etc/openldap/ldap.conf:

FILE **`/etc/openldap/ldap.conf`**

- In settings/local.py:

FILE **`settings/local.py`**

```
AUTH_LDAP_SERVER_URI = 'ldap://evidence.tampakrap.gr'
 
AUTH_LDAP_CONNECTION_OPTIONS = {
    ldap.OPT_X_TLS_DEMAND: False,
}
 
AUTH_LDAP_BIND_DN = ''
AUTH_LDAP_BIND_PASSWORD = ''
 
AUTH_LDAP_ADMIN_BIND_DN = '(the rootDN you got from tampakrap)'
AUTH_LDAP_ADMIN_BIND_PASSWORD = '(the rootpw you got from tampakrap)'
 
AUTH_LDAP_USER_ATTR = 'uid'
AUTH_LDAP_USER_BASE_DN = 'ou=users,dc=tampakrap,dc=gr'
 
AUTH_LDAP_PERMIT_EMPTY_PASSWORD = False
 
AUTH_LDAP_START_TLS = True
 
# objectClasses that are used by any user
AUTH_LDAP_USER_OBJECTCLASS = ['top', 'person', 'organizationalPerson',
                               'inetOrgPerson', 'posixAccount', 'shadowAccount',
                               'ldapPublicKey', 'gentooGroup']
# additional objectClasses that are used by developers
AUTH_LDAP_DEV_OBJECTCLASS = ['gentooDevGroup']
```
