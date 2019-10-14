.. _binder-docs:

====================
Binder Documentation
====================

.. image:: https://badges.gitter.im/jupyterhub/binder.svg
   :alt: Join the chat at https://gitter.im/jupyterhub/binder
   :target: https://gitter.im/jupyterhub/binder

.. image:: https://img.shields.io/discourse/https/discourse.jupyter.org/users.svg?colorB=blue&label=discourse&style=flat
   :alt: Join our community Discourse page at https://discourse.jupyter.org
   :target: https://discourse.jupyter.org


**mybinder.org** ...

    create custom computing environments that can be shared
    and used by many remote users. 

        Open any public git repository in an executable environment from your browser

        create sharable, interactive, reproducible environments from your public git repository.

        gives you one-click workstations in the cloud

        with one click, launch a workstation in the cloud

        lets you launch customizable environments to execute your code

        is a free notebook environment that you can customize, share, and run entirely in the cloud







For more information about the mybinder.org deployment and the team that runs it, see
:ref:`about`.

This documentation provides information for those who
wish to use a pre-existing BinderHub deployment such as `mybinder.org <https://mybinder.org>`_.
If you'd like documentation on how to create and administer your own BinderHub deployment,
see the `BinderHub documentation <http://binderhub.readthedocs.io>`_, which
guides you through deploying your own BinderHub.

Getting Started with Binder
===========================

If you're just getting started with Binder, see the pages below...

.. toctree::
   :caption: Getting started with Binder
   :maxdepth: 2

   introduction
   using

How-To guides and Tutorials
===========================

The following sections discuss some more in-depth topics on preparing and sharing
your Binder repository. How-To guides are shorter, actionable patterns that
accomplish something specific. Tutorials are more high-level and thorough,
and often cover more conceptual topics.

.. note::

   Binder is a research pilot, whose main goal is to understand usage patterns
   and workloads for future evolution and development. It is not a service that
   can be relied on for critical operations.

.. toctree::
   :caption: How to...
   :maxdepth: 1

   howto/languages
   howto/user_interface
   howto/badges
   howto/repo_data
   howto/lab_workspaces

.. toctree::
   :maxdepth: 2
   :caption: Tutorials and in-depth guides

   tutorials/reproducibility
   tutorials/dockerfile


Sample repositories and configuration
=====================================

The following is a list of sample repositories showing off various things
you can do with Binder configuration files.

.. toctree::
   :maxdepth: 2
   :caption: Sample repositories and configuration

   sample_repos
   config_files

Finally, here is a list of interesting Binder repositories from around the web:

.. toctree::

   examples

See the `Binder Examples <https://github.com/binder-examples>`_ GitHub
organization for more Binder repositories demonstrating its functionality.


The Binder Community
====================

These pages cover more advanced topics in the Binder ecosystem, community
guidelines for users and developers, and information about the ``mybinder.org``
service.

.. toctree::
   :maxdepth: 1
   :caption: Binder community

   user-guidelines
   faq
   status
   about
   reliability
   more-info

.. _citing:

Citing Binder
=============

If you publish work that uses Binder, please consider citing the
`Binder paper from the 2018 SciPy proceedings <http://conference.scipy.org/proceedings/scipy2018/project_jupyter.html>`_!

Here is a citation that you can use:

.. code-block:: none

   Jupyter et al., "Binder 2.0 - Reproducible, Interactive, Sharable
   Environments for Science at Scale." Proceedings of the 17th Python
   in Science Conference. 2018. 10.25080/Majora-4af1f417-011


TODO: Extra content
======

It is powered by `BinderHub <https://github.com/jupyterhub/binderhub>`_,
which is an open-source tool that deploys the Binder service in the cloud.
One-such deployment lives here, at `mybinder.org <https://mybinder.org>`_, and is free to use.
