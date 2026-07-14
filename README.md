Jenkins node
============
Provisioning for a [Jenkins CI node](http://jenkins-ci.org/) on Ubuntu 14.04 for [Plone](https://plone.org/) projects.

Requirements
------------
None.

Role variables
--------------
* `python_versions`: list of Python versions to install with `uv` (default: 3.9 through 3.14).
  Each version ends up as `/srv/pythonX.Y/bin/python3`.
* `uv_python_install_dir`: where `uv` stores the interpreters (default: `/srv/uv`).
* `nvm_version`: version of [nvm](https://github.com/nvm-sh/nvm) to install for the jenkins user (default: `0.40.3`).
* `nvm_node_version`: node version that gets installed and set as default via nvm (default: `22`).

Example playbook
----------------
    - hosts: node5.jenkins.plone.org
      roles:
         - { role: plone.jenkins_node }

The role variables can be overridden in the playbook.
For example to install a different node version:

    - hosts: node5.jenkins.plone.org
      roles:
        - role: plone.jenkins_node
          nvm_node_version: "24"

Server playbook
---------------
Looking for the ansible playbook for the jenkins master?

Look no further: [plone.jenkins_server](https://galaxy.ansible.com/list#/roles/1183)

Usage
-----

Make a checkout of the repo and go to this directory.
**Do not give a different name to the directory**:
it must be called `plone.jenkins_node` otherwise some stuff (roles) cannot be found.

Install the `ansible` command.
For example on Mac:

    brew install ansible

The Python interpreters are installed with [`uv`](https://docs.astral.sh/uv/)
(pre-built binaries, nothing gets compiled), so no extra galaxy roles are needed.

Create an `inventory.yml` mentioning your host:

```
nodes:
  hosts:
    jenkinsnode1.yourserver.io
```

Change `ansible.yml` to refer to your host instead of `localhost`.
Here you can also override role variables like `nvm_node_version`
(see the example playbook above).
It may be better to put `inventory.yml` and `ansible.yml` in a different directory that is not in this repo, or not under source control.

Now you can run the playbook:

    ansible-playbook -i inventory.yml --check ansible.yml

This does not make any changes yet, but may be a good initial test.

Some tasks depend on the changes of earlier tasks (for example the Python
symlinks need the interpreters that uv installs), so at some point `--check`
gives an error and has lost its usefulness.
Then we run the command for real:

    ansible-playbook -i inventory.yml ansible.yml

This can take a long time, with sometimes nothing being reported.
Have patience.

If you need to login with a different remote user, use the `-u` parameter:

    ansible-playbook -i inventory.yml -u <remoteuser> ansible.yml


Manual steps
------------

The robot jobs take far too much memory.
24 GB seems wanted...
If you don't have enough memory on hard machine, you can add swap.
To create a swapfile of 32 GB, as root (or sudo):

```
time dd if=/dev/zero of=/swapfile1 bs=1M count=32768
mkswap /swapfile1
chmod 600 /swapfile1
swapon /swapfile1
echo "/swapfile1 none swap sw 0 0" >> /etc/fstab
```

We need newer node/npm versions to run robotframework tests.
The playbook installs nvm for the jenkins user and sets a default
node version (see the `nvm_version` and `nvm_node_version` role
variables). You can check it like this:

```
sudo su -l jenkins
node --version; npm --version
```

You should have as minimum node version 22.

Register node in Jenkins
------------------------

* Login on jenkins.plone.org.
* Go to https://jenkins.plone.org/manage/computer/
* Click the button to add a new node.
* Give it a name.  Itdoes not have to be NodeX.  No spaces please.
* Select an existing node to copy.
* Change the IP and maybe the Port.


License
-------
GPLv2

Author information
------------------
[Plone](https://plone.org/) Community.
