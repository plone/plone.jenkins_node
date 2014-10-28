Jenkins Node
============

Provisioning for a [Jenkins CI node](http://jenkins-ci.org/) on Ubuntu 14.04 for [Plone](https://plone.org/) projects.


Requirements
------------

None

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

- [plone.jenkins_server](https://galaxy.ansible.com/list#/roles/1183)

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: node5.jenkins.plone.org
      roles:
         - { role: plone.jenkins_node }

License
-------

GPLv2

Author Information
------------------

[Plone](https://plone.org/) Community
