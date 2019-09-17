.. _setup:

------------------------
 Setup and Installation
------------------------

This guide deal with the general setup and local installation of the ``xtb``
program.

.. contents::

Getting the Program
===================

The `xtb <https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/xtb/>`_ program
is available for academic use free of charge on request
from Stefan Grimme at the `xtb-mailing list <xtb@thch.uni-bonn.de>`_.
It usually comes as a tarball with following content

.. code-block:: none

  bin/xtb
  lib/libxtb.so     -> libxtb.so.6
  lib/libxtb.so.6   -> libxtb.so.6.2
  lib/libxtb.so.6.2
  include/xtb.h
  python/xtb.py
  .xtbrc
  Config_xtb_env.bash
  Config_xtb_env.csh
  man/xtb.1.html
  man/xtb.1.pdf
  man/man1/xtb.1
  man/xcontrol.7.html
  man/xcontrol.7.pdf
  man/man7/xcontrol.7
  info/RELEASE_NOTES.html
  info/RELEASE_NOTES.pdf

The binary is usually compiled with the Intel Fortran compiler and statically
linked against Intel's Math Kernel Library (Intel MKL).
Newer versions of ``xtb`` (6.2 and newer) additionally include a shared library,
the header specification of the C-API and a Python wrapper to use the API
within the Atomic Simulation Environment (`ASE`_).

.. _ASE: https://wiki.fysik.dtu.dk/ase/

First check the version by

.. code:: bash

  > xtb --version

This should print some fancy banner, the version number, say 6.1 RC1, the
last programmer worked on the project (usually ``SAW``, meaning Sebastian Ehlert)
and the date the program was last compiled and tested by this programmer,
as ``YYMMDD``.

Setting up ``xtb``
==================

This section will give you the basic information you need to
know about the ``xtb`` program. Some of the steps are elemental
for your calculation to succeed, so please consider to follow
my instructions carefully.

Some part of the ``xtb`` program can be quite wasteful with stack memory,
to avoid stack overflows when calculating large molecules, you should
unlimit the system stack, *e.g.* with ``bash`` by

.. code:: bash

  > ulimit -s unlimited

Note that the memory management of ``xtb`` is constantly improved to avoid
using large amounts of stack memory, but to be on the save side
include this option for production runs.

Parallelisation
---------------

The ``xtb`` program uses OMP parallelisation, to calculate larger systems
an appropriate OMP stacksize must be provided, chose a reasonable large number by

.. code:: bash

  > export OMP_STACKSIZE=1G

To distribute the number of threads reasonable in the OMP section
it is recommended to use

.. code:: bash

  > export OMP_NUM_THREADS=<ncores>,1

You might want to deactivate nested OMP constructs by

.. code:: bash

  > export OMP_MAX_ACTIVE_LEVELS=1
  
The default linear algebra backend of `xtb` is the Math Kernel Library,
to make the linear algebra run in parallel export

.. code:: bash

  > export MKL_NUM_THREADS=<ncores>

Environment Variables for ``xtb``
---------------------------------

A number of environment variables is used by ``xtb`` to perform calculations.
Please set the ``XTBPATH`` variable to include all locations were
you store information relevant for your ``xtb`` calculation, like configuration
files and parameter files.
The present working directory is implicitly included for most files that
are searched in the ``XTBPATH``.

The old ``XTBHOME`` variable is used if you have not set the ``XTBPATH``
variable and is used in the same manner. ``xtb`` will print the values
of ``XTBPATH`` and ``XTBHOME`` at the beginning of each calculation
if set to verbose mode.

An easy way to setup the environment variables is to use the distributed ``Config_xtb_env``.
For a ``bash`` shell this might be done locally for one session by sourcing the
``Config_xtb_env.bash`` script. To use this setup in every session include

.. code:: bash

   source $XTBHOME/Config_xtb_env.bash

in your ``.bashrc`` (requires that ``XTBHOME`` is set to the appropiate directory).

Configuration Script
--------------------

The “configuration” scripts ``Config_xtb_env.*`` hardly deserve to be called
that way, in fact they contains the lines you would manually write to your
``.bashrc`` or ``.cshrc`` if you would “install” ``xtb`` locally by hand.
If you prefer to do it by hand or differently, just ignore the script.

Just take a look into one, there is some neat trick included found in
a Turbomole “configuration” script to find the location of the script
and the most probable location of the content of the tarball, but that's it.
Here is the contents of the one shipped with 6.2 for quick reference:

.. code:: bash

   #!/bin/bash
   # run this script to set up a xtb environment
   # requirements: $XTBHOME is set to `pwd`
   if [ -z "${XTBHOME}" ]; then
      XTBHOME="$(cd -P "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
   fi

   # set up path for xtb, using the xtb directory and the users home directory
   XTBPATH=${XTBHOME}:${HOME}

   # to include the documentation we include our man pages in the users manpath
   MANPATH=${MANPATH}:${XTBHOME}/man

   # finally we have to make the binaries and scripts accessable
   PATH=${PATH}:${XTBHOME}/bin:${XTBHOME}/python
   LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${XTBHOME}/lib
   PYTHONPATH=${PYTHONPATH}:${XTBHOME}/python

   export PATH XTBPATH MANPATH LD_LIBRARY_PATH PYTHONPATH

It will set ``XTBHOME`` to the location of the script if you have not
set it already and just assumes that ``XTBHOME`` contains the content
of shipped tarball, then it will append the directories ``bin/`` and ``python/``
to your ``PATH`` variable, ``man/`` to your ``MANPATH``,
``lib/`` to your ``LD_LIBRARY_PATH`` and ``python/`` to your ``PYTHONPATH``.

Getting Help from ``xtb``
=========================

Beside this manual you can check the in-program help by

.. code:: bash

  > xtb --help

Unfortunately, this might be outdated,
therefore, you should refer to the man-pages distributed with the ``xtb`` program.
Please check for the man-pages of ``xtb(1)`` and ``xcontrol(7)``.
There is also an online documentation, but you already now that one, of course.

The Verbose Mode
----------------

If you think some information is missing in your calculation you can
switch to the verbose mode by using ``--verbose`` in the command line
arguments. This will increase the print level almost everywhere in the
``xtb`` program, also the input parser will print a lot of information
that might be interesting for your current calculation.

Overall this can be an awful lot of information, so it is not recommended
as a default option.

Using xTB with Orca
===================

Orca 4.2 implements support for xTB calculations using an IO based interface
calling the ``xtb`` binary and parsing its output.

The binaries of Orca will call an executable called ``otool_xtb``, which
should be placed in the directory containing the Orca binaries.
We recommend to create a symbolic link to your local ``xtb`` binary by

.. code-block:: bash

   > ln -s $(which xtb) otool_xtb

You can invoke xTB calculations in Orca by using one of the simple keywords

.. code-block:: none

   ! XTB1 # for GFN1-xTB
   ! XTB2 # for GFN2-xTB

in your Orca input file, for more details refer to the Orca manual.

Orca will communicate with ``xtb`` mainly by using commandline arguments,
requesting singlepoint calculations and parsing the total energy and
gradient from the program output.

Of course you should setup the ``xtb`` related environment variables,
such that ``xtb`` can find its parameter files and configuration files.
The ``.xtbrc`` is still read if it is contained in ``XTBPATH`` and can
be used to change the behaviour of xTB calculations in Orca, *e.g.* for
setting the electronic temperature.
