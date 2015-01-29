.. Ansible Cheatsheet documentation master file, created by
   sphinx-quickstart on Thu Jan 29 13:59:27 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Ansible Cheatsheet's documentation!
==============================================

Contents:

.. toctree::
   :maxdepth: 2

  index
  configuration
  inventory
  playbook
  task



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`




Ansible Cheatsheet

Configuration file
------------------------

Syntax:  .ini file

See [The Ansible Configuration File](http://docs.ansible.com/intro_configuration.html)

Inventory directory
--------------------------

Default: `/etc/ansible`

Playbook directory
--------------------------

Default: current dir

Inventory file
------------------

Default: /etc/ansible/hosts
Change: env ANSIBLE_HOSTS=<filepath>
Syntax:  .ini file

basically a list of hostnames or IP addresses, one per line. Can include port with `hostname:port` or `address:port`. 

Ranges: Including `[n:m]` in a line will repeat the line for every value from n through m.  m and n can be numbers or letters.

Host variables: Can specify per-host options after hostname on the same line.  E.g.    ` jumper ansible_ssh_port=5555 ansible_ssh_host=192.168.1.50`.  See also *variables files*.

    [groupname]
    another list

Group variables: add `[groupname:vars]` section and put var definitions in it, one per line.  See also *variables files*.

Groups of groups: add `[newgroupname:children]` and put other group names in it, one per line.

Variables
-------------

Variables can be defined globally, per-host, per-group, per-task, per-role, per-playbook, and probably other ways.

Some variables alter the behavior of ansible (see http://docs.ansible.com/intro_inventory.html#list-of-behavioral-inventory-parameters for a list).

Any of them can be used anywhere Jinja templating is in effect.

Variables file
----------------
A YAML file whose content is a single dictionary defining variables

Variables files
-------------------

Ansible will look in *inventory directory* and *playbook directory* for directories named `host_vars` or `group_vars`.  Inside those directories, you can put a single *variables file* with the same name as a host or group (respectively) and Ansible will use those variables definitions. Or you can create a directory with the same name as a host or group and Ansible will use all the files in that directory as variables files.

Ad-hoc command
-------------------------

    ansible *host pattern* -m <module> [options]

e.g.

    $ ansible all -m ping --ask-pass

Shortcut to run a command:

    $ ansible all -a "/bin/echo hello"

<hosts>:

    "all" = all hosts in inventory file


options:  see output of "ansible --help" for now

See [ad-hoc commands](http://docs.ansible.com/intro_adhoc.html)

Host pattern
------------------

See [patterns](http://docs.ansible.com/intro_patterns.html#patterns) for now

Playbook
-------------

Syntax: A YAML file defining a list of *play*s.

Play
------

A dictionary.  Required keys:

* hosts: _text_. One or more *host pattern*s separated by colons
* tasks: list of *task*s

Optional keys:

* handlers: list of *handler*s
* vars: A dictionary defining additional variables
* remote_user: _text_. user to login as remotely
* sudo: yes|no
* sudo_user: _text_. user to sudo to remotely

Task
------

A dictionary. Required keys:

* name: _text_
* _module_ _name_: _module_ _options_

Optional keys that can be used on any task:

* remote_user:  _text_. user to login as remotely
* sudo: yes|no
* sudo_user:  _text_. user to sudo to remotely

Additional keys might be required and optional depending on the module being used.

Handler
-----------

Same syntax as a task, it just gets triggered under different circumstances.

Running a playbook
---------------------------

    $ ansible-playbook <filepath of playbook> [options]
