=================
Setting up QCxMS
=================

.. contents::

Setup
=====

.. note::
   The CID mode describes experimental features that are not yet released. The EI mode is not influenced hereby.
   For this reason, QCEIMS version 4.2 is currently the most recent version available.

Untar the zipped archive:

.. code-block:: text

   > tar -xvzf QCxMS_5.0.tar.gz

The following files are being extracted:

+---------------+----------------------------------------------------------------------------------------------+
| qceims        |  main program (`executable`)                                                                 |
+---------------+----------------------------------------------------------------------------------------------+
| plotms        |  required for the generation of visualized spectra (`executable`)                            |
+---------------+----------------------------------------------------------------------------------------------+
| pqceims       |  script that runs trajectories in parallel locally (`executable`)                            |
+---------------+----------------------------------------------------------------------------------------------+
| q-batch       |  script that runs trajectories on a cluster with a queueing system  (`executable`)           |
+---------------+----------------------------------------------------------------------------------------------+
| getres        |  script that gives the status of the parallel QCEIMS production run (`executable`)           |
+---------------+----------------------------------------------------------------------------------------------+ 
| .mass_raw.agr |  xmgrace template                                                                            |
+---------------+----------------------------------------------------------------------------------------------+
| .XTBPARAM     |  folder involving the xTB-parameterfiles .param_gfn.xtb, .param_gfn2.xtb and .param_ipea.xtb |
+---------------+----------------------------------------------------------------------------------------------+
| example       |  folder hosting an example input and coordinate file.                                        |
+---------------+----------------------------------------------------------------------------------------------+


1. Place the `executables` into your ``$HOME/bin`` directory or path. 
2. Place the `.XTBPARAM` folder and `.mass.arg` file into your `$HOME` directory (these files can appear to be hidden). 

For easy visualization of the calculated spectra we recommend the useage of ``xmgrace``.
Examples for an input file and coordinate file are to be found in the `examples` file.


Quantum Chemistry Codes Required
================================

The low cost QC methods GFN1-xTB and GFN2-xTB are implemented in QCxMS. No further rights or installations of 
third-party software is required and the methods can be called directly by the program. For this reason, 
using GFN2-xTB is set to run as default.

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
| (xTB)     |  Optional. Newer xTB can be called upon if needed.                    |
+-----------+-----------------------------------------------------------------------+

Please contact the development teams of these programs directly for executables since we do not have the 
right to distribute them. 
