.. _pbc:

------------------------------
 Periodic Boundary Conditions
------------------------------

Periodic xTB calculations are either possible with the standalone ``xtb``
program or the atomic simulation environment (ASE) using the ``libxtb.so``
and the C-API (see :ref:`python`).

``xtb`` is supposed to throw a lot of *feature-not-implemented* errors at
you as you try out the very recently added periodic boundary conditions.
You can make use of the C-API and the ASE calculator class to get around this
errors for now, but we promise to add more features and runtypes in the future.

.. contents::

Input Formats
=============

We support Turbomole's coordinate files in a ``riper`` compatible format.
It requires to have the ``$periodic`` information, one of ``$lattice`` or ``$cell``
and, of course, a ``$coord`` data group present.
In contrast to Turbomole we want them all in one file,
so the ``file=<elsewhere>`` does not work with ``xtb``.

A valid input for diamond is

.. code-block:: none

   $periodic 3
   $cell angs
       3.570    3.570    3.570  90  90  90
   $coord angs
     0.00000  0.00000  0.00000  C
     0.89250  0.89250  0.89250  C
     1.78500  1.78500  0.00000  C
     2.67750  2.67750  0.89250  C
     1.78500  0.00000  1.78500  C
     2.67750  0.89250  2.67750  C
     0.00000  1.78500  1.78500  C
     0.89250  2.67750  2.67750  C
   $end

or using different keywords and order like for calciumfluoride here

.. code-block:: text

   $coord frac
       0.250000000     0.250000000     0.250000000     f
       0.750000000     0.750000000     0.750000000     f
       0.000000000     0.000000000     0.000000000     ca
   $lattice angs
       3.153833580     1.115048556     1.931320751
       0.000000000     3.345145667     1.931320751
       0.000000000     0.000000000     3.862641503
   $periodic 3
   $end

.. note:: we do not care if you ``$end`` your file or maybe even all your
          data groups, since the parser politely ignores its presence.

While this format in principle is able to specify also 1D and 2D periodic
systems ``xtb`` does not support them right now.

Both Vasp 4 and Vasp 5 POSCAR and CONTCAR files are supported, but we
require to have the information on the atomtypes present in the file.
For details on the format refer to the documentation of Vasp.

.. tip:: You can use ``ase convert`` to bring your ``cif`` or ``fort.34`` files
         into Vasp format, as ``xtb`` currently cannot read them.

Geometry Optimizations
======================

.. note:: feature implemented in version 6.2

To perform geometry optimizations with ``xtb`` on periodic systems we
recommend to use an input file like

.. code-block:: none

   $opt
      engine=inertial
   $end

Since the ANC optimizers do not support periodic boundary conditions right now,
use the inertial relaxation procedure instead.

The optimization log is written in Vasp 5 POSCAR format and contains the
current energy and gradient norm in the first (comment) line.

With the Atomic Simulation Environment
--------------------------------------

.. note:: This guide assumes that you were able to acquire the shared-library
          and to include it and the wrapper script to your systems path variables.

On its own the ``xtb.py`` wrapper is able to perform geometry optimizations
using the preconditioned FIRE optimizer as implemented in the atomic simulation
environment (ASE). We patch the Optimizer-Class to make sure that the convergence
thresholds are tight enough and correspond to normal convergence thresholds
used in the ``xtb`` standalone.

To start a geometry optimization from a POSCAR like

.. code:: text

   C 
   1.0
       3.570    0.000    0.000
       0.000    3.570    0.000
       0.000    0.000    3.570
   8
   Cartesian
     0.00000  0.00000  0.00000
     0.89250  0.89250  0.89250
     1.78500  1.78500  0.00000
     2.67750  2.67750  0.89250
     1.78500  0.00000  1.78500
     2.67750  0.89250  2.67750
     0.00000  1.78500  1.78500
     0.89250  2.67750  2.67750

call ``xtb.py`` as follows

.. code:: bash

   > xtb.py POSCAR --optcell --precon --logfile - --trajectory peeqopt.traj
   Initial energy: eV, Eh -456.729799615 -16.78451069
   preconFIRE:   0  14:22:01     -456.729800       0.0000       0.1207
   preconFIRE:   1  14:22:01     -456.731178       0.0000       0.1200
   ...
   preconFIRE:  25  14:22:02     -456.843257       0.0000       0.0023
   preconFIRE:  26  14:22:02     -456.843269       0.0000       0.0018
   preconFIRE:  27  14:22:02     -456.843280       0.0000       0.0013
   Final energy: eV, Eh -456.843279887 -16.7886810131
   > ls
   xtbopt.POSCAR    xtbopt.traj      xtb.out       POSCAR

After the optimization you find the optimized structure in ``xtbopt.POSCAR``
and the details on the last calculation in ``xtb.out``.
The optimization can be viewed by opening the trajectory-file using ``ase gui``.
