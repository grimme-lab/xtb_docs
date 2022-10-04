*****************
Setting up QCxMS
*****************

.. contents:: Table of Contents

Pre-compiled binaries from GitHub
=================================

A pre-compiled executable of the latest program version can be found on the `latest release
<https://github.com/qcxms/QCxMS/releases/latest>`_ page on GitHub.
Follow the instructions to use QCxMS without the need of installing or building anything. 

Untar the zipped archive:

.. code-block:: text

   > tar -xvzf QCxMS.v.X.X.tar.xz

Replace the *X.X* with the current version number. 
The program is designed to run on Linux operating systems.

The following files are being extracted:

+-----------------+----------------------------------------------------------------------------------------------+
| qcxms           |  main program (`executable`)                                                                 |
+-----------------+----------------------------------------------------------------------------------------------+
| pqcxms          |  script that runs trajectories in parallel locally (`executable`)                            |
+-----------------+----------------------------------------------------------------------------------------------+
| q-batch         |  script that runs trajectories on a cluster with a queueing system  (`executable`)           |
+-----------------+----------------------------------------------------------------------------------------------+
| getres          |  script that gives the status of the parallel QCxMS production run (`executable`)            |
+-----------------+----------------------------------------------------------------------------------------------+ 
| `.XTBPARAM`     |  folder involving the xTB-parameterfiles .param_gfn.xtb, .param_gfn2.xtb and .param_ipea.xtb |
+-----------------+----------------------------------------------------------------------------------------------+
| EXAMPLE         |  folder hosting an example input and coordinate file.                                        |
+-----------------+----------------------------------------------------------------------------------------------+


1. Place the `executables` into your ``$HOME/bin`` directory or path. 
2. Place the `.XTBPARAM` folder into your ``$HOME`` directory (these files can appear to be hidden). 


For easy visualization of the calculated spectra, the **PlotMS** program was developed.
An instruction can be found in section :ref:`plotms` and the program can be downloaded at the 
`PlotMS repository <https://github.com/qcxms/PlotMS>`_. 
To display the results, we recommend the useage of ``xmgrace``.
Exemplary input and coordinate files can be found in the `EXAMPLES` folder.


Installing with Conda
=====================

.. note::

   To bootstrap a conda installation we recommend to either use
   the conda-forge distribution
   `miniforge <https://github.com/conda-forge/miniforge/releases/latest>`_
   or the anaconda distribution
   `miniconda <https://docs.conda.io/en/latest/miniconda.html>`_.

Installing ``qcxms`` from the conda-forge channel can be achieved by adding conda-forge to your channels with:

.. code-block:: none

   conda config --add channels conda-forge

Once the conda-forge channel has been enabled, ``qcxms`` can be installed with:

.. code-block:: none

   conda install qcxms

It is possible to list all of the versions of ``qcxms`` available on your platform with:

.. code-block:: none

   conda search qcxms --channel conda-forge

.. note::

   The conda package manager can become quite slow when adding large channels
   like conda-forge, for a more performant alternative you can try to use
   `mamba <https://github.com/thesnakepit/mamba>`_ instead, which can be conveniently
   installed from the conda-forge channel with

   .. code-block:: none

      conda install mamba -c conda-forge

   Alternatively, the `mambaforge <https://github.com/conda-forge/miniforge/releases>`_
   installed can be used to bootstrap a conda installation with mamba preinstalled.



Quantum Chemistry Codes Required
================================

The low cost QC methods **GFN1-xTB** and **GFN2-xTB** are directly implemented into QCxMS `via` the `tblite framework <https://github.com/tblite/tblite>`_. 
With this, no further installations of third-party software is required to create a spectrum at the semi-empirical level of theory.
GFN2-xTB is set to run as default for QCxMS calculations.

To run other QC methods (e.g. DFT), ORCA or TURBOMOLE have to be installed on your system,
either by using ``enviornment modules`` or by placing the executables in `/usr/local/bin`.

+-----------+-----------------------------------------------------------------------+
| MNDO99    |                                                                       |
+-----------+-----------------------------------------------------------------------+
| DFTB+     |  place Slater-Koster parameters `atom-atom.spl` in `/usr/local/dftb+` |
+-----------+-----------------------------------------------------------------------+
| ORCA      |  All orca utilities must also be in your path.                        |
+-----------+-----------------------------------------------------------------------+
| TURBOMOLE |  The ridft and rdgrad programs of Turbomole 7.3 are used.             |
+-----------+-----------------------------------------------------------------------+

Please contact the development teams of these programs directly for executables since we do not have the 
right to distribute them. 
