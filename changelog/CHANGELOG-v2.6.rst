=======================================
vbotka.freebsd_nagios 2.6 Release Notes
=======================================

.. contents:: Topics


2.6.0
=====

Release Summary
---------------
Ansible 2.17 update

Major Changes
-------------
* Supported FreeBSD: 13.3, 14.0, 14.1
* Renamed tasks/packages.yml to tasks/pkg.yml
* Add tasks onf/nrpe_files.yml and conf/nsca_files.yml. Enable to
  create missing config files from samples in one step (-t
  bsd_nagios_conf_files,bsd_nsca_conf_files,bsd_nrpe_conf_files)

Minor Changes
-------------
* Update README.
* Update ansible-lint conf.
* Fix ansible-lint errors and warnings.
* Add changelog.
* Globally replace filter default() with alias d()
* Update handlers. Listen on lowercase names.

Bugfixes
--------
* PR #1

Breaking Changes / Porting Guide
--------------------------------
