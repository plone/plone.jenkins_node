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

Usage
-----

Make a checkout of the repo and go to this directory.
**Do not give a different name to the directory**:
it must be called `plone.jenkins_node` otherwise some stuff (roles) cannot be found.

Install the `ansible-galaxy` command.
For example on Mac:

    brew install ansible

You need to explicitly install some required galaxy roles (packages):

    ansible-galaxy install -r roles.yml

Currently this installs [`gforcada/ansible-compile-python`](https://github.com/gforcada/ansible-compile-python).
We will want to switch to installing Pythons with `uv`, but for now this gets the job done.

Please take a note of where this role gets installed.
In my case it was in the parent directory.
If you need to fix anything in the role, that is where you can edit things.

Create an `inventory.yml` mentioning your host:

```
nodes:
  hosts:
    jenkinsnode1.yourserver.io
```

Change `ansible.yml` to refer to your host instead of `localhost`.
It may be better to put `inventory.yml` and `ansible.yml` in a different directory that is not in this repo, or not under source control.

Now you can run the playbook:

    ansible-playbook -i inventory.yml --check ansible.yml

This does not make any changes yet, but may be a good initial test.

After a while it may give an error like this:

    Source '/tmp/py3.9.23.tar.xz' does not exist

That just means that the `--check` option has lost its usefulness, so we run the command for real:

    ansible-playbook -i inventory.yml ansible.yml

This can take a long time, with sometimes nothing being reported.
Have patience.

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

Also, we need newer node/npm versions to run robotframework tests.
This should go in to ansible, but for now we will use nvm.
Check https://github.com/nvm-sh/nvm for the latest nvm version.

```
sudo su -l jenkins
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

Close and reopen the terminal so nvm is fully available.
Then install newer versions, again as user jenkins:

```
nvm install --lts && nvm use --lts
node --version; npm --version; yarn --version
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
