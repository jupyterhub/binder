.. _dockerfile:

Use a Dockerfile for your Binder repository
===========================================

Binder supports configuration files for package
installation, environment specification, post-build shell scripts, and more.
It should be possible to create the environment that you want *without*
using a Dockerfile. For more information about the different environment
configuration files that Binder can create, see
:ref:`preparing_repositories`.

However, in case you cannot meet all your needs with these configuration
files, it is also possible to use a Dockerfile to define your environment.
This is considered an **advanced use case**, and we cannot guarantee that the
Dockerfile will work.

This guide will help you in preparing your Dockerfile so that it has the
components needed to run JupyterHub, allowing it to work on Binder
deployments.

.. important::

   We recommend against using a ``Dockerfile`` as a way to make your repository
   usable with binder. Use them as a last resort after all methods in
   :ref:`preparing_repositories` have failed.

.. note::

  Binder's requirements for Dockerfiles are in beta and subject to change.
  Dockerfiles may break on Binder from time to time during the beta period.


When should you use a Dockerfile?
---------------------------------

Below are a few use-cases where you *might* want to use a Dockerfile with
Binder.

When you must inherit from a popular Docker image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to use a pre-existing Docker image, you may source it in your
Dockerfile. For example, this code sources the Jupyter-Scipy notebook:

.. code-block:: Dockerfile

    # Note that there must be a tag
    FROM jupyter/scipy-notebook:cf6258237ff9

See `Preparing your Dockerfile`_ for instructions on how to
do this properly.

When you are building complex software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most Binder configurations can be achieved without a Dockerfile.
Before resorting to a Dockerfile, we recommend trying to use ``postBuild``
commands for configuration.  See
`the repo2docker documentation <http://repo2docker.readthedocs.io/en/latest/>`_
for examples.

When you're using a language that is not directly supported
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Binder supports many languages, but not all of them. If your need to use
a different language, it may be possible to accomplish this with a Dockerfile.

For a list of languages that Binder supports with configuration files, see
`the repo2docker documentation <http://repo2docker.readthedocs.io/en/latest/>`_.

.. note::

   We welcome contributions to ``repo2docker`` to add support for new
   languages. If interested, please
   `open an issue <https://github.com/jupyter/repo2docker/issues>`_.


Preparing your Dockerfile
-------------------------

For a Dockerfile to work on Binder, it must meet the following requirements:

1. It must install a recent version of Jupyter Notebook.
   This should be installed via ``pip`` with the `notebook` package.
   So in your dockerfile, you should have a command such as:

   .. code-block:: Dockerfile

       RUN pip install --no-cache-dir notebook==5.*

2. It must explicitly specify a tag in the image you source.

   When sourcing a pre-existing Docker image with ``FROM``,
   **a tag is required**. The tag *cannot* be ``latest``. Note that tag
   naming conventions differ between images, so we recommend using
   the SHA tag of the image.

   Here's an example of a Dockerfile ``FROM`` statement that would work.

   .. code-block:: Dockerfile

      FROM jupyter/scipy-notebook:cf6258237ff9

   .. note::

       The following examples would **not** work:

       .. code-block:: Dockerfile

          FROM jupyter/scipy-notebook

       or

       .. code-block:: Dockerfile

          FROM jupyter/scipy-notebook:latest

3. It must set up a user whose uid is `1000`.
   It is bad practice to run processes in containers as root, and on binder
   we do not allow root container processes. If you are using an ubuntu or
   debian based container image, you can create a user easily with the following
   directives somewhere in your Dockerfile:

   .. code-block:: Dockerfile

      ARG NB_USER=jovyan
      ARG NB_UID=1000
      ENV USER ${NB_USER}
      ENV NB_UID ${NB_UID}
      ENV HOME /home/${NB_USER}

      RUN adduser --disabled-password \
          --gecos "Default user" \
          --uid ${NB_UID} \
          ${NB_USER}

   This is the user that will be running the jupyter notebook process
   when your repo is launched with binder. So any files you would like to
   be writeable by the launched binder notebook should be owned by this user.

4. It must copy its contents to the ``$HOME`` directory and change permissions.

   To make sure that your repository contents are available to users,
   you must copy all contents to ``$HOME`` and then make this folder
   owned by the user you created in step 3. If you used the snippet provided
   in step 3, you can accomplish this copying with the following snippet:

   .. code-block:: Dockerfile

       # Make sure the contents of our repo are in ${HOME}
       COPY . ${HOME}
       USER root
       RUN chown -R ${NB_UID} ${HOME}
       USER ${NB_USER}

   This chown is required because Docker will be default
   set the owner to ``root``, which would prevent users from editing files. Note that the repository 
   should in general be clone with ``COPY``; although ``RUN git clone ...`` is a valid command for the
   ``Dockerfile``, it does not invalidate the build cache of mybinder. Thus, if available, the the cached
   repository will be used even after changes to the repository. 

5. It must accept command arguments. The Dockerfile will effectively be launched as:

   .. code-block:: sh

      docker run --security-opt=no-new-privileges:true <image> jupyter-notebook --ip=0.0.0.0 --port=8888 <arguments from the mybinder launcher>

   where `<arguments ...>` includes details of little relevance for local debugging such as:
   
   .. code-block:: sh
   
      --NotebookApp.base_url=/user/some-partially-random-path/
      --NotebookApp.token=some-random-token
      --NotebookApp.trust_xheaders=True
      --NotebookApp.allow_origin=*
      --NotebookApp.allow_origin_pat=.*

   If your Dockerfile sets or inherits the Docker `ENTRYPOINT` instruction, the program
   specified as the `ENTRYPOINT` *must* `exec` the arguments passed by docker. Inherited
   Dockerfiles may unset the entrypoint with `ENTRYPOINT []`.

   For more information, and a shell wrapper example, please see the `Dockerfile best practices: ENTRYPOINT  <https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#entrypoint>`_ documentation.

Ensuring reproducibility with Dockerfiles
-----------------------------------------

Ensuring that your Binder environment is reproducible requires extra
considerations when using a Dockerfile. This section provides some guidelines
for making sure your Binder environment does not change in unexpected ways.

As mentioned above, make sure that you source your Dockerfile from a **tag**
of another image. This ensures that you will continue building off of
the same image even if the image is updated to a new version.

Next, make sure that all packages installed with your Dockerfile
are pinned to specific versions. You should do this with the image you are
sourcing as well.
