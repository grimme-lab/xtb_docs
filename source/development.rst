-----------------------
 Compiling from Source
-----------------------

This guide is intended to help developers to get our programs running.

.. note:: To install our programs refer to :ref:`setup`.
          If you come here from :ref:`setup`, since our instructions have not
          worked for you, why not drop us a hint at the issue tracker?

The ``xtb`` source code is hosted at `GitHub <https://github.com/grimme-lab/xtb>`_.

Building ``xtb``
================

The ``xtb`` program source comes with a ``meson`` build-system
(see `mesonbuild.com <https://mesonbuild.com/index.html>`_ for details).
Despite being a rather young build-system, we decided to commit to the
idea of using it for ``xtb`` due to its simplicity and speed compared
to competing build-systems like Scons or Make.

From version 0.49 ``meson`` implements a decent Fortran support, any
older versions will **fail** to build Fortran code using modern language
features. Since we are requiring a rather new version it is unlikely that
this version is available for most Linux distributions from standard
repositories, in this case install it with

.. code:: bash

   > pip3 install meson [--user]

You will need ``python`` version 3.5 or newer, through.
The ``meson`` backend used for building is usually ``ninja`` 
(see `ninja-build.org <https://ninja-build.org/>`_ for details)
so you should have a version 1.5 or newer on your machine as well.

Linear Algebra Backend
----------------------

Having setup ``meson`` you have to provide versions of the
basic linear algebra subroutines (BLAS) and the linear algebra package (LAPACK).
The most basic solution is to install the development libraries ``libblas-dev``
and ``liblapack-dev`` (Ubuntu 16) and jump ahead.

The recommended solution is to get a version of the math kernel library (MKL)
and link against its version of BLAS and LAPACK, which usually results in
a significant performance gain.

Compilier Choice
----------------

We tested so far different versions of the Intel and GNU compilers.
We found that ``xtb`` can be compiled with

* ``ifort`` and ``icc`` version 17 or newer
* ``gfortran`` and ``gcc`` version 8 or newer

Building with ``meson``
-----------------------

After having setup all the infrastructure run

.. code:: bash

   > export FC=ifort CC=icc
   > meson setup build_intel [-Dla_backend='mkl'] --optimization=2
   > ninja -C build_intel test

For a production build with the Intel compilers and the MKL as backend.
To repeat the build you now only need to call ``ninja``.

To install the ``xtb`` binaries to ``/usr/local`` use (might require ``sudo``)

.. code:: bash

   ninja -C build_intel install

