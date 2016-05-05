etcd
====

A role which installs and manages a clustered etcd

Role ready status
-----------------

[![Galaxy](http://img.shields.io/badge/galaxy-ansible--etcd-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/6974)

Requirements
------------

None

Role Variables
--------------

See defaults/main.yml

Dependencies
------------

None

Example Playbook
----------------

    - hosts: etcd
      roles:
        - etcd

Testing
-------

Tests are performed by [Molecule](http://molecule.readthedocs.org/en/latest/).

    $ pip install -r requirements.txt
    $ molecule test

Tests are currently broken because we require Ansible 2.x for the `yum` task, but molecule only currently supports 1.x, for more information take a look at this [issue](https://github.com/philpep/testinfra/issues/49).

License
-------

MIT
