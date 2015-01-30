.. Ansible Cheatsheet documentation master file, created by
   sphinx-quickstart on Thu Jan 29 13:59:27 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Ansible Cheatsheet
==================

Contents:

.. toctree::

  configuration
  inventory
  playbook
  role
  task
  variables
  conditionals
  loops


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. _ad-hoc-command:

Ad-hoc command
-------------------------

    ansible :ref:`host-pattern` -m <module> [options]

e.g.

    $ ansible all -m ping --ask-pass

Shortcut to run a command:

    $ ansible all -a "/bin/echo hello"

options:  see output of "ansible --help" for now

See `ad-hoc commands <http://docs.ansible.com/intro_adhoc.html>`_.

.. _host-pattern:

Host pattern
------------------

See [patterns](http://docs.ansible.com/intro_patterns.html#patterns) for now

<hosts>:

    "all" = all hosts in inventory file
