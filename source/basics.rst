.. _quickstart:

----------------------------
 Quickstart into Production
----------------------------

This chapter should serve as a quickstart tutorial guiding you through your first
calculation employing the xTB methods. 
As an example, the equilibrium geometry of a water molecule is calculated.
The description here is based on ``xtb`` version 6.2.2.

.. contents::


.. note:: The program can almost entirely controlled by the command-line, if you
          need more control you should resort to the :ref:`detailed-input` file.

There are four main run types in ``xtb``, most other run types are
composite types that try to provide convenient combinations from
those main run types.

Singlepoint Calculations
========================

Independent of all other commands, there will always be a singlepoint
calculation carried out at the very beginning. To calculate something
``xtb`` needs information about the molecular geometry and the kind
of atoms present.

The default input format is either the Turbomole coordinate file
as a ``$coord`` data group.

.. code:: bash

  > cat coord
  $coord
   0.00000000000000      0.00000000000000     -0.73578586109551      o
   1.44183152868459      0.00000000000000      0.36789293054775      h
  -1.44183152868459      0.00000000000000      0.36789293054775      h
  $end

  > xtb coord

Like in Turbomole the coordinates can be given in Bohr (default)
or Ångström (``$coord angs``) or fractional coordinates (``$coord frac``).
Optional data groups like the systems periodicity (``$periodic``),
lattice parameters (``$lattice [bohr|angs]``) and cell parameters
(``$cell [bohr|angs]``) can be provided as well.

The following input for a MgO crystal utilize this data groups:

.. code:: text

   $coord frac
       0.00      0.00      0.00      mg
       0.50      0.50      0.50      o
   $periodic 3
   $cell
    5.798338236 5.798338236 5.798338236 60. 60. 60.
   $end

.. note:: In previous version the coord data group had to start in the
          first line, this constraint has been relaxed by now.

Any valid xyz file can be used to provide coordinates and requires the
file extension ``.xyz``. This format does not support periodic boundary conditions.

``xtb`` also supports DFTB+ genFormat files (``.gen``), protein database files (``.pdb``)
mol-files (``.mol``) and structure-data files (``.sdf``), if the corresponding suffix
is encountered. Note that you cannot provide a mol file with the extension sdf.
Vasp's POSCAR or CONTCAR files are read if the file is named as such.

By default ``xtb`` will search for ``.CHRG`` and ``.UHF`` files and obtain
from these the molecular charge and the number of unpaired electrons,
respectively. The molecular charge can also be specified by

.. code:: sh

  > xtb molecule.xyz --chrg +1

which is equivalent to

.. code:: sh

  > echo +1 > .CHRG && xtb molecule.xyz


This also works for the unpaired electrons as in

.. code:: sh

  > xtb --uhf 2 input.sdf

Note that the position of the input coordinates is totally unaffected
by any command-line arguments, if you are not sure, whether ``xtb`` tries
to interpret your filename as flag use ``--`` to stop the parsing
as command-line options for all following arguments.

.. code:: sh

  > xtb -- -oh.xyz

To select the parametrization of the xTB method you can currently choose
from three different geometry, frequency and non-covalent interactions (GFN)
parametrization, which differ mostly in the cost--accuracy ratio,

.. code:: sh

  > xtb --gfn 2 coord

to choose GFN2-xTB, which is also the default parametrization. Also
available are GFN1-xTB, and GFN0-xTB.

Sometimes you might face difficulties converging the self consistent
charge iterations, in this case it is usually a good idea to increase
the electronic temperature and to restart at normal temperature

.. code:: sh

  > xtb --etemp 1000.0 coord && xtb --restart coord

Geometry Optimizations
======================

The main purpose of the xTB methods is to provide good geometries,
so the ``xtb`` comes with a build-in geometry optimizer, which usually
does a decent job. It is invoked by

.. code:: bash

  > xtb coord --opt
  > ls
  coord   xtbopt.coord   xtbopt.log   ...

The optimized coordinates is written to a new file (``xtbopt.coord``), which is
in the same format as the input geometry. You can view the geometry optimization
by opening the ``xtbopt.log`` with your favorite molecule viewer.
The log-file is in Xmol format and contains the current total energy
and the gradient norm in the comment line, ``gmolden`` usually works fine
for this.

A successful geometry optimization will print somewhere along the lines

.. code::

     *** GEOMETRY OPTIMIZATION CONVERGED AFTER 43 ITERATIONS ***

   ------------------------------------------------------------------------
    total energy gain :          -0.0094907 Eh       -5.9555 kcal/mol
    total RMSD        :           0.7677834 a0        0.4063 Å
   ------------------------------------------------------------------------

