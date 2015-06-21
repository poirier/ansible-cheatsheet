.. _organizing:

Organizing hosts, playbooks, etc
================================

There's an extended example in the Ansible docs that
`starts here <http://docs.ansible.com/playbooks_best_practices.html#how-to-differentiate-stage-vs-production>`_.

Here's a shorter recapitulation of it, with my own explanations.

Inventory files
---------------

Make an inventory file for each environment, e.g. `staging` and `production`.
Each inventory file should define the same groups, but of course with different
hosts in them.  (I'm assuming you're not running staging and production
on the same hosts; you'd have to modify this to do that.)

Groups
------

In each inventory file, define groups based on the "roles" the servers
play *and* any other differentiation that might require us to apply
different variables to them. (I use "role" here to mean role in the conventional
sense, not an Ansible role)::

    [webservers-east]
    web1

    [webservers-west]
    web2

    [webservers:children]
    webservers-east
    webservers-west

    [dbservers-east]
    db1

    [dbservers-west]
    db2

    [dbservers:children]
    dbservers-east
    dbservers-west

    [servers-east:children]
    dbservers-east
    webservers-east

    [servers-west:children]
    dbservers-west
    webservers-west

Group variables
---------------

Define variables for groups in group variable files. You might use
these for variables that only make sense for some "roles"::

    ---
    # file: group_vars/webservers
    apacheMaxRequestsPerChild: 3000
    apacheMaxClients: 900

or for variables that differ between machines with the same "role"::

    ---
    # file: group_vars/servers-east
    ntp: ntp-atlanta.example.com
    backup: backup-atlanta.example.com

Default or common values can go in a group variables file too::

    ---
    # file: group_vars/all
    ntp: ntp-boston.example.com
    backup: backup-boston.example.com


Playbooks
---------

Have a top-level playbook, `site.yml`, that includes all the other
playbooks mentioned here::

    ---
    # file: site.yml
    - include: webservers.yml
    - include: dbservers.yml

Then have a playbook for each "role" group::

    ---
    # file: webservers.yml
    - hosts: webservers
      roles:
        - common
        - webtier

and::

    ---
    # file: dbservers.yml
    - hosts: dbservers
      roles:
        - common
        - dbtier

How to use this organization
----------------------------

Update our production webservers::

    ansible-playbook -i production webservers.yml

Update all our staging servers::

    ansible-playbook -i staging site.yml

Update our production webservers, but only in the eastern data center::

    ansible-playbook -i production webservers.yml --limit servers-east

There are a lot more ways to limit things, so you can do things like
a rolling update to a few servers at a time, but you'll have to go read
the Ansible docs for those.

Applying subsets of tasks using tags
------------------------------------

This requires more work up front, but might be useful in some cases.

Add tags to individual tasks using the ``tags`` key, then
use the ``--tags`` option when running ansible to further limit what
gets applied to only the tasks that have the specified tag or tags::

    ansible-playbook -i production site.yml --tags ntp
