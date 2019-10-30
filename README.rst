.. _section-role-mentat-dev:

Role **mentat_dev**
================================================================================

Ansible role for convenient installation of the development version of
`Mentat IDS and SIEM system <https://mentat.cesnet.cz/>`__ directly from source
Git repository. This type of installation is particularly usefull for
development/debugging/testing purposes.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/mentat_dev>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-mentat-dev>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-mentat-dev>`__


Description
--------------------------------------------------------------------------------

This role attempts to install the Mentat system directly from source Git repository.

.. note::

    This role supports the :ref:`template customization <section-overview-customize-templates>` feature.


Requirements
--------------------------------------------------------------------------------

Python3 with pip3 utility should already be installed on target system.


Dependencies
--------------------------------------------------------------------------------

This role is dependent on following roles:

* :ref:`postgresql <section-role-postgresql>`

No other roles have direct dependency on this role.


Managed files
--------------------------------------------------------------------------------

This role directly manages content of following files on target system:

* ``/etc/nagios/nrpe.d/mentat.cfg``
* ``/opt/system-status/system-status.d/40-mentat``


Role variables
--------------------------------------------------------------------------------

There are following internal role variables defined in ``defaults/main.yml`` file,
that can be overriden and adjusted as needed:

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

.. envvar:: hm_mentat_dev__package_list

    List of Mentat-related packages, that will be installed on target system.

    * *Datatype:* ``list of strings``
    * *Default:* (please see YAML file ``defaults/main.yml``)

.. envvar:: hm_mentat_dev__python_venv

    Location for custom Mentat`s Python virtual environment.

    * *Datatype:* ``string``
    * *Default:* ``/home/mentat/mentat-ng/venv``

.. envvar:: hm_mentat_dev__apt_force_update

    Force APT cache update before installing any packages ('yes','no').

    * *Datatype:* ``string``
    * *Default:* ``no``

.. envvar:: hm_mentat_dev__check_queue_size:

    Monitoring configuration setting for checking queue size in the *incoming* directory.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 5000, 'c': 10000}``

.. envvar:: hm_mentat_dev__check_queue_dirs:

    Monitoring configuration setting for checking queue size in other than *incoming*
    directories.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 100, 'c': 1000}``

Additionally this role makes use of following built-in Ansible variables:

.. envvar:: ansible_lsb['codename']

    Debian distribution codename is used for :ref:`template customization <section-overview-customize-templates>`
    feature.

.. envvar:: group_names

    See section *Group memberships* below for details.


Foreign variables
--------------------------------------------------------------------------------

This role uses following foreign variables defined in other roles:

:envvar:`hm_monitored__service_name`

    Name of the NRPE service in case the server is in **servers_monitored**
    group and the playbook is automagically configuring monitoring of the Mentat
    system.


Group memberships
--------------------------------------------------------------------------------

* **servers-development** or **servers-testing**

  I like to use certain groups for dividing servers according to the service
  level. Currently following levels are recognized:

  * servers-development
  * servers-testing

  This role in particular currently recognizes only ``servers-development`` and
  ``servers-testing`` groups. You may use membership in aforementioned groups
  to choose which package suite (*development* or *testing*) will be installed
  on target host.

* **servers_monitored**

  In case the target server is member of this group Nagios monitoring is automagically
  configured for the Mentat system.

* **servers_commonenv**

  In case the target server is member of this group system status script is automagically
  configured for the Mentat system.


Installation
--------------------------------------------------------------------------------

To install the role `honzamach.mentat <https://galaxy.ansible.com/honzamach/mentat_dev>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.mentat_dev

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-mentat-dev <https://github.com/honzamach/ansible-role-mentat-dev>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-mentat-dev.git honzamach.mentat_dev

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


Example Playbook
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers-development]
    localhost

    [servers_mentat-dev]
    localhost

Example content of role playbook file ``playbook.yml``::

    - hosts: servers_mentat_dev
      remote_user: root
      roles:
        - role: honzamach.mentat_dev
      tags:
        - role-mentat-dev

Example usage::

    ansible-playbook -i inventory playbook.yml
    ansible-playbook -i inventory playbook.yml --extra-vars '{"hm_mentat__apt_force_update":"yes"}'


License
--------------------------------------------------------------------------------

MIT


Author Information
--------------------------------------------------------------------------------

Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