after finishing the optimization procedures, while in all other cases
that not exit in error

.. code::

     *** FAILED TO CONVERGE GEOMETRY OPTIMIZATION IN 500 ITERATIONS ***

will be printed, additionally a ``NOT_CONVERGED`` file is created in the
working directory, which might become handy for bulk jobs.

To get a geometry optimization to converge can be a hard job, usually
the xTB methods can repair a lot, you might want to start from GFN0-xTB
which does not have convergence issues and than improve with GFN2-xTB.
Maybe you have to adjust the geometry by hand again, if even this fails.

``xtb`` offers eight predefined levels for the geometry optimization,
which can be chosen by appending the level to the optimization flag as in

.. code:: bash

  > xtb coord --opt tight

The thresholds defined by simple keywords are given here

+---------+----------+--------------+----------+
|  level  | Econv/Eh | Gconv/Eh·α⁻¹ | Accuracy |
+---------+----------+--------------+----------+
| crude   | 5 × 10⁻⁴ | 1 × 10⁻²     | 3.00     |
+---------+----------+--------------+----------+  
| sloppy  | 1 × 10⁻⁴ | 6 × 10⁻³     | 3.00     |
+---------+----------+--------------+----------+
| loose   | 5 × 10⁻⁵ | 4 × 10⁻³     | 2.00     |
+---------+----------+--------------+----------+
| lax     | 2 × 10⁻⁵ | 2 × 10⁻³     | 2.00     |
+---------+----------+--------------+----------+
| normal  | 5 × 10⁻⁶ | 1 × 10⁻³     | 1.00     |
+---------+----------+--------------+----------+
| tight   | 1 × 10⁻⁶ | 8 × 10⁻⁴     | 0.20     |
+---------+----------+--------------+----------+
| vtight  | 1 × 10⁻⁷ | 2 × 10⁻⁴     | 0.05     |
+---------+----------+--------------+----------+
| extreme | 5 × 10⁻⁸ | 5 × 10⁻⁵     | 0.01     |
+---------+----------+--------------+----------+


The energy convergence (Econv) is the allowed change in the total energy
at convergence, while the gradient convergence (Gconv) is the
allowed change in the gradient norm at convergence. The accuracy
is handed to the singlepoint calculations for integral cutoffs and
self consistent field convergence criteria and is adjusted to fit
the geometry convergence thresholds automatically.

The xTB methods are completely analytical, so you can in principle
converge your results down to machine precision. Converging it
down to the lower limit is more a development feature than a
real life application but always possible.

Characterisation of Stationary Points
=====================================

In ``xtb`` second derivatives are implemented by finite differences methods
(numerical second derivatives). Normally you want to calculate the Hessian
directly after a successful geometry optimization, this is done by using

.. code:: bash

  > xtb coord --ohess

For the calculation on the input geometry use ``--hess`` instead.

Dealing with Small Imaginary Frequencies
----------------------------------------

For small imaginary modes ``xtb`` offers an automatic distortion feature
of these modes, say you have optimized a geometry and performed
a frequency calculation which leads to an imaginary frequency of
14 wavenumbers:

.. code::

  > xtb coord --ohess
   ...
             -------------------------------------------------
            |               Frequency Printout                |
             -------------------------------------------------
   projected vibrational frequencies (cm-1)
  eigval :       -0.00    -0.00     0.00     0.00     0.00     0.00
  eigval :      -14.26     8.12     9.26    12.09    15.85    17.73
  eigval :       19.45    28.85    39.18    41.30    64.61    71.84
   ...
  imag cut-off (cm-1) :    5.00
   found            1  significant imaginary frequency
   writing imag mode distorted coords to <xtbhess.coord>
   for further optimization.
   ...

In this case ``xtb`` will generate a distorted structure, you can continue to
optimize with

.. code::

  > xtb xtbhess.coord --ohess
   ...
             -------------------------------------------------
            |               Frequency Printout                |
             -------------------------------------------------
   projected vibrational frequencies (cm-1)
  eigval :       -0.00    -0.00    -0.00    -0.00     0.00     0.00
  eigval :        2.02     7.99    10.10    12.08    16.16    18.57
  eigval :       23.88    28.93    38.35    42.18    64.86    73.76
   ...

The optimization will only take a few steps and the artifical imaginary
frequency is gone after checking the frequency calculation.
