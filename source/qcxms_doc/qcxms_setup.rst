=================
Setting up QCxMS
=================

.. contents::

Setup
=====

Follow the instructions to use QCxMS without the need of installing or building anything. 

Untar the zipped archive:

.. code-block:: text

   > tar -xvzf QCxMS_v.X.X.tar.xz

Replace the *X.X* with the current version number. The program is  only runs on Linux operating systems.

The following files are being extracted:

+---------------+----------------------------------------------------------------------------------------------+
| qcxms         |  main program (`executable`)                                                                 |
+---------------+----------------------------------------------------------------------------------------------+
| pqcxms        |  script that runs trajectories in parallel locally (`executable`)                            |
+---------------+----------------------------------------------------------------------------------------------+
| q-batch       |  script that runs trajectories on a cluster with a queueing system  (`executable`)           |
+---------------+----------------------------------------------------------------------------------------------+
| getres        |  script that gives the status of the parallel QCEIMS production run (`executable`)           |
+---------------+----------------------------------------------------------------------------------------------+ 
| .mass_raw.agr |  xmgrace template                                                                            |
+---------------+----------------------------------------------------------------------------------------------+
| .XTBPARAM     |  folder involving the xTB-parameterfiles .param_gfn.xtb, .param_gfn2.xtb and .param_ipea.xtb |
+---------------+----------------------------------------------------------------------------------------------+
| EXAMPLE       |  folder hosting an example input and coordinate file.                                        |
+---------------+----------------------------------------------------------------------------------------------+


1. Place the `executables` into your ``$HOME/bin`` directory or path. 
2. Place the `.XTBPARAM` folder and `.mass_raw.arg` file into your `$HOME` directory (these files can appear to be hidden). 


For easy visualization of the calculated spectra, the :ref:`PlotMS` program was developed. It can be found at the `PlotMS repository <https://github.com/qcxms/PlotMS>`_. 
To display the results, we recommend the useage of ``xmgrace``.
Exemplary input and coordinate files can be found in the `EXAMPLES` folder.


Quantum Chemistry Codes Required
================================

The low cost QC methods **GFN1-xTB** and **GFN2-xTB** are directly implemented into QCxMS. No further rights or installations of 
third-party software is required and the methods can be called directly by the program. GFN2-xTB is set to run as default for
QCxMS calculations.

To run other QC methods, QCxMS needs ORCA or TURBOMOLE to function. 
If available, place the following QC executables in `/usr/local/bin`:

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


Building the program 
====================

.. note::
The source code is provided on the official `QCxMS GitHub page <https://github.com/qcxms/QCxMS>`_. 
Download the source code and untar the compressed source archive:

.. code::

   tar -xvzf QCxMS-v.X.X.tar.gz

Enter the newly created folder. All source files can be found in the `qcxms` folder.
To build the program, run the following commands:

.. code-block:: 

   cd QCxMS/qcxms/
   
The program uses [meson](https://mesonbuild.com/) (version 0.57.2 and higher) and [ninja](https://ninja-build.org/) (version 1.10 and higher) as building system. 
To build the program, use the following commands:

.. code:: 
  export FC=ifort CC=icc
  meson setup build -Dfortran_link_args=-static
  ninja -C build 

This will build a static linked binary in the `build` folder. Copy the binary from `build/qcxms` file into a directory in your path, e.g. `~/bin/`.

