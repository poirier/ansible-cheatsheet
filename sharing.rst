.. _sharing:

Sharing Ansible files
=====================

Which files can reasonably be shared, and which not? How
should you share them and use them?

Which files
-----------

Roles, tasks, and modules are the Ansible files that
you can easily share. They should not contain anything
specific to a particular environment or deploy. Use
variables where necessary, with reasonable defaults if
possible, to allow a play that uses them to customize
their behavior. Use facts to make them work on a wider
range of host types.

Inventories, playbooks, and group and host variable files
are where the information specific to your environment
or deploy lives. These can't readily be shared.

How to share
------------

The main mechanism in Ansible for re-using files from
elsewhere is the configuration variable :ref:`roles-path`.
This lists other directories that will be searched for
roles directories after the local ``roles`` directory.

How you get the additional roles directories' content
is left up to you. For example, you might just copy the
files there from somewhere else, or network mount a filesystem
containing them, or check them out from some version control
system, etc.

An important thing to consider is controlling changes
to shared files. If you just set things up so you're always
using the latest versions of the files, and someone makes a change
to the shared files that breaks your deploy, then your deploy will
break the next time you try to use it, and you will not be able
to deploy again without fixing your files to be compatible again.

It's much better if your deploy uses a very specific version
of the shared files. Then you can choose when you want to switch
to a newer version of them, test the changes, and always go back
to using a version that you know works if you need to. In other
words, there will always be a way you can get to a working deploy
system if you need to.
