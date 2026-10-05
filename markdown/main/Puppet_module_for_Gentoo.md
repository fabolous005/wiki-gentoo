<!-- source: https://wiki.gentoo.org/wiki/Puppet_module_for_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Puppet module for Gentoo -->
---
title: Puppet module for Gentoo
url: https://wiki.gentoo.org/wiki/Puppet_module_for_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-05-17"
fingerprint: "1ebf8e2a1f9239d0"
license: CC BY-SA 4.0
---

# Puppet module for Gentoo

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page is aimed to document the progress of the GSoC 2013 project: Puppet modules for Gentoo.

## Deliverables

- Extensions to [puppet-portage](https://github.com/adrienthebo/puppet-portage)
  - Support for
    - package slots
    - portage sets
    - package.env
    - layman
    - webapp-config
  - Bugfixes and improvements on existing features
  - Native types and providers using puppet dsl and ruby
  - Unit tests and integration tests using rspec and rspec-puppet
- (Improved) Gentoo support in third-party puppet modules using puppet-portage for package installation and configuration management

## Timeline

- Week 1
  - Slot support in portage::package
- Week 2
  - Tests for slots
- Week 3
  - Fix package and wildcard regexes from eapi5
  - Improve eselect providers
- Week 4
  - webapp-config type and provider
- Week 5
  - webapp-config (contd.)
- Week 6
- Week 7
- Week 8
  - Support for package.env
  - Set default property values in various types
  - Support for layman
- Week 9
  - Support for layman (contd.)
  - OpenRC provider for puppet-network
