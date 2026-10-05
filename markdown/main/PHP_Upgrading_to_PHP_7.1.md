<!-- source: https://wiki.gentoo.org/wiki/PHP/Upgrading_to_PHP_7.1 | group: Gentoo Wiki (Main) | wiki-title: PHP/Upgrading to PHP 7.1 -->
---
title: PHP/Upgrading to PHP 7.1
url: https://wiki.gentoo.org/wiki/PHP/Upgrading_to_PHP_7.1
hostname: gentoo.org
sitename: PHP/Upgrading to PHP 7.1
date: "2024-12-20"
fingerprint: "36b4576cad8fbbcc"
license: CC BY-SA 4.0
---

# PHP/Upgrading to PHP 7.1

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Compatibility check

PHP 7 has some major changes from the previous 5.x versions. All scripts should be checked before upgrading.

[PHP 7 Compatiblity Checker](https://github.com/sstalle/php7cc/releases) is a wonderful tool to do this automatically.

[PHP Static Analysis Tool](https://github.com/phpstan/phpstan) is an amazing tool to check your code as if it was running.  This is beyond syntax checking.

## PHP Targets

To upgrade a system to PHP 7.1, first set the `PHP_TARGETS` variable in make.conf:

**`/etc/portage/package.use/php`**

**Setting the target PHP version**

```
*/* PHP_TARGETS: php7-1
```
## Installation

Install PHP 7.1:

`root #``emerge --ask dev-lang/php:7.1`
## Replace APC

To build with the `opcache` USE flag (the default) while allowing the use of APC userland functions such as `apc_fetch()`, install APCu, which emulates APC but only has user caching functions. This may be necessary for older projects that rely on APC for caching.

Install APCu:

`root #``emerge --ask dev-php/pecl-apcu`
The PHP 7 versions of pecl-apcu have a new syntax which need changes to support.
Fortunately, there is a compatibility package, [dev-php/pecl-apcu\_bc](https://packages.gentoo.org/packages/dev-php/pecl-apcu_bc):

`root #``emerge --ask dev-php/pecl-apcu_bc`
## Switch to PHP 7.1

Switch to it using eselect for all SAPIs:

`root #````
eselect php set cli php7.1
```
`root #````
eselect php set fpm php7.1
```
When other SAPIs are in use, be sure to run eselect for those as well.

## Re-install compatible extensions

Since changing `PHP_TARGETS` is technically a USE flag change, it suffices to simply emerge @world with flags checking for new USE flags and all extensions should re-build.

`root #``emerge --ask --changed-use --deep @world`
## Migrate the configuration files

The different slots have different configuration paths. Please take care to migrate the configuration files before making use of a new slot.

## Uninstall old PHP versions

For example, to remove PHP 5.6:

`root #``emerge --ask -vc dev-lang/php:5.6`
## Restart PHP-FPM

Restart PHP-FPM if applicable:

`root #``/etc/init.d/php-fpm restart`
