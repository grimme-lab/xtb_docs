.. _pbc:

------------------------------
 Periodic Boundary Conditions
------------------------------

.. warning:: ``peeq`` is still work in progress!

Periodic xTB calculations are currently not possible with ``xtb``,
instead we ramped up a new code to develop periodic boundary conditions
independent from the main code base.
The periodic electronegativity equilibration (PEEQ) inspired the naming
of this new program package, called ``peeq``.
In contrast to ``xtb`` it cannot be used on its own but relies heavily on
being used by its API (see :ref:`python`). This guide assumes that you
were able to acquire the shared-library and to include it and the wrapper script
to your systems path variables.

.. contents::

Geometry Optimizations
======================

On its own the ``peeq.py`` wrapper is able to perform geometry optimizations
using the preconditioned FIRE optimizer as implemented in the atomic simulation
environment (ASE). We patch the Optimizer-Class to make sure that the convergence
thresholds are tight enough and correspond to normal convergence thresholds
used in ``xtb``.

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

call ``peeq.py`` as follows

.. code:: bash

   > peeq.py POSCAR --optcell --precon --logfile - --trajectory peeqopt.traj
   Initial energy: eV, Eh -456.729799615 -16.78451069
   preconFIRE:   0  14:22:01     -456.729800       0.0000       0.1207
   preconFIRE:   1  14:22:01     -456.731178       0.0000       0.1200
   ...
   preconFIRE:  25  14:22:02     -456.843257       0.0000       0.0023
   preconFIRE:  26  14:22:02     -456.843269       0.0000       0.0018
   preconFIRE:  27  14:22:02     -456.843280       0.0000       0.0013
   Final energy: eV, Eh -456.843279887 -16.7886810131
   > ls
   peeqopt.POSCAR   peeqopt.traj     peeq.out      POSCAR

After the optimization you find the optimized structure in ``peeqopt.POSCAR``
and the details on the last calculation in ``peeq.out``.
The optimization can be viewed by opening the trajectory-file using ``ase gui``.
