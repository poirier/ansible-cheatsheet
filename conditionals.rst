Conditionals
============

See http://docs.ansible.com/playbooks_conditionals.html

.. _host-conditionals:

Conditionals with host specs
----------------------------

There are a number of places you can provide a :ref:`host-spec` to
limit which hosts something is applied to.

Ad-hoc command line
~~~~~~~~~~~~~~~~~~~

You must provide a host spec on an :ref:`ansible-command` ad-hoc command.
'Nuf said, we don't really use ad-hoc commands.

--limit option
~~~~~~~~~~~~~~

The ``--limit <hostspec>`` option can be provided on the
:ref:`ansible-command` and :ref:`ansible-playbook` command.

This *does not* say which hosts to apply the command to. It
says *do not apply the command to any host not in this hostspec*,
which is slightly different.

Plays
~~~~~

In a play, the ``hosts`` key is the hostspec to apply the play to.

Remember that a playbook may contain multiple plays. You can use
this to apply different sets of tasks and roles to different hosts
within a single playbook.

Use the ``hosts`` key to limit the hosts that the play's roles
and tasks will be applied to. Assume that the play might be invoked
without previous limiting, e.g. included from a playbook that applies
to more hosts, or used on a command line without giving a limit, perhaps
inadvertently.

To see what hosts would be affected by a playbook before you run it,
you can do this::

    ansible-playbook playbook.yml --list-hosts
