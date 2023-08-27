# Patchi

This is a minimal role to automate security updates to linux servers (OS Patching).

Role Variables
--------------

The default varibales are defined in `defaults/main.yml`.

Features
--------

1. Support for all or selective upgrades.
2. Prompt for confirmation.
3. Produce log trail for the update operation result.
4. Rollback history (based on `yum/dnf history`).

Dependencies
------------

RHEL/Oracle Linux.


Example Playbook
----------------

Install the role:

```yaml
- name: abarrak.patchi
```

Apply patching to all only-security available updates:

```yaml
- hosts: dev,prod
  become: true
  roles:
  - { role: abarrak.patchi, update_security: true }
```

Apply patching to specific set of packages (with versions optionally, defaults to latest):

```yaml
- hosts: all
  roles:
  - role: abarrak.patchi
    vars:
      package_list:
      - { name: httpd, version: 1.22.2 }
      - { name: expat }
```

Apply patching to all available updates (everything):

```yaml
- hosts: dev
  roles:
  - { role: abarrak.patchi, update_all: true }
```

**Note:** In case any upgrade fails, import the rollback tasks and run it.
It will revert the last yum transaction from the history log.

```yaml
- hosts: prod
  import_role:
    name: abarrak.patchi
    tasks_from: rollback
```

Story
------

The name comes from a [nice chocolate shop in KSA.](http://patchi.com) 😋

<image src="https://raw.githubusercontent.com/abarrak/patchi/main/docs/readme-project-name.jpg" width="90%">

The idea is to make patching sweet and less tedious!

TODOs
-----

* Rollback support.
* Add (pre, post) hooks to handle custom shutdown/startup for critical services.
* Categorization (support tags).


License
-------

MIT.
