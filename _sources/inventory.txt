Inventory
=========

.. _inventory-directory:

Inventory directory
--------------------------

Default
    ``/etc/ansible``

.. _picking-inventory:

Picking an inventory file
-------------------------

Different ways::

    ansible-playbook -i <inventoryfile> ...
    

.. _inventory-file:

Inventory file
------------------

Default
    ``/etc/ansible/hosts``

Change
    ``env ANSIBLE_HOSTS=<filepath>``

Syntax
    .ini file

basically a list of hostnames or IP addresses, one per line. Can
include port with ``hostname:port`` or ``address:port``.

Ranges: Including ``[n:m]`` in a line will repeat the line for every
value from n through m.  m and n can be numbers or letters.

Host :ref:`variables`: Can specify per-host options after hostname on the
same line.  E.g.  ``jumper ansible_ssh_port=5555
ansible_ssh_host=192.168.1.50``.  See also :ref:`variables-files`.

Group :ref:`variables`: add ``[groupname:vars]`` section and put var definitions in it, one per line.  See also :ref:`variables-files`.

Groups of groups: add ``[newgroupname:children]`` and put other group names in it, one per line.

