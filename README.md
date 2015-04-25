Jenkins node
============
Provisioning for a [Jenkins CI node](http://jenkins-ci.org/) on Ubuntu 14.04 for [Plone](https://plone.org/) projects.

Requirements
------------
None.

Role variables
--------------
None.

Example playbook
----------------
    - hosts: node5.jenkins.plone.org
      roles:
         - { role: plone.jenkins_node }

Server playbook
---------------
Looking for the ansible playbook for the jenkins master?

Look no further: [plone.jenkins_server](https://galaxy.ansible.com/list#/roles/1183)

License
-------
GPLv2

Author information
------------------
[Plone](https://plone.org/) Community.
