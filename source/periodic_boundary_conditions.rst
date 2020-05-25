.. _pbc:

------------------------------
 Periodic Boundary Conditions
------------------------------

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
