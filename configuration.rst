Configuration
=============

.. _configuration-file:

Configuration file
------------------------

Syntax
    .ini file

See `The Ansible Configuration File <http://docs.ansible.com/intro_configuration.html>`_.

Ansible uses the first config file it finds on this list:
    * ANSIBLE_CONFIG (an environment variable)
    * ansible.cfg (in the current directory)
    * .ansible.cfg (in the home directory)
    * /etc/ansible/ansible.cfg

 
Some useful vars in the ``[defaults]`` section:

.. _hostfile:

hostfile or inventory
.....................

    Hostfile is the older, deprecated name; use `inventory` in
    new configurations with Ansible 1.9 and later.

    `This <http://docs.ansible.com/intro_configuration.html#inventory-file>`_
    is the default location of the inventory file, script, or directory
    that Ansible will use to determine what hosts it has available to talk to:

    inventory = /etc/ansible/hosts

    You can also specify this on the ansible-playbook command line
    with `-i`.

.. _roles-path:

roles_path
..........
    The `roles path <http://docs.ansible.com/intro_configuration.html#roles-path>`_
    setting indicates additional directories beyond the ‘roles/’ subdirectory
    of a playbook project to search to find Ansible roles. For instance, if
    there was a source control repository of common roles and a different
    repository of playbooks, you might choose to establish a convention to
    checkout roles in /opt/mysite/roles like so:

        roles_path = /opt/mysite/roles

    Additional paths can be provided separated by colon characters,
    in the same way as other pathstrings:

        roles_path = /opt/mysite/roles:/opt/othersite/roles

    Roles will be first searched for in the playbook directory. Should
    a role not be found, it will indicate all the possible paths that
    were searched.
