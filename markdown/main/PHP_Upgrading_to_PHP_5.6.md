<!-- source: https://wiki.gentoo.org/wiki/PHP/Upgrading_to_PHP_5.6 | group: Gentoo Wiki (Main) | wiki-title: PHP/Upgrading to PHP 5.6 -->
---
title: PHP/Upgrading to PHP 5.6
url: https://wiki.gentoo.org/wiki/PHP/Upgrading_to_PHP_5.6
hostname: gentoo.org
sitename: PHP/Upgrading to PHP 5.6
date: "2022-07-11"
fingerprint: bed417643f8f99cf
license: CC BY-SA 4.0
---

# PHP/Upgrading to PHP 5.6

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

To upgrade a system to PHP 5.6, first set the `PHP_TARGETS` variable in make.conf:

**`/etc/portage/make.conf`**

**Setting the target PHP version**

```
PHP_TARGETS="php5-6"
```
## Installation

Install PHP 5.6:

`root #``emerge --ask dev-lang/php:5.6`
## Replace APC

To build with the `opcache` USE flag (the default) while allowing the use of APC userland functions such as `apc_fetch()`, install APCu, which emulates APC but only has user caching functions. This may be necessary for older projects that rely on APC for caching.

If necessary, unmask it:

`root #``echo 'dev-php/pecl-apcu ~amd64' >> /etc/portage/package.accept_keywords`
Install APCu:

`root #``emerge --ask dev-php/pecl-apcu`
For PHP 7 compatibility:

`root #``emerge --ask dev-php/pecl-apcu_bc`
## Switch to PHP 5.6

Switch to it using eselect for all SAPIs:

`root #````
eselect php set cli php5.6
```
`root #````
eselect php set fpm php5.6
```
When other SAPIs are in use, be sure to run eselect for those as well.

## Re-install compatible extensions

Since changing `PHP_TARGETS` is technically a USE flag change, it suffices to simply emerge @world with flags checking for new USE flags and all extensions should re-build.

`root #``emerge --ask --changed-use --deep @world`
## Uninstall incompatible extensions

Remove all old extensions that do not build for PHP 5.6. eix can be used for this task. Note that when piping to emerge, interactive mode cannot be used so `--ask` on the emerge command is not applicable here.

`root #``emerge --ask app-portage/eix``root #``eix -I -C dev-php --use php_targets_php5-5 --not --use php_targets_php5-6 --only-names | xargs emerge -c`
Be sure to migrate USE flags if versions have been specified in the package.use file(s).

Now that extensions have been removed, it may be useful to run a dependency clean up at this point for older ones that are no longer compatible.

`root #``emerge --ask --depclean`
## Migrate the configuration files

The different slots have different configuration paths. Please take care to migrate the configuration files before making use of a new slot.

## Uninstall old PHP versions

For example, to remove PHP 5.5:

`root #``emerge --ask -vc dev-lang/php:5.5`
## Restart PHP-FPM

Restart PHP-FPM if applicable:

`root #``/etc/init.d/php-fpm restart`
