=======================================
vbotka.freebsd_nagios 2.6 Release Notes
=======================================

.. contents:: Topics


2.6.1
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------
* Enable the installation of nsca and nrpa if you want to use the
  packages. See Breaking Changes below.

Minor Changes
-------------
* Add variable ma_role_version
* Update tasks debug.yml
* Update README.

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------
* Add variables bsd_nsca_install and bsd_nrpe_install
  default=false. Add variables bsd_nsca_packages and
  bsd_nrpe_packages. nsca and nrpe packages removed from
  bsd_nagios_packages. See defaults/main/main.yml
* Enable the installation of nsca and nrpa if you want to use the
  packages.


2.6.0
=====

Release Summary
---------------
Ansible 2.17 update

Major Changes
-------------
* Supported FreeBSD: 13.3, 14.0, 14.1
* Renamed tasks/packages.yml to tasks/pkg.yml
* Add tasks conf/nrpe_files.yml and conf/nsca_files.yml. Enable to
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
