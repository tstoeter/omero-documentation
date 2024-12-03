Installation
------------

.. note:: The |CLI| is currently untested on Windows
    but may be supported in the future.

Since OMERO 5.6, only Python 3 is supported.
We assume that you have already installed Python 3.6 or higher. You can ensure that your python executable is correct with the ``python --version`` command.

We recommend installing client library ``omero-py`` and the |CLI| plugins
in a Python virtual environment.
You can create one using either ``venv`` or ``conda`` (preferred).
If you opt for Conda_, you will need
to install it first, see `miniconda <https://docs.conda.io/en/latest/miniconda.html>`_ for more details.

.. Note:: On Ubuntu 20.04, you may need to install ``libssl-dev`` before installing the |CLI|.

Before installing ``omero-py``, we recommend to install the `Zeroc IcePy 3.6 <https://zeroc.com/ice/downloads/3.6>`_ Python bindings.

Our commercial partner `Glencoe Software <https://www.glencoesoftware.com>`_ has produced several Python wheels to install the Ice-Python bindings depending on the desired Python version and the operating system. Please visit the `Ice binaries for omero <https://www.glencoesoftware.com/blog/2023/12/08/ice-binaries-for-omero.html>`_ page to find the full URLs to the wheels that are used in the examples below.

To install omero-py using pip in a conda environment with Python 3.11 on Ubuntu 22.04::

    conda create -n myenv python=3.11
    conda activate myenv
    pip install https://github.com/glencoesoftware/zeroc-ice-py-linux-x86_64/releases/download/20240202/zeroc_ice-3.6.5-cp311-cp311-manylinux_2_28_x86_64.whl
    pip install omero-py==5.19.5

Alternatively install ``omero-py`` using venv with Python 3.9 or higher::

    python -m venv myenv
    . myenv/bin/activate
    pip install https://github.com/glencoesoftware/zeroc-ice-py-linux-x86_64/releases/download/20240202/zeroc_ice-3.6.5-cp311-cp311-manylinux_2_28_x86_64.whl
    pip install omero-py==5.19.5



The ``omero`` command is now available in the terminal where the environment has been activated::

    omero login

If you install ``omero-py>=5.8.0`` the |CLI| provides all functionalities except the ``import`` functionality.


The ``import`` functionality requires a supported version of :ref:`version requirements java`, and some JARs which are automatically downloaded the first time you do an import.

To install Java, go to :doc:`/sysadmins/unix/server-installation`
and select the walkthrough corresponding to your OS.

omero-py < 5.8.0
^^^^^^^^^^^^^^^^

If you are using an older version of ``omero-py`` you must download the JARs manually and place them under the :envvar:`OMERODIR` directory:

#. download the OMERO.server zip from the `Downloads page <https://www.openmicroscopy.org/omero/downloads/>`_
#. unzip the zip file 
#. set ``$OMERODIR`` to the unzipped directory::

    export OMERODIR=/path/to/OMERO.server-x.x.x-ice36-bxx

The ``import`` functionality is now available::

    omero import /path/to/image.tiff
