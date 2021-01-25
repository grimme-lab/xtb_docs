.. _setup:

------------------------
 Setup and Installation
------------------------

This guide deal with the general setup and local installation of the ``xtb``
program.

.. contents::


Getting the Program
===================

There are several ways to obtain the ``xtb`` program.


Precompiled Binaries from GitHub
--------------------------------

A precompiled version of the program can be obtained from the
`latest release page <https://github.com/grimme-lab/xtb/releases/latest>`_
on GitHub.
To get the newest features an automatically generated
`development version <https://github.com/grimme-lab/xtb/releases/tag/bleed>`_
is available for the latest commit on the default branch.
At the release page you find the a compressed tarball (``tar xf xtb*.tar.xz``)
usually containing the following content:

.. code-block:: none

   .
   ├── bin
   │   └── xtb
   ├── include
   │   └── xtb
   │       └── xtb.h
   ├── lib
   │   ├── libxtb.so -> libxtb.so.6
   │   ├── libxtb.so.6 -> libxtb.so.6.3.1
   │   ├── libxtb.so.6.3.1
   │   └── pkgconfig
   │       └── xtb.pc
   └── share
       ├── man
       │   ├── man1
       │   │   └── xtb.1
       │   └── man7
       │       └── xcontrol.7
       ├── modules
       │   └── modulefiles
       │       └── xtb
       │           └── 6.3.1
       └── xtb
           ├── param_gfn2-xtb.txt
           ├── param_gfn1-xtb.txt
           ├── param_gfn0-xtb.txt
           ├── param_ipea-xtb.txt
           └── .param_gfnff.xtb

The binary is usually compiled with the Intel Fortran compiler and statically
linked against Intel's Math Kernel Library (Intel MKL).
Newer versions of ``xtb`` (6.2 and newer) additionally include a shared library,
the header specification of the C-API.

First check the version by

.. code:: bash

  > xtb --version
        -----------------------------------------------------------      
       |                   =====================                   |     
       |                           x T B                           |     
       |                   =====================                   |     
       |                         S. Grimme                         |     
       |          Mulliken Center for Theoretical Chemistry        |     
       |                    University of Bonn                     |     
        -----------------------------------------------------------      
  
     * xtb version 6.2.1 (bf8695d) compiled by 'ehlert@majestix' on 2019-10-25
  
  normal termination of xtb

This should print some fancy banner, the version number, say 6.2.1, the
git commit hash, the user and host name of the programmer compiled the
program and at date.
Make sure the version is matching the version from the release page and
the commit hash is the actual commit hash of the release, also the date
of the compilation should match.
We compile the program on our clusters in Bonn, so the host name is usually
``majestix`` (or ``hedy20`` for the old-kernel version).
You get also the user name of the programmer drafting the release,
testing the binary and packing the tarball.

This line is very important when reporting a bug to us, because we can
validate the binary you were using and easier reproduce it.


Installing with Conda
---------------------

.. note::

   To bootstrap a conda installation we recommend to either use
   the conda-forge distribution
   `miniforge <https://github.com/conda-forge/miniforge/releases/latest>`_
   or the anaconda distribution
   `miniconda <https://docs.conda.io/en/latest/miniconda.html>`_.

Installing ``xtb`` from the conda-forge channel can be achieved by adding conda-forge to your channels with:

.. code-block:: none

   conda config --add channels conda-forge

Once the conda-forge channel has been enabled, ``xtb`` can be installed with:

.. code-block:: none

   conda install xtb

It is possible to list all of the versions of ``xtb`` available on your platform with:

.. code-block:: none

   conda search xtb --channel conda-forge

.. note::

   The conda package manager can become quite slow when adding large channels
   like conda-forge, for a more performant alternative you can try to use
   `mamba <https://github.com/thesnakepit/mamba>`_ instead, which can be conveniently
   installed from the conda-forge channel with

   .. code-block:: none

      conda install mamba -c conda-forge
      
   Alternatively, the `mambaforge <https://github.com/conda-forge/miniforge/releases>`_
   installed can be used to bootstrap a conda installation with mamba preinstalled.


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

The ``xtb`` program uses shared memory OpenMP parallelisation, to calculate larger systems
an appropriate OMP stacksize must be provided, chose a reasonable large number by

.. code:: bash

  > export OMP_STACKSIZE=4G
  
.. note::

   Note that the memory requirement will increase with the system size *and* the number
   of requested threads.
   Currently, for 3000 atom system each thread requires around 16GB of memory due to
   the usage of dense matrix algebra for the xTB Hamiltonian.
   You have to account for this by allowing an accordingly larger OpenMP stacksize.

To distribute the number of threads reasonable in the OpenMP section
it is recommended to use

.. code:: bash

  > export OMP_NUM_THREADS=<ncores>,1

You might want to deactivate nested OMP constructs by

.. code:: bash

  > export OMP_MAX_ACTIVE_LEVELS=1

.. tip::
   
   Most OpenMP regions allow to customize the scheduling by setting ``OMP_SCHEDULE``,
   for many threads the ``dynamic`` schedule has proven to give a good load-balance
   between all threads.

The default linear algebra backend of `xtb` is the Math Kernel Library,
which usually comes as OpenMP threaded version and respects the settings
of ``OMP_NUM_THREADS``. You can however still adjust the MKL parallelisation
separately by setting

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


Environment Module
~~~~~~~~~~~~~~~~~~

.. note:: Available since version 6.3.2

A tcl environment module is provided and can be used with usual module systems.
For installations from the tarball the ``prefix`` variable in the module file
has to be adjusted accordingly

.. code-block:: diff

   --- ./share/modules/modulefiles/xtb/6.3.1
   +++ ./share/modules/modulefiles/xtb/6.3.1
   @@ -1,5 +1,5 @@
    #%Module
   -set prefix /
   +set prefix /absolute/path/to/xtb
    
    module-whatis "Semiempirical Extended Tight-Binding Program Package"
    

If the ``share/modules/modulesfiles`` directory is included in your ``MODULEPATH``
you should be able to load ``xtb`` with

.. code-block:: none

   module load xtb

.. important::

   If you plan to use the ``xtb`` shared library in your build system you have
   to do a similar adjustment to the ``lib/pkgconfig/xtb.pc`` file.


Sourceable Shell Scripts
~~~~~~~~~~~~~~~~~~~~~~~~

Example scripts to be sourced in your shells rc file are included in the
distributed tarball:

.. code-block:: bash

   source ./share/xtb/config_env.bash

and should setup all environment variables correctly in most cases.


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
   
.. important:: ``xtb`` version 6.2.3 produces an energy printout which cannot
               be processes by the reader in Orca, to fix this issue, use
               the provided `script`_ to wrap the ``xtb`` binary instead
               of creating a symbolic link.

               .. _script: https://github.com/grimme-lab/xtb/releases/download/v6.2.3/otool_xtb

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
