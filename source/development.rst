-----------------------
 Compiling from Source
-----------------------

This guide is intended to help developers to get our programs running.

.. note:: To install our programs refer to :ref:`setup`.
          If you come here from :ref:`setup`, since our instructions have not
          worked for you, why not drop us a hint at the issue tracker?

The ``xtb`` source code is hosted at `GitHub <https://github.com/grimme-lab/xtb>`_.

.. contents::


Building with meson
===================

The ``xtb`` program source comes with a meson build-system
(see `mesonbuild.com <https://mesonbuild.com/index.html>`_ for details).
Despite being a rather young build-system, we decided to commit to the
idea of using it for ``xtb`` due to its simplicity and speed compared
to competing build-systems like Scons or Make.

To build ``xtb`` from the source the `meson build system <https://mesonbuild.com>`_ can be used.
For a decent Fortran support verson 0.51 of meson or newer is required,
additionally the default backend `ninja <https://ninja-build.org/>`_ is required with version 1.7 or newer.

Getting meson
-------------

To install the meson build system first check your package manager for an up-to-date meson version,
usually this will also install ninja as dependency.
Alternatively you can install the latest version of meson and ninja with ``pip`` (or ``pip3`` depending on your system):

.. code-block:: none

   pip install meson ninja [--user]

If you prefer ``conda`` as a package manage you can install meson and ninja from the conda-forge channel.
Make sure to select the conda-forge channel for searching packages.

.. code-block:: none

   conda config --add channels conda-forge
   conda install meson ninja

Configure Intel Fortran build with MKL
--------------------------------------

The recommended build for ``xtb`` is with Intel Parallel Studio using the Intel Fortran compiler and the Math Kernel Library as default backend.
Precompiled, statically linked ``xtb`` binaries for Linux are provided at the `release page <https://github.com/grimme-lab/xtb/releases/latest>`_.
The setup for the linear algebra backend defaults to MKL, therefore, only the compilers have to exported before configuring the build:

.. code-block::

   export FC=ifort CC=icc
   meson setup build --buildtype=release

After the configuration step the build can be performed with ninja:

.. code-block::

   ninja -C build

Note, ninja will by default use all the threads available on your system.

.. note::

   If you share the build machine with others it might be helpful to reduce the number of concurrent jobs using the ``-j`` flag.

By default the binary will be linked statically, other supported backends are:

============ ======================================
 backend      linked against
============ ======================================
 mkl-static   static MKL (default)
 mkl          shared MKL
 mkl-rt       MKL real time library
 openblas     OpenBLAS and if required LAPACK
 netlib       BLAS and LAPACK
 custom       ``-Dcustom_libraries=...``
============ ======================================

.. note::

   If you are using the MKL provided by conda-forge you have to link against the netlib backend


Configure GCC build with OpenBLAS
---------------------------------

``xtb`` can also be compiled with GCC version 8 or later.
For this example we additonally choose to change the linear algebra backend to OpenBLAS, if you have Intel Parallel Studio installed, you can leave out the last argument to get the MKL backend.

.. code-block::

   export FC=gfortran CC=gcc
   meson setup build --buildtype=release -Dla_backend=openblas

The build system will check if the OpenBLAS library provides LAPACK features as well, if this is not the case it will additionally search for LAPACK.
If you are compiling ``xtb`` on Darwin platforms, ensure that GCC is the actual GCC and not clang.
The build can be performed just like before:

.. code-block::

   ninja -C build


Testing the build with meson
----------------------------

After successfully building the `xtb` program ensure that it is working as expected.
Run the testsuite with

.. code-block::

   ninja -C build test

All tests should pass, otherwise `open an issue <https://github.com/grimme-lab/xtb/issues/new/choose>`_.


Installing with meson
---------------------

To use ``xtb`` in production or to pack a release with precompiled binaries the project should be installed with ninja.
The installation prefix defaults to ``/usr/local`` on Linux systems, you might want to adjust this first by configuring your build with

.. code-block:: none

   meson configure build --prefix=$HOME/.local

To perform the actual installation run

.. code-block:: none

   ninja -C build install

Depending on the installation prefix and your user rights ninja might ask for the ``root`` access to perform the installation.
