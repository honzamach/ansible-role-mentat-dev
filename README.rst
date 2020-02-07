.. _section-role-mentat-dev:

Role **mentat_dev**
================================================================================

.. note::

    This documentation page and role itself is still work in progress.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/mentat_dev>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-mentat-dev>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-mentat-dev>`__

Ansible role for convenient installation of the development version of
`Mentat IDS and SIEM system <https://mentat.cesnet.cz/>`__ directly from source
Git repository. This type of installation is particularly usefull for
development/debugging/testing purposes.

**Table of Contents:**

* :ref:`section-role-mentat-dev-installation`
* :ref:`section-role-mentat-dev-dependencies`
* :ref:`section-role-mentat-dev-usage`
* :ref:`section-role-mentat-dev-variables`
* :ref:`section-role-mentat-dev-files`
* :ref:`section-role-mentat-dev-author`

This role is part of the `MSMS <https://github.com/honzamach/msms>`__ package.
Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-mentat-dev-installation:

Installation
--------------------------------------------------------------------------------

To install the role `honzamach.mentat_dev <https://galaxy.ansible.com/honzamach/mentat_dev>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.mentat_dev

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-mentat-dev <https://github.com/honzamach/ansible-role-mentat-dev>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-mentat-dev.git honzamach.mentat_dev

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


.. _section-role-mentat-dev-dependencies:

Dependencies
--------------------------------------------------------------------------------

This role is dependent on following roles:

* :ref:`postgresql <section-role-postgresql>`
* :ref:`geoip <section-role-geoip>`

No other roles have dependency on this role.


.. _section-role-mentat-dev-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_development]
    your-server

    [servers_mentat_dev]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_mentat_dev
      remote_user: root
      roles:
        - role: honzamach.mentat_dev
      tags:
        - role-mentat-dev

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml --extra-vars '{"hm_mentat__apt_force_update":"yes"}'


.. _section-role-mentat-dev-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_mentat_dev__user

    Name of the UNIX system user for Mentat system.

    * *Datatype:* ``string``
    * *Default:* ``mentat``

.. envvar:: hm_mentat_dev__group

    Name of the UNIX system group for Mentat system.

    * *Datatype:* ``string``
    * *Default:* ``mentat``

.. envvar:: hm_mentat_dev__repository_url

    Base URL to package repository.

    * *Datatype:* ``string``
    * *Default:* ``https://homeproj.cesnet.cz/git/mentat-ng.git/``

.. envvar:: hm_mentat_dev__repo_branch

    Branch to checkout.

    * *Datatype:* ``string``
    * *Default:* ``devel``

.. envvar:: hm_mentat_dev__install_path

    Installation path on target hosts, without trailing slash.

    * *Datatype:* ``string``
    * *Default:* ``/home/mentat/mentat-ng``

.. envvar:: hm_mentat_dev__install_packages

    List of packages defined separately for each linux distribution and package manager,
    that MUST be present on target system. Any package on this list will be installed on
    target host. This role currently recognizes only ``apt`` for ``debian``.

    * *Datatype:* ``dict``
    * *Default:* (please see YAML file ``defaults/main.yml``)
    * *Example:*

    .. code-block:: yaml

        hm_mentat_dev__install_packages:
          debian:
            apt:
              - mentat-ng
              - ...


.. envvar:: hm_mentat_dev__python_venv

    Location for custom Mentat`s Python virtual environment.

    * *Datatype:* ``string``
    * *Default:* ``/home/mentat/mentat-ng/venv``

.. envvar:: hm_mentat_dev__apt_force_update

    Force APT cache update before installing any packages ('yes','no').

    * *Datatype:* ``string``
    * *Default:* ``no``

.. envvar:: hm_mentat_dev__check_queue_size

    Monitoring configuration setting for checking queue size in the *incoming* directory.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 5000, 'c': 10000}``

.. envvar:: hm_mentat_dev__check_queue_dirs

    Monitoring configuration setting for checking queue size in other than *incoming*
    directories.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 100, 'c': 1000}``


Foreign variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This role uses following foreign variables defined in other roles:

:envvar:`hm_monitored__service_name`

    Name of the NRPE service in case the server is in **servers_monitored**
    group and the playbook is automagically configuring monitoring of the Mentat
    system.


Built-in Ansible variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: ansible_lsb['codename']

    Debian distribution codename is used for :ref:`template customization <section-overview-role-customize-templates>`
    feature.


Group memberships
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* **servers_monitored**

  In case the target server is member of this group Nagios monitoring is automagically
  configured for the Mentat system.

* **servers_commonenv**

  In case the target server is member of this group system status script is automagically
  configured for the Mentat system.


.. _section-role-mentat-dev-files:

Managed files
--------------------------------------------------------------------------------

This role directly manages content of following files on target system:

* ``/etc/nagios/nrpe.d/mentat.cfg`` *[TEMPLATE]*
* ``/opt/system-status/system-status.d/40-mentat`` *[TEMPLATE]*


.. _section-role-mentat-dev-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2019 Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
| *Author:* Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
| Use of this role is governed by the MIT license, see LICENSE file.
|
